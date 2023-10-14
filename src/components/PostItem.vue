<template>
  <div
    class="post"
    @click="$router.push(`/posts/${post.id}`)"
  >
    <div class="post__body">
      <div class="post__img">
        <icon-user />
      </div>
      <div class="post__content">
        <div><strong>Title:</strong> {{ post.title }}</div>
        <div><strong>Description:</strong> {{ post.body }}</div>
        <div class="post__comments">
          <span class="post__date">{{ formattedDate }}</span>
          <span style="color: gray">{{ comments }}</span>
          <icon-comment />
        </div>
      </div>
    </div>
    <div class="post__btns">
      <button @click.stop="$emit('remove', post)">
        <icon-delete />
      </button>
    </div>
  </div>
</template>

<script>
import IconComment from "./icons/IconComment.vue";
import IconDelete from "./icons/IconDelete.vue";
import IconUser from "./icons/IconUser.vue";
export default {
  components: { IconDelete, IconUser, IconComment },
  props: {
    post: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      comments: 0,
    };
  },
  computed: {
    formattedDate() {
      const date = new Date();
      const day = date.getDate();
      const month = date.getMonth() + 1;
      const year = date.getFullYear();
      return `${day < 10 ? "0" : ""}${day}.${
        month < 10 ? "0" : ""
      }${month}.${year}`;
    },
  },
};
</script>

<style scoped>
.post {
  padding: 15px;
  border: 2px solid teal;
  border-radius: 20px;
  margin-top: 15px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.post__body {
  display: flex;
}
.post__content {
  align-self: center;
}
.post__comments {
  display: flex;
  gap: 4px;
}
.post__date {
  color: teal;
  margin-right: 8px;
}
.post__btns {
  display: flex;
}
button {
  border: 0;
  background: none;
  cursor: pointer;
}
</style>
