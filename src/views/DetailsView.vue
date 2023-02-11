<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import { api } from '../lib/api';

const route = useRoute();
const pokemon = ref({});

async function fetchPokemon(id) {
    const { data } = await api.get(`pokemon/${id}`)
    
    pokemon.value = data;
}

onMounted(() => {
    fetchPokemon(route.params.id)
})

</script>

<template>
    <div class="flex flex-col items-center justify-center mt-40">
        <div class="bg-gray-200 flex flex-col items-center justify-center rounded-lg h-full w-full border-2 border-black">
            <h1 class="flex justify-center items-center text-3xl font-semibold">{{pokemon.name}}</h1>
            <img :src="pokemon?.sprites?.other?.home?.front_default" class="h-72 w-72"/>
            <div class="bg-white flex flex-row justify-between w-full h-full mb-6 mt-6 rounded-lg">
                <template v-for="stats in pokemon.stats">
                    <div class="flex items-center justify-center gap-10 ">
                        <span class="font-bold text-lg">{{ stats.stat.name }}:</span>
                        <span class="font-bold text-lg">{{ stats.base_stat }}</span>
                    </div>
                </template>
            </div>
        </div>
    </div>

</template>