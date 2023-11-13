<template>
  <q-page style="background-color: #e7ecf1;">
    <q-tabs
      v-model="activeTab"
      dense
      class="q-mb-md"
      active-color="positive"
      indicator-color="positive"
    >
      <q-tab name="pending">Pendentes</q-tab>
      <q-tab name="inProgress">Em Andamento</q-tab>
      <q-tab name="completed">Concluídas</q-tab>
    </q-tabs>

    <q-btn @click="addTask" icon="add" color="positive" />

    <q-page-section>
      <q-tab-panels v-model="activeTab" animated>
        <q-tab-panel name="pending">
          <TaskCard
            v-for="task in getTasksByStatus('pending')"
            :key="task.id"
            :task="task"
            :onToggle="toggleTask"
            :onDelete="deleteTask"
          />
        </q-tab-panel>

        <q-tab-panel name="inProgress">
          <TaskCard
            v-for="task in getTasksByStatus('inProgress')"
            :key="task.id"
            :task="task"
            :onToggle="toggleTask"
            :onDelete="deleteTask"
          />
        </q-tab-panel>

        <q-tab-panel name="completed">
          <TaskCard
            v-for="task in getTasksByStatus('completed')"
            :key="task.id"
            :task="task"
            :onToggle="toggleTask"
            :onDelete="deleteTask"
          />
        </q-tab-panel>
      </q-tab-panels>
    </q-page-section>
  </q-page>
</template>

<script>
import TaskCard from '../components/TaskCard.vue'
export default {
  name: 'IndexPage',
  components: {
    TaskCard
  },
  data () {
    return {
      tasks: [
        { id: 1, description: 'Tarefa 1', status: 'pending' },
        { id: 2, description: 'Tarefa 2', status: 'inProgress' },
        { id: 3, description: 'Tarefa 3', status: 'completed' }
      ],
      activeTab: 'pending'
    }
  },
  computed: {
    getTasksByStatus () {
      return status => this.tasks.filter(task => task.status === status)
    }
  },
  methods: {
    addTask () {
      const newTask = {
        id: this.tasks.length + 1,
        description: `Tarefa ${this.tasks.length + 1}`,
        status: 'pending'
      }
      this.tasks.push(newTask)
    },
    toggleTask (task) {
      task.status = task.status === 'pending' ? 'completed' : 'pending'
    },
    deleteTask (task) {
      const taskIndex = this.tasks.indexOf(task)
      if (taskIndex !== -1) {
        this.tasks.splice(taskIndex, 1)
      }
    }
  }
}
</script>
