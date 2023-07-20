<script setup>
  import {ref} from "vue"
  const showModal = ref(false) //setting the state to false
  const newNote = ref("")

  let currentCard = ref(-1)

  const notes = ref([])

  const errorMsg = ref("")

    function getRandomColor() {
      return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  }

  const addNote = ()=>{
    if(newNote.value.length <= 10){
      return errorMsg.value = "Hey, you have to enter at least 10 characters."
    }
    notes.value.push({
      id: Math.floor(Math.random()*100000),
      text: newNote.value,
      date: new Date(),
      bgColor: getRandomColor(), 
    })
    currentCard++
    showModal.value = false
    newNote.value = ""
    errorMsg.value = ""
    
  }

  const deleteNote = (index)=>{
    notes.value.splice(index, 1);
  }


</script>

<template>
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@20..48,100..700,0..1,-50..200" />

  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMsg">{{ errorMsg }}</p>
        <button id="modal-button" @click="addNote">Add Note</button>
        <button id="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button id="header-button" @click="showModal=true">+</button>
      </header>

      <div class="cards-container">
        <div v-for="note, index in notes" :key="note.id" class="card" :style="{backgroundColor: note.bgColor}">
          <p class="main-text">{{ note.text }}</p>
          <div class="note-metas">
            <p class="date">{{ note.date.toLocaleDateString("eu-BG") }}</p>
            <button id="deleteBtn" @click="deleteNote(index)"><span class="material-symbols-outlined">delete</span></button>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main{
  height: 100vh;
  width: 100vw;
}

.container{
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header{
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1{
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 55px;
}

#header-button{
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  border-radius: 100%;
  color: white;
  font-size: 20px;
}

.card{
  width: 225px fit-content;
  height: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.cards-container{
  display: flex;
  flex-wrap: wrap;
  white-space: pre-wrap;
}

.overlay{
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal{
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

#modal-button{
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
}

.modal #close{
  background-color: red;
  margin-top: 7px;
  width: 100%;
  border: none;
  color: white;
  cursor: pointer;
  font-size: 20px;
  padding: 10px 20px;
}

#note{
  resize: none;
  font-size: 15px;
  font-family: 'Roboto', sans-serif;
}

.note-metas{
  display: flex;
  justify-content: space-between;
}

#deleteBtn{
  background: none;
  border: none;
  cursor: pointer;
}
</style>