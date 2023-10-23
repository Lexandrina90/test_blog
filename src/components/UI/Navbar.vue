<template>
  <div class="navbar">
    <div @click="$router.push('/')" class="navbar__link">
      <icon-home/>
    </div>
  </div>
</template>

<script>
import router from '@/router/router';
import MyButton from './MyButton.vue';
import IconHome from '../icons/IconHome.vue';
export default {
  components: { MyButton, IconHome },
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
  padding-left: 5px;
}
.navbar__link:hover {
  color: rgb(36, 187, 187);
  cursor: pointer;
}
.navbar__btns {
  margin-left: auto;
}
</style>