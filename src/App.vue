<template>
  <div id="app">
    <div class="main_container">
    <h1>To-Do List</h1>
    <div class="container">
      <div class="input-container">
        <input type="text" v-model="newTask" placeholder="Enter a new task..." @keyup.enter="addTask" />
        <button class="submit-btn" @click="addTask">Add Task</button>
      </div>

      <div class="tasks-container">
        <div class="column incomplete-tasks">
          <h2>Incomplete Tasks</h2>
          <ul>
            <li v-for="task in openTasks" :key="task.id" class="task">
              <span>{{ task.title }}</span>
              <div class="button-container">
                <button class="delete-btn" @click="deleteTask(task.id)">Delete</button>
                <button class="complete-btn" @click="completeTask(task.id)">Complete</button>
                <button class="edit-btn" @click="editTask(task.id)">Edit</button>
              </div>
            </li>
          </ul>
        </div>

        <div class="gap"></div>

        <div class="column completed-tasks">
          <h2>Completed Tasks</h2>
          <ul>
            <li v-for="task in completedTasks" :key="task.id" class="task completed">
              <span>{{ task.title }}</span>
              <span class="completed-time">{{ task.completedDate }}</span>
              <div class="button-container">
                <button class="delete-btn" @click="deleteTask(task.id)">Delete</button>
              </div>
            </li>
          </ul>
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
      newTask: "",
      tasks: [],
    };
  },
  computed: {
    openTasks() {
      return this.tasks.filter((task) => !task.completed);
    },
    completedTasks() {
      return this.tasks.filter((task) => task.completed);
    },
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== "") {
        const newTask = {
          id: new Date().getTime(),
          title: this.newTask,
          completed: false,
          date: new Date().toLocaleString(),
        };
        this.tasks.push(newTask);
        this.saveTasks();
        this.newTask = "";
      }
    },
    completeTask(id) {
      const index = this.tasks.findIndex((task) => task.id === id);
      if (index !== -1) {
        this.tasks[index].completed = true;
        this.tasks[index].completedDate = new Date().toLocaleString();
        this.saveTasks();
      }
    },
    editTask(id) {
      const index = this.tasks.findIndex((task) => task.id === id);
      if (index !== -1) {
        const newTitle = prompt("Enter new title:");
        if (newTitle !== null && newTitle.trim() !== "") {
          this.tasks[index].title = newTitle;
          this.saveTasks();
        }
      }
    },
    deleteTask(id) {
      const index = this.tasks.findIndex((task) => task.id === id);
      if (index !== -1) {
        this.tasks.splice(index, 1);
        this.saveTasks();
      }
    },
    saveTasks() {
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    loadTasks() {
      const tasks = JSON.parse(localStorage.getItem("tasks"));
      this.tasks = tasks ? tasks : [];
    },
  },
  mounted() {
    this.loadTasks();
  },
};
</script>

<style scoped>
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  margin-top: 50px;
}

.main_container{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.container {
  margin: 0 2rem;
}

h1 {
  font-weight: bold;
  margin-bottom: 20px;
}

.input-container {
  margin-bottom: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.input-container input[type="text"] {
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
  flex: 1;
}

.input-container .submit-btn {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #4caf50;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-left: 10px;
}

.tasks-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}

.column {
  /* margin: 0 20px; */
  flex-grow: 1;
}

.incomplete-tasks {
  order: 1;
}

.completed-tasks {
  order: 2;
}

.gap {
  flex-basis: 100%;
  height: 20px;
}

h2 {
  font-weight: bold;
  margin-bottom: 10px;
}

ul {
  list-style-type: none;
  padding: 0;
}

.task {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #ccc;
  padding: 10px 0;
}

.completed {
  text-decoration: line-through;
  color: #888;
}

.completed-time {
  font-size: 0.8em;
  color: #888;
}

.button-container button {
  padding: 5px 10px;
  font-size: 14px;
  cursor: pointer;
  border: none;
  border-radius: 5px;
  margin-right: 10px;
}

.button-container .delete-btn {
  background-color: #f44336;
  color: #fff;
}

.button-container .complete-btn {
  background-color: #4caf50;
  color: #fff;
}

.button-container .edit-btn {
  background-color: #2196f3;
  color: #fff;
}

@media screen and (min-width: 1024px) {
  .tasks-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
}
</style>
