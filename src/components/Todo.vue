<template>
  <div>
    <!-- Checkbox for todo item -->
    <div class="checkbox" @click="toggleComplete">
      <div class="checkbox-icon" :class="{ checked: todo.completed }">
        <span class="checkmark"></span>
      </div>
    </div>
    <div v-if="!isEditing">
      <!-- Display the todo title -->
      <span :class="{ 'completed-task': todo.completed }">{{ todo.title }}</span>
      <!-- Button to toggle edit mode -->
      <button @click="toggleEdit"><i class="fas fa-edit"></i></button>
    </div>
    <div v-else>
      <!-- Input field for editing the todo title -->
      <input v-model="editedTitle" ref="editInput" />
      <!-- Button to save the edited todo title -->
      <button @click="saveEdit"><i class="fas fa-check"></i></button>
    </div>
    <!-- Button to delete the todo item -->
    <button @click="deleteTodo"><i class="fas fa-trash"></i></button>
  </div>
</template>

<script>
export default {
  props: ['todo'],
  data() {
    return {
      isEditing: false,
      editedTitle: ''
    };
  },
  methods: {
    // Todo item methods
  }
};
</script>

<style scoped>
/* Styles for the todo item component */
.completed-task {
  /* Styling for completed tasks with line-through text */
  text-decoration: line-through;
}

.checkbox {
  /* Styling for the checkbox container */
  display: inline-block;
  width: 16px;
  height: 16px;
  cursor: pointer;
}

.checkbox-icon {
  /* Styling for the checkbox icon */
  border: 1px solid #ccc;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.checkbox-icon.checked {
  /* Styling for the checked checkbox icon */
  background-color: #ccc;
}

.checkmark {
  /* Styling for the checkmark icon */
  width: 10px;
  height: 10px;
  background-color: #333;
  visibility: hidden;
}

.checkbox-icon.checked .checkmark {
  /* Styling for the checkmark icon when checkbox is checked */
  visibility: visible;
}

button {
  /* Styling for buttons */
  margin-left: 10px;
  background-color: transparent;
  border: none;
  cursor: pointer;
}

button i {
  /* Styling for icon inside buttons */
  color: blue;
}
</style>
