<template>
  <div @click="updateStatus" @click.right.prevent=""
       class="task" :class="{ marginright: isLast, pending: style.isPending, 'in-progress': style.isInProgress, done: style.isDone}">
      <h3>{{ status.title }}</h3>
      <p>{{ status.desc }}</p>
  </div>
</template>

<script>
import { ref } from '@vue/reactivity'
export default {
  props: [ 'task', 'tasksLength', 'style' ],
  emits: [ 'update' ],
  setup(props, {emit}) {
    // Check if we need to add margin to the right
    const isLast = ref(false)
    if (props.task.id === props.tasksLength) isLast.value = true

    // Getting all the info about the task
    const status = ref({   
      id: props.task.id,
      title: props.task.title,
      desc: props.task.desc
    })

    // On update event, sends out updated task info
    const updateStatus = () => {
      emit('update', status.value)
    }

    return { isLast, updateStatus, status }
  }
}
</script>

<style>
.task {
  flex: 1;
  align-self: stretch;
  text-align: center;
  border-radius: 15px;
  min-width: 70px;
  min-height: 150px;
  padding: 20px 20px;
  margin: 20px 10px;
  box-shadow: 5px 5px 10px #888888; 
  transition: 0.5s;
}
.task.pending{
  background: #f7f7f788;
}
.task.pending:hover {
  background: #d1d1d188;
}
.task.in-progress{
  background: #fdf47588;
}
.task.in-progress:hover {
  background: #dfd66088;
}
.task.done{
  background: #91ff8e88;
}
.task.done:hover {
  background: #72db6f88;
}

.task.marginright{
  margin-right: 30px;
}
.task p {
  color: #6e6e6e;
  font-size: 1em;
  cursor: default;
  transition: 0.5s;
}
.task h3 {
  color: #444444;
  font-size: 1.4em;
  cursor: default;
  transition: 0.5s;
}
</style>