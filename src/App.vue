<template>
  <button @click="nextTurn" class="btn btn-primary">Next Turn</button>
  <div class="card-container">
    <CharacterCard
      v-for="character in sortedCharacters"
      :key="currentTurn + '-' + character.name"
      :character="character"
      @apply-damage="applyDamage"
    />
  </div>
</template>

<script>
import CharacterCard from "./components/CharacterCard.vue";
import mainCharacters from "./data/characters/mainCharacters.json";
import enemyCharacters from "./data/monsters/monsters.json";

export default {
  name: "App",
  components: {
    CharacterCard,
  },
  data() {
    return {
      characters: [],
      currentTurn: 0,
    };
  },
  computed: {
    sortedCharacters() {
      // Sort characters by initiative in descending order
      return [...this.characters].sort((a, b) => b.initiative - a.initiative);
    },
  },
  created() {
    this.characters = [...mainCharacters, ...enemyCharacters];
  },
  methods: {
    applyDamage(character, damageAmount) {
      damageAmount = Math.max(0, Math.floor(damageAmount));

      // Find the index of the updated character in the characters array
      const index = this.characters.findIndex(
        (char) => char.name === character.name
      );

      // Update the character in the characters array
      if (index !== -1) {
        this.characters[index].damageTaken += damageAmount;
      }
    },
    nextTurn() {
      console.log("Next turn button clicked");
      console.log("Before sorting:", this.sortedCharacters);

      // Implement logic to update the turn order
      // For example, move the character with the highest initiative to the end
      this.sortedCharacters.push(this.sortedCharacters.shift());

      console.log("After sorting:", this.sortedCharacters);
      this.currentTurn++;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.card-container {
  display: flex;
  flex-wrap: wrap; /* Wrap to the next row when there's not enough space */
  justify-content: space-between; /* Center horizontally */
  align-items: center; /* Center vertically */
  margin: 2rem;
}
</style>
