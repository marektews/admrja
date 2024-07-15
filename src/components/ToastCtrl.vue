<template>
    <div class="toast-container">
        <div 
            id="saveOk" 
            class="toast position-relative bottom-0 start-50 bg-success text-white"
            role="alert" 
            aria-atomic="true" 
            aria-live="assertive"
        >
            <div class="toast-header">
                <FontAwesomeIcon :icon="faCircleInfo" />
                <strong class="ms-2 me-auto">Zapis rozkładu jazdy</strong>
                <button type="button" class="btn-close" data-bs-dismiss="toast" aria-label="Close"></button>
            </div>
            <div class="toast-body">
                Zapis zakończony pomyślnie.
            </div>
        </div>

        <div 
            id="saveError" 
            class="toast position-relative bottom-0 start-50 bg-danger text-white"
            role="alert" 
            aria-atomic="true" 
            aria-live="assertive"
        >
            <div class="toast-header">
                <FontAwesomeIcon :icon="faCircleInfo" />
                <strong class="ms-2 me-auto">Zapis rozkładu jazdy</strong>
                <button type="button" class="btn-close" data-bs-dismiss="toast" aria-label="Close"></button>
            </div>
            <div class="toast-body">
                Zapis zakończony z błędem, za chwilę spróbuj ponownie.
            </div>
        </div>
    </div>
</template>

<script setup>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faCircleInfo } from '@fortawesome/free-solid-svg-icons';
</script>