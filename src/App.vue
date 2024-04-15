<template>
  <Header />
  <div class="container">
    <Balance :total="total"/>
    <IncomeExpense :income="+income" :expenses="+expenses"/>
    <TransactionList :transactions="transactions" @btnDelete="handlerDelet"/>
    <AddTransaction @transactionSubmitted="handlerSubmitted"/>
  </div>
</template>

<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpense from './components/IncomeExpense.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';

  import { useToast } from 'vue-toastification';
  import { ref, computed, onMounted } from 'vue'

  const toast = useToast();

    const transactions = ref([]);

    // add localstorage
    onMounted(() => {
      const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
      if(savedTransactions){
        transactions.value = savedTransactions;
      }
    });

    // get total
    const total = computed(() => {
      return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.amount;
      }, 0)
    });

    // get income
    const income = computed(() => {
      return transactions.value
      .filter((transaction) => transaction.amount > 0)
      .reduce((acc, transaction) => {
        return acc + transaction.amount
      }, 0).toFixed(2)
    });

    // get expenses
    const expenses = computed(() => {
      return transactions.value
      .filter((transaction) => transaction.amount < 0)
      .reduce((acc, transaction) => {
        return acc + transaction.amount
      }, 0).toFixed(2)
    })

    // add transaction
    const handlerSubmitted = (transactionData) => {
      transactions.value.push({
        id: generateUniqueId(),
        Text: transactionData.text, 
        amount: transactionData.price
      });
      saveTransactionsLocalStorage();
      toast.success('Transação adicionada')
    }

    // generate id
    const generateUniqueId = () => {
      return Math.floor(Math.random() * 1000)
    }

    // delete transaction
    const handlerDelet = (id) => {
      transactions.value = transactions.value.filter((transaction) => transaction.id !== id);
      saveTransactionsLocalStorage();
      toast.success('Transação deletada')
    };

    // save to localstorage
    const saveTransactionsLocalStorage = () => {
      localStorage.setItem('transactions', JSON.stringify(transactions.value) || []);
    }

</script>
