<script setup>
import { ref } from "vue";
const showModal = ref(false);
const newNote = ref("");
const notes = ref([]);
const errorMsg = ref("");
let id = 0;

const addBtn = () => {
  if (newNote.value.length < 10) {
    return errorMsg.value = "Текст заметки должен содержать не менее 10 символов.";
  } else {
    notes.value.push({
      id: id++,
      text: newNote.value,
      date: new Date().toLocaleString('ru-RU', { dateStyle: "long" }),
      color: getRandomColor()
    });
    showModal.value = false;
    newNote.value = "";
    errorMsg.value = "";
  }
}

const closeBtn = () => {
  showModal.value = false;
  newNote.value = "";
  errorMsg.value = "";
}
const getRandomColor = () => {
  return "hsl(" + Math.floor(Math.random()*360) + ", 100%, 75%)";
}

</script>
<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMsg">{{ errorMsg }}</p>
        <button @click="addBtn">Добавить</button>
        <button @click="closeBtn" class="close">Закрыть</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Заметки</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div v-for="note in notes" class="card" :style="{ backgroundColor: note.color }">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date }}</p>
        </div>
      </div>
    </div>
  </main>
</template>
<style scoped>
main {
  width: 100vw;
  height: 100vh;
}
  .container {
    width: 1000px;
    padding: 10px;
    margin: 0 auto;
  }
  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  h1 {
    font-weight: bold;
    margin-bottom: 25px;
    font-size: 75px;
  }
  header button {
    border: none;
    padding: 10px;
    height: 50px;
    width: 50px;
    cursor: pointer;
    background-color: #151414;
    border-radius: 100%;
    color: #ffffff;
    font-size: 20px;
  }
  .card {
    width: 225px;
    height: 225px;
    background-color: #ebfc57;
    padding: 10px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
  }
  .date {
    font-size: 12.5px;
    font-weight: bold;
  }
  .cards-container {
    display: flex;
    flex-wrap: wrap;
  }
  .overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.77);
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .modal {
    width: 750px;
    background-color: white;
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
  }
  .modal button {
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    background-color: #530d94;
    border: none;
    color: white;
    cursor: pointer;
    margin-top: 15px;
  }
  .modal .close {
    background-color: #bd0202;
    margin-top: 7px;
  }
  .modal textarea {
    font-size: 16px;
  }
  .modal p {
    color: #ff0000;
  }
</style>