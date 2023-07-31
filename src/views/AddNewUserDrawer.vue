<script setup>
import { Portuguese } from 'flatpickr/dist/l10n/pt.js'
import { PerfectScrollbar } from 'vue3-perfect-scrollbar'

const props = defineProps({
  isDrawerOpen: {
    type: Boolean,
    required: true,
  },
})

const emit = defineEmits([
  'update:isDrawerOpen',
  'updateFilterValues',
])

const filterValues = ref({
  id: '',
  code: '',
  document: '',
  origem: '',
  email: '',
  telefone: '',
})

const closeNavigationDrawer = () => {
  emit('update:isDrawerOpen', false)
}

const handleDrawerModelValueUpdate = val => {
  emit('update:isDrawerOpen', val)
}

const clearFilters = () => {
  filterValues.value = {
    id: '',
    code: '',
    date: '',
    email: '',
    phone: '',
    origem: null,
    document: '',
  }
}

watch(filterValues, (newVal, oldVal) => {
  emit('updateFilterValues', newVal)
}, { deep: true })

const flatPickerConfig = {
  mode: 'range',
  dateFormat: 'd/m/Y',
  locale: Portuguese,
}
</script>

<template>
  <VNavigationDrawer
    temporary
    :width="400"
    location="end"
    class="scrollable-content"
    :model-value="props.isDrawerOpen"
    @update:model-value="handleDrawerModelValueUpdate"
  >
    <AppDrawerHeaderSection
      title="Filtros avançados"
      @cancel="closeNavigationDrawer"
    />

    <PerfectScrollbar :options="{ wheelPropagation: false }">
      <VCard flat>
        <VCardText>
          <VRow>
            <VCol cols="12">
              <AppTextField
                v-model="filterValues.id"
                placeholder="Digite o ID"
                label="Código ID"
              />
            </VCol>

            <VCol cols="12">
              <AppTextField
                v-model="filterValues.code"
                label="Company"
                placeholder="Digite o código"
              />
            </VCol>

            <VCol cols="12">
              <AppTextField
                v-model="filterValues.document"
                label="Documento"
                placeholder="Digite o Documento"
              />
            </VCol>

            <VCol cols="12">
              <AppSelect
                v-model="filterValues.origem"
                placeholder="Selecione a nacionalidade"
                label="Origem"
                multiple
                chips
                :items="['Brasil', 'Argentina', 'Paraguai', 'Estados Unidos']"
              />
            </VCol>

            <VCol cols="12">
              <AppTextField
                v-model="filterValues.email"
                label="E-mail"
                placeholder="Digite o e-mail"
              />
            </VCol>

            <VCol cols="12">
              <AppTextField
                v-model="filterValues.telefone"
                label="Telefone"
                placeholder="Digite o Telefone"
              />
            </VCol>

            <VCol cols="12">
              <AppDateTimePicker
                v-model="filterValues.date"
                label="Range"
                :config="flatPickerConfig"
              />
            </VCol>

            <VCol cols="12">
              <VBtn
                type="reset"
                variant="tonal"
                color="secondary"
                @click="clearFilters"
              >
                Limpar filtros
              </VBtn>
            </VCol>
          </VRow>
        </VCardText>
      </VCard>
    </PerfectScrollbar>
  </VNavigationDrawer>
</template>
