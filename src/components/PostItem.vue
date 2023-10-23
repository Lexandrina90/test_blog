<template>
  <div class="post" @click="$router.push(`/posts/${post.id}`)">
    <div class="post__body">
      <div class="post__img">
        <icon-user />
      </div>
      <div class="post__content">
        <div><strong>Title:</strong> {{ post.title }}</div>
        <div><strong>Description:</strong> {{ post.body }}</div>
        <div class="post__footer">
          <span class="post__date">
            <icon-date />
            {{ displayedDate }}</span
          >
          <span class="post__comments" style="color: gray">
            {{ commentsCount}}
            <icon-comment />
          </span>
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
import IconDate from "@/components/icons/IconDate.vue";
export default {
  components: { IconDelete, IconUser, IconComment, IconDate },
  props: {
    post: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      comments: [],
      datesMap: [],
    };
  },
  computed: {
    displayedDate() {
      const postId = this.post.id;
      if (this.datesMap[postId]) {
        const storedDate = this.datesMap[postId];
        const parts = storedDate.split("-");
        if (parts.length === 3) {
          const [year, month, day] = parts;
          return `${day}.${month}.${year}`;
        }
      }
      return "";
    },
    commentsCount() {
    const postId = this.post.id;
    const storedComments = localStorage.getItem('comments');
    
    if (storedComments) {
      const comments = JSON.parse(storedComments);
      const postComments = comments[postId] || [];
      return postComments.length;
    }
    
    return 0; 
  },
  },
  methods: {
    saveDateToLocalStorage() {
    },
  },
  mounted() {
    const storedDates = localStorage.getItem("datesMap");
    if (storedDates) {
      try {
        this.datesMap = JSON.parse(storedDates);
      } catch (error) {
        console.error("Error while parsing data from localStorage", error);
      }
    }
    const postId = this.post.id;
    if (!this.datesMap[postId]) {
      const currentDate = new Date().toISOString().slice(0, 10);
      this.datesMap[postId] = currentDate;
      localStorage.setItem("datesMap", JSON.stringify(this.datesMap));
    }

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
.post__footer {
  margin-top: 4px;
  display: flex;
  gap: 8px;
}
.post__date {
  display: flex;
  gap: 4px;
  color: teal;
  margin-right: 8px;
}
.post__comments {
  display: flex;
  gap: 4px;
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
