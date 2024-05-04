<script>
  import * as d3 from "d3";
  import { onMount } from "svelte";
  import Accordion from "./Accordion.svelte";

  /* Array donde guardaremos la data */
  let series = [];

  let circles_entry = {};

  let data_by_month = {};

  let blob_size = 80;

  let diference_from_each_circle = 50;

  let initial_size = 1000;

  // Ve o no series
  let watch_series = d3
    .scaleOrdinal()
    .domain(["Si", "No"])
    .range([true, false]);

  // Cantidad promedio de capítulos vistos
  let episodes = d3.scaleLinear().range([-5000, -1000]);

  // Géneros favoritos
  let genreColor = d3
    .scaleOrdinal()
    .domain([
      "Drama",
      "Comedia",
      "Misterio",
      "Crimen",
      "Romance",
      "Ciencia Ficción",
    ])
    .range([
      "rgb(0, 224, 255, 0.5)",
      "rgb(255, 245, 0, 0.5)",
      "rgb(255, 15, 15, 0.5)",
      "rgb(255, 15, 15, 0.5)",
      "rgb(255, 120, 209, 1)",
      "rgb(36, 255, 0, 0.5)",
    ])
    .unknown("rgb(128, 0, 255, 0.5)");

  // Plataformas
  let platforms = d3
    .scaleOrdinal()
    .domain(["Netflix", "Disney+", "Star+", "HBO", "Amazon Prime"])
    .range(["moon", "plus", "star", "asterisk", "cross"])
    .unknown("minus");

  // Rating de la serie favorita
  let rating = d3.scaleLinear().range([1, 4]).unknown(0);

  function divide_genres(s) {
    let res = [];
    let splittedGenres = s.split(", ");
    for (let i = 0; i < splittedGenres.length; i++) {
      res.push(genreColor(splittedGenres[i]));
    }
    res.sort(() => Math.random() - 0.5);
    return [...new Set(res)];
  }

  onMount(() => {
    d3.csv("./data/series.csv", d3.autoType).then((data) => {
      let minMaxEpisodes = d3.extent(data, (d) => d.episodes);
      episodes = episodes.domain(minMaxEpisodes);

      let minMaxRating = d3.extent(data, (d) => d.favRate);
      rating = rating.domain(minMaxRating);

      series = data;

      data_by_month = fill_data_by_month_dict(series);
      console.log(data_by_month);
      fill_circles_entry();
    });
  });

  function fill_circles_entry() {
    for (var clave in data_by_month) {
      circles_entry[clave] = transformar(
        data_by_month[clave].length,
        initial_size / 2 - (diference_from_each_circle * (clave - 1)) / 2
      );
    }
  }

  function fill_data_by_month_dict(list_data) {
    let res = {};
    for (let i = 0; i < list_data.length; i++) {
      if (list_data[i].watch == "Si") {
        let month_of_data_i = parseInt(list_data[i].birthDate.split("/")[1]);
        if (!(month_of_data_i in res)) {
          res[month_of_data_i] = [list_data[i]];
        } else {
          res[month_of_data_i].push(list_data[i]);
        }
      }
    }
    console.log(res);
    return res;
  }

  function divide_platforms(s, separador) {
    let res = [];
    let splittedPlatforms = s.split(", ");
    for (let i = 0; i < splittedPlatforms.length; i++) {
      res.push(platforms(splittedPlatforms[i]));
    }
    return [...new Set(res)];
  }

  // Función para posicionar n elementos equidistantes sobre un círculo
  function transformar(n, r) {
    // Número de vértices (triángulo)
    const radius = r; // Radio del círculo grande
    let smallCircles = [];
    for (let i = 0; i < n; i++) {
      const angle = ((2 * Math.PI) / n) * i;
      const x = radius * Math.cos(angle);
      const y = radius * Math.sin(angle);
      smallCircles.push({ x, y });
    }
    return smallCircles;
  }

  function calculateOffset(genreLength, index) {
    if (index == 0) {
      return 0;
    }
    return index * (100 / (genreLength - 1));
  }

  document.addEventListener("DOMContentLoaded", function () {
    var switch_button = document.getElementById("switch");
    function changeMode() {
      let container1 = document.querySelector(".container1");
      let container2 = document.querySelector(".container2");

      if (switch_button.checked) {
        container1.style.display = "none";
        container2.style.display = "flex";
      } else {
        container1.style.display = "flex";
        container2.style.display = "none";
      }
    }

    switch_button.addEventListener("click", changeMode);
  });
</script>

