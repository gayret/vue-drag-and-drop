<script setup>
import { reactive, computed } from 'vue'

const todos = reactive([
  {
    id: 1,
    title: 'Watch The Matrix',
    completed: false,
  },
  {
    id: 2,
    title: 'Write an essay about Vue',
    completed: false,
  },
  {
    id: 3,
    title: 'Learn NextJS',
    completed: true,
  },
])

const thingToDo = computed(() => {
  return todos.filter((item) => !item.completed)
})

const completed = computed(() => {
  return todos.filter((item) => item.completed)
})

const onSubmit = (e) => {
  todos.push({
    id: todos.length + 1,
    title: e.target.value,
    completed: false,
  })
}

const onDrag = (event, item) => {
  event.dataTransfer.setData('itemIndex', item.id)
}

const onDrop = (event) => {
  const itemIndex = event.dataTransfer.getData('itemIndex') - 1
  todos[itemIndex].completed = !todos[itemIndex].completed
}
</script>

<template>
  <header>
    <h2>U can drag and drop to change the order of todos</h2>
  </header>
  <div class="new-todo">
    <input placeholder="Insert new todo" type="text" @keyup.enter="onSubmit" />
  </div>
  <div class="cards">
    <div class="left" @drop="onDrop($event, test)" @dragenter.prevent @dragover.prevent>
      <h2>Todos</h2>
      <ul v-if="thingToDo.length > 0">
        <li
          v-for="todo in thingToDo"
          :key="todo.id"
          draggable="true"
          @dragstart="onDrag($event, todo)"
        >
          <label>
            <input type="checkbox" v-model="todo.completed" />
            {{ todo.title }}
          </label>
        </li>
      </ul>
    </div>
    <div class="right" @drop="onDrop($event, test)" @dragenter.prevent @dragover.prevent>
      <h2>Completed</h2>
      <ul v-if="completed.length > 0">
        <li
          v-for="todo in completed"
          :key="todo.id"
          draggable="true"
          @dragstart="onDrag($event, todo)"
        >
          <label>
            <input type="checkbox" v-model="todo.completed" />
            {{ todo.title }}
          </label>
        </li>
      </ul>
    </div>
  </div>
  <footer>
    Follow me on <a href="https://safa.medium.com">Medium</a> and
    <a href="https://github.com/gayret">GitHub</a>
  </footer>
</template>

<style scoped>
header {
  text-align: center;
  margin-block: 2em;
}
.new-todo {
  display: flex;
  justify-content: center;
}

.new-todo input {
  min-width: 50%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 16px;
}
.cards {
  display: flex;
  justify-content: space-around;
  padding-top: 4em;
}

ul {
  list-style: none;
  padding: 1em;
  border: 1px solid lightgray;
}

li {
  margin-block: 0.5em;
  padding: 1em;
}

.left,
.right {
  border: 1px solid lightgray;
  padding: 1em 2em;
  min-width: 300px;
}

.left li {
  background-color: lightsalmon;
}

.right li {
  background-color: lime;
}

footer {
  position: absolute;
  bottom: 0;
  right: 0;
  padding: 3em;
}

@media (max-width: 768px) {
  .cards {
    display: block;
  }
}
</style>
