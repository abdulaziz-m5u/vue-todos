<template>
  <div class="card">
    <input
      class="card-input"
      type="text"
      placeholder="New Task..."
      @keyup.enter="addTodo"
      v-model="newTodo"
    />
    <div
      class="card-body"
      v-for="(todo, index) in todosFiltered"
      :key="todo.id"
    >
      <input type="checkbox" v-model="todo.completed" />
      <label
        v-if="!todo.editing"
        @dblclick="editTodo(todo)"
        class="card-label"
        :class="{ completed: todo.completed }"
        >{{ todo.title }}</label
      >
      <input
        v-else
        class="todo-item-edit"
        type="text"
        v-model="todo.title"
        @blur="doneEdit(todo)"
        @keyup.enter="doneEdit(todo)"
        @keyup.esc="cancelEdit(todo)"
        v-focus
      />

      <div class="card-update" @click="editTodo(todo)">
        <i class="bx bxs-pencil"></i>
      </div>
      <div class="card-remove" @click="removeTodo(index)">
        <i class="bx bx-x"></i>
      </div>
    </div>
    <div class="card-footer">
      <div class="extra-container">
        <div>{{ remaining }} items left</div>
        <div>
          <label
            ><input
              type="checkbox"
              @click="checkAllTodos"
              :checked="!anyRemaining"
            />
            Check All</label
          >
        </div>
      </div>

      <div class="extra-container">
        <div class="buttons">
          <button :class="{ active: filter == 'all' }" @click="filter = 'all'">
            All
          </button>
          <button
            :class="{ active: filter == 'active' }"
            @click="filter = 'active'"
          >
            Active
          </button>
          <button
            :class="{ active: filter == 'completed' }"
            @click="filter = 'completed'"
          >
            Completed
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      newTodo: '',
      todoId: 0,
      beforeEditCache: '',
      filter: '',
      todos: [],
    };
  },
  computed: {
    remaining() {
      return this.todos.filter((todo) => !todo.completed).length;
    },
    anyRemaining() {
      return this.remaining != 0;
    },
    todosFiltered() {
      if (this.filter == 'all') {
        return this.todos;
      } else if (this.filter == 'active') {
        return this.todos.filter((todo) => !todo.completed);
      } else if (this.filter == 'completed') {
        return this.todos.filter((todo) => todo.completed);
      }
      return this.todos;
    },
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus();
      },
    },
  },
  methods: {
    addTodo() {
      this.todos.push({
        id: this.todoId,
        title: this.newTodo,
        completed: false,
        editing: false,
      });

      this.newTodo = '';
      this.todoId++;
    },
    editTodo(todo) {
      this.beforeEditCache = todo.title;
      todo.editing = true;
    },
    doneEdit(todo) {
      if (todo.title.trim() == '') {
        todo.title = this.beforeEditCache;
      }
      todo.editing = false;
    },
    cancelEdit(todo) {
      todo.title = this.beforeEditCache;
      todo.editing = false;
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    checkAllTodos() {
      this.todos.forEach((todo) => (todo.completed = event.target.checked));
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.active {
  background: lightgreen;
}
.card {
  max-width: 500px;
  margin: auto;
  border: 1px solid #b4b4b4;
  padding: 1rem;
  border-radius: 0.7rem;
}
.card-input {
  border: 1px solid #b4b4b4;
  padding: 0.7rem;
  border-radius: 0.9rem;
  width: 80%;
  text-align: center;
  margin-bottom: 1.5rem;
}

.card-body {
  cursor: pointer;
}

.card-body {
  display: flex;
  align-items: center;
  column-gap: 0.7rem;
  margin-bottom: 1rem;
}

.card-label {
  font-size: 1.3rem;
}

.card-remove {
  border-radius: 0.4rem;
  cursor: pointer;
  background-color: #f7b4b4;
  color: #f74f4f;
  padding: 0.2rem;
}
.card-update {
  margin-left: auto;
  border-radius: 0.4rem;
  cursor: pointer;
  background-color: #44b985;
  color: #35495e;
  padding: 0.2rem;
}

.card-remove i,
.card-update i {
  font-size: 1.2rem;
}

.completed {
  text-decoration: line-through;
  color: grey;
}

.todo-item-edit {
  font-size: 1.3rem;
  padding: 0.2rem;
  border-radius: 0.2rem;
  border: 1px solid #b4b4b4;
}

.card-footer {
  border-top: 2px solid #b4b4b4;
  padding-top: 1rem;
  display: block;
}

.extra-container {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.buttons {
  margin: auto;
  display: flex;
  gap: 0.5rem;
}

.buttons button {
  padding: 0.5rem 1.3rem;
  border-radius: 0.3rem;
  outline: none;
  border: none;
}
</style>
