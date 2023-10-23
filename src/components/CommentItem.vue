<template>
  <div class="comment">
    <div class="comment__body">
      <div class="comment__img">
        <icon-comment-user />
      </div>
      <div class="comment__content">
        <div><strong>Name:</strong> {{ comment.name }}</div>
        <div><strong>E-mail:</strong> {{ comment.email }}</div>
        <div style="margin-top: 10px;"> {{ comment.body }}</div>
      </div>
    </div>
    <div class="comment__btns">
      <button @click.stop="$emit('remove', post)">
        <icon-delete />
      </button>
    </div>
  </div>
</template>

<script>
import IconCommentUser from './icons/IconCommentUser.vue';
import IconDelete from "./icons/IconDelete.vue";
export default {
  components: { IconDelete, IconCommentUser },
  props: {
    comment: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      
    };
  },
};
</script>

<style scoped>
.comment {
  padding: 15px;
  border-bottom: 2px solid teal;
  margin-top: 15px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.comment__body {
  display: flex;
}
.comment__content {
  align-self: center;
}
.comment__img {
  margin-right: 10px;
}

.comment__btns {
  display: flex;
}
button {
  border: 0;
  background: none;
  cursor: pointer;
}
</style>
