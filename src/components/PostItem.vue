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
        <div class="post__footer">
          <span class="post__date">
            <icon-date/>
            {{ displayedDate }}</span>
          <span class="post__comments" style="color: gray">
            {{ comments }}
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
import IconDate from '@/components/icons/IconDate.vue';
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
      comments: 0,
      datesMap:[],
    };
  },
  computed: {
    // storedDate() {
    //   if (this.datesArray.length > 0) {
    //     return new Date(this.datesArray[this.datesArray.length - 1]);
    //   }
    //   return null;
    // },
    displayedDate() {
      const postId = this.post.id;
      if (this.datesMap[postId]) {
        return this.datesMap[postId];
      }
      return "";
    },
  },
  methods: {
    saveDateToLocalStorage() {
      const date = new Date();
      const formattedDate = `${date.getFullYear()}-${(date.getMonth() + 1).toString().padStart(2, '0')}-${date.getDate().toString().padStart(2, '0')}`;
      const datesMap = this.datesMap || [];
      datesMap.push(formattedDate);
      // const newId = Object.keys(datesMap).length + 1;
      // datesMap[newId] = formattedDate;
      localStorage.setItem("datesMap", JSON.stringify(datesMap));
    },
  },
  created() {
    const storedDatesMap = localStorage.getItem("datesMap");
    if (storedDatesMap) {
      this.datesMap = JSON.parse(storedDatesMap);
    }
    this.saveDateToLocalStorage();
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
