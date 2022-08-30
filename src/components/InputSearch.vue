<template>
    <div class="search-control-group">
        <input placeholder="Поиск" class="search-control" type="search" v-model="props.modelValue" @input="emit('update:modelValue', $event.target.value)" />
        <svg class="search-icon" width="17" height="17" viewBox="0 0 17 17" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M11.4142 11L15.6569 15.2426M13 7C13 10.3137 10.3137 13 7 13C3.68629 13 1 10.3137 1 7C1 3.68629 3.68629 1 7 1C10.3137 1 13 3.68629 13 7Z" stroke="#8E9CBB" stroke-width="2" stroke-linecap="round"/>
        </svg>

        <button  @click="emit('update:modelValue', '')" type="button" class="search-control-clear" :class="{'search-control-clear_show': props.modelValue.length > 0}">
            <svg width="12" height="12" viewBox="0 0 12 12" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M0.325608 10.746C-0.0649162 10.3555 -0.0649162 9.72233 0.325608 9.33181L9.36312 0.292893C9.75364 -0.0976313 10.3868 -0.0976309 10.7773 0.292893C11.1679 0.683418 11.1679 1.31658 10.7773 1.70711L1.73982 10.746C1.3493 11.1365 0.716132 11.1365 0.325608 10.746Z" fill="#FF238D"/>
                <path d="M10.6836 10.6835C11.0741 10.293 11.0741 9.65986 10.6836 9.26933L6.19533 4.78111L1.70711 0.292893C1.31658 -0.0976313 0.683417 -0.0976309 0.292893 0.292893C-0.0976311 0.683418 -0.0976311 1.31658 0.292893 1.70711L9.26935 10.6835C9.65987 11.0741 10.293 11.0741 10.6836 10.6835Z" fill="#FF238D"/>
            </svg>
        </button>
    </div>
</template>

<script setup>
    import { defineProps, defineEmits } from 'vue'

    const emit = defineEmits(['modelValue'])

    const props = defineProps({
        modelValue: {
            type: String
        }
    })
</script>

<style scoped>

</style>