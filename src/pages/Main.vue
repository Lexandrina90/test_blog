<template>
  <div>
    <h1>Blog Post</h1>
    <post-list 
      :posts="posts"
      @remove="removePost"
      v-if="!isPostsLoading"
    />
    <div v-else>Loading...</div>
  </div>
</template>

<script>
import PostList from "@/components/PostList.vue";
import axios from 'axios';

export default {
  components: {
    PostList
  },
  data() {
    return {
      posts: [],
      isPostsLoading: false,
      selectedSort: '',
    }
  },
  methods: {
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id);
      localStorage.setItem('posts', JSON.stringify(this.posts));
    },
    async fetchPosts() {
      try {
        this.isPostsLoading = true;
          const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10');
          this.posts = [...this.posts, ...response.data];

          localStorage.setItem('posts', JSON.stringify(this.posts));
      } catch (e) {
        alert('Error')
      } finally {
        this.isPostsLoading = false;
      }
    },
  },
  mounted() {
    const storedPosts = localStorage.getItem('posts');
    if (storedPosts) {
      this.posts = JSON.parse(storedPosts);
    } else {
      this.fetchPosts();
    }
  },
  computed: {
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    }
  },
}
</script>

<style>
.app__btns {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}
.page__wrapper {
  display: flex;
  margin-top: 15px;
}
.page {
  border: 1px solid white;
  padding: 10px;  
}
.current-page {
  border: 2px solid teal;
}
.observer {
  height: 30px;
  background: rgb(195, 195, 229);
}
</style>