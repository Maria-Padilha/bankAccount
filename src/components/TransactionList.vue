<template>
    <h3>Histórico</h3>
    <ul id="list" class="list">
        <li v-for="transaction in transactions" :key="transaction.id" 
        :class="transaction.amount < 0 ? 'minus' : 'plus'">
            {{ transaction.Text }} <span>R$ {{ transaction.amount }}</span>
            <button @click="deleteTransaction(transaction.id)" class="delete-btn">X</button>
        </li>
    </ul>
</template>

<script setup>
    import { defineProps, defineEmits } from 'vue';

    const emit = defineEmits(['btnDelete'])

    const props = defineProps({
        transactions: {
            type: Array, 
            required: true
        }
    })

    const deleteTransaction = (id) => {
        emit('btnDelete', id)
    }

</script>