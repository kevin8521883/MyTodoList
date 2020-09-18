<template>
  <div class="todo-item p-3 mt-2" :class="{'background-stared':todo.stared}">
    <div class="todo-header row">
      <div class="todo-check col-1">
        <input
          type="checkbox"
          id="123"
          style="width:24px; height:24px"
          v-model="todo.complete"
          @change="complete"
        />
        <label for="123"></label>
      </div>
      <div
        class="todo-title text-truncate col-9"
        :class="{'text-complete':todo.complete}"
      >{{ todo.title }}</div>
      <div class="todo-control ml-auto col-2">
        <a href="#" class="text-muted" @click.prevent="stared(todo.id)">
          <i class="far fa-star mx-3" :class="{'stared-color':todo.stared,'fas':todo.stared}"></i>
        </a>
        <!-- <a href="#" class="text-muted">
          <i class="fas fa-pencil-alt mr-3"></i>
        </a> -->
        <a href="#" class="text-muted" @click.prevent="delTodo(todo.id)">
          <i class="fas fa-times"></i>
        </a>
      </div>
    </div>
    <div class="todo-footer row">
      <div class="col-1"></div>
      <div class="col-5 text-secondary">
        <i class="far fa-calendar-alt"></i>
        {{todo.day}}
        <span v-if="todo.deadLine">～{{todo.deadLine}}</span>
      </div>
      <div v-if="todo.comment" class="col-4 text-secondary">
        <i class="far fa-comment-dots" data-toggle="tooltip" data-placement="bottom" :title="`${todo.comment}`"></i>
      </div>
    </div>
  </div>
</template>

<script>
import $ from "jquery";
export default {
  props: ["todo"],
  methods: {
    complete() {
      this.$emit("complete");
    },
    stared(id) {
      this.$emit("stared", id);
    },
    delTodo(id) {
      this.$emit("delTodo", id);
    },
  },
  created() {
    $(function () {
      $('[data-toggle="tooltip"]').tooltip();
    });
  },
};
</script>
