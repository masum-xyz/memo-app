<script setup>
import { ref } from 'vue';

const showFrom = ref(false);

const newMemo = ref('');

const memos = ref([]);

const errorMsg = ref('');

function addMemo() {
  if (!newMemo.value) {
    errorMsg.value = 'Please enter a memo';
    return;
  } else {
    errorMsg.value = '';
  };
  memos.value.push({
    id: Date.now(),
    memo: newMemo.value,
    date: new Date().toLocaleDateString("en-GB"),
    backgroundColor: getRandomColour(),
  });
  newMemo.value = '';
  showFrom.value = false;
}

function getRandomColour() {
  return `#${Math.floor(Math.random() * 16777215).toString(16)}`;
}

// delete memo
function deleteMemo(id) {
  memos.value = memos.value.filter((memo) => memo.id !== id);
}

</script>

<template>
  <main>
    <div class="container">
      <header>
        <h1 class="header-title">Memo</h1>
        <button @click="showFrom = true" class="header-button">+</button>
      </header>

      <div class="card-container">
        <!-- card 1 -->
        <div v-for="memo in memos" :key="memo.id" class="card" :style="{backgroundColor: memo.backgroundColor}">
          <p class="card-content">{{ memo.memo }}</p>
          <p class="card-date">{{ memo.date }}</p>
          <button @click="deleteMemo(memo.id)" class="card-btn">Delete</button>
        </div>

      </div>

    </div>

    <!-- Popup Input -->
    <div v-if="showFrom" class="form-overlay">
      <div class="form-modal">

        <button @click="showFrom = false" class="form-close-btn">
          &times;
        </button>

        <p  v-if="errorMsg" class="form-error"> {{ errorMsg }}</p>
       

        <textarea v-model="newMemo" name="memo" id="memo" cols="30" rows="10"></textarea>
        <button @click="addMemo" class="form-save-btn">Save</button>
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
  max-width: 900px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header-title {
  font-size: 48px;
  font-weight: bold;
  margin-bottom: 25px;
  color: #495A72;
}

.header-button {
  font-size: 30px;
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  border-radius: 100%;
  background-color: #495A72;
  color: white;
}

.card {
  width: 225px;
  height: 225px;
  padding: 10px;
  background-color: #2196f3;
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;

}
.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.card-btn {
  padding: 5px 10px;
  background-color: #495A72;
  color: white;
  border: none;
  cursor: pointer;
  margin-top: auto;
  border-radius: 3px;
}

.form-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.form-modal {
  width: 420px;
  background-color: white;
  border-radius: 10px;
  padding: 40px 30px 25px 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.form-save-btn {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: #495a7d;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  margin-top: 15px;
  color: white;
}

.form-close-btn {
  position: absolute;
  top: 5px;
  right: 10px;
  width: 30px;
  height: 30px;
  background-color: transparent;
  border: none;
  font-size: 30px;
  cursor: pointer;
}

.form-error {
  color: red;
  margin-bottom: 10px;
}

</style>