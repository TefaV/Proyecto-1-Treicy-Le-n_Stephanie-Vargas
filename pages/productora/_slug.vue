<template>
  <div class="container">
    <HeaderView />
    <div class="row">
      <div class="three columns"></div>
      <div class="six columns">
        <!-- Mostrar la imagen de la productora -->
        <div v-if="productora">
          <img
            :src="getProductoraImage(productora.slug)"
            :alt="productora.nombre"
            class="productora-imagen"
          />
        </div>

        <!-- Mostrar el nombre de la productora -->
        <h4>{{ productora ? productora.nombre : 'Productora no encontrada' }}</h4>

        <!-- Mostrar películas asociadas a la productora -->
        <div v-if="peliculas.length > 0">
          <h5>Películas producidas por {{ productora.nombre }}:</h5>
          <ul>
            <li v-for="pelicula in peliculas" :key="pelicula.slug">
              <!-- Enlace a la página de la película -->
              <NuxtLink :to="{ name: 'peliculas-slug', params: { slug: pelicula.slug } }">
                {{ pelicula.titulo }} ({{ pelicula.año }})
              </NuxtLink>
            </li>
          </ul>
        </div>
        <div v-else>
          <p>No hay películas asociadas a esta productora.</p>
        </div>
      </div>
      <div class="two columns"></div>
    </div>
    <FooterView />
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    try {
      // Cargar todas las productoras
      const productoras = await $content('productora').fetch();
      const productora = productoras.find(p => p.slug === params.slug);

      if (!productora) {
        return { productora: null, peliculas: [] };
      }

      // Cargar todas las películas
      const peliculas = await $content('pelicula').fetch();

      // Filtrar las películas que pertenecen a esta productora
      const peliculasAsociadas = peliculas.filter(p => p.productora_id === productora.id);

      return { productora, peliculas: peliculasAsociadas };
    } catch (error) {
      console.error('Error fetching data:', error);
      return { productora: null, peliculas: [] };
    }
  },
  methods: {
    // Generar la ruta de la imagen de la productora en base al slug
    getProductoraImage(slug) {
      
      return `/images/${slug}.png`;
    }
  }
}
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

/* Imagen de la productora */
.productora-imagen {
  display: block;
  margin: 0 auto 20px;
  max-width: 100%;
  height: auto;
  box-shadow: 0px 0px 15px rgba(0, 0, 0, 0.7); /* Sombra para la imagen */
}

/* Encabezado */
header {
  background-color: #1a1a1a;
  color: #ff3333;
  text-align: center;
  padding: 15px 0;
  border-bottom: 2px solid #333;
  font-size: 2rem;
}

/* Estilos de columnas */
.row {
  display: flex;
  justify-content: center;
}

.three.columns, .six.columns, .two.columns {
  padding: 15px;
}

/* Título de la productora */
h4 {
  font-size: 2.5rem;
  color: #ff3333;
  text-transform: uppercase;
  text-align: center;
  margin-bottom: 20px;
  text-shadow: 2px 2px 10px rgba(255, 0, 0, 0.8);
}

/* Título de las películas */
h5 {
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

/* Estilo para los bordes y sombras */
.container,
header,
footer {
  box-shadow: 0px 0px 15px rgba(0, 0, 0, 0.7);
}

/* Añadir un efecto de parpadeo tenue a los
