<script setup>
import { ref } from 'vue'

const showModal = ref(false)
const newNote = ref("")
const errorMessage = ref("")
const notes = ref([])


function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}


const addNote = () => {
  if (newNote.value.length < 10) {
    return errorMessage.value = "Note must be at least 10 characters long"
  }
  notes.value.push({
    id: Math.floor(Math.random() * 10000000),
    text: newNote.value,
    date: new Date(),
    shadowColor: getRandomColor(),
  })
  showModal.value = false
  newNote.value = ""
  errorMessage.value = ""
}
</script >

<template>
  <main>
    <div class="overlay" v-if="showModal" @click="showModal = false">
      <div class="modal" @click.stop>
        <div class="card newCard">{{ newNote }}</div>
        <textarea v-model.trim="newNote" name="notes" id="notes" cols="30" rows="10"></textarea>
        <p class="error" v-if="errorMessage">{{ errorMessage }}</p>
        <div class="button-area">
          <button class="confirm-button" @click="addNote()">+Add Note</button>
        </div>
      </div>
    </div>

    <div class="container">
      <header>
        <h1>MyNotes App</h1>
        <button class="add-btn" @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div class="card" v-for="note in notes" :key="notes.id"
          :style="{ boxShadow: `inset 0 0 10px 5px  ${note.shadowColor}` }" :id="note.id">
          <p class="main-text">
            <hr />
            {{ note.text }}
            <hr />
          </p>
          <p class="date">{{ note.date.toLocaleDateString("en-US") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>


<style scoped>
main {
  min-height: 100vh;
  width: 100vw;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-size: 55px;
  font-weight: bold;
  text-shadow: 0 0 5px yellow;
  margin-bottom: 25px;
  padding: 10px;
  border-radius: 4px;
  background-image: linear-gradient(to right, #8e2de2, #4a00e0);
  user-select: none;

}

.add-btn {
  border: none;
  padding: 10px;
  min-width: 50px;
  height: 50px;
  cursor: pointer;
  background-image: linear-gradient(to right, #c98cd8, #8e2de2);
  border-radius: 50%;
  font-size: 20px;
  transition: all 0.2s;
}

.add-btn:active {
  transform: scale(0.9);
}

.card {
  width: 225px;
  height: 225px;
  background-image: linear-gradient(to right, #e0ca00, #e28d2d);
  border-radius: 4px;
  padding: 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.newCard {
  margin: 10px auto 30px auto;
}

.main-text {

  padding: 2px;
  word-wrap: break-word;
}

.date {
  font-size: 12px;
  text-align: end;
  padding: 2px;
  color: #8b8b8b;
  font-weight: 500;
  text-shadow: 1px 1px 1px rgb(63, 63, 63);
  background-color: rgba(0, 0, 0, 0.205);
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: fixed;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.70);
  z-index: 10;
  display: flex;
  justify-content: center;
  align-items: center;
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

.modal .error {
  color: red;
}

.modal .button-area {
  display: flex;
  justify-content: flex-end;
}

.modal .confirm-button {
  padding: 10px 20px;
  font-size: 20px;
  width: 30%;
  background-color: #009414;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 10px;
  border-radius: 4px;
}
</style>