<template>
  <Header />
  <div class="container">
    <Balance :total="total"/>
    <IncomeExpense :income="+income" :expenses="+expenses"/>
    <TransactionList :transactions="transactions"/>
    <AddTransaction @transactionSubmitted="handlerSubmitted"/>
  </div>
</template>

<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpense from './components/IncomeExpense.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';

  import { ref, computed } from 'vue'

    const transactions = ref([
        { id: 1, Text: 'Ifood', amount: -23.5 },
        { id: 2, Text: 'Vale', amount: 250 },
        { id: 3, Text: 'Avon', amount: -30 }
    ])

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
        text: transactionData.text, 
        amount: transactionData.price
      })
    }

    // generate id
    const generateUniqueId = () => {
      return Math.floor(Math.random() * )
    }

</script>
