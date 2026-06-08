<script setup>
import { ref, onMounted } from 'vue'

const terminals = ref([])
const emit = defineEmits(['selected'])

onMounted(() => {
    loadTerminals()
})

function loadTerminals() {
    fetch('/api/rja/terminals')
    .then(response => response.json())
    .then(d => {
        terminals.value = d
    })
    .catch(err => {
        console.error('Load terminals error:', err)
    })    
}

</script>

<template>
    <div class="btn-group" role="group">
        <template v-for="(terminal, index) in terminals" :key="index">
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