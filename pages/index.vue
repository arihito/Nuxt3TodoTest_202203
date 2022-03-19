<script setup>
const state = ref({
  title: "My Todo App",
  tasks: [
    {
      name: "ストレッチする",
      isComplete: true,
    },
    {
      name: "本を読む",
      isComplete: false,
    },
  ],
  newTask: "",
})

const addTask = () => {
  state.tasks.push({
    name: state.newTask,
    isComplete: false,
  })
  state.newTask = ""
}

const deleteTask = (index) => {
  state.tasks.forEach((task, loopIndex) => {
    if (index === loopIndex) {
      state.tasks.splice(index, 1)
    }
  })
}

const completeTask = (index) => {
  state.tasks.forEach((task, loopIndex) => {
    if (index === loopIndex) {
      task.isComplete = !task.isComplete;
    }
  })
  state.saveToLocalStorage()
}


const getLocalStorageData = () => {
  const data = localStorage.getItem('todo');

  if (data) {
    this.tasks = JSON.parse(data);
  }
}
</script>

<template>
  <div class="container">
    <div class="wrapper">
      <h2 class="title">{{ state.title }}</h2>
      <section class="add-task">
        <input type="text" v-model="state.newTask" />
        <button @click="addTask">追加する</button>
      </section>
      <!-- <AtomsTheButton /> -->
      <section class="tasks">
        <ul>
          <li
            v-for="task in state.tasks"
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
