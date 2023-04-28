<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        @keyup.enter="addTask"
        v-model="newTask"
        class="col"
        filled
        placeholder="Add Task"
        bg-color="white"
        dense>
        <template v-slot:append>
          <q-btn
            @click="addTask"
            round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list separator bordered class="bg-white">
      <q-item
        clickable
        v-for="(task, index) in tasks" :key="task.title"
        @click="task.done = !task.done"
        :class="{'done bg-blue-1' : task.done}"
        v-ripple>
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section
          side
          v-if="task.done">
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            round
            color="primary"
            dense
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon size="100px" name="check" color="primary" />
      <div class="text-h5 text-primary text-center">
        No tasks
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from "vue";
import { useQuasar } from "quasar";

const $q = useQuasar();

let tasks = ref([
  // {
  //   title: "Write Code",
  //   done: false
  // },
  // {
  //   title: "Test Code",
  //   done: true
  // },
  // {
  //   title: "Deploy Code",
  //   done: false
  // }

]);

let newTask = ref('')
const deleteTask = (index) => {
  $q.dialog({
    title: "Confirm",
    message: "Would you like to turn on the wifi?",
    cancel: true,
    persistent: true
  }).onOk(() => {
    tasks.value.splice(index, 1);
    $q.notify({ message: "Task deleted", type: "positive" });
  });

};

const addTask = () => {
  if (!newTask.value){
    $q.notify({
      type:"negative",
      message:"enter a task!"
    })
    return false;
  }
  tasks.value.push({
    title: newTask.value,
    done: false
  })
  newTask.value = ''
}

</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }

}
.no-tasks{
  opacity: 0.5;
}
</style>
