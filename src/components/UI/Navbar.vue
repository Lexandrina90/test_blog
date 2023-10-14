<template>
  <div class="navbar">
    <div @click="$router.push('/')" class="navbar__link">Home</div>
    <!-- <div class="navbar__btns">
      <my-button @click="$router.push('/')"> Home </my-button>
      <my-button 
        style="margin-left: 20px;" 
        @click="$router.push('/about')"
      > 
        О сайте
      </my-button>
    </div> -->
  </div>
</template>

<script>
import router from '@/router/router';
import MyButton from './MyButton.vue'
export default {
  components: { MyButton },
  
}
</script>

<style scoped> 
.navbar {
  height: 50px;
  background-color: lightgray;
  box-shadow: 2px 2px 4px gray;
  display: flex;
  align-items: center;
  padding: 0 15px;
}
.navbar__link {
  color: teal;
  font-weight: bold;
}
.navbar__link:hover {
  color: rgb(36, 187, 187);
  cursor: pointer;
}
.navbar__btns {
  margin-left: auto;
}
</style>