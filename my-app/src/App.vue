<template>
  <div>

    <Form @refresh="loadRecords" />
    <Records ref="recordsComponent" />
  </div>
</template>

<script setup>
import {ref} from 'vue'
import Form from './components/Forms.vue'
import Records from './components/Records.vue'

const recordsComponent = ref(null)

const loadRecords = () => {
  recordsComponent.value?.loadData?.()
}
</script>

<style scoped>  


body {
  margin: 0;
  padding: 0;
  background: linear-gradient(135deg, #1c1c24, #0a0a0f);
  font-family: 'Segoe UI', sans-serif;
}


</style>