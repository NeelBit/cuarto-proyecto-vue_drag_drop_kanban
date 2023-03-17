<script setup>

    import { ref, defineEmits } from 'vue';

    const text = ref("");
    const emits = defineEmits(["onNewItem"]);

    function handleSubmit() {
        if (text != "") {
            // entonces mandar a llamar un $emit
            emits("onNewItem", text);
            text.value = "";
        }
    }

</script>

<template>

    <form action="" @submit.prevent="handleSubmit">
        <input class="inputNew" type="text" name="inputNew" id="inputNew" v-model="text" placeholder="Escriba nombre de tarea">
    </form>

</template>

<style scoped>

    .inputNew {
        width: 100%;

        border: none;
        padding: .5rem;
        text-align: center;
        outline: none;
        border-radius: var(--redondeo);

        font-weight: lighter;
        
    }

</style>