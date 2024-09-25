<template>
  <!-- background -->
  <div class="fixed inset-0 -z-10 h-full w-full items-center px-5 py-24 [background:radial-gradient(125%_125%_at_50%_10%,#000_40%,#63e_100%)]"></div>
  
  <!-- main content -->
  <div class="flex flex-col h-screen">
    <!-- navbar -->
    <div class="h-[80px] grid grid-cols-[auto,1fr,auto] px-5 mb-2">
      <img src="./assets/todo-list.svg" height="60" width="60" class="my-auto" />
      <span></span>
      <button @click="clearLocalStorage" class="uppercase text-rose-500 border-2 border-rose-500 rounded-xl my-auto px-2 hover:bg-rose-500 hover:shadow-lg hover:shadow-rose-500/40 hover:text-white active:bg-inherit active:text-rose-500">Clear Storage</button>
    </div>

    <!-- main content area -->
    <div class="flex flex-col flex-grow container mx-auto px-4">
      <div class="flex gap-2">
        <input class="border-[1px] border-green-500 rounded-l-full bg-inherit p-2 text-white" type="text" placeholder="New Task..." @keyup.enter="addTask" v-model="newTaskDescription" />
        <button class="text-green-500 border-[1px] rounded-r-full border-green-500 bg-inherit p-2 hover:bg-green-500 hover:text-white active:bg-inherit active:text-green-500" @click="addTask">Add Task</button>
      </div>
      <div class="flex flex-col md:flex-row md:gap-4 flex-grow overflow-hidden">
        <TodoList title="To Do:" show-add-button="true" :tasks="incompleteTasks" @delete-task="onDeleteTask" @toggle-completion="onToggleCompletion" />
        <TodoList title="Completed:" :tasks="completeTasks" @delete-task="onDeleteTask" @toggle-completion="onToggleCompletion" />
      </div>
    </div>
    
    <!-- footer -->
    <div class="flex justify-center items-center min-h-[80px] w-full mt-4 backdrop-blur-3xl">
      <ul class="flex list-none text-white gap-4">
        <li><a href="https://github.com/Noah-Black-80" target="_blank">GitHub</a></li>
        <li><a href="https://www.linkedin.com/in/noah-r-black/" target="_blank">LinkedIn</a></li>
        <li><a href="mailto:nblack2024@gmail.com" target="_blank">nblack2024@gmail.com</a></li>
      </ul>
    </div>
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue';

export default {
  name: 'App',
  components: {
    TodoList
  },
  data() {
    return {
      tasks: [],
      newTaskDescription: ''
    }
  },
  mounted() {
    this.loadTasksFromLocalStorage();
  },
  methods: {
    addTask() {
      if (this.newTaskDescription.trim()) {
        this.tasks.push({
          id: Date.now(),
          description: this.newTaskDescription,
          completed: false,
          editing: false
        })
        
        this.saveTasksToLocalStorage()
        this.newTaskDescription = ''
      } else {
        alert('Please enter a description before adding a new task to the list.')
      }
    },
    onDeleteTask(task) {
      this.tasks = this.tasks.filter(t => t.id !== task.id)
      this.saveTasksToLocalStorage()
    },
    onToggleCompletion(task) {
      task.completed = !task.completed
      this.saveTasksToLocalStorage()
    },
    saveTasksToLocalStorage() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
    loadTasksFromLocalStorage() {
      const tasksFromStorage = localStorage.getItem('tasks')
      if (tasksFromStorage) {
        this.tasks = JSON.parse(tasksFromStorage)
      } else {
        []
      }
    },
    clearLocalStorage() {
      localStorage.clear()
      this.tasks = []
    }
  },
  computed: {
    incompleteTasks() {
      return this.tasks.filter(task => !task.completed)
    },
    completeTasks() {
      return this.tasks.filter(task => task.completed)
    }
  }
}
</script>

<style scoped>
  li {
    text-decoration: none;
  }
  a:hover {
    text-decoration: underline;
    text-decoration-color: brown;
    text-decoration-thickness: 4px;
    text-underline-offset: 8px;
  }
</style>