<main>
  <div class="header">
    <img src="/images/Movie.svg" width="100" alt="movie" />
    <h3 class="headline">
      <b>Gustos de series</b>
      Serie favorita, generos y plataformas
    </h3>
    <p class="bajada">Explorando los gustos a traves de datos</p>
    <div class="switch-container">
      <input id="switch" type="checkbox" />
      <label for="switch" class="label-switch"></label>
    </div>
  </div>

  <Accordion open={false}>
    <span slot="head">Lenguaje</span>
    <div slot="details">
      <p>These are the second details.</p>
      <p>This can naturally span multiple lines.</p>
    </div>
  </Accordion>

  <!--EEE-->
  <div class="container1">
    {#each series as entry, id}
      <div class="circle-entry">
        {#if watch_series(entry.watch)}
          {#each transformar(divide_platforms(entry.platforms, ", ").length, 100) as { x, y }, i}
            <div
              class="small-circle"
              style="transform: translate({x}px, {y}px);"
            >
              <img
                src="/images/{divide_platforms(entry.platforms, ', ')[i]}.svg"
                alt="plataforma"
                class="platform-img"
              />
            </div>
            <!-- Agregar más si necesitas múltiples círculos -->
          {/each}

          {#if parseInt(rating(entry.favRate)) == 1}
            <div class="data-entry">
              <svg
                viewBox="0 0 500 500"
                xmlns="http://www.w3.org/2000/svg"
                xmlns:xlink="http://www.w3.org/1999/xlink"
                width="{blob_size}%"
                id="blobSvg"
              >
                <defs>
                  <linearGradient
                    id="gradient{id}"
                    x1="0%"
                    y1="0%"
                    x2="0%"
                    y2="100%"
                  >
                    {#each divide_genres(entry.genres) as genreType, i}
                      <stop
                        offset="{calculateOffset(
                          divide_genres(entry.genres).length,
                          i
                        )}%"
                        stop-color={genreType}
                        stop-opacity="1"
                      />
                    {/each}
                  </linearGradient>
                </defs>
                <path id="blob1" fill="url(#gradient{id})">
                  <animate
                    attributeName="d"
                    dur="{Math.abs(episodes(entry.episodes))}ms"
                    repeatCount="indefinite"
                    values="
                    M394,270Q372,290,397,332.5Q422,375,396,395.5Q370,416,336.5,415Q303,414,276.5,400.5Q250,387,218.5,415Q187,443,170,413Q153,383,149,354.5Q145,326,91,322.5Q37,319,46,284.5Q55,250,81,227Q107,204,83.5,158Q60,112,115,126Q170,140,181.5,108Q193,76,221.5,87Q250,98,270.5,111.5Q291,125,332.5,102Q374,79,397,103Q420,127,436,155.5Q452,184,434,217Q416,250,394,270Z;
                    M471,287.5Q479,325,465,360.5Q451,396,412,408Q373,420,338.5,417.5Q304,415,277,421.5Q250,428,223,421.5Q196,415,172,402.5Q148,390,116,380.5Q84,371,51.5,348Q19,325,15.5,287.5Q12,250,52,224.5Q92,199,74.5,154.5Q57,110,111.5,122.5Q166,135,184,118Q202,101,226,76.5Q250,52,285.5,41Q321,30,341,63.5Q361,97,368,128Q375,159,424,168.5Q473,178,468,214Q463,250,471,287.5Z;
                    M393,271Q380,292,393.5,328Q407,364,394,397Q381,430,348,439.5Q315,449,282.5,429Q250,409,223.5,411Q197,413,183.5,386.5Q170,360,127,365.5Q84,371,59.5,345.5Q35,320,46,285Q57,250,77,225Q97,200,96,168.5Q95,137,117,117.5Q139,98,158,62.5Q177,27,213.5,75.5Q250,124,270,125.5Q290,127,322,117Q354,107,396,110.5Q438,114,417.5,158Q397,202,401.5,226Q406,250,393,271Z;
                    M394,270Q372,290,397,332.5Q422,375,396,395.5Q370,416,336.5,415Q303,414,276.5,400.5Q250,387,218.5,415Q187,443,170,413Q153,383,149,354.5Q145,326,91,322.5Q37,319,46,284.5Q55,250,81,227Q107,204,83.5,158Q60,112,115,126Q170,140,181.5,108Q193,76,221.5,87Q250,98,270.5,111.5Q291,125,332.5,102Q374,79,397,103Q420,127,436,155.5Q452,184,434,217Q416,250,394,270Z;
                    "
                  ></animate>
                </path>
              </svg>
              <p class="label-entry">
                {entry.fav}
              </p>
            </div>
          {:else if parseInt(rating(entry.favRate)) == 2}
            <div class="data-entry">
              <svg
                viewBox="0 0 500 500"
                xmlns="http://www.w3.org/2000/svg"
                xmlns:xlink="http://www.w3.org/1999/xlink"
                width="{blob_size}%"
                id="blobSvg"
              >
                <defs>
                  <linearGradient
                    id="gradient{id}"
                    gradientTransform="rotate(150, 0.5, 0.5)"
                    x1="50%"
                    y1="0%"
                    x2="50%"
                    y2="100%"
                  >
                    {#each divide_genres(entry.genres) as genreType, i}
                      <stop
                        offset="{calculateOffset(
                          divide_genres(entry.genres).length,
                          i
                        )}%"
                        stop-color={genreType}
                        stop-opacity="1"
                      />
                    {/each}
                  </linearGradient>
                </defs>
                <path id="blob2" fill="url(#gradient{id})">
                  <animate
                    attributeName="d"
                    dur="{Math.abs(episodes(entry.episodes))}ms"
                    repeatCount="indefinite"
                    values="
                    M423,322.5Q395,395,322.5,438Q250,481,207,408.5Q164,336,89.5,293Q15,250,82,199.5Q149,149,199.5,81Q250,13,333.5,48Q417,83,434,166.5Q451,250,423,322.5Z;
                    M376,286Q322,322,286,381.5Q250,441,212.5,383Q175,325,92.5,287.5Q10,250,50,170Q90,90,170,90Q250,90,306,114Q362,138,396,194Q430,250,376,286Z;
                    M379,327.5Q405,405,327.5,378.5Q250,352,207.5,343.5Q165,335,149.5,292.5Q134,250,109,167Q84,84,167,110.5Q250,137,331,112.5Q412,88,382.5,169Q353,250,379,327.5Z;
                    M349.5,298.5Q347,347,298.5,370Q250,393,192.5,379Q135,365,96.5,307.5Q58,250,95,191Q132,132,191,129.5Q250,127,324.5,114Q399,101,375.5,175.5Q352,250,349.5,298.5Z;
                    M423,322.5Q395,395,322.5,438Q250,481,207,408.5Q164,336,89.5,293Q15,250,82,199.5Q149,149,199.5,81Q250,13,333.5,48Q417,83,434,166.5Q451,250,423,322.5Z;
                    "
                  >
                  </animate>
                </path>
              </svg>
              <p class="label-entry">
                {entry.fav}
              </p>
            </div>
          {:else if parseInt(rating(entry.favRate)) == 3}
            <div class="data-entry">
              <svg
                viewBox="0 0 500 500"
                xmlns="http://www.w3.org/2000/svg"
                xmlns:xlink="http://www.w3.org/1999/xlink"
                width="{blob_size}%"
                id="blobSvg"
              >
                <defs>
                  <linearGradient
                    id="gradient{id}"
                    x1="0%"
                    y1="0%"
                    x2="0%"
                    y2="100%"
                  >
                    {#each divide_genres(entry.genres) as genreType, i}
                      <stop
                        offset="{calculateOffset(
                          divide_genres(entry.genres).length,
                          i
                        )}%"
                        stop-color={genreType}
                        stop-opacity="1"
                      />
                    {/each}
                  </linearGradient>
                </defs>
                <path id="blob3" fill="url(#gradient{id})">
                  <animate
                    attributeName="d"
                    dur="{Math.abs(episodes(entry.episodes))}ms"
                    repeatCount="indefinite"
                    values="
                    M430.5,301Q478,352,418,361Q358,370,333,399.5Q308,429,266.5,457.5Q225,486,180.5,467Q136,448,140,387.5Q144,327,124.5,304Q105,281,87,246Q69,211,92.5,182Q116,153,132.5,113.5Q149,74,189,61Q229,48,272.5,46.5Q316,45,348.5,75Q381,105,398,141Q415,177,399,213.5Q383,250,430.5,301Z;
                    M433,285.5Q409,321,399.5,363Q390,405,357,442Q324,479,276,467.5Q228,456,201,419Q174,382,134,373Q94,364,87,325Q80,286,89,252Q98,218,84,168.5Q70,119,118,111.5Q166,104,200,102.5Q234,101,271,86.5Q308,72,357,74Q406,76,423.5,120.5Q441,165,449,207.5Q457,250,433,285.5Z;
                    M417.69454,292.9459Q442.09289,335.8918,416.87377,371.68688Q391.65464,407.48197,352.86339,427.52186Q314.07213,447.56176,273.10437,434.82077Q232.13661,422.07979,205.96393,396.6623Q179.79126,371.24481,158.40601,351.48962Q137.02076,331.73443,100.65082,310.52186Q64.28088,289.30929,90.65464,255.72295Q117.02841,222.13661,119.98852,189.59672Q122.94862,157.05682,136.20109,116.46393Q149.45356,75.87104,192.5541,96.29508Q235.65464,116.71912,275.96393,81.53607Q316.27322,46.35301,365.27322,57.03989Q414.27322,67.72678,424.74098,117.60437Q435.20874,167.48197,414.25247,208.74098Q393.29619,250,417.69454,292.9459Z;
                    M460.5,289Q426,328,412.5,372Q399,416,346.5,400Q294,384,260,429Q226,474,178.5,465Q131,456,138,391.5Q145,327,127.5,303.5Q110,280,86.5,245Q63,210,105,192.5Q147,175,151.5,131Q156,87,195.5,97Q235,107,268,99.5Q301,92,346.5,92.5Q392,93,380.5,145Q369,197,432,223.5Q495,250,460.5,289Z;                  
                    M430.5,301Q478,352,418,361Q358,370,333,399.5Q308,429,266.5,457.5Q225,486,180.5,467Q136,448,140,387.5Q144,327,124.5,304Q105,281,87,246Q69,211,92.5,182Q116,153,132.5,113.5Q149,74,189,61Q229,48,272.5,46.5Q316,45,348.5,75Q381,105,398,141Q415,177,399,213.5Q383,250,430.5,301Z;
                    "
                  ></animate>
                </path>
              </svg>
              <p class="label-entry">
                {entry.fav}
              </p>
            </div>
          {:else if parseInt(rating(entry.favRate)) == 4}
            <div class="data-entry">
              <svg
                viewBox="0 0 500 500"
                xmlns="http://www.w3.org/2000/svg"
                xmlns:xlink="http://www.w3.org/1999/xlink"
                width="{blob_size}%"
                id="blobSvg"
              >
                <defs>
                  <linearGradient
                    id="gradient{id}"
                    x1="0%"
                    y1="0%"
                    x2="0%"
                    y2="100%"
                  >
                    {#each divide_genres(entry.genres) as genreType, i}
                      <stop
                        offset="{calculateOffset(
                          divide_genres(entry.genres).length,
                          i
                        )}%"
                        stop-color={genreType}
                        stop-opacity="1"
                      />
                    {/each}
                  </linearGradient>
                </defs>
                <path id="blob4" fill="url(#gradient{id})">
                  <animate
                    attributeName="d"
                    dur="{Math.abs(episodes(entry.episodes))}ms"
                    repeatCount="indefinite"
                    values="
                    M445.5,292.5Q397,335,376.5,384.5Q356,434,303,391Q250,348,196.5,392Q143,436,96,401Q49,366,108,308Q167,250,165,225Q163,200,164.5,152Q166,104,208,138Q250,172,291,140Q332,108,335,153.5Q338,199,416,224.5Q494,250,445.5,292.5Z;
                    M450.5,303Q434,356,364,341.5Q294,327,272,369Q250,411,199.5,418Q149,425,106.5,391Q64,357,103.5,303.5Q143,250,136,215Q129,180,150,146.5Q171,113,210.5,115.5Q250,118,310.5,79Q371,40,388,100.5Q405,161,436,205.5Q467,250,450.5,303Z;
                    M402.17929,286.8931Q377.81312,323.78619,354.26094,356.85775Q330.70876,389.9293,290.35438,388.10354Q250,386.27777,194.25589,415.00337Q138.51178,443.72896,97.5101,402.61195Q56.50842,361.49495,48.43686,305.74748Q40.36531,250,69.32491,206.0101Q98.28451,162.0202,142.13384,150.62037Q185.98317,139.22054,217.99158,96.39646Q250,53.57238,307.50337,52.0303Q365.00673,50.48822,383.50505,106.49327Q402.00337,162.49832,414.27441,206.24916Q426.54545,250,402.17929,286.8931Z;
                    M467.62404,309.41016Q455.89564,368.82031,382.0698,359.89357Q308.24396,350.96683,279.12198,373.43573Q250,395.90463,225.67622,364.49897Q201.35245,333.09331,154.53317,332.77506Q107.71389,332.45681,120.04975,291.2284Q132.38561,250,109.6662,203Q86.94679,156,105.79717,94.59987Q124.64755,33.19973,187.32378,78.94126Q250,124.68278,298.6009,103.55977Q347.2018,82.43676,331.44575,147.4689Q315.68971,212.50103,397.52108,231.25052Q479.35245,250,467.62404,309.41016Z;
                    M445.5,292.5Q397,335,376.5,384.5Q356,434,303,391Q250,348,196.5,392Q143,436,96,401Q49,366,108,308Q167,250,165,225Q163,200,164.5,152Q166,104,208,138Q250,172,291,140Q332,108,335,153.5Q338,199,416,224.5Q494,250,445.5,292.5Z;
                    "
                  >
                  </animate>
                </path>
              </svg>
              <p class="label-entry">
                {entry.fav}
              </p>
            </div>
          {/if}
        {:else}
          <div class="data-entry">
            <svg
              width="200"
              height="200"
              viewBox="0 0 918 918"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <g filter="url(#filter0_f_87_1461)">
                <path
                  id="blob0"
                  fill="#606060"
                  d="M334.766 210.456C472.274 141.598 639.567 197.25 708.425 334.758C777.283 472.267 721.631 639.56 584.123 708.418C446.614 777.276 279.321 721.624 210.463 584.115C141.605 446.607 197.257 279.314 334.766 210.456Z"
                >
                </path>
              </g>
              <defs>
                <filter
                  id="filter0_f_87_1461"
                  x="0.930054"
                  y="0.922607"
                  width="917.028"
                  height="917.029"
                  filterUnits="userSpaceOnUse"
                  color-interpolation-filters="sRGB"
                >
                  <feFlood flood-opacity="0" result="BackgroundImageFix" />
                  <feBlend
                    mode="normal"
                    in="SourceGraphic"
                    in2="BackgroundImageFix"
                    result="shape"
                  />
                  <feGaussianBlur
                    stdDeviation="60"
                    result="effect1_foregroundBlur_87_1461"
                  />
                </filter>
              </defs>
            </svg>
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

  <!-- Contenedor de las entidades -->
  <div class="container2">
    <div class="big-circle-container">
      {#each Object.keys(data_by_month) as month_i, i}
        <div
          class="big-circle"
          style="height: {initial_size -
            diference_from_each_circle * i}px; width: {initial_size -
            diference_from_each_circle * i}px;"
        >
          {#each data_by_month[month_i] as entry, id}
            <div class="circle-container">
              <div
                class="circle-entry"
                style="transform: translate({circles_entry[month_i][id]
                  .x}px, {circles_entry[month_i][id].y}px); animation: none;"
              >
                {#if watch_series(entry.watch)}
                  {#each transformar(divide_platforms(entry.platforms, ", ").length, 100) as { x, y }, i}
                    <div
                      class="small-circle"
                      style="transform: translate({x}px, {y}px);"
                    >
                      <img
                        src="/images/{platforms(
                          divide_platforms(entry.platforms, ', ')[i]
                        )}.svg"
                        alt="plataforma"
                        class="platform-img"
                      />
                    </div>
                    <!-- Agregar más si necesitas múltiples círculos -->
                  {/each}

                  {#if parseInt(rating(entry.favRate)) == 1}
                    <div class="data-entry">
                      <svg
                        viewBox="0 0 500 500"
                        xmlns="http://www.w3.org/2000/svg"
                        xmlns:xlink="http://www.w3.org/1999/xlink"
                        width="{blob_size}%"
                        id="blobSvg"
                      >
                        <defs>
                          <linearGradient
                            id="gradient{id}_{month_i}"
                            x1="0%"
                            y1="0%"
                            x2="0%"
                            y2="100%"
                          >
                            {#each divide_genres(entry.genres) as genreType, i}
                              <stop
                                offset="{calculateOffset(
                                  divide_genres(entry.genres).length,
                                  i
                                )}%"
                                stop-color={genreType}
                                stop-opacity="1"
                              />
                            {/each}
                          </linearGradient>
                        </defs>
                        <path id="blob1" fill="url(#gradient{id}_{month_i})">
                          <animate
                            attributeName="d"
                            dur="{Math.abs(episodes(entry.episodes))}ms"
                            repeatCount="indefinite"
                            values="
                      M394,270Q372,290,397,332.5Q422,375,396,395.5Q370,416,336.5,415Q303,414,276.5,400.5Q250,387,218.5,415Q187,443,170,413Q153,383,149,354.5Q145,326,91,322.5Q37,319,46,284.5Q55,250,81,227Q107,204,83.5,158Q60,112,115,126Q170,140,181.5,108Q193,76,221.5,87Q250,98,270.5,111.5Q291,125,332.5,102Q374,79,397,103Q420,127,436,155.5Q452,184,434,217Q416,250,394,270Z;
                      M471,287.5Q479,325,465,360.5Q451,396,412,408Q373,420,338.5,417.5Q304,415,277,421.5Q250,428,223,421.5Q196,415,172,402.5Q148,390,116,380.5Q84,371,51.5,348Q19,325,15.5,287.5Q12,250,52,224.5Q92,199,74.5,154.5Q57,110,111.5,122.5Q166,135,184,118Q202,101,226,76.5Q250,52,285.5,41Q321,30,341,63.5Q361,97,368,128Q375,159,424,168.5Q473,178,468,214Q463,250,471,287.5Z;
                      M393,271Q380,292,393.5,328Q407,364,394,397Q381,430,348,439.5Q315,449,282.5,429Q250,409,223.5,411Q197,413,183.5,386.5Q170,360,127,365.5Q84,371,59.5,345.5Q35,320,46,285Q57,250,77,225Q97,200,96,168.5Q95,137,117,117.5Q139,98,158,62.5Q177,27,213.5,75.5Q250,124,270,125.5Q290,127,322,117Q354,107,396,110.5Q438,114,417.5,158Q397,202,401.5,226Q406,250,393,271Z;
                      M394,270Q372,290,397,332.5Q422,375,396,395.5Q370,416,336.5,415Q303,414,276.5,400.5Q250,387,218.5,415Q187,443,170,413Q153,383,149,354.5Q145,326,91,322.5Q37,319,46,284.5Q55,250,81,227Q107,204,83.5,158Q60,112,115,126Q170,140,181.5,108Q193,76,221.5,87Q250,98,270.5,111.5Q291,125,332.5,102Q374,79,397,103Q420,127,436,155.5Q452,184,434,217Q416,250,394,270Z;
                      "
                          ></animate>
                        </path>
                      </svg>
                      <p class="label-entry">
                        {entry.fav}
                      </p>
                    </div>
                  {:else if parseInt(rating(entry.favRate)) == 2}
                    <div class="data-entry">
                      <svg
                        viewBox="0 0 500 500"
                        xmlns="http://www.w3.org/2000/svg"
                        xmlns:xlink="http://www.w3.org/1999/xlink"
                        width="{blob_size}%"
                        id="blobSvg"
                      >
                        <defs>
                          <linearGradient
                            id="gradient{id}_{month_i}"
                            gradientTransform="rotate(150, 0.5, 0.5)"
                            x1="50%"
                            y1="0%"
                            x2="50%"
                            y2="100%"
                          >
                            {#each divide_genres(entry.genres) as genreType, i}
                              <stop
                                offset="{calculateOffset(
                                  divide_genres(entry.genres).length,
                                  i
                                )}%"
                                stop-color={genreType}
                                stop-opacity="1"
                              />
                            {/each}
                          </linearGradient>
                        </defs>
                        <path id="blob2" fill="url(#gradient{id}_{month_i})">
                          <animate
                            attributeName="d"
                            dur="{Math.abs(episodes(entry.episodes))}ms"
                            repeatCount="indefinite"
                            values="
                      M423,322.5Q395,395,322.5,438Q250,481,207,408.5Q164,336,89.5,293Q15,250,82,199.5Q149,149,199.5,81Q250,13,333.5,48Q417,83,434,166.5Q451,250,423,322.5Z;
                      M376,286Q322,322,286,381.5Q250,441,212.5,383Q175,325,92.5,287.5Q10,250,50,170Q90,90,170,90Q250,90,306,114Q362,138,396,194Q430,250,376,286Z;
                      M379,327.5Q405,405,327.5,378.5Q250,352,207.5,343.5Q165,335,149.5,292.5Q134,250,109,167Q84,84,167,110.5Q250,137,331,112.5Q412,88,382.5,169Q353,250,379,327.5Z;
                      M349.5,298.5Q347,347,298.5,370Q250,393,192.5,379Q135,365,96.5,307.5Q58,250,95,191Q132,132,191,129.5Q250,127,324.5,114Q399,101,375.5,175.5Q352,250,349.5,298.5Z;
                      M423,322.5Q395,395,322.5,438Q250,481,207,408.5Q164,336,89.5,293Q15,250,82,199.5Q149,149,199.5,81Q250,13,333.5,48Q417,83,434,166.5Q451,250,423,322.5Z;
                      "
                          >
                          </animate>
                        </path>
                      </svg>
                      <p class="label-entry">
                        {entry.fav}
                      </p>
                    </div>
                  {:else if parseInt(rating(entry.favRate)) == 3}
                    <div class="data-entry">
                      <svg
                        viewBox="0 0 500 500"
                        xmlns="http://www.w3.org/2000/svg"
                        xmlns:xlink="http://www.w3.org/1999/xlink"
                        width="{blob_size}%"
                        id="blobSvg"
                      >
                        <defs>
                          <linearGradient
                            id="gradient{id}_{month_i}"
                            x1="0%"
                            y1="0%"
                            x2="0%"
                            y2="100%"
                          >
                            {#each divide_genres(entry.genres) as genreType, i}
                              <stop
                                offset="{calculateOffset(
                                  divide_genres(entry.genres).length,
                                  i
                                )}%"
                                stop-color={genreType}
                                stop-opacity="1"
                              />
                            {/each}
                          </linearGradient>
                        </defs>
                        <path id="blob3" fill="url(#gradient{id}_{month_i})">
                          <animate
                            attributeName="d"
                            dur="{Math.abs(episodes(entry.episodes))}ms"
                            repeatCount="indefinite"
                            values="
                      M430.5,301Q478,352,418,361Q358,370,333,399.5Q308,429,266.5,457.5Q225,486,180.5,467Q136,448,140,387.5Q144,327,124.5,304Q105,281,87,246Q69,211,92.5,182Q116,153,132.5,113.5Q149,74,189,61Q229,48,272.5,46.5Q316,45,348.5,75Q381,105,398,141Q415,177,399,213.5Q383,250,430.5,301Z;
                      M433,285.5Q409,321,399.5,363Q390,405,357,442Q324,479,276,467.5Q228,456,201,419Q174,382,134,373Q94,364,87,325Q80,286,89,252Q98,218,84,168.5Q70,119,118,111.5Q166,104,200,102.5Q234,101,271,86.5Q308,72,357,74Q406,76,423.5,120.5Q441,165,449,207.5Q457,250,433,285.5Z;
                      M417.69454,292.9459Q442.09289,335.8918,416.87377,371.68688Q391.65464,407.48197,352.86339,427.52186Q314.07213,447.56176,273.10437,434.82077Q232.13661,422.07979,205.96393,396.6623Q179.79126,371.24481,158.40601,351.48962Q137.02076,331.73443,100.65082,310.52186Q64.28088,289.30929,90.65464,255.72295Q117.02841,222.13661,119.98852,189.59672Q122.94862,157.05682,136.20109,116.46393Q149.45356,75.87104,192.5541,96.29508Q235.65464,116.71912,275.96393,81.53607Q316.27322,46.35301,365.27322,57.03989Q414.27322,67.72678,424.74098,117.60437Q435.20874,167.48197,414.25247,208.74098Q393.29619,250,417.69454,292.9459Z;
                      M460.5,289Q426,328,412.5,372Q399,416,346.5,400Q294,384,260,429Q226,474,178.5,465Q131,456,138,391.5Q145,327,127.5,303.5Q110,280,86.5,245Q63,210,105,192.5Q147,175,151.5,131Q156,87,195.5,97Q235,107,268,99.5Q301,92,346.5,92.5Q392,93,380.5,145Q369,197,432,223.5Q495,250,460.5,289Z;                  
                      M430.5,301Q478,352,418,361Q358,370,333,399.5Q308,429,266.5,457.5Q225,486,180.5,467Q136,448,140,387.5Q144,327,124.5,304Q105,281,87,246Q69,211,92.5,182Q116,153,132.5,113.5Q149,74,189,61Q229,48,272.5,46.5Q316,45,348.5,75Q381,105,398,141Q415,177,399,213.5Q383,250,430.5,301Z;
                      "
                          ></animate>
                        </path>
                      </svg>
                      <p class="label-entry">
                        {entry.fav}
                      </p>
                    </div>
                  {:else if parseInt(rating(entry.favRate)) == 4}
                    <div class="data-entry">
                      <svg
                        viewBox="0 0 500 500"
                        xmlns="http://www.w3.org/2000/svg"
                        xmlns:xlink="http://www.w3.org/1999/xlink"
                        width="{blob_size}%"
                        id="blobSvg"
                      >
                        <defs>
                          <linearGradient
                            id="gradient{id}_{month_i}"
                            x1="0%"
                            y1="0%"
                            x2="0%"
                            y2="100%"
                          >
                            {#each divide_genres(entry.genres) as genreType, i}
                              <stop
                                offset="{calculateOffset(
                                  divide_genres(entry.genres).length,
                                  i
                                )}%"
                                stop-color={genreType}
                                stop-opacity="1"
                              />
                            {/each}
                          </linearGradient>
                        </defs>
                        <path id="blob4" fill="url(#gradient{id}_{month_i})">
                          <animate
                            attributeName="d"
                            dur="{Math.abs(episodes(entry.episodes))}ms"
                            repeatCount="indefinite"
                            values="
                      M445.5,292.5Q397,335,376.5,384.5Q356,434,303,391Q250,348,196.5,392Q143,436,96,401Q49,366,108,308Q167,250,165,225Q163,200,164.5,152Q166,104,208,138Q250,172,291,140Q332,108,335,153.5Q338,199,416,224.5Q494,250,445.5,292.5Z;
                      M450.5,303Q434,356,364,341.5Q294,327,272,369Q250,411,199.5,418Q149,425,106.5,391Q64,357,103.5,303.5Q143,250,136,215Q129,180,150,146.5Q171,113,210.5,115.5Q250,118,310.5,79Q371,40,388,100.5Q405,161,436,205.5Q467,250,450.5,303Z;
                      M402.17929,286.8931Q377.81312,323.78619,354.26094,356.85775Q330.70876,389.9293,290.35438,388.10354Q250,386.27777,194.25589,415.00337Q138.51178,443.72896,97.5101,402.61195Q56.50842,361.49495,48.43686,305.74748Q40.36531,250,69.32491,206.0101Q98.28451,162.0202,142.13384,150.62037Q185.98317,139.22054,217.99158,96.39646Q250,53.57238,307.50337,52.0303Q365.00673,50.48822,383.50505,106.49327Q402.00337,162.49832,414.27441,206.24916Q426.54545,250,402.17929,286.8931Z;
                      M467.62404,309.41016Q455.89564,368.82031,382.0698,359.89357Q308.24396,350.96683,279.12198,373.43573Q250,395.90463,225.67622,364.49897Q201.35245,333.09331,154.53317,332.77506Q107.71389,332.45681,120.04975,291.2284Q132.38561,250,109.6662,203Q86.94679,156,105.79717,94.59987Q124.64755,33.19973,187.32378,78.94126Q250,124.68278,298.6009,103.55977Q347.2018,82.43676,331.44575,147.4689Q315.68971,212.50103,397.52108,231.25052Q479.35245,250,467.62404,309.41016Z;
                      M445.5,292.5Q397,335,376.5,384.5Q356,434,303,391Q250,348,196.5,392Q143,436,96,401Q49,366,108,308Q167,250,165,225Q163,200,164.5,152Q166,104,208,138Q250,172,291,140Q332,108,335,153.5Q338,199,416,224.5Q494,250,445.5,292.5Z;
                      "
                          >
                          </animate>
                        </path>
                      </svg>
                      <p class="label-entry">
                        {entry.fav}
                      </p>
                    </div>
                  {/if}
                {:else}
                  <div class="data-entry">
                    <svg
                      width="200"
                      height="200"
                      viewBox="0 0 918 918"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <g filter="url(#filter0_f_87_1461)">
                        <path
                          id="blob0"
                          fill="#606060"
                          d="M334.766 210.456C472.274 141.598 639.567 197.25 708.425 334.758C777.283 472.267 721.631 639.56 584.123 708.418C446.614 777.276 279.321 721.624 210.463 584.115C141.605 446.607 197.257 279.314 334.766 210.456Z"
                        >
                        </path>
                      </g>
                      <defs>
                        <filter
                          id="filter0_f_87_1461"
                          x="0.930054"
                          y="0.922607"
                          width="917.028"
                          height="917.029"
                          filterUnits="userSpaceOnUse"
                          color-interpolation-filters="sRGB"
                        >
                          <feFlood
                            flood-opacity="0"
                            result="BackgroundImageFix"
                          />
                          <feBlend
                            mode="normal"
                            in="SourceGraphic"
                            in2="BackgroundImageFix"
                            result="shape"
                          />
                          <feGaussianBlur
                            stdDeviation="60"
                            result="effect1_foregroundBlur_87_1461"
                          />
                        </filter>
                      </defs>
                    </svg>
                  </div>
                {/if}

                <!--<div class="small-circle">
            <img src="/images/minus.svg" alt="minus" class="minus">
          </div>
          
          <div class="small-circle">
            <img src="/images/minus.svg" alt="minus" class="minus">
          </div> -->
              </div>
            </div>
          {/each}
        </div>
      {/each}
    </div>
  </div>
