<template>
    <h3>Add new transacation</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" v-model="text" placeholder="Enter text..." />
        </div>
        <div class="form-control">
            <label for="amount"
                >Amount <br />
                (negative - expense, positive - income)</label
            >
            <input type="number" id="amount" v-model="amount" placeholder="Enter amount..." />
        </div>
        <button class="btn">Add transaction</button>
    </form>
</template>
<script setup>
import {ref} from 'vue';
import { useToast } from "vue-toastification";




const text = ref('');
const amount = ref(0);
const emit = defineEmits(['transactionSubmitted']);

const onSubmit = () => {
    if(!text.value || !amount.value){
        useToast().error('Please enter text and amount');
        return;
    }

    const transactionData = {
        id: Math.floor(Math.random() * 100000000),
        text: text.value,
        amount: amount.value
    }

    emit('transactionSubmitted', transactionData);

}
</script>