<script setup>
defineEmits(['add', 'save', 'timing'])
</script>

<template>
    <div class="btn-group">
        <button class="btn btn-outline-primary" @click="$emit('add')">
            <i class="fa-solid fa-plus" />
            Dodaj autokar
        </button>

        <button class="btn btn-outline-primary" @click="$emit('timing')">
            <i class="fa-solid fa-clock" />
            Timing magic
        </button>

        <button class="btn btn-outline-primary" @click="$emit('save')">
            <i class="fa-solid fa-save" />
            Zapis ustawień
        </button>
    </div>
</template>