<script setup>
import { h, ref } from 'vue'
import EditButton from '@/components/EditButton.vue'
import TableTanstack from '@/components/TableTanstack.vue'
import axios from 'axios'

const users = ref([])
const loading = ref(false)

const getUsers = () => {
  loading.value = true
  axios.get('https://jsonplaceholder.typicode.com/users').then((res) => {
    users.value = res?.data
  }).catch((err) => {
    alert('ERROR: ' + JSON.stringify(err))
  }).finally(() => {
    loading.value = false
  })
}

getUsers()

const columnsPeople = [
  {
    accessorKey: 'id',
    header: 'ID',
    enableSorting: false,
  },
  {
    accessorKey: 'name',
    header: 'First name',
  },
  {
    accessorKey: 'username',
    header: 'Username',
  },
  {
    accessorKey: 'email',
    header: 'Email',
  },
  {
    accessorKey: 'edit',
    header: ' ',
    cell: ({ row }) => h(EditButton, { id: row.original.id }),
    enableSorting: false,
  },
]
</script>

<template>
  <div class="container mx-auto px-8 py-8">
    <div v-if="loading">Loading users...</div>
    <TableTanstack v-else :data="users" :columns="columnsPeople" />
  </div>
</template>
