<template>
  <div class="card-scene">
    <div class="card-inner" :class="{ 'is-flipped': isFlipped }">

      <div class="card-face card-back">
        <div class="pokeball-center"></div>
      </div>

      <div class="card-face card-front pokemon-card" :class="pokemon.type">
        <div class="card-header">
          <h2 class="name">{{ pokemon.name }}</h2>
          <span class="hp">HP {{ pokemon.hp }}</span>
        </div>
        
        <div class="image-box">
          <img :src="pokemon.image" :alt="pokemon.name" />
        </div>
        
        <div class="stats-bar">
          <span>NO. {{ pokemon.id }}</span>
          <span>HT: {{ pokemon.height }}m</span>
          <span>WT: {{ pokemon.weight }}kg</span>
        </div>
        
        <div class="abilities-container">
          <div v-for="(ability, i) in pokemon.abilities" :key="i" class="ability">
            <img :src="`https://raw.githubusercontent.com/duiker101/pokemon-type-svg-icons/master/icons/${ability.type}.svg`" class="energy-icon-img" />
            <span class="ability-name">{{ ability.name }}</span>
          </div>
        </div>
        
        <div class="card-footer">
          <span v-for="tipo in pokemon.allTypes" :key="tipo" class="type-badge">
            <img :src="`https://raw.githubusercontent.com/duiker101/pokemon-type-svg-icons/master/icons/${tipo}.svg`" :alt="tipo" class="type-icon" />
            {{ tipo }}
          </span>
        </div>
      </div>

    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const props = defineProps({
  pokemon: Object,
  index: Number 
});

const isFlipped = ref(false);

onMounted(() => {
  setTimeout(() => {
    isFlipped.value = true;
  }, props.index * 60 + 100); 
});
</script>

<style scoped>
.card-scene {
  width: 300px;
  height: 460px;
  perspective: 1000px; 
}

.card-inner {
  width: 100%;
  height: 100%;
  position: relative;
  transition: transform 0.6s cubic-bezier(0.4, 0.2, 0.2, 1); 
  transform-style: preserve-3d;
  transform: rotateY(180deg); 
}

.card-inner.is-flipped {
  transform: rotateY(0deg);
}

.card-face {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  -webkit-backface-visibility: hidden; 
  backface-visibility: hidden; 
  border-radius: 12px;
  box-sizing: border-box;
}

.card-front {
  transform: rotateY(0deg);
}

.card-back {
  background-color: #2a4b8d;
  border: 12px solid #fbdf7e;
  transform: rotateY(180deg);
  display: flex;
  justify-content: center;
  align-items: center;
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}

.pokeball-center {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  background: radial-gradient(circle, white 30%, #fbdf7e 35%, #fbdf7e 100%);
  border: 10px solid #1a2a5e;
}

.pokemon-card {
  border: 12px solid #fbdf7e;
  padding: 10px;
  display: flex;
  flex-direction: column;
  background-color: #f0f0f0;
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
  text-transform: capitalize;
  color: #111;
}

.pokemon-card.dark, 
.pokemon-card.ghost { color: white; }

.pokemon-card.fire { background-color: #f47a60; }
.pokemon-card.water { background-color: #71bbf2; }
.pokemon-card.grass { background-color: #8bd478; }
.pokemon-card.electric { background-color: #f6d854; }
.pokemon-card.psychic { background-color: #e57ea1; }
.pokemon-card.normal { background-color: #b5b5a6; }
.pokemon-card.dark { background-color: #6c594a; }
.pokemon-card.fairy { background-color: #f3a7e7; }
.pokemon-card.poison { background-color: #a453a8; color: white; }
.pokemon-card.rock { background-color: #c9b787; }
.pokemon-card.dragon { background-color: #7b62e3; color: white; }
.pokemon-card.ground { background-color: #e0c068; }
.pokemon-card.bug { background-color: #a8b820; }
.pokemon-card.fighting { background-color: #c03028; color: white; }
.pokemon-card.ice { background-color: #98d8d8; }
.pokemon-card.ghost { background-color: #705898; }
.pokemon-card.steel { background-color: #b8b8d0; }
.pokemon-card.flying { background-color: #a890f0; }

.card-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 8px; }
.name { margin: 0; font-size: 1.1rem; font-weight: 800; }
.hp { font-weight: bold; color: #d03030; }

.pokemon-card.dark .hp, .pokemon-card.ghost .hp, .pokemon-card.fighting .hp, .pokemon-card.dragon .hp, .pokemon-card.poison .hp {
  color: white;
}

.image-box {
  background: radial-gradient(circle, #fff 0%, #d4d4d4 100%);
  border: 4px solid #b5b5b5; height: 190px; flex-shrink: 0;
  display: flex; justify-content: center; align-items: center; overflow: hidden; margin-bottom: 10px;
}
.image-box img { width: 100%; height: 100%; object-fit: contain; padding: 10px; }

.stats-bar {
  background-color: #e2e2e2; font-size: 0.65rem; font-weight: bold;
  display: flex; justify-content: space-around; padding: 2px 0; margin: 4px 0; color: #333;
}

.abilities-container { flex-grow: 1; display: flex; flex-direction: column; justify-content: center; gap: 10px; padding: 0 5px; }
.ability { display: flex; align-items: center; gap: 8px; font-size: 0.9rem; font-weight: bold; }

.card-footer {
  display: flex; justify-content: center; gap: 10px; margin-top: auto;
  padding-top: 10px; border-top: 1px solid rgba(0,0,0,0.1);
}

.type-badge {
  background-color: rgba(255, 255, 255, 0.6); padding: 4px 12px; border-radius: 20px;
  font-size: 0.8rem; font-weight: bold; display: flex; align-items: center; gap: 5px;
  border: 1px solid rgba(0,0,0,0.2); box-shadow: 0 2px 4px rgba(0,0,0,0.1); color: #111;
}

.type-icon, .energy-icon-img { width: 18px; height: 18px; filter: invert(100%); }
.energy-icon-img { width: 22px; height: 22px; }
</style>