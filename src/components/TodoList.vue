<template>
  <a-list class="demo-loadmore-list" :loading="loading" v-on:refresh="fetchTodo" itemLayout="horizontal" :dataSource="data">
    <a-list-item slot="renderItem" slot-scope="item">
      <a slot="actions" v-on:click="updateTodo(item)">done</a>
      <a slot="actions" v-on:click="deleteTodo(item)">delete</a>
      <div v-bind:class="{ done: item.done }">
          <!-- <a-checkbox v-on:change="updateTodo(item)"></a-checkbox> -->
          {{item.name}}</div>
    </a-list-item>
  </a-list>
</template>
<script>
  import axios from 'axios';
  import { bus } from "../main";
  import config from '../config/config';

    const endpoint = config.endpointUrl;

  export default {
    data() {
      return {
        loading: true,
        data: [],
      };
    },
    created: function() {
        this.fetchTodo();
        this.listenToEvents();
    },
    methods: {
      getData() {
          return axios.get(endpoint).then(res => {
              return res.data;
          });
      },
      fetchTodo() {
        this.loading = true;
        this.getData().then(res => {
            this.loading = false;
            this.data = res;
        });
    },
      listenToEvents() {
        bus.$on("refresh", () => {

            this.fetchTodo();
        });
      },
    updateTodo: async function(todo) {
        /* eslint-disable no-console */
        let id = todo._id;
        todo.done = !todo.done;
        await axios.put(endpoint+id, todo)
        this.fetchTodo();
        console.log("Update todo id " + id);
    },
    deleteTodo: async function(todo) {
        /* eslint-disable no-console */
        let id = todo._id;
        await axios.delete(endpoint+id)
        this.fetchTodo();
        console.log("Delete todo id "+id);
    },
  }
}
</script>
<style>
  .demo-loadmore-list {
    min-height: 350px;
  }
  .done {
      text-decoration: line-through;
  }
</style>