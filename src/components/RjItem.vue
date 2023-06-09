<script setup>
import { computed } from 'vue'
import ZborSelector from './ZborSelector.vue'

defineEmits(['delete'])

const props = defineProps({
    index: { type: Number },
    rjItem: { type: Object },
    zbory: { type: Array },
    sra: { type: Array },
})

const _zbor_id = computed({
    get() {
        // console.log('_zbor_id : get()', props.rjItem)
        let tmp = props.sra?.find((item) => item.id === props.rjItem.sra_id)
        return tmp?.zbor_id
    },
    set(zbor_id) {
        // console.log('Select zbor id:', zbor_id)
        let tmp = props.sra.find((item) => item.zbor_id === zbor_id)
        props.rjItem.sra_id = tmp.id
    }
})

const _d1 = computed({
    get() { return props.rjItem.d1 },
    set(v) { props.rjItem.d1 = v }
})
const _d2 = computed({
    get() { return props.rjItem.d2 },
    set(v) { props.rjItem.d2 = v }
})
const _d3 = computed({
    get() { return props.rjItem.d3 },
    set(v) { props.rjItem.d3 = v }
})
</script>

<template>
    <tr>
        <th scope="row">{{ index }}</th>
        <td>
            <ZborSelector 
                v-model="_zbor_id"
                :zbory="props.zbory"
            />
        </td>
        <td>
            <input v-model="_d1" type="time" class="form-control" required />
        </td>
        <td>
            <input v-model="_d2" type="time" class="form-control" required />
        </td>
        <td>
            <input v-model="_d3" type="time" class="form-control" required />
        </td>
        <td>
            <button class="btn btn-danger" title="Kasowanie" @click="$emit('delete')">
                <i class="fa-solid fa-trash" />
            </button>
        </td>
    </tr>
</template>

<style scoped>
.ops-layout {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    gap: 6pt;
}
</style>