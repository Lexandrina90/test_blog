<template>
  <div>
    <div class="main__header">
      <h1>Blog Post</h1>
      <div class="main__btns">
        <my-button @click="showDialog"> Create new Post </my-button>
      </div>
      <my-dialog v-model:show="dialogVisible">
        <post-form @create="createPost" />
      </my-dialog>
    </div>
    <post-list :posts="posts" @remove="removePost" v-if="!isPostsLoading" />
    <div v-else>Loading...</div>
  </div>
</template>

<script>
import PostList from "@/components/PostList.vue";
import PostForm from "@/components/PostForm.vue";
import axios from "axios";

export default {
  components: {
    PostList,
    PostForm,
  },
  data() {
    return {
      posts: [],
      isPostsLoading: false,
      selectedSort: "",
      dialogVisible: false,
      datesMap: {},
      comments:[],
    };
  },
  methods: {
    removePost(post) {
      this.posts = this.posts.filter((p) => p.id !== post.id);
      localStorage.setItem("posts", JSON.stringify(this.posts));

      const storedDatesMap = JSON.parse(localStorage.getItem("datesMap"));
      delete storedDatesMap[post.id];
      localStorage.setItem("datesMap", JSON.stringify(storedDatesMap));
    },
    showDialog() {
      this.dialogVisible = true;
    },
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false;
      localStorage.setItem("posts", JSON.stringify(this.posts));
    },
    async fetchPosts() {
      try {
        this.isPostsLoading = true;
        const response = await axios.get(
          "https://jsonplaceholder.typicode.com/posts?_limit=10"
        );
        const currentDate = "2023-10-15";
        response.data.forEach((post) => {
          post.text = `Lorem ipsum, dolor sit amet consectetur adipisicing elit. Architecto, blanditiis sequi. Ad aliquid soluta sapiente eum sint provident perspiciatis ullam. Saepe possimus unde doloribus hic vero aliquid ut necessitatibus quos.
          Eos pariatur illo blanditiis. Pariatur libero sed illum in reiciendis dicta recusandae ipsa incidunt autem ullam placeat veniam molestiae iusto totam qui accusamus, suscipit quaerat natus aperiam perspiciatis. Exercitationem, labore.
          Voluptatem quidem corporis saepe placeat ipsam, culpa a reiciendis dolore esse ea ut laudantium iusto. Qui voluptate quisquam reiciendis, iure in culpa aspernatur placeat vero assumenda, amet autem quae ad.
          Ut voluptatibus libero quam tenetur in! Similique laudantium corporis quidem alias nisi suscipit, facilis, accusantium tempora pariatur aut, repellat numquam quod consectetur rerum quis ipsum eos architecto? Eaque, vero sed?
          Minus, aut. Officiis ex corrupti provident facilis, ipsa necessitatibus quasi neque alias quidem saepe in illo obcaecati dolores vero doloribus dolore repudiandae, nihil nobis rerum ut sequi amet nesciunt! Sit?`;

          this.datesMap[post.id] = currentDate;
          this.fetchCommentsForPost(post.id);
        });
        this.posts = [...this.posts, ...response.data];

        localStorage.setItem("posts", JSON.stringify(this.posts));
        localStorage.setItem("datesMap", JSON.stringify(this.datesMap));
      } catch (e) {
        alert("Error");
      } finally {
        this.isPostsLoading = false;
      }
    },
    async fetchCommentsForPost(postId) {
  try {
    const savedComments = JSON.parse(localStorage.getItem("comments")) || {};

    for (let postId = 1; postId <= 10; postId ++) {
     
        const response = await axios.get(
          `https://jsonplaceholder.typicode.com/comments?postId=${postId}`
        );
        savedComments[postId] = response.data;
        localStorage.setItem("comments", JSON.stringify(savedComments));
      
    }
  } catch (e) {
    alert("Error");
  }
},
  },
  mounted() {
    const storedPosts = localStorage.getItem("posts");
    if (storedPosts) {
      this.posts = JSON.parse(storedPosts);
    } else {
      this.fetchPosts();
    }
  },
  computed: {
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter((post) =>
        post.title.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },
  },
};
</script>

<style>
.main__header {
  display: flex;
  justify-content: space-between;
}
.main__btns {
  display: flex;
  gap: 10px;
}
</style>
