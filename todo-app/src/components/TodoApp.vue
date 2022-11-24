<template>
  <div class="todo-wrapper" v-if="boardData">
      <div class="todo-boards" v-for="board in boardData.Boards" :key="board">
        <TodoBoard @boardclicked="boardClicked" @taskclicked="taskClicked"
                   :boardData="board" @taskupdate="taskUpdate" :key="rerender" />
      </div>    
      <div v-if="showModal">
        <TaskModal @modalclose="toggleModal" @modalsubmit="modalSubmit" @taskdelete="taskDelete" @taskedit="taskEdit" @taskmoveback="taskMoveBack" @deletecookies="deleteCookies"
                   :boardid="passBoardId" :isedit="isEdit" :task="passTask" /> 
      </div>
  </div>
  <div v-else>
    <p>Loading, please wait...</p>
  </div>
</template>

<script>
import TodoBoard from './TodoBoard.vue'
import { onMounted, ref } from '@vue/runtime-core'
import TaskModal from './TaskModal.vue'
import VueCookies from 'vue-cookies'
import defaultData from '../assets/default.json'


// TODO:
// 3. Try to fix shit with griding when there's a lot of tasks
// 4. Try to figure out how to animate any moving
// 5. Look around code and comment everything + fix shit i don't like

export default {
  name: 'TodoApp', 
  components: { TodoBoard, TaskModal },
  setup() {
    // define the board-task data array
    const boardData = ref([])

    // Saving data to cookies
    const setCookies = (json) => {
      VueCookies.set('db', json, { expires: '7d' })
      console.log("Saved to cookie!")
    }
    // Getting data from cookies, if there is any
    const getCookies = () => {
      let db = VueCookies.get('db')
      if (db) {
        return db
      } else {
        return null
      }
    }
    // Deleting cookies
    const deleteCookies = () => {
      VueCookies.remove('db')
      boardData.value = defaultData
      forceRerender()
      toggleModal()
      console.log("Removed cookie storage")
    }

    // :key manipulation to force rerender on component
    const rerender = ref(0)
    const forceRerender = () => {
      rerender.value += 1
    }

    // Modal variables setup
    const showModal = ref(false)
    const isEdit = ref(false)
    // Declaring variables that would be passed into modal
    const passBoardId = ref()
    const passTask = ref({})

    // Toggle modal function
    const toggleModal = (boardid, task) => {
      passBoardId.value = boardid
      passTask.value = task
      showModal.value = !showModal.value
    }

    // Function to move tasks
    const moveTask = (boardId, newBoardId, task) => {
      // delete task from previous board
      boardData.value.Boards[boardId].tasks.splice(task.id-1, 1)
      // assign new id for a task and push it in new board
      task.id = boardData.value.Boards[newBoardId].tasks.length+1
      boardData.value.Boards[newBoardId].tasks.push(task)
      setCookies(JSON.stringify(boardData.value))
    }

    // If user clicked on board
    const boardClicked = (id) => {
      isEdit.value = false
      toggleModal(id, null)
    }
    // Or on a task
    const taskClicked = (task, boardid) => {
      isEdit.value = true
      toggleModal(boardid, task)
    }
    // Then toggle modal :)

    // On submit 
    const modalSubmit = (taskTitle, taskDesc) => {
      // Push the new task into data array and save it
      boardData.value.Boards[0].tasks.push({ id: boardData.value.Boards[0].tasks.length+1, title: taskTitle, desc: taskDesc })
      setCookies(JSON.stringify(boardData.value))
      forceRerender()
      toggleModal()
    }

    // Updating task status
    const taskUpdate = (boardId, taskStatus) => {
      // If the board isn't the last one, move task to the next board
      if (boardId < 2) {
        moveTask(boardId, boardId+1, taskStatus)
      }
      forceRerender()
    }
    // Delete task and save to the cookies
    const taskDelete = (boardId, task) => {
      boardData.value.Boards[boardId].tasks.splice(task.id-1, 1)
      setCookies(JSON.stringify(boardData.value))
      forceRerender()
      toggleModal()
    }
    // Edit a task
    const taskEdit = (boardId, taskId, taskTitle, taskDesc) => {
      boardData.value.Boards[boardId].tasks[taskId-1].title = taskTitle
      boardData.value.Boards[boardId].tasks[taskId-1].desc = taskDesc
      setCookies(JSON.stringify(boardData.value))
      forceRerender()
      toggleModal()
    }
    // Move the task to previous board
    const taskMoveBack = (boardId, taskId) => {
      moveTask(boardId, boardId-1, boardData.value.Boards[boardId].tasks[taskId-1])
      setCookies(JSON.stringify(boardData.value))
      forceRerender()
      toggleModal()
    }

    // Initial request for data
    onMounted(() => { 
      let cookies = getCookies()
      if (cookies === null) {
        boardData.value = defaultData
      } else {
        boardData.value = cookies
      }
    })

    return { boardData, taskUpdate, taskDelete, taskClicked, taskEdit, boardClicked, taskMoveBack, deleteCookies,
             showModal, toggleModal, modalSubmit, passBoardId, passTask, isEdit, forceRerender, rerender }
  }
}
</script>

<style>
.todo-wrapper {
  display: block;
  float: left;
  min-width: 900px;
  width: 100%;
}
</style>