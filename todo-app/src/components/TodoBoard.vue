<template>
  <div class="board-task-wrapper">
    <div class="board" :class="{ 'apply-shake': shake, pending: style.isPending, 'in-progress': style.isInProgress, 
                                                       done: style.isDone}" 
    @click="shakeAnimation" @click.right.prevent="shakeAnimation(true)">
      <h3>{{ boardData.title }}</h3>
      <span class="desc"><p>{{ boardData.desc }}</p></span>
    </div>
    <div class="tasks">   
      <div v-for="task in boardData.tasks" :key="task">
        <TodoBoardTask :task="task" :tasksLength="boardData.tasks.length" :style="style" 
                       @update="taskUpdate" @click.right="taskClicked(task)" />
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from '@vue/reactivity'
import TodoBoardTask from './TodoBoardTask.vue'

export default {
  name: "TodoBoard",
  emits: [ 'taskupdate', 'boardclicked', 'taskclicked' ],
  components: { TodoBoardTask },
  props: [ 'boardData' ],
  setup(props, {emit}) {
    // Shake animation
    const shake = ref(false)
    const shakeAnimation = (rightclick) => {
      if (props.boardData.id > 0 | (props.boardData.id === 0 & rightclick === true)) {
        shake.value = true
        setTimeout(() => {
          shake.value = false
        }, 500)
      } else {
        emit('boardclicked', props.boardData.id)
      }
    }

    // Defining styling
    const style = ref({ isPending: false, isInProgress: false, isDone: false, isCancelled: false })
         if (props.boardData.id === 0) style.value.isPending = true
    else if (props.boardData.id === 1) style.value.isInProgress = true
    else if (props.boardData.id === 2) style.value.isDone = true

    // Tasks manipulations
    const taskUpdate = (taskStatus) => {
      emit('taskupdate', props.boardData.id, taskStatus)
    }
    const taskClicked = (task) => {
      emit('taskclicked', task, props.boardData.id)
    }

    return { taskUpdate, taskClicked, shake, shakeAnimation, style }
  }
}
</script>

<style>
.board {
  text-align: left;
  border-radius: 20px;
  width: 10%;
  padding: 20px 20px;
  margin: 20px;
  margin-left: 30px;
  margin-right: 10px;
  box-shadow: 5px 5px 10px #888888; 
  transition: 0.5s;
}
.board.pending{
  background: #f7f7f7;
}
.board.pending:hover {
  background: #d1d1d1;
}
.board.in-progress{
  background: #fdf475;
}
.board.in-progress:hover {
  background: #dfd660;
}
.board.done{
  background: #91ff8e;
}
.board.done:hover {
  background: #72db6f;
}
.board .desc {
  color: #6e6e6e;
  font-size: 1em;
  cursor: default;
}
.board h3 {
  color: #444444;
  font-size: 1.4em;
  cursor: default;
}
.board-task-wrapper {
  display: flex;
}
.tasks {
  display: flex;
  flex-direction: row;
}

@keyframes shake {
  10%, 90%      { transform: translate3d(-1px, 0, 0); }
  20%, 80%      { transform: translate3d(1px, 0, 0); }
  30%, 50%, 70% { transform: translate3d(-2px, 0, 0); }
  40%, 60%      { transform: translate3d(2px, 0, 0); }
}
.apply-shake {
  animation: shake 0.5s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
}
</style>