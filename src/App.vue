<script setup>
import { ref, reactive } from 'vue'
import InputSearch from './components/InputSearch.vue'
import DocList from './components/DocList.vue'

  const searchKey = ref('')


</script>

<template>
  <header class="header">
      <h2 class="header__title">Документы</h2>
      <div class="header__buttons">
        <button type="button" class="btn btn_circle">
          <svg width="12" height="15" viewBox="0 0 12 15" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M11 13.8571L6 10.2857L1 13.8571V2.42857C1 2.04969 1.15051 1.68633 1.41842 1.41842C1.68633 1.15051 2.04969 1 2.42857 1H9.57143C9.95031 1 10.3137 1.15051 10.5816 1.41842C10.8495 1.68633 11 2.04969 11 2.42857V13.8571Z"
                  stroke="#8E9CBB" stroke-width="2"
                  stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
        <button type="button" class="btn btn_pills">
          <svg width="14" height="15" viewBox="0 0 14 15" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M6.99697 14.0083C6.44681 14.0083 6.00082 13.5595 6.00082 13.0059L5.99983 1.01071C5.99983 0.457096 6.44582 0.00830078 6.99599 0.00830078C7.54615 0.00830078 7.99215 0.457097 7.99215 1.01071L7.99313 13.0059C7.99313 13.5595 7.54714 14.0083 6.99697 14.0083Z" fill="#0066FF"/>
            <path d="M14 7.01688C14 6.46326 13.554 6.01447 13.0038 6.01447L0.996158 6.01448C0.445995 6.01448 0 6.46327 0 7.01689C0 7.57051 0.445995 8.0193 0.996159 8.0193L13.0038 8.0193C13.554 8.0193 14 7.5705 14 7.01688Z" fill="#0066FF"/>
          </svg> <span class="btn__text">Новый тип</span>
        </button>
        <button type="button" class="btn btn_pills">
          <svg width="14" height="15" viewBox="0 0 14 15" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M6.99697 14.0083C6.44681 14.0083 6.00082 13.5595 6.00082 13.0059L5.99983 1.01071C5.99983 0.457096 6.44582 0.00830078 6.99599 0.00830078C7.54615 0.00830078 7.99215 0.457097 7.99215 1.01071L7.99313 13.0059C7.99313 13.5595 7.54714 14.0083 6.99697 14.0083Z" fill="#0066FF"/>
            <path d="M14 7.01688C14 6.46326 13.554 6.01447 13.0038 6.01447L0.996158 6.01448C0.445995 6.01448 0 6.46327 0 7.01689C0 7.57051 0.445995 8.0193 0.996159 8.0193L13.0038 8.0193C13.554 8.0193 14 7.5705 14 7.01688Z" fill="#0066FF"/>
          </svg> <span class="btn__text">Новый документ</span>
        </button>
      </div>
  </header>
  <div class="search-block">
      <InputSearch v-model="searchKey"/>
  </div>
  <DocList :search-key="searchKey" />
</template>

<style scoped>

</style>
