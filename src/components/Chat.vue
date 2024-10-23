
<template>
  <div class="chat-container">
    <h2>Comments</h2>
    <ul>
      <li v-for="comment in comments" :key="comment.id">
        <strong>{{ comment.username }}:</strong> {{ comment.text }}
      </li>
    </ul>
    <form @submit.prevent="addComment">
      <input v-model="newComment.username" placeholder="Enter your username" required />
      <input v-model="newComment.text" placeholder="Enter your comment" required />
      <button type="submit">Add Comment</button>
    </form>
  </div>
</template>

<script>
import { reactive, onMounted } from "vue";

export default {
  name: "Chat",
  setup() {
    const comments = reactive([]);
    const newComment = reactive({
      username: "",
      text: ""
    });

    const fetchComments = async () => {
      try {
        const response = await fetch("https://lab5-p379.onrender.com/api/v1/messages");
        const data = await response.json();
        comments.push(...data);
      } catch (error) {
        console.error("Failed to fetch comments:", error);
      }
    };

    const addComment = async () => {
      try {
        const response = await fetch("https://lab5-p379.onrender.com/api/v1/messages", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(newComment),
        });
        const createdComment = await response.json();
        comments.unshift(createdComment); // Add new comment to the top
        newComment.username = "";
        newComment.text = "";
      } catch (error) {
        console.error("Failed to add comment:", error);
      }
    };

    onMounted(fetchComments);

    return {
      comments,
      newComment,
      addComment,
    };
  },
};
</script>

<style scoped>
.chat-container {
  max-width: 600px;
  margin: 0 auto;
}
form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
button {
  padding: 10px;
  background-color: #007BFF;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
button:hover {
  background-color: #0056b3;
}
</style>
