<script setup>
const props = defineProps({
    terminals: { type: Array, required: true },
})
const emit = defineEmits(['selected'])
</script>

<template>
    <div class="btn-group" role="group">
        <template v-for="(terminal, index) in props.terminals" :key="index">
            <input 
                type="radio" 
                class="btn-check"
                name="terminals" 
                :id="`terminal${index}`" 
                autocomplete="off"
            >
            <label 
                class="btn btn-outline-primary" 
                :for="`terminal${index}`"
                @click="emit('selected', terminal)"
            >
                {{ terminal.name }}
            </label>
        </template>
    </div>
</template>