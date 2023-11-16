<template>
  <q-page class="row q-col-gutter-lg q-pa-xl" style="background-color: #e7ecf1;">
    <q-page-section class="col-3">
      <div class="row items-center q-gutter-xs q-pb-md q-pl-sm">
        <q-icon name="alarm" color="negative" size="sm" />
        <span class="text-h5">Pendentes</span>
        <q-btn
          class="q-ml-md"
          color="positive"
          icon="add"
          size="md"
          @click="openAddTaskDialog()"
        />
      </div>

      <TaskCard
        v-for="task in getTasksByStatus('pending')"
        :key="task.id"
        :task="task"
        :onToggle="toggleTask"
        :onDelete="deleteTask"
      />
    </q-page-section>

    <q-page-section class="col-3">
      <div class="row items-center q-gutter-xs q-pb-md q-pl-sm">
        <q-icon name="timelapse" color="blue-7" size="sm" />
        <span class="text-h5 ">Em andamento</span>
      </div>

      <TaskCard
        v-for="task in getTasksByStatus('inProgress')"
        :key="task.id"
        :task="task"
        :onToggle="toggleTask"
        :onDelete="deleteTask"
      />
    </q-page-section>

    <q-page-section class="col-3">
      <div class="row items-center q-gutter-xs q-pb-md q-pl-sm">
        <q-icon name="approval" color="orange-9" size="sm" />
        <span class="text-h5 ">Aguardando aprovação</span>
      </div>

      <TaskCard
        v-for="task in getTasksByStatus('approval')"
        :key="task.id"
        :task="task"
        :onToggle="toggleTask"
        :onDelete="deleteTask"
      />
    </q-page-section>

    <q-page-section class="col-3">
      <div class="row items-center q-gutter-xs q-pb-md q-pl-sm">
        <q-icon name="done" color="positive" size="sm" class="text-bold" />
        <span class="text-h5 ">Concluídas</span>
      </div>

      <TaskCard
        v-for="task in getTasksByStatus('completed')"
        :key="task.id"
        :task="task"
        :onToggle="toggleTask"
        :onDelete="deleteTask"
      />
    </q-page-section>

    <q-dialog ref="dialog" v-model="showAddTaskDialog">
      <q-card>
        <q-card-section class="q-pb-none">
          <q-card-title class="text-h6">Adicionar Nova Tarefa</q-card-title>
        </q-card-section>
        <q-card-section>
          <q-form ref="refForm" autocomplete="off">
            <q-input
              v-model="newTask.description"
              outlined
              label="Descrição*"
              lazy-rules
              :rules="[ (val) => (val && val.length > 0) || 'Descrição deve ser informada']"
            />
            <q-input
              v-model="newTask.project"
              label="Projeto*"
              lazy-rules
              :rules="[ (val) => (val && val.length > 0) || 'Projeto deve ser informada']"
            />
          </q-form>
        </q-card-section>
        <q-card-actions align="center">
          <q-btn v-close-popup label="Cancelar" color="primary" />
          <q-btn label="Adicionar" color="positive" @click="addTask" />
        </q-card-actions>
      </q-card>
    </q-dialog>
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
        { id: 1, description: 'Tarefa 1', status: 'pending', project: 'Projeto 1' },
        { id: 2, description: 'Tarefa 2', status: 'inProgress', project: 'Projeto 1' },
        { id: 3, description: 'Tarefa 3', status: 'completed', project: 'Projeto 2' },
        { id: 4, description: 'Tarefa 4', status: 'approval', project: 'Projeto 2' },
        { id: 5, description: 'Tarefa 5', status: 'pending', project: 'Projeto 3' },
        { id: 6, description: 'Tarefa 6', status: 'inProgress', project: 'Projeto 2' },
        { id: 7, description: 'Tarefa 7', status: 'approval', project: 'Projeto 1' }
      ],
      showAddTaskDialog: false,
      newTask: {
        description: '',
        project: ''
      }
    }
  },
  computed: {
    getTasksByStatus () {
      return status => this.tasks.filter(task => task.status === status)
    }
  },
  methods: {
    openAddTaskDialog () {
      this.showAddTaskDialog = true
    },
    addTask () {
      const newTask = {
        id: this.tasks.length + 1,
        description: this.newTask.description,
        project: this.newTask.project,
        status: 'pending'
      }

      // Vai verificar se os campos estão corretos antes de inserir
      this.$refs.refForm.validate()
        .then(valid => {
          if (valid) {
            this.tasks.push(newTask)
            this.$refs.dialog.hide()
          }
        })
    },
    deleteTask (task) {
      const taskIndex = this.tasks.indexOf(task)
      if (taskIndex !== -1) {
        this.tasks.splice(taskIndex, 1)
      }
    },
    toggleTask (task, newStatus) {
      task.status = newStatus
    }
  }
}
</script>
