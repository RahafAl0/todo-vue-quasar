<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-accent">
      <q-input
        v-model="newTask"
        @keyup.enter="addTask"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="Add task"
        dense>
        <template v-slot:append>
          <q-btn
           @click="addTask"
            round
            dense
            flat
            icon="add" />
        </template>
      </q-input>
    </div>
    <q-list
      class="bg-white"
      separator
      bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="toggleTask(index)"
        clickable
        :class="{ 'done bg-blue-1' : task.done }"
        v-ripple>
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="accent" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{task.title}}</q-item-label>
        </q-item-section>
        <q-item-section
          v-if="task.done"
          side>
          <q-btn
            @click.stop='deleteTask(index)'
            flat
            round
            color="primary"
            icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
    <div
      v-if="!tasks.length"
      class="no-tasks absolute-center">
      <q-icon
        name="check"
        size="100px"
        color="accent" />
      <div class="text-h5 text-accent text-center">
        No tasks
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const tasks = ref([
  { title: 'Learn Vue 3', done: false },
  { title: 'Learn Quasar', done: false },
  { title: 'Build an app', done: false },
  { title: 'Deploy it', done: false },
  { title: 'Share it', done: false },
]);

const newTask = ref('');

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push({
      title: newTask.value,
      done: false
    });
    newTask.value = '';
  }
};

const deleteTask = (index: number) => {
  const deletedTask = tasks.value[index].title;
  tasks.value.splice(index, 1);
  console.log('Task deleted:', deletedTask);
};

const toggleTask = (index: number) => {
  tasks.value[index].done = !tasks.value[index].done;
};
</script>

<style scoped>
.done .q-item__label {
  text-decoration: line-through;
  color: #bbb;
}

.no-tasks {
  opacity: 0.5;
}
</style>
