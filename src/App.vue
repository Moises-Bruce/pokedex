<script setup>
  import { RouterView } from 'vue-router'
</script>

<template>
  <div class="container min-h-screen mx-auto flex flex-col p-4 gap-2">
    <header>
        <nav>
          <span class="text-2xl font-bold">Pokédex Challenger</span>  
        </nav>
    </header>
    <div class="flex-1">
      <RouterView />
    </div>
  </div>
</template>

