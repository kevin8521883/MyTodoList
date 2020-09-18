<template>
  <div id="app">
    <div class="bg-primary">
      <div class="container d-flex todo-nav">
        <a href="#" @click="status='all'" :class="{'active':status==='all'}">全部（{{todos.length}}）</a>
        <a
          href="#"
          @click="status='unfinished'"
          :class="{'active':status==='unfinished'}"
        >進行中（{{unfinished}}）</a>
        <a
          href="#"
          @click="status='Completed'"
          :class="{'active':status==='Completed'}"
        >完成（{{completed}}）</a>
      </div>
    </div>
    <div class="container my-4">
      <div v-if="!isActive" class="position-relative">
        <i class="fas fa-plus fa-lg text-gray position-absolute" style="left: 1rem; top: 1.15rem"></i>
        <input
          type="text"
          class="form-control form-control-lg pl-5"
          placeholder="Add Task"
          @click="isActive=true"
        />
      </div>
      <div class="mt-4">
        <EditTodoItem v-if="isActive" @cancel="isActive=false" @addTodo="addTodo"></EditTodoItem>
        <TodoItem
          :todo="item"
          @delTodo="delTodo"
          @stared="stared"
          @complete="updata"
          v-for="item in filterTodos"
          :key="item.id"
        ></TodoItem>
      </div>
    </div>
  </div>
</template>

<script>
import TodoItem from "./components/TodoItem";
import EditTodoItem from "./components/EditTodoItem";
export default {
  name: "App",
  data() {
    return {
      isActive: false,
      status: "all",
      todos: JSON.parse(localStorage.getItem("todos")) || [],
    };
  },
  components: {
    TodoItem,
    EditTodoItem,
  },
  methods: {
    addTodo(item) {
      const vm = this;
      const date = new Date();
      const today = `${date.getFullYear()}-${
        date.getMonth() + 1
      }-${date.getDate()}`;
      if (item.title) {
        const timeInMs = Date.now();
        const addNew = {
          title: item.title,
          day: today,
          deadLine: item.deadLine,
          comment: item.comment,
          id: timeInMs,
          stared: false,
          complete: false,
        };
        vm.todos.push(addNew);
        vm.updata();
        vm.isActive = false;
      }
    },
    editTodo(){
      
    },
    stared(id) {
      const vm = this;
      vm.todos.forEach((item) => {
        if (item.id === id) {
          item.stared = !item.stared;
        }
      });
      vm.updata();
    },
    delTodo(id) {
      const vm = this;
      vm.todos.forEach((item, i) => {
        if (item.id === id) {
          vm.todos.splice(i, 1);
        }
      });
      vm.updata();
    },
    updata() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
  },
  computed: {
    filterTodos() {
      const vm = this;
      if (vm.status === "all") {
        return vm.todos;
      } else if (vm.status === "unfinished") {
        return vm.todos.filter((item) => {
          return !item.complete;
        });
      } else if (vm.status === "Completed") {
        return vm.todos.filter((item) => {
          return item.complete;
        });
      } else {
        return 0;
      }
    },
    unfinished() {
      const vm = this;
      const unfinished = vm.todos.filter((item) => {
        return !item.complete;
      });
      return unfinished.length;
    },
    completed() {
      const vm = this;
      const unfinished = vm.todos.filter((item) => {
        return item.complete;
      });
      return unfinished.length;
    },
  },
};
</script>

<style lang="scss">
@import "./assets/scss/all.scss";
</style>