</main>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins&display=swap");

  :global(body) {
    font-family: "Poppins", sans-serif;
  }

  .label-switch {
    display: inline-block;
    width: 65px;
    height: 33px;
    background-color: gray;
    border-radius: 100px;
    cursor: pointer;
    position: relative;
    transition: 0.2s;
  }

  .big-circle-container {
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    top: 500px;
  }

  .label-switch::after {
    content: "";
    display: block;
    width: 25px;
    height: 25px;
    background-color: white;
    border-radius: 100px;
    position: absolute;
    top: 4px;
    left: 4px;
    transition: 0.2s;
  }

  #switch:checked + .label-switch::after {
    left: 36px;
  }

  #switch:checked + .label-switch {
    background-color: aqua;
  }

  #switch {
    visibility: hidden;
    position: absolute;
  }

  .platform-img {
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

  #blob0 {
    transform-origin: center;
    animation: pulse 1s ease-in-out infinite alternate;
  }

  @keyframes pulse {
    100% {
      transform: scale(1.2);
    }
  }

  @keyframes scaleAnimation {
    0% {
      transform: scale(1);
    }
    100% {
      transform: scale(0.5);
    }
  }

  .data-entry {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .label-entry {
    opacity: 0;
    position: absolute;
  }

  .data-entry:hover svg {
    opacity: 0;
  }

  .data-entry:hover .label-entry {
    opacity: 1;
  }

  .big-circle {
    border-radius: 50%;
    height: 1000px;
    width: 1000px;
    display: flex;
    position: absolute;
    justify-content: center;
    align-items: center;
    -webkit-animation: spin 6s linear infinite;
    -moz-animation: spin 6s linear infinite;
    animation: spin 6s linear infinite;
    border: solid 5px red;
  }

  .circle-container {
    position: absolute;
  }

  .circle-entry {
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
    animation: float 10s ease-in-out infinite;
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
  .container1 {
    display: flex;
    justify-content: center;
    align-items: end;
    margin: auto;
    flex-wrap: wrap;
    max-width: 1000px;
    gap: 30px;
    margin-bottom: 100px;
  }

  .container2 {
    display: none;
    justify-content: center;
    align-items: end;
    margin: auto;
    flex-wrap: wrap;
    max-width: 1000px;
    gap: 30px;
    margin-bottom: 100px;
  }

  @-moz-keyframes spin {
    100% {
      -moz-transform: rotate(360deg);
    }
  }
  @-webkit-keyframes spin {
    100% {
      -webkit-transform: rotate(360deg);
    }
  }
  @keyframes spin {
    100% {
      -webkit-transform: rotate(360deg);
      transform: rotate(360deg);
    }
  }

  @keyframes float {
    0% {
      transform: translatey(0px);
    }
    50% {
      transform: translatey(-20px);
    }
    100% {
      transform: translatey(0px);
    }
  }
</style>
