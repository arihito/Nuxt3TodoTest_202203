<script setup>
const state = reactive({
  title: "Nuxt3 Todo App",
  tasks: [],
  newTask: "",
})

onMounted(() => {
  getLocalStorageData()
})

const addTask = () => {
  state.tasks.push({
    name: state.newTask,
    isComplete: false,
  })
  localStorage.setItem('todo', JSON.stringify(state.tasks))
  state.newTask = ""
}

const deleteTask = (index) => {
  state.tasks.forEach((task, loopIndex) => {
    if (index === loopIndex) {
      state.tasks.splice(index, 1)
    }
  })
  localStorage.setItem('todo', JSON.stringify(state.tasks))
}

const completeTask = (index) => {
  state.tasks.forEach((task, loopIndex) => {
    if (index === loopIndex) {
      task.isComplete = !task.isComplete;
    }
  })
  localStorage.setItem('todo', JSON.stringify(state.tasks))
}

const getLocalStorageData = () => {
  const data = localStorage.getItem('todo');
  if (data) {
    state.tasks = JSON.parse(data);
  }
}
</script>

<template>
  <div class="container">
    <div class="wrapper">
      <h2 class="title">{{ state.title }}</h2>
      <section class="add-task">
        <input type="text" v-model="state.newTask" autofocus />
        <button @click="addTask">追加する</button>
      </section>
      <section class="tasks">
        <ul>
          <li
            v-for="(task, index) in state.tasks"
            :key="task"
            :id="index"
            :class="{ complete: task.isComplete }"
            @click="completeTask(index)"
          >
            {{ task.name }}
            <button @click="deleteTask(index)" class="trash-icon">
              <i class="fas fa-trash"></i>
            </button>
          </li>
        </ul>
      </section>
    </div>
  </div>
</template>
