<template>
  <div class="comment">
    <div class="comment__body">
      <div class="comment__img">
        <icon-user />
      </div>
      <div class="comment__content">
        <div><strong>Name:</strong> {{ comment.name }}</div>
        <div><strong>E-mail:</strong> {{ comment.email }}</div>
        <div><strong>Comment:</strong> {{ comment.body }}</div>
        <!-- <span class="comments__date">{{ formattedDate }}</span> -->
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
import IconDelete from "./icons/IconDelete.vue";
import IconUser from "./icons/IconUser.vue";
export default {
  components: { IconDelete, IconUser },
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
  // computed: {
  //   formattedDate() {
  //     const date = new Date();
  //     const day = date.getDate();
  //     const month = date.getMonth() + 1;
  //     const year = date.getFullYear();
  //     return `${day < 10 ? "0" : ""}${day}.${
  //       month < 10 ? "0" : ""
  //     }${month}.${year}`;
  //   },
  // },
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
/* .post__date {
  color: teal;
  margin-right: 8px;
} */
.comment__btns {
  display: flex;
}
button {
  border: 0;
  background: none;
  cursor: pointer;
}
</style>
