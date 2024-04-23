<script>
  import * as d3 from "d3";
  import { onMount } from "svelte";

  /* Array donde guardaremos la data */
  let series = [];

  // Ve o no series
  let watch_series = d3
    .scaleOrdinal()
    .domain(["Si", "No"])
    .range([true, false]);

  // Cantidad promedio de capítulos vistos
  let episodes = d3.scaleLinear().range([100, 200]);

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
    .range(["blue", "yellow", "black", "black", "purple", "green"])
    .unknown("orange");

  // Plataformas
  let platforms = d3
    .scaleOrdinal()
    .domain(["Netflix", "Disney+", "Star+", "HBO", "Amazon Prime"])
    .range(["moon", "plus", "star", "asterisk", "cross"])
    .unknown("minus");

  // Rating de la serie favorita
  let rating = d3.scaleLinear().range([1, 5]).unknown(0);

  function divide_platforms(s) {
    let res = s.split(", ");
    return res;
  }

  function divide_genres(s) {
    let res = [];
    let splittedGenres = s.split(", ");
    for (let i = 0; i < splittedGenres.length; i++) {
      res.push(genreColor(splittedGenres[i]));
    }
    return [...new Set(res)];
  }

  onMount(() => {
    d3.csv("./data/series.csv", d3.autoType).then((data) => {
      let minMaxEpisodes = d3.extent(data, (d) => d.episodes);
      episodes = episodes.domain(minMaxEpisodes);

      let minMaxRating = d3.extent(data, (d) => d.favRate);
      rating = rating.domain(minMaxRating);

      series = data;
    });
  });

  // Función para posicionar n elementos equidistantes sobre un círculo
  function transformar(n) {
    // Número de vértices (triángulo)
    const radius = 100; // Radio del círculo grande
    let smallCircles = [];
    for (let i = 0; i < n; i++) {
      const angle = ((2 * Math.PI) / n) * i;
      const x = radius * Math.cos(angle);
      const y = radius * Math.sin(angle);
      smallCircles.push({ x, y });
    }
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

  <!-- Contenedor de las entidades -->
  <div class="container">
    {#each series as entry}
      <div class="circle-entry">
        {#if watch_series(entry.watch)}
          {#each transformar(divide_platforms(entry.platforms).length) as { x, y }, i}
            <div
              class="small-circle"
              style="transform: translate({x}px, {y}px);"
            >
              <img
                src="/images/{platforms(
                  divide_platforms(entry.platforms)[i]
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
                width="{episodes(entry.episodes)}px"
                height="{episodes(entry.episodes)}px"
                viewBox="0 0 721 794"
                fill="url(#blob1color)"
                xmlns="http://www.w3.org/2000/svg"
              >
                <g filter="url(#filter0_f_87_1376)">
                  <path
                    d="M312.097 345.072C382.533 345.072 439.633 287.972 439.633 217.536C439.633 147.1 382.533 90 312.097 90C241.661 90 184.561 147.1 184.561 217.536C184.561 287.972 241.661 345.072 312.097 345.072Z"
                  />
                  <path
                    d="M502.831 452.248C432.395 452.248 375.295 395.148 375.295 324.712C375.295 254.276 432.395 197.176 502.831 197.176C573.267 197.176 630.367 254.276 630.367 324.712C630.367 395.148 573.267 452.248 502.831 452.248Z"
                  />
                  <path
                    d="M460.354 494.725C389.917 494.725 332.818 437.625 332.818 367.189C332.818 296.753 389.917 239.653 460.354 239.653C530.79 239.653 587.89 296.753 587.89 367.189C587.89 437.625 530.79 494.725 460.354 494.725Z"
                  />
                  <path
                    d="M276.753 703.086C366.08 703.086 438.495 630.672 438.495 541.344C438.495 452.016 366.08 379.602 276.753 379.602C187.425 379.602 115.011 452.016 115.011 541.344C115.011 630.672 187.425 703.086 276.753 703.086Z"
                  />
                  <path
                    d="M326.921 628.975C416.248 628.975 488.663 556.561 488.663 467.233C488.663 377.905 416.248 305.491 326.921 305.491C237.593 305.491 165.179 377.905 165.179 467.233C165.179 556.561 237.593 628.975 326.921 628.975Z"
                  />
                  <path
                    d="M266.628 213.218C321.104 225.329 355.447 279.309 343.336 333.784C331.224 388.26 277.245 422.603 222.77 410.492C168.294 398.38 133.951 344.401 146.062 289.925C158.174 235.45 212.153 201.107 266.628 213.218Z"
                  />
                  <path
                    d="M260.44 230.852C159.681 238.188 83.6531 321.783 90.6266 417.567C97.6 513.352 184.934 585.054 285.693 577.718C386.452 570.382 462.48 486.787 455.507 391.003C448.533 295.218 361.199 223.516 260.44 230.852Z"
                  />
                  <path
                    d="M556.028 413.289C592.335 473.283 575.533 549.897 518.5 584.411C461.468 618.926 385.801 598.27 349.495 538.276C313.188 478.282 329.99 401.668 387.022 367.153C444.055 332.639 519.721 353.294 556.028 413.289Z"
                  />
                  <path
                    d="M353.876 503.88C441.089 503.88 511.789 433.18 511.789 345.967C511.789 258.754 441.089 188.054 353.876 188.054C266.663 188.054 195.963 258.754 195.963 345.967C195.963 433.18 266.663 503.88 353.876 503.88Z"
                  />
                  <path
                    d="M195.393 576.851C244.194 576.851 283.756 537.29 283.756 488.488C283.756 439.687 244.194 400.125 195.393 400.125C146.591 400.125 107.03 439.687 107.03 488.488C107.03 537.29 146.591 576.851 195.393 576.851Z"
                  />
                </g>
                <defs>
                  <radialGradient id="blob1color">
                    {#each divide_genres(entry.genres) as genreType, i}
                      {console.log(genreType)}
                      <stop
                        offset="{i *
                          (100 / divide_genres(entry.genres).length)}%"
                        stop-color={genreType}
                      />
                    {/each}
                  </radialGradient>
                  <filter
                    id="filter0_f_87_1376"
                    x="0.182861"
                    y="0"
                    width="720.184"
                    height="793.086"
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
                      stdDeviation="45"
                      result="effect1_foregroundBlur_87_1376"
                    />
                  </filter>
                </defs>
              </svg>
            </div>
          {:else if parseInt(rating(entry.favRate)) == 2}
            <div class="data-entry">
              <svg
                width="{episodes(entry.episodes)}px"
                height="{episodes(entry.episodes)}px"
                viewBox="0 0 800 800"
                fill="url(#blob2color)"
                xmlns="http://www.w3.org/2000/svg"
              >
                <mask
                  id="mask0_87_1388"
                  style="mask-type:luminance"
                  maskUnits="userSpaceOnUse"
                  x="0"
                  y="0"
                  width="800"
                  height="800"
                >
                  <path d="M800 0H0V800H800V0Z" fill="white" />
                </mask>
                <g mask="url(#mask0_87_1388)">
                  <g filter="url(#filter0_f_87_1388)">
                    <path
                      d="M407.945 549.497C348.827 588.958 268.913 573.025 229.452 513.907C189.99 454.79 205.924 374.876 265.041 335.414C324.158 295.953 404.072 311.887 443.534 371.004C482.996 430.121 467.062 510.035 407.945 549.497Z"
                    />
                    <path
                      d="M563.313 437.924C519.525 376.064 433.879 361.413 372.018 405.202C310.157 448.99 295.507 534.636 339.295 596.497C383.084 658.358 468.73 673.008 530.591 629.22C592.452 585.431 607.102 499.785 563.313 437.924Z"
                    />
                    <path
                      d="M217.239 286.258C239.307 343.152 303.318 371.384 360.212 349.316C417.105 327.248 445.337 263.237 423.27 206.344C401.202 149.45 337.191 121.218 280.297 143.286C223.404 165.353 195.172 229.364 217.239 286.258Z"
                    />
                    <path
                      d="M348.407 239.554C373.186 175.669 445.062 143.968 508.947 168.747C572.832 193.526 604.533 265.403 579.753 329.287C554.974 393.172 483.098 424.873 419.213 400.094C355.329 375.314 323.627 303.438 348.407 239.554Z"
                    />
                    <path
                      d="M332.834 465.642C414.67 508.879 516.061 477.588 559.299 395.752C602.536 313.916 571.245 212.525 489.409 169.288C407.573 126.051 306.181 157.341 262.944 239.177C219.707 321.013 250.998 422.405 332.834 465.642Z"
                    />
                  </g>
                </g>
                <defs>
                  <radialGradient id="blob2color">
                    {#each divide_genres(entry.genres) as genreType, i}
                      <stop
                        offset="{i *
                          (100 / divide_genres(entry.genres).length)}%"
                        stop-color={genreType}
                      />
                    {/each}
                  </radialGradient>
                  <filter
                    id="filter0_f_87_1388"
                    x="117.781"
                    y="45.778"
                    width="560.765"
                    height="698.675"
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
                      stdDeviation="45"
                      result="effect1_foregroundBlur_87_1388"
                    />
                  </filter>
                </defs>
              </svg>
            </div>
          {:else if parseInt(rating(entry.favRate)) > 2}
            <div class="data-entry">
              <img
                src="/images/Blob{parseInt(rating(entry.favRate))}.svg"
                alt="vacio"
                width="{episodes(entry.episodes)}px"
                height="{episodes(entry.episodes)}px"
              />
            </div>
          {/if}
        {:else}
          <div class="data-entry">
            <img
              src="/images/Blob{parseInt(rating(entry.favRate))}.svg"
              alt="vacio"
              width="150px"
              height="150px"
            />
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
</main>

<style>
  :global(body) {
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
    -webkit-animation: spin 4s linear infinite;
    -moz-animation: spin 4s linear infinite;
    animation: spin 4s linear infinite;
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
</style>
