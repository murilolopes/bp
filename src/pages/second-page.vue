<script setup lang="ts">
import type { SalesDetails } from '@/@fake-db/types'
import AddNewUserDrawer from '@/views/AddNewUserDrawer.vue'
import avatar3 from '@images/avatars/avatar-3.png'
import avatar8 from '@images/avatars/avatar-8.png'
import product7 from '@images/eCommerce/7.png'
import product9 from '@images/eCommerce/9.png'
import { VDataTable } from 'vuetify/labs/VDataTable'

const search = ref('')
const status = ref('')
const selectedItems = ref([])
const productList = ref<SalesDetails[]>([
  {
    product: {
      id: 19,
      name: 'OnePlus 7 Pro ',
      slug: 'one-plus-7-pro-19',
      brand: 'Philips',
      category: 'Smart Phone',
      price: 14.99,
      image: product9,
      hasFreeShipping: false,
      rating: 4,
      description: 'The OnePlus 7 Pro features a brand new design, with a glass back and front and curved sides. The phone feels\n    very premium but\u2019s it\u2019s also very heavy. The Nebula Blue variant looks slick but it\u2019s quite slippery, which\n    makes single-handed use a real challenge. It has a massive 6.67-inch \u2018Fluid AMOLED\u2019 display with a QHD+\n    resolution, 90Hz refresh rate and support for HDR 10+ content. The display produces vivid colours, deep blacks\n    and has good viewing angles.',
    },
    date: '30 Apr 2020',
    buyer: {
      name: 'Ana Smith',
      avatar: avatar3,
    },
    payment: {
      total: 984,
      received_payment_status: 'Fully Paid',
      paid_amount: 984,
      status: 'Completed',
    },
  },
  {
    product: {
      id: 21,
      name: 'Google - Google Home',
      slug: 'google-google-home-white-slate-fabric-21',
      brand: 'Google',
      category: 'Google Home',
      price: 129.29,
      image: product7,
      hasFreeShipping: true,
      rating: 4,
      description: 'Simplify your everyday life with the Google Home, a voice-activated speaker powered by the Google Assistant. Use\n    voice commands to enjoy music, get answers from Google and manage everyday tasks. Google Home is compatible with\n    Android and iOS operating systems, and can control compatible smart devices such as Chromecast or Nest.',
    },
    date: '11 Jul 2020',
    buyer: {
      name: 'Lindsay Green',
      avatar: avatar8,
    },
    payment: {
      total: 1101,
      received_payment_status: 'Fully Paid',
      paid_amount: 1101,
      status: 'Completed',
    },
  },
])

const headers = [
  { title: 'Usuário', key: 'buyer.name' },
  { title: 'Origem', key: 'slug' },
  { title: 'Telefone', key: 'price' },
  { title: 'Documento', key: 'brand' },
  { title: 'Cadastro', key: 'date', sortable: false },
  { title: '', key: 'actions', sortable: false },
]

const isAddNewUserDrawerVisible = ref(false)

const addNewUser = payload => {
  console.log(1, payload)
}

const items = ['Todos', 'Ativos', 'Bloqueados']
const items2 = ['CSV', 'Excel']
const items3 = ['Ativar usuário', 'Bloquear usuário']
const actions = [
  {
    title: 'Editar usuário',
    action: () => {
      console.log(1)
    }
  },
  {
    title: 'Enviar recuperação',
    action: () => {
      console.log(2)
    }
  },
  {
    title: 'Alterar senha do aplicativo',
    action: () => {
      console.log(3)
    }
  },
  {
    title: 'Alterar senha de pagamento',
    action: () => {
      console.log(4)
    }
  },
  {
    title: 'Bloquear usuário',
    icon: 'tabler-search',
    action: () => {
      console.log(5)
    }
  },
]

const statusBadgeColor = value => {
  const colors = {
    Todos: '#424B5A',
    Ativos: '#198CFF',
    Bloqueados: '#E2004F',
  }

  return colors[value]
}
</script>

