<script setup>
import { computed } from 'vue'
import BusSelector from './BusSelector.vue'

defineEmits(['delete'])

const props = defineProps({
    rjItem: { type: Object },
    zbory: { type: Array },
    sra: { type: Array },
})

const _sra_id = computed({
    get()  { return props.rjItem.sra_id },
    set(v) { props.rjItem.sra_id = v }
})

// odjazdy
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

//podstawienia
const _a1 = computed({
    get() { return props.rjItem.a1 },
    set(v) { props.rjItem.a1 = v }
})
const _a2 = computed({
    get() { return props.rjItem.a2 },
    set(v) { props.rjItem.a2 = v }
})
const _a3 = computed({
    get() { return props.rjItem.a3 },
    set(v) { props.rjItem.a3 = v }
})
</script>

<template>
    <tr>
        <th scope="row">{{ props.rjItem?.tura }}</th>
        <td>
            <BusSelector 
                v-model="_sra_id"
                :zbory="props.zbory"
                :sra="props.sra"
            />
        </td>
        <td>
            <div class="times-layout">
                <input v-model="_a1" type="time" class="form-control" required /> /
                <input v-model="_d1" type="time" class="form-control" required />
            </div>
        </td>
        <td>
            <div class="times-layout">
                <input v-model="_a2" type="time" class="form-control" required /> /
                <input v-model="_d2" type="time" class="form-control" required />
            </div>
        </td>
        <td>
            <div class="times-layout">
                <input v-model="_a3" type="time" class="form-control" required /> /
                <input v-model="_d3" type="time" class="form-control" required />
            </div>
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

.times-layout {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    gap: 1pt;
    align-items: center;
}
</style>