<script setup>
import { reactive, ref, computed, onMounted } from 'vue'


const shallowCopy = (obj) => Object.assign(Object(), obj)

const tasks = ref([
  { id: 1, text: 'make a todo app', priority: true, done: false },
  { id: 2, text: 'refactor', priority: false, done: false },
])
const reversedTasks = computed(() => [...tasks.value].reverse())
const nextTaskId = computed(() => tasks.value.length + 1)

const newTask = reactive({
  // id: nextTaskId.value,
  // text: '',
  // priority: false,
  // done: false
})

const minLength = 3
const maxLength = 50
const newTaskLength = computed(() => newTask.text ? newTask.text.length : 0)

const resetNewTask = () => {
  newTask.id = nextTaskId.value
  newTask.text = ''
  newTask.priority = false
  newTask.done = false
  // newTask.value = {
  //   id: nextTaskId.value,
  //   text: '',
  //   priority: false,
  //   done: false
  // }
}


const validateTask = (t) => t.text && t.text.length >= minLength

const addTask = () => {
  if (validateTask(newTask)) {
    tasks.value.push(shallowCopy(newTask))
    resetNewTask()
  } else {
    console.warn(`Not a valid task: ${newTask.text}`)
  }
}

const toggleDone = (t) => {
  t.done = !t.done
}

onMounted(() => {
  resetNewTask()
})

</script>


<template>
  <main>
    <h1>Todo app</h1>
    <form @submit.prevent="addTask">
      <div v-if="false">
        <pre>{{ newTask }}</pre>
      </div>
      <input autofocus type="text" :minlength="minLength" :maxlength="maxLength" v-model="newTask.text">
      <span>{{ newTaskLength }} / {{ maxLength }}</span>
      <label>
        <input type="checkbox" v-model="newTask.priority">
        Important
      </label>
      <button class="btn btn-primary" type="submit">Add</button>
    </form>

    <div class="task-list">
      <p v-show="!tasks.length">go ahead and add a task &hellip;</p>
      <ul>
        <li v-for="t in reversedTasks" :class="{ 'task-done': t.done, 'task-priority': t.priority }"
          @click="toggleDone(t)">
          {{ t.id }}: {{ t.text }}
        </li>
      </ul>
    </div>

  </main>
</template>


<style scoped>
header {
  line-height: 1.5;
}

form input,
form button,
form span {
  margin-left: 10px;
}

.task-list {
  font-size: 20px;
  margin-top: 20px;
}

.task-done {
  text-decoration: line-through;
  color: darkgrey;
  /* font-weight: normal; */
}

.task-priority {
  color: #ff5000;
  font-weight: bold;
}

.task-priority.task-done {
  color: #ff9090;
}
</style>