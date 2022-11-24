<template>
  <div class="issues">
    <div class="issue" v-for="n in MAX_IDS" :key="n" :id="`issue-${MAX_IDS-(n-1)}`">  
      <img :src="require(`../assets/cover_issue_${MAX_IDS-(n-1)}.png`)" alt="issue cover">
      <div v-if="n == 6">
        <p>Issue #{{ MAX_IDS-(n-1) }} is sold out.</p>
        <p>If you are lucky, you may get the last pieces in <a href="#">selected stores</a></p>   
      </div> <div v-else>
        <p>Issue #{{ MAX_IDS-(n-1) }}</p>
        <p><a href="#">BUY HERE</a></p>
        <p>or in <a href="#">selected stores</a></p>
      </div>
    </div>
  </div>  

  <div class="issues-count">
    <span v-for="n in MAX_IDS" :key="n">
      <a href="#" @click.prevent="moveTo(MAX_IDS-(n-1))">Issue #{{ MAX_IDS-(n-1) }}</a>
    </span>
  </div>
</template>

<script>
import { onMounted, ref } from '@vue/runtime-core';

export default {
  props: [ 'MAX_IDS' ],
  emits: [ 'movedto' ],
  setup(props, { emit }) {
    const currentId = ref(props.MAX_IDS)

    const moveTo = (id) => {
      document.getElementById("issue-"+id).scrollIntoView({ behavior: "smooth" })
      currentId.value = id
      emit("movedto", id)
    }

    const scrollEvent = (ev) => {
      ev.preventDefault()
      ev.stopPropagation()

      if (ev.type=='wheel') {
        if (ev.deltaY > 0) {
          if (currentId.value > 1) {
            moveTo(currentId.value - 1)
          }
        } else {
          if (currentId.value < props.MAX_IDS) {
            moveTo(currentId.value + 1)
          }
        }
      }

      setTimeout(() => {
        window.addEventListener('wheel', scrollEvent, { once: true, passive: false })
      }, 700)
    }

    onMounted(() => {
      window.addEventListener('wheel', scrollEvent, { once: true, passive: false })
    })

    return { moveTo }
  }
}
</script>

<style scoped>
.issues {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  flex: 1;
  overflow: hidden;
}

.issue {
  text-align: center;
  height: 98vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.issue img {
  width: 420px;
  vertical-align: middle;
  align-self: center;
}
.issue p {
  font-weight: 700;
  font-size: 1em;
}
.issue a {
  font-weight: 700;
  color: #fff;
  text-decoration: none;
}
.issue a:hover {
  text-decoration: underline;
}


.issue#issue-6 a {
  font-weight: 700;
  color: #E568AC;
  text-decoration: none;
}
.issue#issue-6 a:hover {
  text-decoration: underline;
}


.issues-count {
  display: flex;
  flex-direction: column;
  flex: 1;
  gap: 8px;
  position: fixed;
  bottom: 0;
  right: 0;
  font-size: 1.12em;
  margin: 22px 15px;
}
.issues-count a {
  text-decoration: none;
  color: #000;
}
</style>