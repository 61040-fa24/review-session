<script setup lang="ts">
import { ref } from "vue";
import { fetchy } from "../../utils/fetchy";

const friend = ref("");
const emit = defineEmits(["refreshFriends"]);

const createFriend = async () => {
  try {
    await fetchy(`/api/friends/${friend.value}`, "POST", {
      body: { friend: friend.value },
    });
  } catch (_) {
    return;
  }
  emit("refreshFriends");
  emptyForm();
};

const emptyForm = () => {
  friend.value = "";
};
</script>

<template>
  <form @submit.prevent="createFriend">
    <label for="friend">Potential Friend Username:</label>
    <input type="text" v-model="friend" id="friend" placeholder="Add a new Friend!" required />
    <button type="submit" class="pure-button-primary pure-button">Add Friend</button>
  </form>
</template>

<style scoped>
form {
  background-color: var(--base-bg);
  border-radius: 1em;
  display: flex;
  flex-direction: column;
  gap: 0.5em;
  padding: 1em;
}

textarea {
  font-family: inherit;
  font-size: inherit;
  height: 6em;
  padding: 0.5em;
  border-radius: 4px;
  resize: none;
}
</style>
