<script setup>
import { reactive } from 'vue'
const emit = defineEmits(['ok'])

const timings = reactive({
    d1: localStorage.getItem("timing-d1") || '', 
    d2: localStorage.getItem("timing-d2") || '', 
    d3: localStorage.getItem("timing-d3") || '', 
    step: localStorage.getItem("timing-step") || 10
})

function onOK() {
    localStorage.setItem("timing-d1", timings.d1)
    localStorage.setItem("timing-d2", timings.d2)
    localStorage.setItem("timing-d3", timings.d3)
    localStorage.setItem("timing-step", timings.step)
    emit('ok', timings)
}
</script>

<template>
    <div class="modal modal-dark" tabindex="-1">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Automat czasowy</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Zamknij"></button>
                </div>
                <div class="modal-body">
                    <p>Podaj czasy odjazdu pierwszej tury</p>
                    <p class="timings-layout">
                        <div>Piątek:</div>
                        <input type="time" class="form-control" v-model="timings.d1">
                        
                        <div>Sobota:</div>
                        <input type="time" class="form-control" v-model="timings.d2">
                        
                        <div>Niedziela:</div>
                        <input type="time" class="form-control" v-model="timings.d3">

                        <div>Odstęp pomiędzy turami (minuty):</div>
                        <input type="number" class="form-control" v-model.number="timings.step">
                    </p>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Zamknij</button>
                    <button type="button" class="btn btn-primary" data-bs-dismiss="modal"
                        @click="onOK"
                        :disabled="timings.d1.length==0 || timings.d2.length==0 || timings.d3.length==0"
                    >
                        Ustaw czasy sektora
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<style>
@media (prefers-color-scheme: dark) {
    div.modal-content {
        background-color: var(--color-background) !important;
    }

    .btn-close {
        color: var(--color-text) !important;
    }
}

.timings-layout {
    display: grid;
    grid-template-columns: auto auto;
    align-items: center;
    gap: 2pt 6pt;
}
</style>