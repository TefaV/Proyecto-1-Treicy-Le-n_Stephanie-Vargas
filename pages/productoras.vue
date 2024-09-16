<template>
    <div class="container">
      <HeaderView />
      <h3>Productoras</h3>
      <ul>
        <li v-for="productora in productoras" :key="productora.slug">
          <!-- Enlace a la página de detalles de la productora -->
          <NuxtLink :to="{ name: 'productora-slug', params: { slug: productora.slug } }">
            {{ productora.nombre }}
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
        // Cargar todas las productoras
        const productoras = await $content('productora').fetch();
        return { productoras };
      } catch (error) {
        console.error('Error fetching productoras:', error);
        return { productoras: [] };
      }
    }
  }
  </script>
  