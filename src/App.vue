<template>
  <div class="card">
    <h1>notes-app</h1>

    <input
      type="text"
      v-model="inputText"
      placeholder="اكتب ملاحظة"
    />

    <button @click="addNote">addNote</button>

    <div v-for="note in notes" :key="note.id" class="note-card">
      <div class="note-header">
        <p class="text">{{ note.text }}</p>
   
        <button class="delete-btn" @click="deleteNote(note.id)">
          delete
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";

const inputText = ref("");
const notes = ref([]);

// إضافة ملاحظة
function addNote() {
  if (inputText.value.trim() === "") return;

  notes.value.push({
    id: Date.now(),
    text: inputText.value,
  });

  inputText.value = "";
}

// حذف ملاحظة واحدة
function deleteNote(id) {
  notes.value = notes.value.filter((note) => note.id !== id);
}

// تحميل من localStorage
onMounted(() => {
  const saved = localStorage.getItem("notes");
  if (saved) {
    notes.value = JSON.parse(saved);
  }
});

// حفظ تلقائي في localStorage
watch(
  notes,
  (newVal) => {
    localStorage.setItem("notes", JSON.stringify(newVal));
  },
  { deep: true }
);
</script>

<style>
body {
  background: linear-gradient(120deg, #f5f7fa, #c3cfe2);
  margin: 0;
  font-family: Arial, sans-serif;
}

/* main card */
.card {
  background: white;
  text-align: center;
  padding: 30px;
  margin: 50px auto;
  border-radius: 20px;
  width: 420px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
}

/* title */
h1 {
  font-size: 26px;
  margin-bottom: 20px;
  color: #333;
}

/* input */
input {
  width: 85%;
  padding: 12px;
  border-radius: 12px;
  border: 1px solid #ddd;
  outline: none;
  margin-bottom: 15px;
  font-size: 14px;
}

input:focus {
  border-color: #6c63ff;
  box-shadow: 0 0 5px rgba(108, 99, 255, 0.3);
}

/* buttons */
button {
  padding: 10px 18px;
  margin: 10px;
  border: none;
  border-radius: 10px;
  background: #6c63ff;
  color: white;
  cursor: pointer;
  transition: 0.3s;
}

button:hover {
  background: #574fd6;
}

/* note card */
.note-card {
  background: #f9f9f9;
  margin: 10px auto;
  padding: 15px;
  border-radius: 12px;
  width: 90%;
  text-align: left;
  border-left: 4px solid #6c63ff;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
  transition: 0.2s;
}

.note-card:hover {
  transform: scale(1.02);
}

/* inside note layout */
.note-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 10px;
}

.text {
  margin: 0;
  flex: 1;
  word-break: break-word;
}

/* delete button */
.delete-btn {
  background: #ff4d4d;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 8px;
  cursor: pointer;
}

</style>