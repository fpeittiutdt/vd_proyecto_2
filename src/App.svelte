<script>
  import * as d3 from "d3";
  import { onMount } from "svelte";

  /* Array donde guardaremos la data */
  let series = [];

  // Ve o no series
  let watch_series = d3.scaleOrdinal().domain(["Si", "No"]).range([true, false])

  // Cantidad promedio de capítulos vistos
  let episodes = d3.scaleLinear().range([10, 20]);

  // Géneros favoritos
  let genres = d3.scaleOrdinal().domain(["Drama", "Comedia", "Misterio", "Crimen", "Romance", "Ciencia Ficción"]).range(["blue", "yellow", "black", "black", "purple", "gree"]).unknown("orange")

  // Plataformas
  let platforms = d3.scaleOrdinal().domain(["Netflix", "Disney+", "Star+", "HBO", "Amazon Prime"]).range(["nombres de archivo"]).unknown("nombres de archivo")
  
  // Rating de la serie favorita
  let rating = d3.scaleLinear().range([0,4])

  onMount(() => {
    d3.csv("./data/series.csv", d3.autoType).then((data) => {
      console.log(data);

      let minMaxEpisodes = d3.extent(data, (d) => d.episodes);
      episodes = episodes.domain(minMaxEpisodes);

      let minMaxRating = d3.extent(data, (d) => d.favRate);
      rating = rating.domain(minMaxRating);

      series = data;
    });
  });
</script>

<main>
  <div class="header">
    <img src="/images/olympics-logo.png" width="100" alt="anillos" />
    <h3 class="headline">
      <b>Triunfos Olímpicos</b>
      Medallas, alturas y continentes
    </h3>
    <p class="bajada">Explorando los logros olímpicos a través de datos</p>
  </div>

  <!-- Conedor de las entidades -->
  <div class="container">
    <!-- Iteramos la data para visualizar c/ entidad -->
    {#each deportistas as dep}
      <div class="person-container">
        <div
          class="medal"
          style="background-color: {colorMedalla(dep.medalla)}"
        ></div>
        <div
          class="person"
          style="border-width: {grosor(dep.edad)}px; 
        background-color:{colorGenero(dep.genero)}; 
        width: {2 * radioAltura(dep.altura)}px; 
        height: {2 * radioAltura(dep.altura)}px; 
        border-color: {colorContinentes(dep.continente)}"
        ></div>
        <p class="name">
          <b>{dep.nombre}</b>
          <br />
          {dep.continente}
        </p>
      </div>
    {/each}
  </div>
</main>

<style>
  .header {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    margin-top: 50px;
    margin-bottom: 80px;
  }
  .headline {
    font-size: 30px;
    line-height: 1.2;
    font-weight: normal;
    text-align: center;
    margin: 20px;
  }
  .bajada {
    font-size: 18px;
    font-weight: normal;
    text-align: center;
    margin: 10px;
  }
  .headline b {
    display: block;
  }
  .container {
    display: flex;
    justify-content: center;
    align-items: end;
    margin: auto;
    flex-wrap: wrap;
    max-width: 1000px;
    gap: 30px;
    margin-bottom: 100px;
  }
  .person-container {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    flex: 180px 0 0;
  }
  .person {
    width: 100px;
    height: 100px;
    border: 10px solid black;
    border-radius: 50%;
    box-sizing: border-box;
    background-color: pink;
  }
  .medal {
    width: 15px;
    height: 15px;
    border-radius: 50%;
    background-color: gold;
    margin: 5px 0;
  }
  .name {
    font-size: 14px;
    color: rgb(65, 65, 65);
    font-weight: normal;
    text-align: center;
    margin-top: 5px;
  }
</style>
