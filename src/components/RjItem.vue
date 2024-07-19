<script setup>
import { computed } from 'vue'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faTrash } from '@fortawesome/free-solid-svg-icons';
import BusSelector from './BusSelector.vue'

const emit = defineEmits(['change', 'delete'])

const props = defineProps({
    rjItem: { type: Object },
    zbory: { type: Array },
    sra: { type: Array },
})

const _sra_id = computed({
    get()  { return props.rjItem.sra_id },
    set(v) { 
        let tmp = props.rjItem
        tmp.sra_id = v
        emit('change', tmp) 
    }
})

// flaga anulowania
const _canceled = computed({
    get() { return props.rjItem.canceled },
    set(v) { 
        let tmp = props.rjItem
        tmp.canceled = v
        emit('change', tmp)
    }
})

// odjazdy
const _d1 = computed({
    get() { return props.rjItem.d1 },
    set(v) { 
        let tmp = props.rjItem
        tmp.d1 = v
        emit('change', tmp)
    }
})
const _d2 = computed({
    get() { return props.rjItem.d2 },
    set(v) { 
        let tmp = props.rjItem
        tmp.d2 = v
        emit('change', tmp)
    }
})
const _d3 = computed({
    get() { return props.rjItem.d3 },
    set(v) { 
        let tmp = props.rjItem
        tmp.d3 = v
        emit('change', tmp)
    }
})

//podstawienia
const _a1 = computed({
    get() { return props.rjItem.a1 },
    set(v) {
        let tmp = props.rjItem
        tmp.a1 = v
        emit('change', tmp)
    }
})
const _a2 = computed({
    get() { return props.rjItem.a2 },
    set(v) {
        let tmp = props.rjItem
        tmp.a2 = v
        emit('change', tmp)
    }
})
const _a3 = computed({
    get() { return props.rjItem.a3 },
    set(v) {
        let tmp = props.rjItem
        tmp.a3 = v
        emit('change', tmp)
    }
})
</script>

<template>
    <tr>
        <th scope="row">{{ props.rjItem?.tura }}</th>
        <td>
            <div class="bus-layout">
                <input v-model="_canceled" type="checkbox" class="form-check" />
                <BusSelector 
                    v-model="_sra_id"
                    :zbory="props.zbory"
                    :sra="props.sra"
                />
            </div>
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
            <button class="btn btn-danger" title="Kasowanie" @click="emit('delete')">
                <FontAwesomeIcon :icon="faTrash" />
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

.bus-layout {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    gap: 3pt;
    align-items: center;
}
</style>