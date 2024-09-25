<template>
  <div class="flex flex-col flex-grow w-full md:w-1/2 pt-4 mb-4 md:mb-0">
    <h2 class="text-white mb-1">{{ title }}</h2>
    <div class="flex flex-col min-h-52 border-t-2 border-purple-500/50 backdrop-blur-3xl overflow-y-auto" style="height: calc(100vh - 300px);">
      <!-- to-do items -->
      <div v-for="task in tasks" :key="task.id" class="flex items-center mx-4 my-4 p-2 border-b border-purple-500/50">
        <div @click="$emit('toggleCompletion', task)" :class="`hover:scale-110 flex-shrink-0 w-7 h-7 border-2 rounded-full border-purple-400 cursor-pointer transition-colors hover:bg-purple-400/50 ${task.completed ? 'bg-purple-400' : 'active:bg-purple-400'}`"></div>
        <input type="text" placeholder="Task..." :disabled="!task.editing" @keyup.enter="finishEditing(task)" @blur="finishEditing(task)" :class="`min-w-0 flex-grow p-1 text-white bg-inherit ml-2 transition ${task.editing && 'border-2 border-blue-500/50 rounded-full'} ${task.completed && 'line-through text-opacity-50'}`" :ref="'input_' + task.id" v-model.trim.lazy="task.description" />
        <button v-if="!task.completed" @click="editTask(task)" class="transition ease-in-out text-blue-500 border-2 border-blue-500 rounded-full p-1 ml-2 hover:scale-110 hover:bg-blue-500 hover:shadow-md hover:shadow-blue-500 hover:text-white active:bg-inherit active:text-blue-500">Edit</button>
        <button @click="$emit('deleteTask', task)" class="transition ease-in-out text-rose-500 border-2 border-rose-500 rounded-full p-1 ml-2 hover:scale-110 hover:bg-rose-500 hover:shadow-lg hover:shadow-rose-500/40 hover:text-white active:bg-inherit active:text-rose-500">Delete</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TodoList',
  data() {
    return {
      originalDescription: ''
    }
  },
  props: {
    title: String,
    tasks: {
      type: Array,
      required: true
    }
  },
  methods: {
    editTask(task) {
      task.editing = true
      this.originalDescription = task.description

      this.$nextTick(() => {
        const input = this.$refs['input_' + task.id][0]
        if (input) {
          input.focus()
        }
      })
    },
    finishEditing(task) {
      task.editing = false

      if (!task.description.trim()) {
        task.description = this.originalDescription
        alert('Task description cannot be empty. Please provide a description before saving your task.')
      }

      this.originalDescription = ''
    }
  },
  emits: {
    toggleCompletion() {
      return true;
    },
    deleteTask() {
      return true;
    }
  }
}
</script>

<style scoped>
  input {
    outline: none;
  }
</style>
