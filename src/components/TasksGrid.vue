<template>
  <div class="task-grid">
      <template v-if="tasks.length">
          <Task v-for="(task, i) in tasks" :task="task"
           :key="task.name" 
           @taskDeleted="$emit('taskDeleted',i)"
           class="tasks"/>
      </template>
      <p v-else class="no-class">Sua vida esta em dia :)</p>
  </div>
</template>

<script>
import Task from './Task'
export default {
    components:{Task},
    props:{
        tasks: {type: Array, required: true}
    }
}
</script>

<style>
    .task-grid{
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
    }
    .task-grid .tasks{ margin: 10px; }
    .no-task{
        color: #aaa;
        font-size: 1.7rem;
    }
</style>