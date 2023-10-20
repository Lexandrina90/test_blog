<template>
  <DynamicFormPost :schema="formSchema" submit="createPost"/>
</template>

<script>
import * as Yup from 'yup';
import DynamicFormPost from './DynamicFormPost.vue';

export default {
 components: {
   DynamicFormPost
 },
 data() {
   return {
     post:{
       title: '',
       description: '',
       body: '',
       id: null,
     },
     formSchema: {
       fields: [
         {
           placeholder: 'Title',
           name: 'title',
           as: 'input',
           rules: Yup.string().required().max(50),
         },
         {
           placeholder: 'Description',
           name: 'body',
           as: 'input',
           rules: Yup.string().required().max(100),
         },
         {
           placeholder: 'Text',
           name: 'text',
           as: 'textarea',
           rules: Yup.string().required().max(255)
         }
       ]
     }
   }
 },
};
</script>


