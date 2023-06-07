<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
    modelValue: { type: Number },
    zbory: { type: Array },
})
watch(() => props.modelValue, (nv) => selected.value = nv)

const emit = defineEmits(['update:modelValue'])

const selected = ref(props.modelValue)
watch(selected, (nv) => emit('update:modelValue', nv))
</script>

<template>
    <select v-model="selected" class="form-select">
        <option v-for="(zbor, index) in zbory" :key="index"
            :value="zbor.id"
        >
            {{ zbor.lang }} - {{ zbor.number }} - {{ zbor.name }}
        </option>
    </select>
</template>