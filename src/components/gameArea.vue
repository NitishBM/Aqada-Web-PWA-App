<template>
  <div class="h-screen w-screen flex flex-col overflow-hidden">
    <!-- Main Content Start -->
    <div class="flex-grow flex items-center justify-center w-full h-full relative overflow-hidden">
      <transition enter-active-class="transition-transform duration-500 ease-in-out opacity-100"
        enter-from-class="-translate-y-full opacity-0"
        leave-active-class="transition-transform duration-500 ease-in-out opacity-100"
        leave-to-class="translate-y-full opacity-0" mode="out-in">
        <iframe v-if="games.length" :key="currentGame" :src="games[currentGame].game_url"
          class="w-full h-full border-none absolute"></iframe>
      </transition>
    </div>
    <!-- Main Content End -->

    <!-- Bottom Content Start -->
    <hr />
    <div class="mt-auto mb-3 w-full">
      <div class="flex justify-between items-center px-4">
        <!-- Back Button -->
        <button @click="showPreviousGame" class="flex justify-center items-center gap-2 ">

          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
            stroke="currentColor" class="size-10">
            <path stroke-linecap="round" stroke-linejoin="round"
              d="m15 11.25-3-3m0 0-3 3m3-3v7.5M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
          </svg>

        </button>

        <!-- Game Name -->
        <div class="text-lg font-semibold" v-if="games.length">
          {{ games[currentGame].game_type_name }}
        </div>

        <!-- Next Button -->
        <button @click="showNextGame" class="flex justify-center items-center gap-2">

          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
            stroke="currentColor" class="size-10">
            <path stroke-linecap="round" stroke-linejoin="round"
              d="m9 12.75 3 3m0 0 3-3m-3 3v-7.5M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
          </svg>

        </button>
      </div>
    </div>
    <!-- Bottom Content End -->
  </div>
</template>

<script>
import axios from "axios";
import Cookies from "js-cookie";

export default {
  name: "GameArea",
  data() {
    return {
      currentGame: 0,
      games: []
    };
  },
  async mounted() {
    await this.fetchGames();
  },
  watch: {
    currentGame(newIndex, oldIndex) {
      if (this.games.length > 0) {
        this.updateGameStatus(oldIndex, "inactive"); // Mark previous game as inactive
        this.updateGameStatus(newIndex, "active"); // Mark new game as active
      }
    }
  },
  methods: {
    async fetchGames() {
      try {
        const response = await axios.get("http://localhost:5000/games");
        if (response.data && response.data.length) {
          this.games = response.data;
          this.updateGameStatus(this.currentGame, "active"); // Set first game as active
        } else {
          console.error("No games found in API response.");
        }
      } catch (error) {
        console.error("Error fetching games:", error);
      }
    },
    updateGameStatus(index, status) {
      if (this.games.length > 0 && this.games[index]) {
        const gameId = this.games[index]._id; // Get the _id of the game
        Cookies.set("_id", gameId, { expires: 7, path: "/" });
        Cookies.set("gameStatus", status, { expires: 7, path: "/" }); // Store game status in cookies
        console.log(`Stored game ID: ${gameId} with status: ${status}`);
      }
    },
    showPreviousGame() {
      const oldIndex = this.currentGame;
      this.currentGame = (this.currentGame - 1 + this.games.length) % this.games.length;
      this.updateGameStatus(oldIndex, "inactive"); // Update previous game to inactive
    },
    showNextGame() {
      const oldIndex = this.currentGame;
      this.currentGame = (this.currentGame + 1) % this.games.length;
      this.updateGameStatus(oldIndex, "inactive"); // Update previous game to inactive
    }
  }
};
</script>
