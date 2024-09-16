<template>
  <div class="container catalog">
    <HeaderView />
    <div class="catalog-content">
      <div class="movie-card">
        <!-- Imagen de la película -->
        <img
          v-if="pelicula"
          :src="getPeliculaImage(pelicula.slug)"
          :alt="pelicula.titulo"
          class="pelicula-imagen"
        />
        <div class="movie-details">
          <h2>{{ pelicula ? pelicula.titulo : 'Película no encontrada' }}</h2>
          <p><strong>Género:</strong> {{ pelicula ? pelicula.genero : 'Desconocido' }}</p>
          <p><strong>Año:</strong> {{ pelicula ? pelicula.año : 'Desconocido' }}</p>
          <p><strong>Duración:</strong> {{ pelicula ? pelicula.duracion + ' minutos' : 'Desconocida' }}</p>
          
          <!-- Mostrar detalles de la productora asociada con enlace a la página de la productora -->
          <p><strong>Productora:</strong> 
            <NuxtLink
              v-if="productora"
              :to="{ name: 'productora-slug', params: { slug: productora.slug } }"
            >
              {{ productora.nombre }}
            </NuxtLink>
            <span v-else>Desconocida</span>
          </p>
          
          <!-- Mostrar detalles del director asociado con enlace a la página del director -->
          <p><strong>Director:</strong> 
            <NuxtLink
              v-if="director"
              :to="{ name: 'director-slug', params: { slug: director.slug } }"
            >
              {{ director.nombre }}
            </NuxtLink>
            <span v-else>Desconocido</span>
          </p>
        </div>
      </div>
    </div>
    <FooterView />
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    try {
      // Cargar todas las películas, productoras y directores
      const peliculas = await $content('pelicula').fetch();
      const productoras = await $content('productora').fetch();
      const directores = await $content('director').fetch();

      // Buscar la película específica basada en el slug
      const pelicula = peliculas.find(p => p.slug === params.slug);

      if (!pelicula) {
        return { pelicula: null, productora: null, director: null };
      }

      // Buscar la productora asociada basada en el productora_id
      const productora = productoras.find(p => p.id === pelicula.productora_id);
      
      // Buscar el director asociado basado en el director_id
      const director = directores.find(d => d.id === pelicula.director_id);

      return { pelicula, productora, director };
    } catch (error) {
      console.error('Error fetching data:', error);
      return { pelicula: null, productora: null, director: null };
    }
  },
  methods: {
    // Generar la ruta de la imagen de la película en base al slug
    getPeliculaImage(slug) {
    
      return `/images/${slug}.png`;
    }
  }
}
</script>

<style scoped>
.container.catalog {
  padding: 20px;
}

.catalog-content {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 70vh;
}

.movie-card {
  background-color: #222;
  color: #ddd;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.5);
  max-width: 600px;
  width: 100%;
  padding: 20px;
  text-align: center;
}

.pelicula-imagen {
  width: 100%;
  height: auto;
  border-bottom: 2px solid #444;
  margin-bottom: 15px;
}

.movie-details h2 {
  color: #f8f9fa;
  margin-bottom: 10px;
}

.movie-details p {
  margin: 5px 0;
}

.movie-details a {
  color: #1e90ff;
  text-decoration: none;
}

.movie-details a:hover {
  text-decoration: underline;
}
</style>
