<template>
  <div class="full-screen-container">
    <nav class="navbar">
      <ul>
        <li><NuxtLink to="/">Películas</NuxtLink></li>
        <li><NuxtLink to="/directores">Directores</NuxtLink></li>
        <li><NuxtLink to="/productoras">Productoras</NuxtLink></li>
      </ul>
    </nav>

    <div class="content">
      <div class="decoration-left">
        <img src="/images/tela_arañaderecha.png" alt="Telaraña izquierda" />
      </div>

      <div class="main-content">
        <h3>Mejores películas de Terror del 2023-2024</h3>
        <p>Películas de Terror</p>
        <ul class="movie-list">
          <li
            v-for="pelicula in peliculas"
            :key="pelicula.slug"
            class="movie-item"
          >
            <NuxtLink
              :to="{ name: 'peliculas-slug', params: { slug: pelicula.slug } }"
            >
              {{ pelicula.titulo }}
            </NuxtLink>
          </li>
        </ul>
      </div>

      <div class="decoration-right">
        <img src="/images/tela_arañaizquierda.png" alt="Telaraña derecha" />
      </div>
    </div>

    <footer class="footer">
      <p>Treicy León y Stephanie 2024 Primer Proyecto Arquitectura</p>
    </footer>
  </div>
</template>

<script>
export default {
  async asyncData({ $content }) {
    try {
      const peliculas = await $content("pelicula").fetch();
      return { peliculas };
    } catch (error) {
      console.error("Error fetching movies:", error);
      return { peliculas: [] };
    }
  },
};
</script>

<style scoped>
/* Configuración para que el contenido ocupe toda la pantalla */
.full-screen-container {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  background-color: #1e1e1e;
  color: #ffffff;
  min-height: 100vh;
  margin: 0;
  padding: 0;
  overflow-x: hidden;
  position: relative;
}

/* Barra de navegación */
.navbar {
  background-color: #2b2b2b;
  padding: 0;
  margin-bottom: 0; /* Elimina el espacio debajo del menú */
  text-align: center;
}

.navbar ul {
  list-style-type: none;
  margin: 0;
  padding: 10px 0; /* Reduce el espacio entre el menú y el contenido */
}

.navbar li {
  display: inline;
  margin: 0 15px;
}

.navbar a {
  color: #ffffff;
  text-decoration: none;
  font-weight: bold;
}

.navbar a:hover {
  color: #ff6347;
}

/* Contenido principal */
.content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-grow: 1;
  position: relative;
  margin-top: 0; /* Elimina el espacio entre el menú y el contenido */
}

/* Elementos de decoración a los lados */
.decoration-left,
.decoration-right {
  width: 150px;
  display: flex;
  align-items: center;
}

.decoration-left img,
.decoration-right img {
  height: 50vh;
  width: auto;
  opacity: 0.3;
  position: fixed;
  top: 50%;
  transform: translateY(-50%);
}

.decoration-left img {
  left: 0;
}

.decoration-right img {
  right: 0;
}

/* Estilo para la sección principal */
.main-content {
  flex-grow: 1;
  text-align: center;
  padding: 10px 0; /* Reduce el padding superior e inferior */
}

.main-content h3 {
  color: #ff6347;
  margin: 0; /* Elimina márgenes innecesarios */
}

.movie-list {
  list-style-type: none;
  padding: 0;
}

.movie-item {
  margin: 10px 0;
}

.movie-item a {
  color: #ffffff;
  font-size: 18px;
  text-decoration: none;
}

.movie-item a:hover {
  color: #ff6347;
}

p {
  font-size: 16px;
  margin: 5px 0; /* Reduce márgenes entre párrafos */
}

/* Estilos para el footer */
.footer {
  background-color: #2b2b2b;
  color: white;
  text-align: center;
  padding: 10px;
  position: static;
  bottom: 0;
  width: 100%;
}

/* Ajustes adicionales para que no aparezcan bordes blancos */
body,
html {
  margin: 0;
  padding: 0;
  min-height: 100vh;
}
</style>
