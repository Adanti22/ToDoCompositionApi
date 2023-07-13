<script>
import { ref } from "vue";
import { reactive } from "vue";
export default {
  setup() {
    const tasks = reactive([]);
    let taskName = ref("");
    let taskDescr = ref("");
    let idCounter = 0;
    function createTask() {
      if (taskName.value.trim() !== "" && taskDescr.value.trim() !== "") {
        tasks.push({
          name: taskName.value,
          description: taskDescr.value,
          id: idCounter,
          completed: false,
        });
        idCounter++;
        taskName.value = "";
        taskDescr.value = "";
      } else {
        alert("fill in the fields");
      }
    }

    function deleteTask(id) {
      tasks.forEach((task, index) => {
        if (task.id == id) {
          tasks.splice(index, 1);
        }
      });
    }

    function checkTask(id) {
      tasks.forEach((task, index) => {
        if (task.id == id) {
          if (task.completed) {
            task.completed = false;
          } else {
            task.completed = true;
          }
        }
      });
    }
    return { taskName, taskDescr, createTask, tasks, deleteTask, checkTask };
  },
};
</script>

<template>
  <div class="wrapper">
    <h1>Composition TODO</h1>
    <div class="todo-form">
      <input
        type="text"
        placeholder="Name of task"
        class="input input-title"
        v-model="taskName"
      />
      <input
        type="text"
        placeholder="Description"
        class="input input-descr"
        v-model="taskDescr"
      />
      <button class="btn" @click="createTask">Add</button>
    </div>
    <div class="tasks-container">
      <div class="task" v-for="task in tasks" :id="task.id">
        <input
          type="checkbox"
          @click="checkTask(task.id)"
          :checked="task.completed"
          class="check"
        />
        <div class="delete" @click="deleteTask(task.id)">X</div>
        <h3 :class="['task-title', task.completed ? 'completed' : '']">
          {{ task.name }}
        </h3>
        <p :class="['description', task.completed ? 'completed' : '']">
          {{ task.description }}
        </p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 5vh;
}
h1 {
  font-size: 40px;
}
.todo-form {
  margin: 0 auto;
  width: 70vw;
  display: flex;
  align-items: center;
  justify-content: space-around;
  padding: 30px 20px;
  border-radius: 10px;
  border: 1px solid white;
}
.input {
  outline: none;
  padding: 10px;
  border-radius: 5px;
  transition: 0.3s ease-in-out;
  background-color: rgb(207, 229, 255);
  border: 1px solid black;
}
.input:hover {
  border-radius: 7.5px;
}
.input:focus {
  border-radius: 10px;
  -webkit-box-shadow: 0px 0px 8px 1px rgba(255, 255, 255, 0.3);
  -moz-box-shadow: 0px 0px 8px 1px rgba(255, 255, 255, 0.3);
  box-shadow: 0px 0px 8px 1px rgba(255, 255, 255, 0.3);
  border: 1px solid rgb(0, 59, 131);
}
.input-title {
  width: 150px;
}
.input-descr {
  width: 300px;
}
.btn {
  padding: 9px 13px;
  outline: none;
  border-radius: 5px;
  border: 1px solid green;
  background-color: rgb(176, 255, 176);
  cursor: pointer;
  transition: 0.2s ease-in-out;
}
.btn:hover {
  border-radius: 7.5px;
  -webkit-box-shadow: 0px 0px 24px 2px rgba(219, 255, 206, 0.3);
  -moz-box-shadow: 0px 0px 24px 2px rgba(219, 255, 206, 0.3);
  box-shadow: 0px 0px 24px 2px rgba(219, 255, 206, 0.3);
}
.btn:active {
  border-radius: 10px;
}
.tasks-container {
  width: 75vw;
  padding: 20px;
  display: flex;
  align-items: center;
  column-gap: 7.5vw;
  row-gap: 3vh;
  flex-wrap: wrap;
}
.task {
  width: 18vw;
  padding: 10px;
  border: 1px solid white;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  position: relative;
}
.delete {
  position: absolute;
  top: 0;
  right: 10px;
  color: red;
  cursor: pointer;
  transition: 0.1s ease-in-out;
}
.delete:hover {
  transform: scale(1.1);
}
.check {
  position: absolute;
  top: 5px;
  left: 10px;
}
.task-title {
  font-size: 25px;
  overflow-wrap: break-word;
  word-wrap: break-word;
  text-align: center;
}
.description {
  overflow-wrap: break-word;
  word-wrap: break-word;
  text-align: center;
}
.completed {
  text-decoration: line-through;
}
</style>
