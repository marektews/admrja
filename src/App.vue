<script setup>
import { ref, onMounted } from 'vue'
import Terminals from './components/Terminals.vue'
import Sectors from './components/Sectors.vue'
import RjItem from './components/RjItem.vue'
import Ops from './components/Ops.vue'
import TimingModal from './components/TimingModal.vue'
import Toasts from './components/Toasts.vue'

const zbory = ref([])
const sra = ref([])
const terminals = ref([])
const sectors = ref([])
const select_sector = ref(undefined)
const sector_buses = ref(undefined)
const loading = ref([true, true, true])
const errors = ref([false, false, false])

onMounted(() => {
    fetch('/api/rja/terminals')
    .then(response => response.json())
    .then(d => {
        terminals.value = d
        loading.value[0] = false
    })
    .catch(err => {
        errors.value[0] = true
        console.error('Load terminals error:', err)
    })

    fetch('/api/rja/zbory')
    .then(response => response.json())
    .then(d => {
        zbory.value = d
        loading.value[1] = false
    })
    .catch(err => {
        errors.value[1] = true
        console.error('Get congregations list error:', err)
    })

    fetch('/api/rja/sra')
    .then(response => response.json())
    .then(d => {
        sra.value = d
        loading.value[2] = false
    })
    .catch(err => {
        errors.value[2] = true
        console.error('Load registered buses error:', err)
    })
})

function onTerminalSelected(terminal) {
    console.log('Terminal selected:', terminal)
    fetch(`/api/rja/sectors/${terminal.tid}`)
    .then(response => response.json())
    .then(d => {
        sectors.value = d
        sector_buses.value = undefined
        console.log('Get sectors of terminal:', terminal, 'list:', d)
    })
    .catch(err => console.error('Get sectors of terminal:', terminal, 'error:', err))
}

function onSectorSelected(sector) {
    console.log('Sector selected:', sector)
    select_sector.value = sector
    load_rja(sector)
}

function load_rja(sector) {
    fetch(`/api/rja/buses/${sector.sid}`)
    .then(response => response.json())
    .then(d => {
        sector_buses.value = d
        console.log('Get buses of sector:', sector, "list:", d)
    })
    .catch(err => console.error('Get buses of sector:', sector, 'error:', err))
}

function find_zbor_id(sra_id) {
    if(sra_id == -1) return {}
    let s = sra.value.find((item) => item.id === sra_id)
    return s.zbor_id
}

function find_zbor(sra_id) {
    let zbor_id = find_zbor_id(sra_id)
    return zbory.value.find((zbor) => zbor.id === zbor_id)
}

function onAddItem() {
    sector_buses.value.push({
        id: -1,
        sid: select_sector.value.sid,
        sra_id: -1,
        d1: "",
        d2: "",
        d3: ""
    })
}

function onSave() {
    let data = {
        sid: select_sector.value.sid,
        rja: sector_buses.value
    }
    console.log('onSave:', data)
    fetch('/api/rja/buses/save', {
        method: "POST",
        headers: {
            "Content-Type": "application/json"
        },
        body: JSON.stringify(data)
    })
    .then(response => {
        if(response.status === 200) {
            console.log('RJA: save buses of sector:', select_sector.value.sid, 'OK')
            load_rja(select_sector.value)
            const toast = new bootstrap.Toast("#saveOk")
            toast.show()
        }
        else {
            console.error('RJA: save buses of sector:', select_sector.value.sid, 'response:', response)
            const toast = new bootstrap.Toast("#saveError")
            toast.show()
        }
    })
    .catch(err => console.error('RJA: save buses of sector:', select_sector.value.sid, 'exception:', err))
}

function onShowTimingModal() {
    const modal = new bootstrap.Modal('#timingModal', {})
    modal.show()
}

function onModifyTiming(timings) {
    console.log('onModifyTiming:', timings)
    let time_d1 = _create_time(timings.d1)
    let time_d2 = _create_time(timings.d2)
    let time_d3 = _create_time(timings.d3)
    let step = parseInt(timings.step)*60
    let delay = 0
    sector_buses.value.forEach((item) => {
        item.d1 = _add_time(time_d1, delay)
        item.d2 = _add_time(time_d2, delay)
        item.d3 = _add_time(time_d3, delay)
        delay += step
    })
}

function _create_time(time_str) {
    let d = new Date()
    let sl = time_str.split(':')
    return new Date(d.getFullYear(), d.getMonth(), d.getDate(), parseInt(sl[0]), parseInt(sl[1]))
}

function _add_time(time, seconds) {
    let t = new Date()
    t.setTime(time.getTime() + seconds*1000)
    return time_format(t)
}

function time_format(d) {
    let hours = format_two_digits(d.getHours())
    let minutes = format_two_digits(d.getMinutes())
    return hours + ":" + minutes
}

function format_two_digits(n) {
    let s = n<10 ? `0${n}` : `${n}`
    return s
}
</script>

<template>
    <div class="container">
        <header>
            <h2>Rozkłady jazdy autokarów</h2>
            <Terminals :terminals="terminals" @selected="onTerminalSelected" />
            <Sectors :sectors="sectors" @selected="onSectorSelected" />
        </header>

        <main v-if="loading.includes(true)">
            Trwa ładowanie danych ...
        </main>
        <main v-else-if="sector_buses == undefined">
            Wybierz terminal i sektor.
        </main>
        <main v-else>
            <table class="table table-dark table-bordered">
                <thead>
                    <tr>
                        <th scope="col">#</th>
                        <th scope="col">Zbór</th>
                        <th scope="col">Piątek</th>
                        <th scope="col">Sobota</th>
                        <th scope="col">Niedziela</th>
                        <th scope="col">Ops</th>
                    </tr>
                </thead>
                <tbody>
                    <RjItem v-for="(item, index) in sector_buses" :key="index" 
                        :index="index+1"
                        :zbory="zbory"
                        :sra="sra"
                        :rj-item="item"
                        @delete="sector_buses.splice(index,1)"
                    />
                </tbody>
                <tfoot>
                    <tr>
                        <td colspan="6">
                            <Ops 
                                @add="onAddItem"
                                @save="onSave"
                                @timing="onShowTimingModal"
                            />
                        </td>
                    </tr>
                </tfoot>
            </table>

            <TimingModal 
                id="timingModal"
                @ok="onModifyTiming"
            />

            <Toasts />
        </main>
    </div>
</template>

<style scoped>
header {
    line-height: 1.5;
    display: flex;
    flex-direction: column;
    gap: 2pt;
    place-items: center;
}

main {
    margin-top: 6pt;
}

tr {
    vertical-align: middle;
}
</style>
