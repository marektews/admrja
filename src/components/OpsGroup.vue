<script setup>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faPlus, faClock, faSave } from '@fortawesome/free-solid-svg-icons';
const emit = defineEmits(['add', 'save', 'timing'])
</script>

<template>
    <div class="btn-group">
        <button class="btn btn-outline-primary" @click="emit('add')">
            <FontAwesomeIcon :icon="faPlus" />
            Dodaj autokar
        </button>

        <button class="btn btn-outline-primary" @click="emit('timing')">
            <FontAwesomeIcon :icon="faClock" />
            Timing magic
        </button>

        <button class="btn btn-outline-primary" @click="emit('save')">
            <FontAwesomeIcon :icon="faSave" />
            Zapis ustawień
        </button>
    </div>
</template>