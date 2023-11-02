<script setup>
  import { ref } from 'vue';
  const showModal = ref(false);
  const newNote = ref("");
  const notes = ref([]);
  const errorMessage = ref("");

  function getRandomColor() {
    return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  }

  const addNote = () => {
    if(newNote.value.length < 10){
      return errorMessage.value = "The note must be more than 10 character";
    }
    notes.value.push({
        id: Math.floor(Math.random() * 10000000),
        text: newNote.value,
        date: new Date(),
        backgroundColor: getRandomColor()
      })
    showModal.value = false
    newNote.value = ""
  }

</script>

<template>
  <main>

    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <span v-if="errorMessage" style="color: red;">{{ errorMessage }}</span>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>

    <div class="container">
      <header>
        <h1 style="color: white;">Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div v-for="note in notes" :key="note.id" class="card" :style="{backgroundColor: note.backgroundColor}">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString("en-US") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: #2d2d2da6;
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.close {
  color: white !important;
  background-color: #b61515 !important;
}

.modal {
  width: 750px;
  background-color: rgb(45, 45, 45);
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

textarea {
  border-radius: 8px;
  color: white;
  background-color: black;
  border: none;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: white;
  border: white;
  color: black;
  border-radius: 8px;
  cursor: pointer;
  margin-top: 15px;
}

main {
  height: 100vh;
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
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(255, 255, 255);
  border-radius: 100%;
  color: black;
  font-size: 20px;
}

.card {
  width: 225px;
  height: 225px;
  background-color: gold;
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.main-text {
  color: black;
}

.date {
  font-size: 12.5px;
  font-weight: 600;
  color: black;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}</style>