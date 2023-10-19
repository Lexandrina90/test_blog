<template>
  <Form @submit="createComment">
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
      <ErrorMessage :name="name" />
    </div>
    <button style="align-self: flex-end; margin-top: 15px"> Add Comment</button>
    <!-- <my-button
      type="button"
      style="align-self: flex-end; margin-top: 15px"
      @click="createComment"
    >
      Add Comment
    </my-button> -->
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
