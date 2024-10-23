<script setup>
import { reactive, onMounted, ref } from "vue";

const name = "Rayen";
const messages = reactive([]);
const message = ref("");

onMounted(() => {
  fetch("https://lab5-p379.onrender.com/api/v1/messages/")
    .then((res) => res.json())
    .then((data) => messages.push(...data));
});

const sendMessage = async () => {
  if (!message.value.trim()) return;

  const newMessage = { user: name, text: message.value };

  try {
    const response = await fetch(
      "https://lab5-p379.onrender.com/api/v1/messages/",
      {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(newMessage),
      }
    );

    if (response.ok) {
      const data = await response.json();
      if (data.status === "success" && data.data) {
        messages.push(data.data);
        message.value = "";
      }
    }
  } catch (error) {
    console.error("Error:", error);
  }
};
</script>

<template>
  <div class="chat-container">
    <div class="comments">
      <ul>
        <li v-for="msg in messages" :key="msg._id">
          <strong>{{ msg.user }}:</strong>
          <p>{{ msg.text }}</p>
        </li>
      </ul>
    </div>
    <div class="comment-section">
      <label for="message">{{ name }}:</label>
      <input
        v-model="message"
        type="text"
        placeholder="Type your message here..."
      />
      <button @click="sendMessage">Send</button>
    </div>
  </div>
</template>

<style scoped>
.chat-container {
  display: flex;
  flex-direction: column;
}

.comment-section {
  display: flex;
  gap: 10px;
  margin-top: 20px;
}

input {
  flex: 1;
  padding: 10px;
  font-size: 1rem;
}

.comments {
  margin-top: 20px;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f9f9f9;
  height: 300px;
  overflow-y: scroll;
}

.comments ul {
  list-style-type: none;
  padding: 0;
}

.comments li {
  margin-bottom: 10px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #fff;
}
</style>
