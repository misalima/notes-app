<script setup>
  import {ref} from "vue";

  const showModal = ref(false);
  const newNote = ref("");
  const errorMessage = ref("");
  const notes = ref([]);

  function formatDate(date) {
    const months = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul","Aug","Sep","Oct","Nov","Dec"];
    return ((date.getDate() + " " + months[(date.getMonth())] + " " + date.getFullYear()));  
  }

  function getRandomColor() {
    return "hsl(" + Math.random() * 360 + ", 100%, 75%";
  }

  const addNote = () => {
    if(newNote.value.length < 9 || newNote.value.length > 210) {
      return errorMessage.value = "Make a note with 10-210 characters"
    }
    notes.value.push({
      id: Math.floor(Math.random() * 1000),
      text: newNote.value,
      date: formatDate(new Date()),
      backgroundColor: getRandomColor()
    });
    showModal.value = false;
    newNote.value = "";
    errorMessage.value = "";
  }

  const deleteNote= (noteId) => {
    notes.value.forEach((note)=> {
      if(note.id === noteId) {
        const index = notes.value.indexOf(note); 
        notes.value.splice(index, 1);
      }
    });
  }

</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea name="note" id="note" cols="30" rows="10" v-model.trim="newNote"></textarea>
        <p style="text-align: center;">{{newNote.length}}/210</p>
        <p class="error-message" v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
        <button @click="showModal = false; errorMessage=''" class="close">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="card-container">
        <div 
          v-for="note in notes"
          :key="note.id" 
          class="card" 
          :style="{backgroundColor: note.backgroundColor}">
          <p>{{ note.text }}</p>
          <div class="date-container">
            <p class="date">{{ note.date }}</p>
            <span @click="deleteNote(note.id)"><img src="./assets/trash.png" alt="trash-png"></span>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
  main {
    height: 100vh;
    width: 100vw;
  }

  .container {
    max-width: 1000px;
    padding: 20px;
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
    font-size: 5rem;
  }

  header button {
    border: none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background-color: green;
    border-radius: 100%;
    color: white;
    font-size: 2rem;
    line-height: 1.7rem;
    transition: all 0.3s;
  }

  header button:hover {
    background-color: greenyellow;
    color: #202020;   
  }
  .card {
    width: 225px;
    height: 225px;
    background-color: #454545;
    padding: 10px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
  }

  .card p {
    overflow-wrap: break-word;
    color: #202020;
  }

  .date-container {
    height: 25px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .date {
    font-size: 0.8rem;
    font-weight: bold;
    clear: both;
  }
  
  .card span img{
    width: 20px;
    cursor: pointer;
    transition: all 0.2s;
  }

  .card span img:hover {
    width: 23px;
  }

  .card-container {
    display: flex;
    flex-wrap: wrap;
  }

  .overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: #466d4662;
    z-index: 3;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .modal {
    width: 750px;
    background-color: #a09f9f;
    border-radius: 20px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
  }

  .modal textarea {
    padding: 15px;
    font-size: 1.2rem;
  }

  .modal button {
    padding: 10px 20px;
    font-size: 1.2rem;
    width: 100%;
    background-color: blueviolet;
    border: none;
    border-radius: 20px;
    color: white;
    cursor: pointer;
    margin-top: 15px;
  }

  .modal .close {
    background-color: rgb(110, 2, 2);
    margin-top: 7px;
  }

  .error-message {
    color: rgb(236, 62, 62);
    margin-top: 7px;
    text-align: center;
  }
</style>