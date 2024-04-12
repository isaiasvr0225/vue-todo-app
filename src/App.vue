<script setup lang="ts">
import EditIcon from './components/icons/EditIcon.vue'
import DeleteIcon from './components/icons/DeleteIcon.vue'
import axios from 'axios'
import { onMounted, ref } from 'vue'

import {
  Table,
  TableBody,
  TableCaption,
  TableCell,
  TableHead,
  TableHeader,
  TableRow
} from '@/shadcnComponents/ui/table'

const axiosClient = axios.create({
  baseURL: 'http://localhost:8080/api/v1' // Reemplaza con la URL de tu API
})

let task = ref({})

const getTasks = async () => {
  try {
    const response = await axiosClient.get('/tasks')
    task.value = response.data
    console.log(task.value)
  } catch (error) {
    console.error(error)
  }
}

const createTask = async (newTask) => {
  try {
    await axiosClient.post('/tasks', newTask)
    // La tarea se ha creado correctamente (manejar la respuesta si es necesario)
  } catch (error) {
    console.error(error)
  }
}

const updateTask = async (updatedTask) => {
  try {
    await axiosClient.put(`/tasks/${updatedTask.id}`, updatedTask)
    // La tarea se ha actualizado correctamente (manejar la respuesta si es necesario)
  } catch (error) {
    console.error(error)
  }
}

const deleteTask = async (taskId) => {
  try {
    await axiosClient.delete(`/tasks/${taskId}`)
    // La tarea se ha eliminado correctamente (manejar la respuesta si es necesario)
  } catch (error) {
    console.error(error)
  }
}

onMounted(() => {
  getTasks()
})
</script>

<template>
  <h1>To-Do List Application</h1>
  <Table>
    <TableCaption>A list of your tasks.</TableCaption>
    <TableHeader>
      <TableRow>
        <TableHead class="w-[100px]"> Id </TableHead>
        <TableHead>Title</TableHead>
        <TableHead>Description</TableHead>
        <TableHead class="text-right"> Due Date </TableHead>
        <TableHead>Status</TableHead>
        <TableHead>Actions</TableHead>
      </TableRow>
    </TableHeader>
    <TableBody>
      <TableRow v-for="{ id, title, description, dueDate, status } in task" :key="id">
        <TableCell class="font-medium"> {{ id }} </TableCell>
        <TableCell>{{ title }}</TableCell>
        <TableCell>{{ description }}</TableCell>
        <TableCell class="text-right"> {{ dueDate }} </TableCell>
        <TableCell class="text-left"> {{ status }} </TableCell>
        <TableCell>
          <div class="flex flex-row justify-evenly">
            <EditIcon></EditIcon>
            <button @click="deleteTask"><DeleteIcon></DeleteIcon></button>
          </div>
        </TableCell>
      </TableRow>
    </TableBody>
  </Table>
</template>

<style scoped>
h1 {
  text-align: center;
}
</style>
