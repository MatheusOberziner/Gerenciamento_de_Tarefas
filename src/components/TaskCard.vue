<template>
  <q-card flat bordered>
    <q-item :v-if="task" clickable v-ripple>
      <q-item-section>
        <q-item-label class="text-bold">
          {{ task.description }}
        </q-item-label>
        <q-item-label caption>
          {{ task.project }}
        </q-item-label>
      </q-item-section>
      <q-item-section side>
        <q-btn class="q-mb-xs" color="primary" icon="menu">
          <q-menu>
            <q-list separator>
              <q-item
                v-for="statusItem in (index, status)"
                :key="statusItem.index"
                clickable
                @click="changeStatus(statusItem.field)"
              >
                <q-item-section class="flex flex-center self-center q-py-xs">
                  <q-icon :name="statusItem.icon" size="sm" :color="statusItem.color" />
                  <p class="text-bold no-margin">{{ statusItem.label }}</p>
                </q-item-section>
              </q-item>
            </q-list>
          </q-menu>
        </q-btn>
        <q-btn @click="confirmDelete" color="negative" icon="delete" />
      </q-item-section>

      <q-dialog v-model="showConfirmDialog">
        <q-card>
          <q-card-section>
            <q-card-title class="text-h6">Confirmação</q-card-title>
          </q-card-section>

          <q-card-section>
            <q-card-main>
              Tem certeza de que deseja excluir esta tarefa?
            </q-card-main>
          </q-card-section>

          <q-card-actions align="center">
            <q-btn v-close-popup label="Cancelar" color="primary" />
            <q-btn label="Excluir" color="negative" @click="deleteTask" />
          </q-card-actions>
        </q-card>
      </q-dialog>
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
  data () {
    return {
      status: [
        { field: 'pending', label: 'Pendente', icon: 'alarm', color: 'negative' },
        { field: 'inProgress', label: 'Em andamento', icon: 'timelapse', color: 'blue-7' },
        { field: 'approval', label: 'Aguardando aprovação', icon: 'approval', color: 'orange-9' },
        { field: 'completed', label: 'Concluídas', icon: 'done', color: 'positive' }
      ],
      showConfirmDialog: false
    }
  },
  methods: {
    deleteTask () {
      this.onDelete(this.task)
      this.showConfirmDialog = false
    },
    confirmDelete () {
      this.showConfirmDialog = true
    },
    changeStatus (newStatus) {
      this.onToggle(this.task, newStatus)
    }
  }
}
</script>
