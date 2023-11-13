<template>
  <q-card flat bordered>
    <q-item :v-if="task" clickable v-ripple>
      <q-item-section avatar>
        <q-avatar>
          <q-icon :name="task.completed ? 'check' : 'alarm'"></q-icon>
        </q-avatar>
      </q-item-section>
      <q-item-section>
        <q-item-label>
          {{ task.description }}
        </q-item-label>
        <q-item-label caption lines="1">
          {{ task.completed ? 'Concluída' : 'Pendente' }}
        </q-item-label>
      </q-item-section>
      <q-item-section side top>
        <q-btn @click="toggleTask" color="primary" :icon="task.completed ? 'alarm' : 'check'">
          {{ task.completed ? 'Reabrir' : 'Concluir' }}
        </q-btn>
        <q-btn @click="deleteTask" color="negative" icon="delete"></q-btn>
      </q-item-section>
    </q-item>
  </q-card>
</template>

<script>
export default {
  name: 'TaskCard',
  props: {
    task: {
      type: Object,
      required: true
    },
    onDelete: {
      type: Function,
      required: true
    },
    onToggle: {
      type: Function,
      required: true
    }
  },
  methods: {
    deleteTask () {
      this.onDelete(this.task)
    },
    toggleTask () {
      this.onToggle(this.task)
    }
  }
}
</script>
