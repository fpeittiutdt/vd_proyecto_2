<script>
  import * as d3 from "d3";
  import { onMount } from "svelte";

  /* Array donde guardaremos la data */
  let series = [];

  // Ve o no series
  let watch_series = d3.scaleOrdinal().domain(["Si", "No"]).range([true, false])

  // Cantidad promedio de capítulos vistos
  let episodes = d3.scaleLinear().range([100, 200]);

  // Géneros favoritos
  let genres = d3.scaleOrdinal().domain(["Drama", "Comedia", "Misterio", "Crimen", "Romance", "Ciencia Ficción"]).range(["blue", "yellow", "black", "black", "purple", "green"]).unknown("orange")

  // Plataformas
  let platforms = d3.scaleOrdinal().domain(["Netflix", "Disney+", "Star+", "HBO", "Amazon Prime"]).range(["moon", "plus", "star", "asterisk", "cross"]).unknown("minus")
  
  // Rating de la serie favorita
  let rating = d3.scaleLinear().range([1,5]).unknown(0);

  function divide_platforms(s){
    let res = s.split(", ");
    console.log(res);
    return res;
  }

  onMount(() => {
    d3.csv("./data/series.csv", d3.autoType).then((data) => {

      let minMaxEpisodes = d3.extent(data, (d) => d.episodes);
      episodes = episodes.domain(minMaxEpisodes);

      let minMaxRating = d3.extent(data, (d) => d.favRate);
      rating = rating.domain(minMaxRating);

      series = data;
      console.log(rating(8.095));
    });

  });


  function transformar(n){ // Número de vértices (triángulo)
    const radius = 100; // Radio del círculo grande
    let smallCircles = [];
    for (let i = 0; i < n; i++) {
      const angle = (2 * Math.PI / n) * i;
      const x = radius * Math.cos(angle);
      const y = radius * Math.sin(angle);
      smallCircles.push({ x, y });
    }
    console.log(smallCircles);
    return smallCircles;
  }

</script>

<main>
  <div class="header">
    <img src="/images/Movie.svg" width="100" alt="movie" />
    <h3 class="headline">
      <b>Gustos de series</b>
      Serie favorita, generos y plataformas
    </h3>
    <p class="bajada">Explorando los gustos a traves de datos</p>
  </div>

  <!-- Conedor de las entidades -->
  <div class="container">
    {#each series as entry}
    <div class="circle-entry">
        {#if watch_series(entry.watch)}
          {#each transformar(divide_platforms(entry.platforms).length) as {x, y}, i}
          <div class="small-circle" style="transform: translate({x}px, {y}px);">
            <img src="/images/{platforms(divide_platforms(entry.platforms)[i])}.svg" alt="plataforma" class="platform-img">
          </div> <!-- Agregar más si necesitas múltiples círculos -->
          {/each}
        <div class="data-entry">
          <img src="/images/Blob{parseInt(rating(entry.favRate))}.svg" alt="vacio" width = "{episodes(entry.episodes)}px" height =  "{episodes(entry.episodes)}px">
        </div>

      {:else}
        <div class="data-entry">
          <img src="/images/Blob{parseInt(rating(entry.favRate))}.svg" alt="vacio" width = "150px" height =  "150px">
        </div>
      {/if}
      
      
      <!--<div class="small-circle">
        <img src="/images/minus.svg" alt="minus" class="minus">
      </div>
      
      <div class="small-circle">
        <img src="/images/minus.svg" alt="minus" class="minus">
      </div> -->
    
    </div>    
    {/each}
  </div>
  <!--  <div class="container">
    Iteramos la data para visualizar c/ entidad
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
  </div> -->
</main>

<style>

  :global(body){
  }


  .platform-img{
    /*position: relative; para centrar "manualmente"
    left: 20%;
    top: 10%;*/
    width: 20px;
  }

  .small-circle {
  width: 40px;
  height: 40px;
  background-color: black; /* Elige el color que prefieras */
  border-radius: 50%;
  position: absolute;
  display: flex;
  justify-content: center;
  /* Ajustar top y left para posicionar correctamente */
  }

/*.small-circle:nth-child(2) {
  transform: translate(-50%, -50%) rotate(-90deg) translate(100px) rotate(90deg);
}
.small-circle:nth-child(3) {
  transform: translate(-200%, -100%) rotate(90deg) translate(100px) rotate(-90deg);
}
/* Añadir más según sea necesario, ajustando los grados de rotación */


  .circle-entry{
    position: relative;
    height: 200px;
    border: 1.5px black solid;
    width: 200px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 50%;
    margin-bottom: 50px;
    margin-right: 20px;
    -webkit-animation:spin 4s linear infinite;
    -moz-animation:spin 4s linear infinite;
    animation:spin 4s linear infinite;
  }

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
  @-moz-keyframes spin { 
    100% { -moz-transform: rotate(360deg); } 
  }
  @-webkit-keyframes spin { 
    100% { -webkit-transform: rotate(360deg); } 
  }
  @keyframes spin { 
    100% { 
        -webkit-transform: rotate(360deg); 
        transform:rotate(360deg); 
    } 
  }

</style>
