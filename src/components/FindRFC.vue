<!--
An example of creating a reusable grid component and using it with external data.
-->

<script setup lang="ts">
import DemoGrid from './Grid.vue'
import data from "../data/data.json";
import { ref } from 'vue';
import { CAlert, CButton } from '@coreui/vue';

import { CIcon } from '@coreui/icons-vue';


const searchQuery = ref('')
const gridColumns = ['Empresa', 'RFC']
const gridData = data;
</script>

<template>
  <form id="search">

    <CAlert class="d-flex align-items-center" color="danger" :visible="liveExampleVisible" dismissible
      @close="() => { liveExampleVisible = false }">
      <CIcon icon=" cil-check-circle" class="flex-shrink-0 me-2" width="24" height="24" />
      <div>
        An example success alert with an icon
      </div>
    </CAlert>

    <CAlert class="d-flex align-items-center" color="success" :visible="liveExampleVisible" dismissible
      @close="() => { liveExampleVisible = false }">
      <CIcon icon="cil-check-circle" class="flex-shrink-0 me-2" width="24" height="24" />
      <div>
        An example success alert with an icon
      </div>
    </CAlert>

    <input name="query" v-model="searchQuery">

    <CButton color="secondary" size="lg" click="() => { liveExampleVisible = true }">Buscar RFC</CButton>

  </form>
  <DemoGrid :data="gridData" :columns="gridColumns" :filter-key="searchQuery">
  </DemoGrid>
</template>