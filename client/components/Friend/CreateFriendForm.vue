<script setup lang="ts">
import { ref } from "vue";
import { fetchy } from "../../utils/fetchy";

const content = ref("");
const emit = defineEmits(["refreshFriends"]);

const createFriend = async (friend: string) => {
  try {
    await fetchy("/api/friends", "POST", {
      body: { friend },
    });
  } catch (_) {
    return;
  }
  emit("refreshFriends");
  emptyForm();
};

const emptyForm = () => {
  content.value = "";
};
</script>

<template>
  <form @submit.prevent="createFriend(friend)">
    <label for="friend">Potential Friend Username:</label>
    <textarea id="friend" v-model="friend" placeholder="Add a new Friend!" required> </textarea>
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
