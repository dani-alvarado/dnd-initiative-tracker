<template>
  <div
    :key="character.name"
    :class="cardClass"
    class="card"
    style="width: 18rem; border-radius: 30px; margin-bottom: 0.5rem"
  >
    <div class="container" style="height: 20rem">
      <img
        :src="character.imageSrc"
        style="
          width: 100%;
          height: 100%;
          padding: 1rem;
          object-fit: cover;
          border-radius: 50px;
        "
        class="card-img-top"
        alt="Character Image"
      />
    </div>
    <div class="card-header">{{ character.name }}</div>
    <div class="card-body">
      <h5 class="card-title">
        {{ character.class }} Level: {{ character.level }}
      </h5>
      <ul class="list-group list-group-flush">
        <li class="list-group-item">
          HP: {{ calculateCurrentHealth() }}/{{ character.healthPoints }}
        </li>
        <li class="list-group-item">Initiative: {{ character.initiative }}</li>
        <li class="list-group-item">Armor: {{ character.armorValue }}</li>
      </ul>
      <div class="input-group">
        <input
          v-model="damageAmount"
          type="number"
          class="form-control"
          placeholder="Damage Amount"
        />
        <div class="input-group-append">
          <button @click="applyDamage" class="btn btn-danger">
            Apply Damage
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CharacterCard",
  props: {
    character: Object,
  },
  data() {
    return {
      damageAmount: "",
      damageTaken: 0, // Initialize damageTaken
    };
  },
  methods: {
    applyDamage() {
      // Convert the damageAmount to a number and apply it to the copied character
      const damage = parseInt(this.damageAmount);
      if (!isNaN(damage) && damage >= 0) {
        this.damageTaken += damage;
        // Clear the input field after applying damage
        this.$emit("apply-damage", this.character);
        this.damageAmount = "";
      } else {
        // Handle invalid input (e.g., show a message to the user)
        alert("Incorrect value, please enter a positive integer");
      }
    },
    calculateCurrentHealth() {
      return this.character.healthPoints - this.damageTaken;
    },
  },
  computed: {
    cardClass() {
      return {
        "enemy-card": this.character.isEnemy,
      };
    },
  },
};
</script>

<style lang="css" scoped>
.enemy-card {
  /* Add styles for the red underglow here */
  border-bottom: 4px solid red;
}
</style>
