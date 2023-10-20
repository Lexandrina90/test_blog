<template>
  <Form class="form" @submit="createPost">
    <div v-for="{ as, name, children, ...attrs } in schema.fields" :key="name">
      <Field
        :as="as"
        :id="name"
        :name="name"
        v-bind="attrs"
        v-model="post[name]"
      >
        <template v-if="children && children.length">
          <component
            v-for="({ tag, text, ...childAttrs }, idx) in children"
            :key="idx"
            :is="tag"
            v-bind="childAttrs"
          >
            {{ text }}
          </component>
        </template>
      </Field>
      <ErrorMessage :name="name" class="error-msg"/>
    </div>
    <button style="align-self: flex-end; margin-top: 15px" >Create Post</button>
  </Form>
</template>
<script>
import { Form, Field, ErrorMessage } from "vee-validate";
export default {
  name: "DynamicFormPost",
  components: {
    Form,
    Field,
    ErrorMessage,
  },
  props: {
    schema: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      post: {
        title: "",
        body: "",
        text: "",
        id: null,
      },
      nextId: 1,
    };
  },

  methods: {
    createPost() {
      const storedPosts = localStorage.getItem("posts");
      this.posts = JSON.parse(storedPosts);
      const maxId = Math.max(...this.posts.map(post => post.id));

      this.nextId = maxId + 1;
      this.post.id = this.nextId;
      this.$emit("create", this.post);
      this.post = {
        title: "",
        body: "",
        text: "",
        id: this.nextId,
      };
      this.nextId++;
    }
  },
};
</script>

<style scoped>
.form {
  display: flex;
  flex-direction: column;
}
input {
  width: 100%;
  border: 1px solid teal;
  padding: 10px 15px;
  margin-top: 15px;
}
textarea {
  width: 100%;
  border: 1px solid teal;
  padding: 10px 15px;
  margin-top: 15px;
}
button {
  padding: 10px 15px;
  background: none;
  color: teal;
  border: 1px solid teal;
}
.error-msg {
  color: rgb(185, 51, 10);
}
</style>