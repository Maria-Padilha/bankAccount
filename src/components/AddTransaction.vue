<template>
    <h3>Nova Transação</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Informe a transação:</label>
            <input v-model="text" type="text" id="text" placeholder="Conta de luz...">
        </div>
        <div class="form-control">
            <label for="amount" class="price">Preço:</label>
            <p class="small">(Negativo: dívida / positivo: lucro)</p>
            <input v-model="price" type="number" id="amount" placeholder="400">
        </div>
        <button class="btn">Adicionar</button>
    </form>
</template>

<script setup>
    import { ref, defineEmits } from 'vue';
    import { useToast } from 'vue-toastification'

    const text = ref('');
    const price = ref('');

    const emit = defineEmits(['transactionSubmitted'])

    const toast = useToast();

    const onSubmit = () => {
        if(!text.value || !price.value){
            toast.error('Campos sem preecher!')
            return
        }

        const transactionData = {
            text: text.value,
            price: parseFloat(price.value)
        }

        emit('transactionSubmitted', transactionData)

        text.value = '';
        price.value = '';
    }
</script>