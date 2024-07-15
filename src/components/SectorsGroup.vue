<script setup>
import { watch, ref, nextTick } from 'vue'

const emit = defineEmits(['selected'])

const props = defineProps({
    sectors: { type: Array, required: true },
})
const _sectors = ref(undefined)

watch(() => props.sectors, (nv) => {
    _sectors.value = undefined
    nextTick(() => {
        _sectors.value = nv
    })
})
</script>

<template>
    <div class="btn-group" role="group">
        <template v-for="(sector, index) in _sectors" :key="index">
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