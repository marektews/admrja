<script setup>
import { ref, computed, watch } from 'vue'

const props = defineProps({
    modelValue: { type: Number },
    zbory: { type: Array },
    sra: { type: Array },
})
watch(() => props.modelValue, (nv) => selected.value = nv)

const emit = defineEmits(['update:modelValue'])

const all_buses = computed(() => {
    let res = []
    if(props.sra.length) {
        props.sra.forEach((elem) => {
            let tmp = { 
                sra: elem,
                zbor: props.zbory.find((z) => z.id === elem.zbor_id)
            }
            res.push(tmp)
        })
        res.sort((a, b) => {
            let tmp = sort_helper(a.zbor.lang, b.zbor.lang)
            if(tmp !== 0) return tmp
            tmp = sort_helper(a.zbor.name, b.zbor.name)
            if(tmp !== 0) return tmp
            return a.sra.lp - b.sra.lp
        })
    }
    return res
})

function sort_helper(a, b) {
    if(a < b) return -1
    if(a > b) return 1
    return 0
}

function opt_format(item) {
    if(item.sra.lp === null)
        return `${item.zbor.lang} | ${item.zbor.number} | ${item.zbor.name}`
    else
        return `${item.zbor.lang} | ${item.zbor.number} | ${item.zbor.name} (${ item.sra.lp })`
}

const selected = ref(props.modelValue)
watch(selected, (nv) => emit('update:modelValue', nv))
</script>

<template>
    <select v-model="selected" class="form-select">
        <option v-for="(item, index) in all_buses" :key="index" :value="item.sra.id">
            {{ opt_format(item) }}
        </option>
    </select>
</template>