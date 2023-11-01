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
      filter: isDead ? 'blur(3px)' : '',
    }"
  >
    <div class="container" :style="{ height: '20rem' }">
      <img
        :src="character.imageSrc"
        :style="{
          width: '100%',
          height: '100%',
          padding: '1rem',
          objectFit: 'cover',
          borderRadius: '50px',
          border: isBloody ? '0.3rem solid red' : '',
          border: isDead ? '0.3rem solid black' : '',
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
      <hr />
      <div class="input-group">
        <input
          v-model="damageRoll"
          type="number"
          class="form-control"
          placeholder="Roll"
        />
        <input
          v-model="damageAmount"
          type="number"
          class="form-control"
          placeholder="Damage Amount"
        />
        <div class="input-group-append">
          <button @click="applyDamage" class="btn btn-danger">DMG</button>
        </div>
      </div>
      <hr />
      <div class="input-group">
        <input
          v-model="healAmount"
          type="number"
          class="form-control"
          placeholder="Heal"
        />
        <div class="input-group-append">
          <button @click="applyHeal" class="btn btn-primary">Heal</button>
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
      damageRoll: "",
      healAmount: "",
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
    isDead() {
      return this.currentHealth <= 0;
    },
  },
  methods: {
    applyDamage() {
      // Convert the damageAmount to a number and apply it to the character
      const damage = parseInt(this.damageAmount);
      const damageRoll = parseInt(this.damageRoll);
      console.log(this.character.armorValue);

      if (!isNaN(damage) && damage >= 0) {
        if (damageRoll >= this.character.armorValue) {
          this.$emit("apply-damage", this.character, damage);
          this.damageAmount = "";
        } else {
          alert("Miss!");
        }
      } else {
        alert("Incorrect value, please enter a positive integer");
      }
    },
    applyHeal() {
      const healAmount = parseInt(this.healAmount);
      alert(this.character.healthPoints);
      if (!isNaN(healAmount) && healAmount >= 0) {
        this.$emit("apply-heal", this.character, healAmount);
        this.healAmount = "";
      }
    },
  },
};
</script>

<style lang="css" scoped>
.enemy-card {
  /* Add styles for the red underglow here */
  border-bottom: 3px solid red;
}
</style>
