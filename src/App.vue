
<template>
  <main class="app">
    <section class="create-todo">
      <h3>CREATE A TODO</h3>
      <form id="new-todo-form" @submit.prevent="addTodo">
        <h4>What's on your todo list?</h4>
        <!-- <input type="text" name="content" id="content" placeholder="e.g. make a video" v-model="input_content" /> -->
        <tinymce-editor  v-model="input_content"></tinymce-editor>
        <!--Add date picker in here-->
        <VueDatePicker v-model="input_date" placeholder="Select Date" model-type="yyyy-MM-dd" range
          :enable-time-picker="false" />
        <input type="submit" value="Add todo" />
      </form>
    </section>

    <section class="todo-list">
      <h3>TODO LIST</h3>
      <div class="list" id="todo-list">

        <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
          <label>
            <input type="checkbox" v-model="todo.done" />
          </label>
          <div class="todo-content">
            <span v-html="todo.content"></span>
            <!-- <input type="text" v-model="todo.content" /> -->
            <span>{{ todo.date[0] }} - {{ todo.date[1] }}</span>
          </div>
          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>

      </div>
    </section>

  </main>
</template>

<script setup>
import { ref, onMounted, computed, watch } from 'vue'
import VueDatePicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css'
import { useToast } from "vue-toastification";
import TinymceEditor from './components/tinymce.vue';
const todos = ref([])
const name = ref('')
const today = new Date();
const input_content = ref('')
const input_date = ref(null);
const toast = useToast()


const todos_asc = computed(() => todos.value.sort((a, b) => {
  return a.createdAt - b.createdAt
}))

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, {
  deep: true
})

watch(todos, () => {
  todos?.value.forEach(todo => {
    let dl = new Date(todo.date[1])
    if(dl.getDate() == today.getDate() && dl.getMonth() == today.getMonth() && dl.getFullYear() == dl.getFullYear()) {
      toast.warning(`${todo.content} is not finished`)
    }
  });
})

const addTodo = () => {
  if (input_content.value.trim() === '' || input_date.value === null) {
    return
  }

  todos.value.push({
    content: input_content.value,
    // category: input_category.value,
    date: input_date.value,
    done: false,
    editable: false,
    createdAt: new Date().getTime()
  })
}

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo)
}

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})



</script>
