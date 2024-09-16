<template>
    <div class="container">
      <HeaderView />
      <h3>Directores</h3>
      <ul>
        <li v-for="director in directores" :key="director.slug">
          <NuxtLink :to="{ name: 'director-slug', params: { slug: director.slug } }">
            {{ director.nombre }}
          </NuxtLink>
        </li>
      </ul>
      <FooterView />
    </div>
  </template>
  
  <script>
  export default {
    async asyncData({ $content }) {
      try {
        // Cargar todos los directores
        const directores = await $content('director').fetch();
        return { directores };
      } catch (error) {
        console.error('Error fetching directors:', error);
        return { directores: [] };
      }
    }
  }
  </script>
  