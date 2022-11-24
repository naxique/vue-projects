<template>
  <div class="backdrop" @click.self="close">
    <div class="modal">
      <div v-if="isedit">
        <input type="text" name="taskTitle" v-model="title" ref="focusTitle" placeholder="Task title">
        <input type="text" name="taskDesc" v-model="desc" placeholder="Task description">
        <button @click="taskEdit" class="edit">Edit</button>
        <button @click="taskDelete" class="delete-task">Delete task</button>
        <button @click="close" class="cancel">Cancel</button>
        <button @click="moveBack" class="move-back" :class="{ 'apply-shake': shake }">Move back</button>
        <button @click="deleteCookies" class="delete-cookies">Delete cookies</button>
      </div>
      <div v-else>
        <input type="text" name="taskTitle" v-model="title" ref="focusTitle" placeholder="Task title">
        <input type="text" name="taskDesc" v-model="desc" placeholder="Task description">
        <button @click="submit" class="create">Create</button>
        <button @click="close" class="close">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from '@vue/reactivity'
import { onMounted } from '@vue/runtime-core'
export default {
  props: [ 'boardid', 'isedit', 'task' ],
  emits: [ 'modalclose', 'modalsubmit', 'taskdelete', 'taskedit', 'taskmoveback', 'deletecookies' ],
  setup(props, {emit}){
    // Shake animation
    const shake = ref(false)
    const shakeAnimation = () => {
      shake.value = true
      setTimeout(() => {
        shake.value = false
      }, 500)
    }

    const title = ref('')
    const desc = ref('')
    const focusTitle = ref(null)

    onMounted(() => {
        // Setting focus on title input field
        focusTitle.value.focus()
        // If it's edit modal, set title and desc to existing task 
        if (props.task) {
          title.value = props.task.title
          desc.value = props.task.desc
        }
    })

    // Button callbacks
    const close = () => {
      emit('modalclose')
    }
    const submit = () => {
      emit('modalsubmit', title.value, desc.value)
    }
    const taskDelete = () => {
      emit('taskdelete', props.boardid, props.task)
    }
    const taskEdit = () => {
      emit('taskedit', props.boardid, props.task.id, title.value, desc.value)
    }
    const deleteCookies = () => {
      emit('deletecookies')
    }

    // Emit task move back if the board isn't first
    const moveBack = () => {
      if (props.boardid > 0) {
        emit('taskmoveback', props.boardid, props.task.id)
      } else {
        shakeAnimation()
      }
    }

    return { close, submit, title, desc, focusTitle, taskDelete, taskEdit, moveBack, shakeAnimation, shake, deleteCookies }
  }
}
</script>

<style>
  .modal {
    width: 300px;
    padding: 20px;
    margin: 100px auto;
    background: #fff;
    border-radius: 10px;
  }
  .backdrop {
    position: fixed;
    top: 0;
    left: 0;
    background: #00000050;
    width: 100%;
    height: 100%;
    justify-content: center;
  }
  .modal input {
    text-align: center;
    margin-top: 0;
    margin: 10px 5px;
    width: 95%;
    min-height: 20px;
    border: none;
    border-bottom: 1px solid #c5c5c5;
    border-radius: 6px;
  }

  .modal button {
    border: none;
    border-radius: 5px;
    margin: 5px 43px;
    padding: 10px 10px;
    transition: 0.5s;
  }

  .modal button.create {
    background: #91ff8e;
    margin: 5px 43px;
  }
  .modal button.create:hover {
    background: #72db6f;
  }

  .modal button.close {
    background: #e4e4e4;
    margin: 5px 43px;
  }
  .modal button.close:hover {
    background: #c5c5c5;
  }

  .modal button.edit {
    background: #fdf475;
    margin: 5px 20px;
  }
  .modal button.edit:hover {
    background: #dfd660;
  }
  
  .modal button.cancel {
    background: #e4e4e4;
    margin: 5px 15px;
  }
  .modal button.cancel:hover {
    background: #c5c5c5;
  }

  .modal button.delete-task {
    background: #ffaeae;
    margin: 5px 15px;
  }
  .modal button.delete-task:hover {
    background: #f39090;
  }

  .modal button.move-back {
    background: #82bbfc;
    margin: 5px 28px;
  }
  .modal button.move-back:hover {
    background: #5c8ec7;
  }

  .modal button.delete-cookies {
    background: #82bbfc;
    margin: 5px 20px;
  }
  .modal button.delete-cookies:hover {
    background: #5c8ec7;
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