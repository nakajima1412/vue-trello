<template>
  <div
    class="column"
    draggable
    @drop="moveTaskOrColumn($event, column.tasks, columnIndex)"
    @dragover.prevent
    @dragenter.prevent
    @dragstart.self="pickupColumn($event, columnIndex)"
    >
    <div class="flex items-center mb-2 float-fold">
      {{ column.name }}
    </div>
    <div class="list-reset">
      <ColumnTask
        v-for="(task, $taskIndex) of column.tasks"
        :key="$taskIndex"
        :task="task"
        :taskIndex="$taskIndex"
        :column="column"
        :columnIndex="columnIndex"
        :board="board"
      />

      <input
        @keyup.enter="createTask($event, column.tasks)"
        type="text"
        class="block pd-2 w-full bg-transparent"
        placeholder="+ Enter New Task"
      />
    </div>
  </div>
</template>

<script>
import ColumnTask from './ColumnTask.vue'
import movingTaskAndColumnsMixin from '@/mixins/movingTaskAndColumnsMixin.js'

export default {
  components: {
    ColumnTask
  },
  mixins: [movingTaskAndColumnsMixin],
  methods: {
    pickupColumn (e, fromColumnIndex) {
      e.dataTransfer.effectAllowed = 'move'
      e.dataTransfer.dropEffect = 'move'

      e.dataTransfer.setData('from-column-index', fromColumnIndex)
      e.dataTransfer.setData('type', 'column')
    },
    createTask (e, tasks) {
      this.$store.commit('CREATE_TASK', {
        tasks,
        name: e.target.value
      })
      e.target.value = ''
    }
  }
}
</script>

<style lang="css">
  .column {
  @apply bg-grey-light p-2 mr-4 text-left shadow rounded;
  min-width: 350px;
  }
</style>
