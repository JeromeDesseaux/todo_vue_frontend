<template>
<a-form :form="form" @submit="handleSubmit">
    <a-form-item label="Todo">
      <a-input
        v-decorator="['todo', { rules: [{ required: true, message: 'Please input your todo!' }] }]"
      />
    </a-form-item>
    <a-form-item>
      <a-button type="primary" html-type="submit">
        Add
      </a-button>
    </a-form-item>
  </a-form>
</template>


<script>
import bus from "../main";
import axios from 'axios';
import config from '../config/config';

export default {
  data() {
    return {
      formLayout: 'horizontal',
      form: this.$form.createForm(this, { name: 'coordinated' }),
    };
  },
  methods: {
    handleSubmit(e) {
      e.preventDefault();
      this.form.validateFields(async (err, values) => {
        if (!err) {
            await axios.post(config.endpointUrl, {name: values.todo, done: false});
            this.refreshTodo();
            this.form.resetFields();
        }
      });
    },
    refreshTodo() {
        bus.$emit("refresh");
    }
  },
};
</script>