<template>
  <VCard>
    <VRow class="pa-4">
      <VCol cols="6" class="d-flex justify-content-between">
        <AppTextField
          v-model="search"
          density="compact"
          placeholder="Pesquisar"
          prepend-inner-icon="tabler-search"
          single-line
          hide-details
          dense
          outlined
        />

        <AppSelect
          class="ms-3"
          :items="items"
          v-model="status"
        >
          <template #selection="{ item }">
            <v-badge dot location="start center" class="d-flex justify-center align-center" :color="statusBadgeColor(item.value)">
              <span class="ml-3"> {{ item.value }} </span>
            </v-badge>
          </template>
        </AppSelect>
    </VCol>
      <VCol cols="6" class="d-flex flex-row-reverse" v-if="!selectedItems.length">
        <VBtn class="ms-3" variant="outlined" @click="isAddNewUserDrawerVisible = true">
          <VIcon
            start
            icon="tabler-filter"
          />
          Filtrar
        </VBtn>
        <div class="w-25">
          <AppSelect
            :items="items2"
            placeholder="Exportar"
            chip
          />
        </div>
      </VCol>
      <VCol cols="6" class="d-flex flex-row-reverse" v-else>
        <div class="w-50">
          <AppSelect
            :items="items3"
            placeholder="Ações em massa"
          />
        </div>
      </VCol>
    </VRow>

    <div class="pa-4">
      <!-- 👉 Data Table  -->
      <VDataTable
        :headers="headers"
        :items="productList"
        :search="search"
        :items-per-page="5"
        class="text-no-wrap"
        v-model="selectedItems"
        show-select
      >
        <template #no-data>
          <div class="d-flex justify-center align-center">
            <VImg src="src/assets/images/datatable-empty-state.svg?raw" :width="400" :height="400" />
          </div>
        </template>

        <template #item.buyer.name="{ item }">
          <div class="d-flex align-center">
            <VAvatar
              size="1.875rem"
              :color="!item.raw.avatar ? 'primary' : undefined"
              :variant="!item.raw.avatar ? 'tonal' : undefined"
            >
              <VImg
                v-if="item.raw.buyer.avatar"
                :src="item.raw.buyer.avatar"
              />
              <span v-else>{{ item.raw.buyer.name.slice(0, 2).toUpperCase() }}</span>
            </VAvatar>
            <div class="d-flex flex-column ms-3">
              <span class="d-block font-weight-medium text-truncate text--primary">{{ item.raw.buyer.name }}</span>
              <span class="text-xs">{{ item.raw.buyer.name }}</span>
            </div>
          </div>
        </template>

        <template #item.actions="{ item }">
          <!-- <v-menu>
            <template v-slot:activator="{ props }">
              <v-btn icon="mdi-dots-vertical" v-bind="props"></v-btn>
            </template>

            <v-list>
              <v-list-item
                v-for="(item, i) in actions"
                :key="i"
              >
                <v-list-item-title @click="item.action">{{ item.title }}</v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu> -->
          <IconBtn
            density="compact"
            color="disabled"
          >
            <VIcon icon="tabler-dots-vertical" />

            <VMenu
              v-if="actions"
              activator="parent"
            >
              <VList
                :items="actions"
              >
              <template #title="item">
                <div @click="items.action">
                  <VIcon  v-if="item.icon" icon="tabler-dots-vertical" />
                  <span>{{ item.title }}</span>
                </div>
              </template>
              <template #repend="item">
                <div @click="items.action">
                  <VIcon  v-if="item.icon" icon="tabler-dots-vertical" />
                </div>
              </template>
              </VList>
            </VMenu>
          </IconBtn>
        </template>
      </VDataTable>
    </div>
  </VCard>
  <AddNewUserDrawer
    v-model:isDrawerOpen="isAddNewUserDrawerVisible"
    @updateFilterValues="addNewUser"
  />
</template>
