<script setup lang="ts">
import { useUserStore } from "@/stores/user";
import { formatDate } from "@/utils/formatDate";
import { storeToRefs } from "pinia";
import { fetchy } from "../../utils/fetchy";

const props = defineProps(["friend"]);
const emit = defineEmits(["refreshFriends"]);
const { currentUsername } = storeToRefs(useUserStore());

const deleteFriend = async () => {
  try {
    await fetchy(`/api/friends/${props.friend}`, "DELETE");
  } catch {
    return;
  }
  emit("refreshFriends");
};
</script>

<template>
  <p class="friend">{{ props.friend}}</p>
  <div class="base">
    <li><button class="button-error btn-small pure-button" @click="deleteFriend">Unfriend</button></li>
    <article class="timestamp">
      <p>Became Friends on: {{ formatDate(props.friend.dateCreated) }}</p>
    </article>
  </div>
</template>

<style scoped>
p {
  margin: 0em;
}

.friend {
  font-weight: bold;
  font-size: 1.2em;
}

menu {
  list-style-type: none;
  display: flex;
  flex-direction: row;
  gap: 1em;
  padding: 0;
  margin: 0;
}

.timestamp {
  display: flex;
  justify-content: flex-end;
  font-size: 0.9em;
  font-style: italic;
}

.base {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.base article:only-child {
  margin-left: auto;
}
</style>
