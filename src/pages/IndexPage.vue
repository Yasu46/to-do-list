<template>
  <q-page class="flex flex-center">
    <div class="q-gutter-md q-pa-md" style="max-width: 400px">
      <q-form
        @submit.prevent="onSubmit"
        @reset="onReset"
        class="q-gutter-md"
        ref="todoRef"
      >
        <div class="text-h6">To-do List</div>
        <q-input
          filled
          v-model="todo"
          label="Enter your task"
          lazy-rules
          :rules="[ val => val && val.length > 0 || 'Please type something']"
        />
        <div>
          <q-btn label="Add" type="submit" color="primary"/>
          <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
        </div>
        <q-list bordered separator v-if="store.todoList.length > 0">
          <q-item v-for="(todo,index) in store.todoList" :key="todo">
            <q-item-section :class="{ completed: store.completed[index] }">{{ todo }}</q-item-section>
            <q-item-section avatar>
              <div class="row">
                <q-icon :name="store.completed[index] ? 'check_box' : 'check_box_outline_blank'" size="xs" class="q-px-md cursor-pointer" @click="onToggle(index)"/>
                <q-icon name="delete" size="xs" class="cursor-pointer" @click="onDelete(index)"/>
              </div>
            </q-item-section>
          </q-item>
        </q-list>
        <div v-else>No Data</div>
      </q-form>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'
import { useCounterStore } from '../stores/example-store'

export default defineComponent({
  name: 'IndexPage',
  data() {
    return {
      store: useCounterStore(),
      todo: null
    }
  },
  methods: {
    onSubmit() {
      this.store.todoList.push(this.todo)
      this.todo = null
      this.$refs.todoRef.reset()
    },
    onReset() {
      this.todo = null
    },
    onToggle(index) {
      this.store.completed[index] = !this.store.completed[index]
    },
    onDelete(index) {
      this.store.todoList.splice(index, 1)
    }
  }
})
</script>

<style>
  .completed {
    text-decoration: line-through;
    color: red;
  }
</style>
