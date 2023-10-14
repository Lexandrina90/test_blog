<template>
  <div>
    <transition-group name="comment-list">
      <comment-item 
        v-for="comment in comments"
        :comment="comment"
        :key="comment.id" 
        @remove="$emit('remove', comment)"
      />
    </transition-group>
  </div>
</template>

<script>
import CommentItem from "@/components/CommentItem";

export default {
  components: {CommentItem},
  props: {
    comments: {
      type: Array,
      required: true,
    }
  }
}
</script>

<style scoped>
.comment-list-item {
  display: inline-block;
  margin-right: 10px;
}
.comment-list-enter-active,
.comment-list-leave-active {
  transition: all 0.4s ease;
}
.comment-list-enter-from,
.comment-list-leave-to {
  opacity: 0;
  transform: translateX(130px);
}
.comment-list-move {
  transition: transform 0.8s ease;
}

</style>