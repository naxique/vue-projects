<template>
  <IssuesPage @movedto="changeBackground" :MAX_IDS="MAX_IDS"/>

  <main>
    <div class="logo">
      <img src="./assets/logo.png" alt="Backstage Talks">
    </div>
    <div class="info">
      <p>Backstage Talks is a magazine of casual, but in depth dialogues on design and business. Our decisions shape and influence this complex world—to have a chance to make the right ones, we need to talk.</p>
      <small>© 2022 <a href="#">Published by studio Milk</a></small>
      <p><a href="#">Privacy Policy</a></p>
    </div>
    <div class="email">
      <p><a href="#">info@backstagetalks.com</a></p>
    </div>
  </main>
</template>

<script>
import IssuesPage from './components/IssuesPage.vue'

export default {
  name: 'App',
  components: { IssuesPage },
  setup() { 
    const MAX_IDS = 6

    const changeBackground = (id) => {
      let body = document.getElementsByTagName("body")[0]
      switch (id) {
        case 6:
          body.style.background = "#fff"; break;
        case 5:
          body.style.background = "#00c1b5"; break;
        case 4:
          body.style.background = "#ff651a"; break;
        case 3:
          body.style.background = "#ffbe00"; break;
        case 2:
          body.style.background = "#1d3fbb"; break;
        case 1:
          body.style.background = "#e30512"; break;
        default:
          body.style.background = "#fff"; break;
      }
    }

    return { changeBackground, MAX_IDS } 
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap');
#app {
  font-family: Roboto, sans-serif;
  overflow: hidden;
}

body {
  background: #fff;
  transition: 700ms;
  overflow: hidden;
}

main .logo {
  position: fixed;
  width: 260px;
  top: 20px;
  left: 15px;
}
main .logo img {
  width: 100%;
  height: 100%;
}

main .info {
  position: fixed;
  padding: 18px 22px;
  width: 360px;
  bottom: 0;
  left: 0;
}
main .info > p {
  font-weight: 600;
  font-size: 1.1em;
  line-height: 1.4em;
  margin-bottom: 0;
  margin-top: 30px;
}
main .info > small {
  margin-top: 0;
  font-size: 0.75em;
}
main .info a {
  color: #000;
}

main .email {
  position: fixed;
  top: 0;
  right: 0;
  padding: 4px 15px;
  font-weight: 700;
  font-size: 1.12em;
}
main .email a {
  color: #000;
  text-decoration: none;
}
</style>
