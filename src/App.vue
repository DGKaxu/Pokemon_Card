<template>
  <div class="app-container">
    <button class="draw-btn" @click="generateDeck" :disabled="loading">
      {{ loading ? 'Preparando mazo...' : 'Generar Mazo de 40 Cartas' }}
    </button>
    
    <div class="deck-grid">
      <!-- Muestra 40 Skeletons mientras está cargando -->
      <template v-if="loading">
        <SkeletonCard v-for="i in 40" :key="'skeleton-'+i" />
      </template>

      <!-- Muestra las cartas reales cuando termina, pasándole su 'index' -->
      <template v-else>
        <PokemonCard 
          v-for="(poke, index) in deck" 
          :key="poke.id" 
          :pokemon="poke"
          :index="index" 
        />
      </template>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import PokemonCard from './components/PokemonCard.vue';
import SkeletonCard from './components/SkeletonCard.vue';

const deck = ref([]);
const loading = ref(false);

const generateDeck = async () => {
  loading.value = true;
  deck.value = [];
  
  const randomIds = new Set();
  while(randomIds.size < 40) {
    randomIds.add(Math.floor(Math.random() * 1025) + 1);
  }

  const fetchPromises = Array.from(randomIds).map(async (id) => {
    try {
      const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${id}`);
      if (!response.ok) return null;
      const data = await response.json();
      
      const numAbilities = Math.floor(Math.random() * 3) + 2; 

      const selectedMoves = (data.moves || [])
        .sort(() => 0.5 - Math.random())
        .slice(0, numAbilities);

      const movesPromises = selectedMoves.map(async (m) => {
        try {
          const moveRes = await fetch(m.move.url);
          const moveData = await moveRes.json();
          return {
            name: moveData.name.replace('-', ' '),
            type: moveData.type.name 
          };
        } catch (err) {
          return { name: m.move.name.replace('-', ' '), type: 'normal' };
        }
      });

      const abilitiesWithTypes = await Promise.all(movesPromises);

      return {
        id: data.id,
        name: data.name,
        hp: data.stats?.find(s => s.stat.name === 'hp')?.base_stat || 50,
        type: data.types[0]?.type.name || 'normal', 
        allTypes: data.types?.map(t => t.type.name) || ['normal'], 
        weight: (data.weight / 10) || 0,
        height: (data.height / 10) || 0,
        image: data.sprites?.other?.['official-artwork']?.front_default || data.sprites?.front_default,
        abilities: abilitiesWithTypes
      };
    } catch (error) {
      return null;
    }
  });

  try {
    const results = await Promise.all(fetchPromises);
    deck.value = results.filter(p => p !== null && p.image !== null);
  } catch (error) {
    console.error(error);
  } finally {
    loading.value = false;
  }
};
</script>
<style>
body {
  margin: 0;
  background-color: #1a1a1a; 
  color: white;
}
</style>

<style scoped>
.app-container {
  padding: 20px;
  font-family: sans-serif;
  text-align: center;
  max-width: 1600px; 
  margin: 0 auto;
}

.draw-btn {
  padding: 12px 24px;
  font-size: 1.2rem;
  font-weight: bold;
  background-color: #e3350d;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  margin-bottom: 20px;
}

.draw-btn:disabled {
  background-color: #888;
}

.deck-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 25px;
  padding: 20px;
  width: 100%;
}
</style>