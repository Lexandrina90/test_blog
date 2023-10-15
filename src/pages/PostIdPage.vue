<template>
 <div>
    <h2 style="color:teal"> Пост № {{ $route.params.id }} </h2>
    <div v-if="post" class="post-content">
      <h1>Title:</h1> {{ post.title }}
      <h2>Description:</h2> {{ post.body }}
      <h2>Text:</h2>
      <div>
        Lorem ipsum, dolor sit amet consectetur adipisicing elit. Architecto, blanditiis sequi. Ad aliquid soluta sapiente eum sint provident perspiciatis ullam. Saepe possimus unde doloribus hic vero aliquid ut necessitatibus quos.
        Eos pariatur illo blanditiis. Pariatur libero sed illum in reiciendis dicta recusandae ipsa incidunt autem ullam placeat veniam molestiae iusto totam qui accusamus, suscipit quaerat natus aperiam perspiciatis. Exercitationem, labore.
        Voluptatem quidem corporis saepe placeat ipsam, culpa a reiciendis dolore esse ea ut laudantium iusto. Qui voluptate quisquam reiciendis, iure in culpa aspernatur placeat vero assumenda, amet autem quae ad.
        Ut voluptatibus libero quam tenetur in! Similique laudantium corporis quidem alias nisi suscipit, facilis, accusantium tempora pariatur aut, repellat numquam quod consectetur rerum quis ipsum eos architecto? Eaque, vero sed?
        Minus, aut. Officiis ex corrupti provident facilis, ipsa necessitatibus quasi neque alias quidem saepe in illo obcaecati dolores vero doloribus dolore repudiandae, nihil nobis rerum ut sequi amet nesciunt! Sit?
      </div>
      <span class="post__date">
        <icon-date/>
        {{ displayedDate }}
      </span>
    </div>
    <div v-else>
      Loading...
    </div>
    <div  class="post-comments">
      <comment-list
        :comments="comments"
        @remove="removeComment"
      />
    </div>
    <my-button
        @click="showDialog"
        style="margin-top: 10px;"
      >
        Add Comment
      </my-button>
    <my-dialog v-model:show="dialogVisible">
      <comment-form @create="createComment"/>
    </my-dialog>
  </div>
</template>

<script>
import CommentList from '@/components/CommentList.vue';
import axios from 'axios';
import IconDate from '@/components/icons/IconDate.vue';
import CommentForm from '@/components/CommentForm.vue';
export default {
  components: { CommentList, IconDate, CommentForm },
  data() {
    return {
      post: [], 
      comments: [],
      dialogVisible: false,
    }
  },
  computed: {
    storedDate() {
      const storedDate = localStorage.getItem('myDate');
      if (storedDate) {
        return new Date(JSON.parse(storedDate));
      }
      return null;
    },
    displayedDate() {
      const date = this.storedDate || new Date();
      const day = date.getDate();
      const month = date.getMonth() + 1;
      const year = date.getFullYear();
      return `${day < 10 ? "0" : ""}${day}.${month < 10 ? "0" : ""}${month}.${year}`;
    },
  },

  methods: {
    removeComment(comment) {
      this.comments = this.comments.filter(c => c.id !== comment.id)
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
        const response = await axios.get(`https://jsonplaceholder.typicode.com/comments?postId=${this.post.id}&_limit=5`);
        this.comments = [...this.comments, ...response.data];
      } catch (e) {
        alert('Error')
      }
    },
    saveDateToLocalStorage() {
      const date = this.storedDate || new Date();
      const formattedDate = `${date.getFullYear()}-${(date.getMonth() + 1).toString().padStart(2, '0')}-${date.getDate().toString().padStart(2, '0')}`;
      localStorage.setItem('myDate', JSON.stringify(formattedDate));
    },
  },
  mounted() {
    const savedPosts = localStorage.getItem('posts');
    const posts = JSON.parse(savedPosts);

    const postId = Number(this.$route.params.id);
    this.post = posts.find(post => post.id === postId);

    this.fetchComments();    
  },
  created() {
    this.saveDateToLocalStorage();
  },
}
</script>

<style scoped>

.post__date {
  position: absolute;
  display: flex;
  gap: 4px;
  right: 40px;
  color: teal;
}

</style>