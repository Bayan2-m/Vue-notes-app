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
/* ===================== RESET ===================== */

/* ===================== RESET ===================== */

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* ===================== BODY ===================== */

body {
  min-height: 100vh;
  font-family: Arial, Helvetica, sans-serif;
  background: linear-gradient(135deg, #eef2ff, #dbeafe);

  display: flex;
  justify-content: center;
  align-items: center;

  padding: 20px;
}

/* ===================== CARD ===================== */

.card {
  width: 100%;
  max-width: 500px;

  background: #ffffff;
  border-radius: 18px;

  padding: 30px;

  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.1);
}

/* ===================== TITLE ===================== */

h1 {
  text-align: center;
  color: #333;
  margin-bottom: 25px;
  font-size: 2rem;
}

/* ===================== INPUT ===================== */

input {
  width: 100%;
  padding: 14px;

  border: 1px solid #d1d5db;
  border-radius: 12px;

  outline: none;

  font-size: 15px;

  transition: 0.3s;
}

input:focus {
  border-color: #6366f1;
  box-shadow: 0 0 8px rgba(99, 102, 241, 0.3);
}

/* ===================== BUTTON ===================== */

button {
  width: 100%;
  margin-top: 15px;

  padding: 13px;

  border: none;
  border-radius: 12px;

  background: #6366f1;
  color: white;

  font-size: 15px;
  font-weight: bold;

  cursor: pointer;
  transition: 0.3s;
}

button:hover {
  background: #4f46e5;
}

/* ===================== NOTES ===================== */

.note-card {
  margin-top: 18px;

  padding: 15px;

  background: #f8fafc;

  border-left: 5px solid #6366f1;
  border-radius: 12px;

  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);

  transition: 0.25s;
}

.note-card:hover {
  transform: translateY(-3px);
}

.note-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
}

.text {
  flex: 1;
  color: #333;
  line-height: 1.5;
  word-break: break-word;
}

/* ===================== DELETE BUTTON ===================== */

.delete-btn {
  width: auto;

  margin-top: 0;

  padding: 8px 14px;

  background: #ef4444;
  color: white;

  border-radius: 8px;
}

.delete-btn:hover {
  background: #dc2626;
}

/* ===================== TABLET ===================== */

@media (max-width: 768px) {
  .card {
    max-width: 90%;
    padding: 25px;
  }

  h1 {
    font-size: 1.8rem;
  }
}

/* ===================== MOBILE ===================== */

@media (max-width: 480px) {
  body {
    padding: 15px;
  }

  .card {
    max-width: 100%;
    padding: 20px;
    border-radius: 15px;
  }

  h1 {
    font-size: 1.6rem;
  }

  input,
  button {
    font-size: 14px;
    padding: 12px;
  }

  .note-header {
    flex-direction: column;
    align-items: stretch;
  }

  .delete-btn {
    width: 100%;
    margin-top: 10px;
  }
}

</style>