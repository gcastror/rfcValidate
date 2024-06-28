<!--
An example of creating a reusable grid component and using it with external data.
-->

<script setup lang="ts">
import DemoGrid from './Grid.vue'
import data from "../data/data.json";
import { ref } from 'vue';
import { CForm, CFormInput, CAlert, CButton, CRow, CCol, CContainer } from '@coreui/vue';

const props = defineProps({
  liveErrorVisible: Boolean,
  liveOkVisible: Boolean,
})

const searchQuery = ref('')
const liveErrorVisible = ref(false)
const liveOkVisible = ref(false)
const errorMessage = ref('')

const gridColumns = ['Id', 'Empresa', 'RFC', 'Status']
const gridData = data;

const search = () => {

  const valueToFind = searchQuery.value.trim();
  const result = data.filter((item) => item.RFC.toUpperCase() == valueToFind.toUpperCase() && item.Status === true)

  if (valueToFind === '') {
    liveOkVisible.value = false;
    liveErrorVisible.value = true;
    errorMessage.value = `Debes ingresar un RFC a validar`
    return;
  }

  if (result.length > 0) {
    liveOkVisible.value = true;
    liveErrorVisible.value = false;
    errorMessage.value = '';
  }
  else {
    liveOkVisible.value = false;
    liveErrorVisible.value = true;
    errorMessage.value = `El RFC ${searchQuery.value} No existe en el sistema`
  }

}
</script>

<template>
  <CForm>

    <CAlert class="d-flex align-items-center" color="danger" :visible="liveErrorVisible" dismissible
      @close="() => { liveErrorVisible = false }">
      <CIcon icon="cil-warning" class="flex-shrink-0 me-2" width="24" height="24" />
      <div>
        {{ errorMessage }}
      </div>
    </CAlert>

    <CAlert class="d-flex align-items-center" color="success" :visible="liveOkVisible" dismissible
      @close="() => { liveOkVisible = false }">
      <CIcon icon="cil-check-circle" class="flex-shrink-0 me-2" width="24" height="24" />
      <div>
        Se ha enontrado el Proveedor con RFC {{ searchQuery }} validado y registrado en el sistema
      </div>
    </CAlert>

    <CRow class="g-3">
      <CCol xs>
        <CFormInput type="text" id="searchInput" v-model="searchQuery" placeholder="Ingresa el RFC a validar"
          aria-describedby="searchInputHelpInline" />
      </CCol>
      <CCol xs>
        <CButton color="secondary" size="sm" @click="search">Buscar RFC</CButton>
      </CCol>
    </CRow>

    º
    <CContainer lg fluid>
      <DemoGrid :data="gridData" :columns="gridColumns" :filter-key="searchQuery">
      </DemoGrid>
    </CContainer>





  </CForm>
</template>