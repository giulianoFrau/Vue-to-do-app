<template>
  <div class="d-flex gap">
    <div class="d-flex flex-column flex-grow-1">
      <label for="text">Titolo Task :</label>
      <input
        type="text"
        v-model="userData.textValue"
        id="text"
        maxlength="50"
        class="inputFields"
        placeholder="Inserisci un titolo"
      />
      <br />
      <label for="text">Descrizione :</label>
      <input
        type="text"
        v-model="userData.textValueDescription"
        id="text"
        maxlength="50"
        class="inputFields"
        placeholder="Inserisci una descrizione"
      />
      <div>
        <button
          @click.prevent="addTask"
          class="btn btn-outline-success btn-lg but"
        >
          Aggiungi
        </button>
        <button
          @click.prevent="deleteAll"
          class="btn btn-outline-danger btn-lg but"
        >
          Elimina lista
        </button>
        <hr />
      </div>
      <div class="flex-grow-1 mt-5">
        <div class="panel panel-default opacity">
          <div class="panel-heading">
            <h3>Lista task cancellati: ❌</h3>
          </div>
          <div class="panel-body">
            <ul>
              <DeleteTask
                :j="j"
                :titolo="taskDel[0]"
                :descrizione="taskDel[1]"
                :key="j"
                v-for="(taskDel, j) in tasksDeleted"
                @resume="resume"
                @deleteDefinitely="deleteDefinitely"
              ></DeleteTask>
            </ul>
          </div>
        </div>
      </div>
    </div>
    <div class="flex-grow-1 panel panel-default opacity">
      <div class="panel-heading">
        <h3>Lista task: 📃</h3>
      </div>
      <div class="panel-body">
        <ul>
          <AddTask
            :key="i"
            :i="i"
            :titolo="task[0]"
            :descrizione="task[1]"
            v-for="(task, i) in tasksArray"
            @deleteTask="deleteTask"
          ></AddTask>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted } from "vue";
import DeleteTask from "./DeleteTask.vue";
import AddTask from "./AddTask.vue";

const tasksArray = ref([]);
const tasksDeleted = ref([]);
const userData = reactive({
  textValue: "",
  textValueDescription: "",
});

function localStorageMethod() {
  const parsed = JSON.stringify(tasksArray.value);
  localStorage.setItem("tasksArray", parsed);
}

function addTask() {
  if (tasksArray.value.length < 5) {
    if (userData.textValue !== "" || userData.textValueDescription !== "") {
      tasksArray.value.push([
        userData.textValue,
        userData.textValueDescription,
      ]);
      userData.textValue = "";
      userData.textValueDescription = "";
      localStorageMethod();
    } else {
      alert("Non hai inserito nessun dato");
    }
  } else {
    alert("La memoria della mia agenda non puoi superare 5 task");
  }
}

function deleteTask(index) {
  tasksDeleted.value.push(tasksArray.value[index]);
  tasksArray.value.splice(index, 1);
  localStorageMethod();
}

function deleteAll() {
  if (tasksArray.value.length > 0) {
    tasksArray.value = [];
    localStorageMethod();
  } else {
    alert("La lista è già vuota");
  }
}

function resume(i) {
  tasksArray.value.push(tasksDeleted.value[i]);
  tasksDeleted.value.splice(i, 1);
  localStorageMethod();
}

function deleteDefinitely(i) {
  tasksDeleted.value.splice(i, 1);
}

onMounted(() => {
  if (localStorage.getItem("tasksArray")) {
    try {
      tasksArray.value = JSON.parse(localStorage.getItem("tasksArray"));
    } catch (e) {
      console.log(e);
    }
  }
});
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@100&family=Shadows+Into+Light&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Indie+Flower&display=swap");
li {
  padding: 1rem;
  font-size: 2rem;
  font-family: "Shadows Into Light", cursive;
}

.gap {
  gap: 50px;
}

label {
  font-size: 2rem;
  font-family: "Indie Flower", cursive;
  font-weight: bolder;
  color: white;
}

.inputFields {
  font-size: 1.8rem;
  border-style: inset;
  border-color: currentColor;
  margin-bottom: 1.5rem;
  border-radius: 5px;
  padding: 0.7rem;
  width: 100%;
  opacity: 0.7;
  font-family: "Indie Flower", cursive;
  font-weight: bolder;
  color: white;
}

.inputFields::placeholder {
  font-family: "Indie Flower", cursive;
}

h1,
h2,
h3,
h4,
h5 {
  font-family: "Indie Flower", cursive;
  font-weight: bolder;
  text-transform: capitalize;
}
.opacity {
  opacity: 0.7;
}

.but {
  margin-right: 2rem;
  margin-top: 1rem;
  padding: 0.8rem;
  font-size: 1.7rem;
}
ul {
  position: relative;
  list-style: none;
  margin-left: 0;
  padding-left: 2.2em;
}
ul li:before {
  content: "📌";
  position: absolute;
  left: 0;
}
</style>
