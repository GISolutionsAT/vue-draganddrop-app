<template>
  <div id="app">
    <nav class="navbar text-info" > 
      <a href="https://gisolutions.at/" target=”_blank”>
        <v-btn icon>
          <v-icon>public</v-icon>
        </v-btn>
        GISolutions e.U
      </a>
    </nav>
    <DragAndDropMap />

    <footer 
      class="footer"
      id="sticky-footer"
    >
      <!-- Social Media Icons -->
      <div class="footer-icons">
        <ul class="footer-icon-social text-center">
          <li class="list-inline-item">
            <a href="https://twitter.com/solutions_gi">
              <i class="fab fa-twitter fa-2x" />
            </a>
          </li>
          <li class="list-inline-item">
            <a href="https://www.linkedin.com/company/gisolutions-e-u/">
              <i class="fab fa-linkedin-in fa-2x" />
            </a>
          </li>
          <li class="list-inline-item">
            <a href="https://www.instagram.com/juergenhahn01/">
              <i class="fab fa-instagram fa-2x" />
            </a>
          </li>
          <li class="list-inline-item">
            <a href="https://github.com/GISolutionsAT">
              <i class="fab fa-github fa-2x" />
            </a>
          </li>
        </ul>
      </div>
    </footer>
  </div>
</template>

<script>
import DragAndDropMap from './components/DragAndDropMap.vue';

export default {
  name: 'App',
  components: {
    DragAndDropMap
  }
}
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Roboto&display=swap");

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body {
    font-family: "Roboto", sans-serif;
    line-height: 1.4;
  }

  .nav {
    color: teal;  
  }

  .navbar a {
    color: teal;
    text-decoration: none;
  }

   .footer-icons {
    display: flex;
    justify-content: center;
    background-color: #2aa198;
  }

  .footer ul {
    margin: 1rem;
  }

  .footer ul li a {
    padding-right: 1.5rem;
    padding-left: 1.5rem;
  }
  
  .footer-icon-social li i {
    color: #fff;
    z-index: 100;
  }
</style>
