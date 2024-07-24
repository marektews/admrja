<template>
    <div class="">
        <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="tura" id="tura_w2" value="2" v-model="selected">
            <label class="form-check-label" for="tura_w2">W2</label>
        </div>
        <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="tura" id="tura_w3" value="3" v-model="selected">
            <label class="form-check-label" for="tura_w3">W3</label>
        </div>
    </div>
</template>

<script setup>
import { ref, watch } from 'vue'

const emit = defineEmits(['selected'])

const selected = ref('2')
watch(selected, (nv) => emit('selected', nv))
</script>