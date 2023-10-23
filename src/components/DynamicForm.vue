<template>
  <Form @submit="createComment" class="form">
    <div v-for="{ as, name, children, ...attrs } in schema.fields" :key="name">
      <Field
        :as="as"
        :id="name"
        :name="name"
        v-bind="attrs"
        v-model="comment[name]"
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
    <button style="align-self: flex-end; margin-top: 15px" class="add-btn">Add Comment</button>
  </Form>
</template>
<script>
import { Form, Field, ErrorMessage } from "vee-validate";
export default {
  name: "DynamicForm",
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
      comment: {
        name: "",
        email: "",
        body: "",
        id: null,
      },
    };
  },

  methods: {
    createComment() {
      (this.comment.id = Date.now()), this.$emit("create", this.comment);
      this.comment = {
        name: "",
        email: "",
        body: "",
        id: Date.now(),
      };
    },
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
.add-btn:hover{
  background-color: teal;
  color: aliceblue;
  cursor: pointer;
}
</style>