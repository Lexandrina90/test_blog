<template>
  <form @submit.prevent>
    <h4>Add comment</h4>
    <my-input
      v-focus
      v-model="comment.name"
      type="text"
      placeholder="Name"
      novalidate="true"
    />
    <my-input
      v-model="comment.email"
      type="email"
      placeholder="E-mail"
    />
    <textarea
      v-model="comment.body"
      type="text"
      placeholder="Text"
    />
    <my-button 
      style="align-self: flex-end; margin-top: 15px;"
      @click="createComment"
    >
      Add Comment
    </my-button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      comment: {
        name: '',
        email: '',
        body: '',
      }
    }
  },
  methods: {
    createComment() {
      this.comment.id = Date.now(),
      this.$emit('create', this.comment);
      this.comment = {
        name: '',
        email: '',
        body: ''
      }
    }
  }
}
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
}
textarea {
  width: 100%;
  border: 1px solid teal;
  padding: 10px 15px;
  margin-top: 15px;
}
</style>
