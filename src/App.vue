<template>
  <button v-if="!showModal"  class="btn" @click="showModal = true">Налоговый вычет</button>
  
  <Card v-else @close="showModal = false, clean = v$.$reset()"/>
</template>

<script>
import Card from './components/Card.vue'
import useVuelidate from '@vuelidate/core'

export default {
  name: 'App',
  setup () {
    return { 
      v$: useVuelidate() 
    }
  },
  data() {
    return {
      showModal: false
    }
  },
 
  components: {
    Card
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Karla:wght@500&display=swap');

:root {
  --main-color: linear-gradient(
    255.35deg,
    #dc3131 0.83%,
    rgba(255, 79, 79, 0) 108.93%
  ),
  linear-gradient(0deg, #ff5e56, #ff5e56);
  --hover-color: #EA0029;
  --popup-duration: 1s;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: #fff;
  font-family: 'Karla', sans-serif;
  font-size: 14px;
  line-height: 1.72;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  /* overflow: hidden; */
  background: var(--main-color);
  scroll-behavior: smooth;
  text-rendering: optimizeSpeed;
}

.btn {
  height: 56px;
  width: 198px;
  background: var(--main-color);
  border: 1px solid #fff;
  border-radius: 6px;
  color: #fff;
  cursor: pointer;
  box-shadow: 0px 0px 24px 0px #ea002954;
}

.btn:hover {
  background: var(--hover-color);
  box-shadow: 0px 0px 24px 0px #EA002954;
}
.btn:disabled {
  background: #BEC5CC;
}
</style>
