<script setup lang="ts">
import CreateFriendForm from "@/components/Friend/CreateFriendForm.vue";
import FriendComponent from "@/components/Friend/FriendComponent.vue";
import { useUserStore } from "@/stores/user";
import { fetchy } from "@/utils/fetchy";
import { storeToRefs } from "pinia";
import { onBeforeMount, ref } from "vue";

const { isLoggedIn } = storeToRefs(useUserStore());

const loaded = ref(false);
let friends  = ref<Array<Record<string, string>>>([]);
let searchAuthor = ref("");

async function getFriends(author?: string) {
  let query: Record<string, string> = author !== undefined ? { author } : {};
  let friendsResults;
  try {
    friendsResults = await fetchy("/api/friends", "GET", { query });
  } catch (_) {
    return;
  }
  searchAuthor.value = author ? author : "";
  friends.value = friendsResults;
}

onBeforeMount(async () => {

  await getFriends();
  loaded.value = true;
});
</script>

<template>
  <section v-if="isLoggedIn">
    <h2>Make a new friend:</h2>
    <CreateFriendForm @refreshFriends="getFriends" />
  </section>

  <section class="friends" v-if="loaded && friends.length !== 0">
    <article v-for="friend in friends" :key="friend._id">
      <FriendComponent :friend="friend" @refreshFriends="getFriends" />
    </article>
  </section>
  <p v-else-if="loaded">No friends found</p>
  <p v-else>Loading...</p>
</template>

<style scoped>
section {
  display: flex;
  flex-direction: column;
  gap: 1em;
}

section,
p,
.row {
  margin: 0 auto;
  max-width: 60em;
}

article {
  background-color: var(--base-bg);
  border-radius: 1em;
  display: flex;
  flex-direction: column;
  gap: 0.5em;
  padding: 1em;
}

.friends {
  padding: 1em;
}

.row {
  display: flex;
  justify-content: space-between;
  margin: 0 auto;
  max-width: 60em;
}
</style>
