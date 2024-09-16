<template>
  <div class="container">
    <HeaderView />
    <div class="row">
      <div class="one column"></div>
      <div class="ten columns">
        <!-- Mostrar la imagen del director -->
        <div v-if="director">
          <img
            :src="getDirectorImage(director.slug)"
            :alt="`Imagen de ${director.nombre}`"
            class="director-imagen"
          />
        </div>

        <!-- Mostrar el nombre del director -->
        <h2>{{ director ? director.nombre : "Director no encontrado" }}</h2>

        <!-- Mostrar películas dirigidas por el director -->
        <div v-if="peliculas.length > 0">
          <h3>Películas dirigidas por {{ director.nombre }}:</h3>
          <ul>
            <li v-for="pelicula in peliculas" :key="pelicula.slug">
              <!-- Enlace a la página de la película -->
              <NuxtLink
                :to="{
                  name: 'peliculas-slug',
                  params: { slug: pelicula.slug },
                }"
              >
                {{ pelicula.titulo }} ({{ pelicula.año }})
              </NuxtLink>
            </li>
          </ul>
        </div>
        <div v-else>
          <p>No hay películas dirigidas por este director.</p>
        </div>
      </div>
      <div class="one column"></div>
    </div>
    <FooterView />
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    try {
      // Cargar todas las películas y directores
      const peliculas = await $content("pelicula").fetch();
      const directores = await $content("director").fetch();

      // Buscar el director específico basado en el slug
      const director = directores.find((d) => d.slug === params.slug);

      if (!director) {
        return { director: null, peliculas: [] };
      }

      // Filtrar todas las películas dirigidas por este director
      const peliculasDirigidas = peliculas.filter(
        (p) => p.director_id === director.id
      );

      return { director, peliculas: peliculasDirigidas };
    } catch (error) {
      console.error("Error fetching data:", error);
      return { director: null, peliculas: [] };
    }
  },
  methods: {
    // Generar la ruta de la imagen del director en base al slug
    getDirectorImage(slug) {
      return `/images/${slug}.png`;
    }
  }
};
</script>

<style scoped>
/* Contenedor principal */
.container {
  padding: 20px;
  background-color: #0d0d0d;
  color: #e0e0e0;
  min-height: 100vh;
  font-family: 'Creepster', cursive;
}

/* Imagen del director */
.director-imagen {
  display: block;
  margin: 0 auto 20px;
  max-width: 100%;
  height: auto;
  box-shadow: 0px 0px 15px rgba(0, 0, 0, 0.7); /* Sombra para la imagen */
}

/* Encabezado */
h2 {
  font-size: 2.5rem;
  color: #ff3333;
  text-transform: uppercase;
  text-align: center;
  margin-bottom: 20px;
  text-shadow: 2px 2px 10px rgba(255, 0, 0, 0.8);
}

/* Título de las películas */
h3 {
  font-size: 1.8rem;
  color: #66ff66;
  text-shadow: 1px 1px 5px rgba(0, 255, 0, 0.6);
  margin-bottom: 15px;
}

/* Lista de películas */
ul {
  list-style-type: none;
  padding: 0;
}

li {
  font-size: 1.2rem;
  margin-bottom: 10px;
}

NuxtLink {
  color: #ffcc00;
  text-decoration: none;
  transition: color 0.3s ease;
}

NuxtLink:hover {
  color: #ff3333;
  text-shadow: 1px 1px 5px rgba(255, 0, 0, 0.8);
}

/* Pie de página */
footer {
  background-color: #1a1a1a;
  color: #ff3333;
  text-align: center;
  padding: 15px 0;
  border-top: 2px solid #333;
  font-size: 1.5rem;
}
</style>
