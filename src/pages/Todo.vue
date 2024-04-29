<template>
  <q-page class="column">
    <div class="row q-pa-sm bg-secondary">
      <q-input @keyup.enter="addTask" filled class="col" square v-model="newTask" placeholder="Добавить задачу" dense>
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list separator bordered>
      <q-item :class="{ 'done bg-green-1': task.done }" @click="task.done = !task.done" clickable
        v-for="(task, index) in tasks" :key="task.title" v-ripple>
        <q-item-section avatar>
          <q-checkbox class="no-pointer-events" v-model="task.done" val="teal" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn @click.stop="deleteTask(index)" flat round color="primary" icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-task absolute-center">
      <q-icon name="check" size="100px" color="primary" />  
      <div class="text-h4 text-primary">Нет задач!</div>
    </div>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: [
      ]
    }
  },
  methods: {
    deleteTask(index) {
      this.$q.dialog({
        title: 'Внимание!',
        message: 'Вы действительно хотите удалить эту запись?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1),
          this.$q.notify('Задача удалена')
      })
    },
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false
      })
      this.newTask = '';
    }
  },
}
</script>

<style lang="scss">
.done {
.q-item__label {
  text-decoration: line-through;
  color: #bbb;
}
};
.no-task {
  opacity: 0.5;
}
</style>
