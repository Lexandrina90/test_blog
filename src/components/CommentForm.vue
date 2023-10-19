<template>
   <DynamicForm :schema="formSchema"/>
</template>

<script>
import * as Yup from 'yup';
import DynamicForm from './DynamicForm.vue';

export default {
  components: {
    DynamicForm
  },
  data() {
    return {
      comment:{
        name: '',
        email: '',
        body: '',
        id: null,
      },
      formSchema: {
        fields: [
          {
            placeholder: 'Name',
            name: 'name',
            as: 'input',
            rules: Yup.string().required(),
          },
          {
            placeholder: 'Email',
            name: 'email',
            as: 'input',
            rules: Yup.string().email(),
          },
          {
            placeholder: 'Text',
            name: 'body',
            as: 'textarea',
            rules: Yup.string().required()
          }
        ]
      }
    }
  },
};
</script>


