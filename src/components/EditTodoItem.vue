<template>
  <div>
    <div class="todo-card">
      <div class="card-header">
        <input type="text" class="form-control form-control-lg" v-model.trim="newTodo.title" placeholder="Task" />
      </div>
      <div>
        <div class="mb-2 mt-4 todo-body">
          <label>
            <i class="far fa-calendar-alt"></i> Dead line
          </label>
          <input type="date" class="form-control w-50" v-model="newTodo.deadLine" />
        </div>
        <div class="mb-2 todo-body">
          <label>
            <i class="far fa-comment-dots"></i> Comment
          </label>
          <textarea class="form-control" name cols="10" rows="3" v-model="newTodo.comment"></textarea>
        </div>
        <div class="mt-5">
          <a href="#" class="btn-cancel w-50" @click.prevent="cancel">
            <i class="fas fa-times"></i> Cancel
          </a>
          <a href="#" class="btn-add w-50" @click.prevent="addTodo">
            <i class="fas fa-plus"></i> Add Task
          </a>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      newTodo: {
				title:'',
				deadLine:'',
				comment:'',
			},
    };
	},
	methods: {
		cancel () {
			this.$emit('cancel')
		},
		addTodo () {
			console.log(this.newTodo)
			this.$emit('addTodo',this.newTodo)
		}
	}
};
</script>