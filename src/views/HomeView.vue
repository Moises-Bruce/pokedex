<script setup>
  import { ref } from 'vue';
  import axios from 'axios';
  import { api } from '../lib/api';

  const name = ref('');
  const message = ref('Digite o nome de um pokémon para saber todos os detalhes dele');
  const pokemon = ref({});
  const evolutions = ref([]);

  async function getEvolvesTo(chain) {
    const evolvesPokemon = []
    let nextEvolve = chain.evolves_to[0];

    do {
      const { data: specie } = await axios.get(nextEvolve.species.url);
      const { data: pokemonData } = await api.get(`pokemon/${specie.id}`);

      evolvesPokemon.push({
        id: specie.id,
        name: nextEvolve.species.name,
        image: pokemonData.sprites.other.dream_world.front_default,
        details: nextEvolve.evolution_details[0],
      })

      nextEvolve = nextEvolve.evolves_to[0];
    } while (Boolean(nextEvolve) && nextEvolve.hasOwnProperty('evolves_to'));

    return evolvesPokemon
  }

  async function searchPokemon(){
    try {
      const pokemonName = String(name.value).toLowerCase();

      pokemon.value = {}
      message.value = 'Estamos buscando o seu pokemon...'

      const { data: pokemonData } = await api.get(`pokemon/${pokemonName}`);
      const { data: evolutionsData } = await api.get(`evolution-chain/${pokemonData.id}`);

      pokemon.value = pokemonData
      evolutions.value = await getEvolvesTo(evolutionsData.chain);

      message.value = 'Digite o nome de um pokémon para você saber todos os detalhes dele'
    } catch (error) {
      console.log(error)
      message.value = 'Não encontramos esse pokemon, tente pesquisar novamente!'
    }
  }
</script>

<template>
  <main>
    <form action="" @submit.prevent="searchPokemon">
      <input 
        type="text" 
        id="namePokemon" 
        placeholder="Qual pokémon você está procurando?" 
        v-model="name" 
        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
      />
    </form>
  </main>

  <template v-if="Object.keys(pokemon).length !== 0">
    <div class="mt-4 flex items-center justify-center">
      <div class="rounded-lg bg-gray-200 w-72 h-72 mt-10 flex flex-col items-center justify-center border-2 border-black">
        <p class="flex justify-center items-center text-2xl font-semibold">{{ pokemon.name }}</p>
        <img :src="pokemon.sprites.other.home.front_default" class="h-60 w-60 flex items-center justify-center"/>
      </div>
    </div>
    <div class="flex items-center justify-center mt-10 border-b-2 border-black">
      <h1 class="text-2xl font-bold mb-2">PRÓXIMAS EVOLUÇÕES</h1>
    </div>
    <div class="flex justify-between">
        <template v-for="evolution in evolutions" :key="evolution.name">
          <div class="border-2 border-black mx-56 mt-16 w-72 h-72 items-center justify-center flex flex-col rounded-lg bg-gray-200">
            <RouterLink :to="{ name: 'details', params: { id: evolution.id } }">
              <p class="flex justify-center items-center text-2xl font-semibold">{{ evolution.name }}</p>
              <img :src="evolution.image" class="h-60 w-60 flex items-center justify-center">
            </RouterLink>
          </div>
        </template>
    </div>
  </template>
  <template v-else>
    <h1 class="text-gray-700 text-2xl font-bold h-96 flex justify-center items-center">{{ message }}</h1> 
  </template>
</template>
