<template>
  <div>
    <div class="page__header">
      <h2 style="color: teal">Пост № {{ $route.params.id }}</h2>
      <button @click="editPost">
        <icon-edit />
      </button>
    </div>
    <div v-if="post" class="post-content">
      <div class="post__title">
        <h1>Title:</h1>
        <div
          :contenteditable="isEdit ? 'true' : 'false'"
          @blur="saveAndExitEditMode"
          @input="editField($event, 'title')"
          style="outline: none"
          :class="{ 'editable': isEdit }"
        >
          {{ post.title }}
        </div>
      </div>

      <div class="post__description">
        <h2>Description:</h2>
        <div
          :contenteditable="isEdit ? 'true' : 'false'"
          @blur="saveAndExitEditMode"
          @input="editField($event, 'body')"
          style="outline: none"
          :class="{ 'editable': isEdit }"
        >
          {{ post.body }}
        </div>
      </div>

      <div class="post__text">
        <h2>Text:</h2>
        <div
          :contenteditable="isEdit ? 'true' : 'false'"
          @blur="saveAndExitEditMode"
          @input="editField($event, 'text')"
          style="outline: none"
          :class="{ 'editable': isEdit }"
        >
          {{ post.text }}
        </div>
        <span class="post__date">
          <icon-date />
          {{ postDate }}
        </span>
      </div>
    </div>
    <div v-else>Loading...</div>
    <div class="post-comments">
      <comment-list :comments="comments" @remove="removeComment" />
    </div>
    <my-button @click="showDialog" style="margin-top: 10px">
      Add Comment
    </my-button>
    <my-dialog v-model:show="dialogVisible">
      <comment-form @create="createComment" />
    </my-dialog>
  </div>
</template>

<script>
import CommentList from "@/components/CommentList.vue";
import axios from "axios";
import IconDate from "@/components/icons/IconDate.vue";
import IconEdit from "@/components/icons/IconEdit.vue";
import CommentForm from "@/components/CommentForm.vue";
export default {
  components: { CommentList, IconDate, CommentForm, IconEdit },
  data() {
    return {
      post: [],
      comments: [],
      dialogVisible: false,
      isEdit: false,
      editedPost: {
        title: "",
        body: "",
        text: "",
      },
    };
  },
  computed: {
    postDate() {
      const savedDates = localStorage.getItem("datesMap");
      const dates = JSON.parse(savedDates);
      const postId = Number(this.$route.params.id);
      const storedDate = dates[postId - 1];
      if (storedDate) {
        const date = new Date(storedDate);
        const day = date.getDate();
        const month = date.getMonth() + 1;
        const year = date.getFullYear();
        return `${day < 10 ? "0" : ""}${day}.${
          month < 10 ? "0" : ""
        }${month}.${year}`;
      } else {
        return "No date available";
      }
    },
  },

  methods: {
    removeComment(comment) {
      this.comments = this.comments.filter((c) => c.id !== comment.id);
    },
    createComment(comment) {
      this.comments.push(comment);
      this.dialogVisible = false;
    },
    showDialog() {
      this.dialogVisible = true;
    },
    async fetchComments() {
      try {
        const response = await axios.get(
          `https://jsonplaceholder.typicode.com/comments?postId=${this.post.id}&_limit=5`
        );
        this.comments = [...this.comments, ...response.data];
      } catch (e) {
        alert("Error");
      }
    },
    editPost() {
      this.isEdit = true;
    },
    editField(event, fieldName) {
      this.editedPost[fieldName] = event.target.innerText;
      console.log(this.post[fieldName]);
      console.log(this.editedPost[fieldName]);
      // if (this.post[fieldName] !== editedPost[fieldName]) {
      //   this.isUpdate = true;
      // }
      // console.log(this.isUpdate)

      const savedPosts = localStorage.getItem("posts");
      const posts = JSON.parse(savedPosts);
      const postId = Number(this.$route.params.id);
      const postIndex = posts.findIndex((post) => post.id === postId);

      if (postIndex !== -1) {
        posts[postIndex][fieldName] = this.editedPost[fieldName];
        localStorage.setItem("posts", JSON.stringify(posts));
      }
    },
    saveAndExitEditMode() {
      this.isEdit = false;
    },
  },
  mounted() {
    const savedPosts = localStorage.getItem("posts");
    const posts = JSON.parse(savedPosts);

    const postId = Number(this.$route.params.id);
    this.post = posts.find((post) => post.id === postId);

    this.fetchComments();
  },
};
</script>

<style scoped>
.post-content {
  display: flex;
  flex-direction: column;
  gap: 4px;
}
.page__header {
  display: flex;
  gap: 12px;
}
.post__date {
  position: absolute;
  display: flex;
  gap: 4px;
  right: 40px;
  color: teal;
}
button {
  border: 0;
  background: none;
  cursor: pointer;
}
input {
  border: none;
  width: 100%;
  outline: none;
  font-size: inherit;
  font-family: inherit;
  white-space: normal;
}
.editable {
  border-bottom: 1px solid teal;
  padding: 5px;
}
</style>
