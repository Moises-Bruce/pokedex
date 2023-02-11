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
    <h1>{{pokemon.name}}</h1>
    <img :src="pokemon?.sprites?.other?.home?.front_default" class="h-52 w-52"/>

    <template v-for="stats in pokemon.stats">
        <div class="flex gap-2">
            <span>{{ stats.stat.name }}</span>
            <span>{{ stats.base_stat }}</span>
        </div>
    </template>
</template>