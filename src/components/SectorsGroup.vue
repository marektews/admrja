<script setup>
import { watch, ref } from 'vue'

const emit = defineEmits(['selected'])

const props = defineProps({
    terminal: { type: [Object, undefined] },
})
const sectors = ref(undefined)      // lista sektorów terminala

function loadSectors(terminal) {
    fetch(`/api/rja/sectors/${terminal.tid}`)
    .then(response => response.json())
    .then(d => {
        sectors.value = d
        console.log('Get sectors of terminal:', terminal, 'list:', d)
    })
    .catch(err => console.error('Get sectors of terminal:', terminal, 'error:', err))
}

watch(() => props.terminal, (nv) => {
    if (nv) {
        loadSectors(nv)
    } else {
        sectors.value = []
    }
})
</script>

<template>
    <div class="btn-group" role="group">
        <template v-for="(sector, index) in sectors" :key="index">
            <input 
                type="radio" 
                class="btn-check"
                name="sectors" 
                :id="`sector${index}`" 
                autocomplete="off"
            />
            <label 
                class="btn btn-outline-success" 
                :for="`sector${index}`"
                @click="emit('selected', sector)"
            >
                {{ sector.name.replace('x','') }}
            </label>
        </template>
    </div>
</template>