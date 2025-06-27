<script setup>
// imports
import { ref } from 'vue';
import noteCard from './components/noteCard.vue';

// cons & vars
const showModal = ref(false);
const newNote = ref("");
const notes = ref([]);
const errorMsg = ref("");

// functions
const getRandomColor = () => {
  const color = `HSL(${Math.floor(Math.random() * 360)}, 100%, 75%)`;
  return color;
};

const addNewNote = () => {
  if (newNote.value.length <= 9) {
    return errorMsg.value = "Note characters should be at least 10 characters";
  }

  notes.value.push({
    id: Math.floor(Math.random() * 10000000),
    text: newNote.value,
    date: new Date().toLocaleDateString("en-uk"),
    backgroundColor: getRandomColor(),
  });

  newNote.value = "";
  showModal.value = false;

  console.log(notes.value);
};
</script>

<template>
  <main>
    <!--============ OVERLAY ============-->
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea name="note" id="note" v-model="newNote" cols="30" rows="10"></textarea>
        <p class="error" v-if="errorMsg">{{ errorMsg }}</p>
        <button @click="addNewNote">add note</button>
        <button @click="showModal = false" class="close">close</button>
      </div>
    </div>

    <!--========== CONTAINER ===========-->
    <div class="container">
      <!-- HEADER -->
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>


      <!-- CARD CONTAINER -->
      <div class="cards-container">
        <div v-for="note in notes" :key="note.id" class="card" :style="{ backgroundColor: note.backgroundColor }">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date }}</p>
        </div><!--./card-->
      </div><!--./cards-container-->
    </div> <!--./container-->
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
}

.overlay {
  display: flex;
  justify-content: center;
  align-items: center;

  position: absolute;
  top: 0;
  left: 0;

  width: 100%;
  height: 100%;

  background-color: rgba(0, 0, 0, 0.7);
  z-index: 2;
}

.overlay .modal {
  display: flex;
  flex-direction: column;

  position: relative;
  width: 750px;

  border-radius: 10px;
  padding: 30px;

  background-color: #fff;
}

.overlay .modal p {
  color: red;
}

.overlay .modal button {
  width: 100%;

  margin-top: 15px;
  padding: 10px 20px;
  border: none;

  background-color: blueviolet;
  color: #fff;

  font-size: 20px;
  cursor: pointer;
}

.overlay .modal .close {
  margin-top: 7px;
  background-color: rgb(193, 15, 15);
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
  margin-bottom: 25px;

  font-weight: bold;
  font-size: 75px;
}

.container header button {
  width: 50px;
  height: 50px;

  border: none;
  padding: 10px;
  border-radius: 100%;

  background-color: rgb(21, 20, 20);
  color: white;

  font-size: 20px;
  text-align: center;
  line-height: 25px;
  cursor: pointer;
}

.card {
  display: flex;
  flex-direction: column;
  justify-content: space-between;

  width: 225px;
  height: 225px;

  margin-right: 20px;
  margin-bottom: 20px;
  padding: 10px;
  border-radius: 15px;

  background-color: rgb(237, 182, 44);
}

.card .date {
  font-size: 12.5px;
  font-weight: bold;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}
</style>