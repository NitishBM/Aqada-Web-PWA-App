<template>
  <div class="h-screen w-screen flex flex-col overflow-hidden">
    <!-- Main Content Start -->
    <div class="flex-grow flex items-center justify-center w-full h-full">
      <iframe :src="gameLinks[currentGame]" class="w-full h-full border-none"></iframe>
    </div>
    <!-- Main Content End -->

    <!-- Bottom Content Start -->
    <hr>
    <div class="container-fluid mt-auto mb-3 w-full">
      <div class="flex justify-between items-center px-4">
        <!-- Back Button -->
        <button @click="showPreviousGame"
          class="flex justify-center items-center gap-2 px-2 py-2 bg-gray-900 text-white rounded">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
            stroke="currentColor" class="w-5 h-5">
            <path stroke-linecap="round" stroke-linejoin="round" d="M8.25 6.75 12 3m0 0 3.75 3.75M12 3v18" />
          </svg>
          Back
        </button>

        <!-- Game Name -->
        <div class="text-lg font-semibold">
          {{ gameTitles[currentGame] }}
        </div>

        <!-- Next Button -->
        <button @click="showNextGame" class="flex justify-center items-center gap-2 p-2 bg-gray-900 text-white rounded">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
            stroke="currentColor" class="w-5 h-5">
            <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 17.25 12 21m0 0-3.75-3.75M12 21V3" />
          </svg>
          Next
        </button>
      </div>
    </div>
    <!-- Bottom Content End -->
  </div>
</template>

<script>
export default {
  name: "GameArea",
  data() {
    return {
      currentGame: 0, // Start with the first game (index 0)
      gameTitles: [
        "Personal Website",
        "Portfolio",
        "Facebook",
        "Google"
      ],
      gameLinks: [
        "https://nitishbm.github.io/Personal-Website/",
        "https://nitishbm.github.io/Portfolio/",
        "https://www.facebook.com/",
        "https://google.com/index.html"
      ]
    };
  },
  methods: {
    showPreviousGame() {
      this.currentGame = (this.currentGame - 1 + this.gameTitles.length) % this.gameTitles.length;
    },
    showNextGame() {
      this.currentGame = (this.currentGame + 1) % this.gameTitles.length;
    }
  }
};
</script>
