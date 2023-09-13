<template>
  <div
    v-if="character.isAvailable || character.isEnemy"
    :key="character.name"
    :class="cardClass"
    class="card"
    :style="{
      width: '18rem',
      borderRadius: '30px',
      marginBottom: '0.5rem',
    }"
  >
    <div class="container" style="height: 20rem">
      <img
        :src="character.imageSrc"
        :style="{
          width: '100%',
          height: '100%',
          padding: '1rem',
          objectFit: 'cover',
          borderRadius: '50px',
          border: isBloody ? '0.3rem solid red' : '',
        }"
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
          HP: {{ character.isEnemy ? "???" : currentHealth }}/{{
            character.isEnemy ? "???" : character.healthPoints
          }}
        </li>
        <li class="list-group-item">Initiative: {{ character.initiative }}</li>
        <li class="list-group-item">
          Armor: {{ character.isEnemy ? "???" : character.armorValue }}
        </li>
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
    };
  },
  computed: {
    cardClass() {
      return {
        "enemy-card": this.character.isEnemy,
      };
    },
    currentHealth() {
      // Calculate the current health by subtracting the damage taken
      return this.character.healthPoints - this.character.damageTaken;
    },
    isBloody() {
      // Determine if the character is in the "Bloody" state (health is at half or less)
      return this.currentHealth <= this.character.healthPoints / 2;
    },
  },
  methods: {
    applyDamage() {
      // Convert the damageAmount to a number and apply it to the character
      const damage = parseInt(this.damageAmount);
      if (!isNaN(damage) && damage >= 0) {
        this.$emit("apply-damage", this.character, damage);
        this.damageAmount = "";
      } else {
        alert("Incorrect value, please enter a positive integer");
      }
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
