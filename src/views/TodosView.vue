<template>
  <div class="todos">
    <!-- Todo input section -->
    <h1>Create Todo</h1>
    <div class="input-container">
      <input v-model="newTodo" placeholder="Enter a new todo" class="todo-input" />
      <button @click="addTodo">Create</button>
    </div>

    <!-- Error messages -->
    <div v-if="emptyTodoErrorMessageVisible" class="error-message">Todo Input cannot be empty!</div>
    <div v-if="noTodosMessageVisible && !emptyTodoErrorMessageVisible" class="message">You have no todos to complete at the moment! Add one!</div>
    <div v-if="allTodosCompletedMessageVisible" class="message">You have completed all your todos!... Hurray!</div>

    <!-- Todos list -->
    <div class="todos-list">
      <div v-for="todo in todos" :key="todo.id" class="todo-item">
        <!-- Edit todo section -->
        <div v-if="todo.id === editingTodoId" class="edit-body">
          <input v-model="editedTodoTitle" class="edit-input" />
          <div class="buttons-container">
            <button @click="saveEdit" class="save-button">Save</button>
            <button @click="cancelEdit" class="cancel-button">Cancel</button>
          </div>
        </div>

        <!-- Task section -->
        <div v-else class="task">
          <span class="task-title" :class="{ 'completed-task': todo.completed }">{{ todo.title }}</span>
          <div class="buttons-container">
            <button @click="toggleComplete(todo.id, !todo.completed)" class="check-button">{{ todo.completed ? 'Uncheck' : 'Check' }}</button>
            <button @click="editTodo(todo.id, todo.title)" class="edit-button">Edit</button>
            <button @click="deleteTodo(todo.id)" class="delete-button">Delete</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      todos: [], // Array to store the todos
      newTodo: '', // Input value for creating a new todo
      noTodosMessageVisible: true, // Flag to control the visibility of the "no todos" message
      allTodosCompletedMessageVisible: false, // Flag to control the visibility of the "all todos completed" message
      emptyTodoErrorMessageVisible: false, // Flag to control the visibility of the "empty todo" error message
      editingTodoId: null, // ID of the todo being edited
      editedTodoTitle: '', // Edited title of the todo
    };
  },

  mounted() {
    this.loadTodos(); // Load todos from local storage on component mount
    this.updateMessagesVisibility(); // Update visibility of messages
  },

  methods: {
    loadTodos() {
      const storedTodos = localStorage.getItem('todos');
      if (storedTodos) {
        this.todos = JSON.parse(storedTodos); // Load todos from local storage into the component's data
      }
    },

    saveTodos() {
      localStorage.setItem('todos', JSON.stringify(this.todos)); // Save todos to local storage
    },

    addTodo() {
      if (this.newTodo.trim() !== '') {
        const newTodo = {
          id: Date.now(),
          title: this.newTodo,
          completed: false,
        };
        this.todos.push(newTodo); // Add the new todo to the todos array
        this.newTodo = ''; // Clear the input field
        this.saveTodos(); // Save the todos to local storage
        this.emptyTodoErrorMessageVisible = false; // Hide the empty todo error message
        this.updateMessagesVisibility(); // Update visibility of messages
      } else {
        this.emptyTodoErrorMessageVisible = true; // Show the empty todo error message
        setTimeout(() => {
          this.emptyTodoErrorMessageVisible = false; // Hide the empty todo error message after 2 seconds
        }, 2000);
      }
    },

    toggleComplete(todoId, value) {
      const todo = this.todos.find((todo) => todo.id === todoId);
      if (todo) {
        todo.completed = value; // Update the completed status of the todo
        this.saveTodos(); // Save the todos to local storage
        this.updateMessagesVisibility(); // Update visibility of messages
      }
    },

    deleteTodo(todoId) {
      const todoIndex = this.todos.findIndex((todo) => todo.id === todoId);
      if (todoIndex !== -1) {
        this.todos.splice(todoIndex, 1); // Remove the todo from the todos array
        this.saveTodos(); // Save the todos to local storage
        this.updateMessagesVisibility(); // Update visibility of messages
      }
    },

    editTodo(todoId, todoTitle) {
      this.editingTodoId = todoId; // Set the ID of the todo being edited
      this.editedTodoTitle = todoTitle; // Set the current title of the todo being edited
    },

    saveEdit() {
      if (this.editedTodoTitle.trim() !== '') {
        const editedTodo = this.todos.find((todo) => todo.id === this.editingTodoId);
        if (editedTodo) {
          editedTodo.title = this.editedTodoTitle; // Update the title of the edited todo
          this.saveTodos(); // Save the todos to local storage
          this.editingTodoId = null; // Reset the editingTodoId to indicate no todo is being edited
          this.editedTodoTitle = ''; // Clear the editedTodoTitle
        }
      }
    },

    cancelEdit() {
      this.editingTodoId = null; // Reset the editingTodoId to indicate no todo is being edited
      this.editedTodoTitle = ''; // Clear the editedTodoTitle
    },

    updateMessagesVisibility() {
      this.noTodosMessageVisible = this.todos.length === 0; // Show the "no todos" message if there are no todos
      this.allTodosCompletedMessageVisible =
        this.todos.length > 0 && this.todos.every((todo) => todo.completed); // Show the "all todos completed" message if all todos are completed
    },
  },
};
</script>


<style scoped>
.todos {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  gap: 12px;
  align-items: center;
}

h1 {
  margin-bottom: 10px;
  
}

.input-container {
  display: flex;
  align-items: center;
}

.todo-input {
 width: 180px;
  padding: 8px 12px;
  margin-right: 10px;
  outline: none;
 
}

button {
  padding: 8px 18px;
  font-size: 11px;
  font-weight: 700;
  cursor: pointer;
  transition: ease-in 3ms;
  background: #008CBA;
  color: white;
  border: none;
}

button:hover {
  opacity: 50%;
}

.error-message {
  display: block;
  margin-top: 10px;
  color: red;
  animation-name: slideInLeft;
  animation-duration: 1s;
  animation-timing-function: ease-in-out;
  animation-fill-mode: forwards;
}

@keyframes slideInLeft {
  from {
    transform: translateX(-100%);
  }
  to {
    transform: translateX(0);
  }
}

.message {
  display: block;
  margin-top: 10px;
  color: gray;
  animation-name: slideInRight;
  animation-duration: 1s;
  animation-timing-function: ease-in-out;
  animation-fill-mode: forwards;
}

@keyframes slideInRight {
  from {
    transform: translateX(100%);
  }
  to {
    transform: translateX(0);
  }
}

.todos-list {
  margin-top: 10px;
  display: flex;
  align-items: center;
  flex-direction: column;
}

.todo-item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.task {
  text-align: center;
  margin-bottom: 10px;
}

.task-title {
  margin-bottom: 10px;
  display: block;
}

.completed-task {
  text-decoration: line-through;
}

.edit-body {
  margin-top: 10px;
  display: flex;
  flex-direction: column;
  gap: 12px;
  align-items: center;
}

.edit-input {
  width: 220px;
  padding: 5px 10px;
  margin-right: 10px;
  outline: none;
 
}

.buttons-container {
  margin-left: 10px;
  display: flex;
  gap: 5px;
}

.check-button {
  background-color: #4caf50;
  color: white;
}

.edit-button {
  background-color: #008CBA;
  color: white;
}

.delete-button {
  background-color: #f44336;
  color: white;
}

.save-button {
  background-color: #4caf50;
  color: white;
}

.cancel-button {
  background-color: #f44336;
  color: white;
}
</style>
