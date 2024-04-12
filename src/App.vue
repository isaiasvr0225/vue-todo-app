<script setup lang="ts">
import EditIcon from './components/icons/EditIcon.vue'
import DeleteIcon from './components/icons/DeleteIcon.vue'
import { reactive } from 'vue'
import axios from 'axios'

import {
  Table,
  TableBody,
  TableCaption,
  TableCell,
  TableHead,
  TableHeader,
  TableRow
} from '@/shadcnComponents/ui/table'

const task = reactive({
  id: 0,
  title: '',
  description: '',
  dueDate: '',
  status: ''
})

const axiosClient = axios.create({
  baseURL: 'localhost:808/api/v1' // Reemplaza con la URL de tu API
})

const getTasks = async () => {
  try {
    const response = await axiosClient.get('/tasks')
    task.data = response.data
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
      <TableRow v-for="tasks in task.data" :key="tasks.id">
        <TableCell class="font-medium"> {{ tasks.id }} </TableCell>
        <TableCell>{{ tasks.title }}</TableCell>
        <TableCell>{{ tasks.description }}</TableCell>
        <TableCell class="text-right"> {{ tasks.dueDate }} </TableCell>
        <TableCell class="text-left"> {{ tasks.status }} </TableCell>
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
