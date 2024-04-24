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
          {:else if parseInt(rating(entry.favRate)) == 3}
            <div class="data-entry">
              <svg
                width="{episodes(entry.episodes)}px"
                height="{episodes(entry.episodes)}px"
                viewBox="0 0 800 800"
                xmlns="http://www.w3.org/2000/svg"
                fill="url(#blob3color)"
              >
                <mask
                  id="mask0_130_82"
                  style="mask-type:luminance"
                  maskUnits="userSpaceOnUse"
                  x="0"
                  y="0"
                  width="800"
                  height="800"
                >
                  <path d="M800 0H0V800H800V0Z" fill="white" />
                </mask>
                <g mask="url(#mask0_130_82)">
                  <g filter="url(#filter0_f_130_82)">
                    <path
                      d="M394.862 433.716C473.208 433.716 536.72 370.204 536.72 291.858C536.72 213.512 473.208 150 394.862 150C316.516 150 253.004 213.512 253.004 291.858C253.004 370.204 316.516 433.716 394.862 433.716Z"
                      fill="url(#blob3color)"
                    />
                    <path
                      d="M527.886 229.797C438.492 205.844 346.607 258.894 322.654 348.287C298.701 437.681 351.751 529.566 441.145 553.519C530.538 577.472 622.424 524.422 646.376 435.029C670.329 345.635 617.279 253.75 527.886 229.797Z"
                      fill="url(#blob3color)"
                    />
                    <path
                      d="M316.877 270.219C418.629 276.47 496.047 364.024 489.796 465.776C483.545 567.528 395.991 644.947 294.239 638.696C192.486 632.445 115.068 544.891 121.319 443.138C127.57 341.386 215.124 263.967 316.877 270.219Z"
                      fill="url(#blob3color)"
                    />
                    <path
                      d="M516.841 332.454C587.544 348.174 632.117 418.233 616.398 488.936C600.679 559.639 530.62 604.212 459.917 588.493C389.214 572.774 344.64 502.715 360.359 432.012C376.079 361.309 446.138 316.735 516.841 332.454Z"
                      fill="url(#blob3color)"
                    />
                    <path
                      d="M460.711 555.92C545.983 555.92 615.111 486.793 615.111 401.52C615.111 316.247 545.983 247.12 460.711 247.12C375.438 247.12 306.311 316.247 306.311 401.52C306.311 486.793 375.438 555.92 460.711 555.92Z"
                      fill="url(#blob3color)"
                    />
                  </g>
                </g>
                <defs>
                  <radialGradient id="blob3color">
                    {#each divide_genres(entry.genres) as genreType, i}
                      <stop
                        offset="{i *
                          (100 / divide_genres(entry.genres).length)}%"
                        stop-color={genreType}
                      />
                    {/each}
                  </radialGradient>
                  <filter
                    id="filter0_f_130_82"
                    x="20.9653"
                    y="50"
                    width="731.163"
                    height="689.049"
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
                      stdDeviation="50"
                      result="effect1_foregroundBlur_130_82"
                    />
                  </filter>
                </defs>
              </svg>
            </div>
          {:else if parseInt(rating(entry.favRate)) == 4}
            <div class="data-entry">
              <svg
                width="{episodes(entry.episodes)}px"
                height="{episodes(entry.episodes)}px"
                viewBox="0 0 933 884"
                xmlns="http://www.w3.org/2000/svg"
                fill="url(#blob4color)"
              >
                <g style="mix-blend-mode:darken">
                  <g opacity="0.7" filter="url(#filter0_f_130_13)">
                    <mask
                      id="mask0_130_13"
                      style="mask-type:luminance"
                      maskUnits="userSpaceOnUse"
                      x="100"
                      y="159"
                      width="522"
                      height="525"
                    >
                      <path
                        d="M621.441 683.259H100.551V159.426H621.441V683.259Z"
                        fill="white"
                      />
                    </mask>
                    <g mask="url(#mask0_130_13)">
                      <path
                        d="M289.373 303.482C393.657 303.482 478.195 388.498 478.195 493.371C478.195 598.243 393.657 683.259 289.373 683.259C185.09 683.259 100.551 598.243 100.551 493.371C100.551 388.498 185.09 303.482 289.373 303.482Z"
                        fill="url(#blob4color)"
                      />
                    </g>
                  </g>
                  <g opacity="0.7" filter="url(#filter1_f_130_13)">
                    <mask
                      id="mask1_130_13"
                      style="mask-type:luminance"
                      maskUnits="userSpaceOnUse"
                      x="278"
                      y="183"
                      width="574"
                      height="577"
                    >
                      <path
                        d="M796.518 759.39L851.146 238.446L333.129 183.509L278.5 704.453L796.518 759.39Z"
                        fill="white"
                      />
                    </mask>
                    <g mask="url(#mask1_130_13)">
                      <path
                        d="M455.782 387.459C444.845 491.754 520.051 585.217 623.76 596.215C727.468 607.214 820.406 531.583 831.343 427.289C842.28 322.994 767.074 229.531 663.365 218.532C559.657 207.534 466.719 283.165 455.782 387.459Z"
                        fill="url(#blob4color)"
                      />
                    </g>
                  </g>
                  <g opacity="0.7" filter="url(#filter2_f_130_13)">
                    <mask
                      id="mask2_130_13"
                      style="mask-type:luminance"
                      maskUnits="userSpaceOnUse"
                      x="16"
                      y="95"
                      width="582"
                      height="579"
                    >
                      <path
                        d="M597.58 551.51L474.889 95L16.9999 217.322L139.691 673.832L597.58 551.51Z"
                        fill="white"
                      />
                    </mask>
                    <g mask="url(#mask2_130_13)">
                      <path
                        d="M187.396 349.168C211.959 440.562 306.185 494.8 397.856 470.311C489.526 445.822 543.928 351.879 519.365 260.485C494.801 169.09 400.575 114.852 308.905 139.342C217.234 163.831 162.833 257.773 187.396 349.168Z"
                        fill="url(#blob4color)"
                      />
                    </g>
                  </g>
                  <g opacity="0.7" filter="url(#filter3_f_130_13)">
                    <mask
                      id="mask3_130_13"
                      style="mask-type:luminance"
                      maskUnits="userSpaceOnUse"
                      x="257"
                      y="0"
                      width="577"
                      height="580"
                    >
                      <path
                        d="M833.552 122.406L711.834 579.234L257.573 456.827L379.292 2.13191e-05L833.552 122.406Z"
                        fill="white"
                      />
                    </mask>
                    <g mask="url(#mask3_130_13)">
                      <path
                        d="M426.616 324.888C450.985 233.43 544.465 179.154 635.41 203.66C726.354 228.167 780.325 322.175 755.957 413.633C731.588 505.092 638.108 559.368 547.163 534.861C456.218 510.355 402.248 416.347 426.616 324.888Z"
                        fill="url(#blob4color)"
                      />
                    </g>
                  </g>
                  <g opacity="0.7" filter="url(#filter4_f_130_13)">
                    <mask
                      id="mask4_130_13"
                      style="mask-type:luminance"
                      maskUnits="userSpaceOnUse"
                      x="307"
                      y="290"
                      width="526"
                      height="529"
                    >
                      <path
                        d="M832.977 401.701L721.908 818.556L307.396 706.86L418.464 290.005L832.977 401.701Z"
                        fill="white"
                      />
                    </mask>
                    <g mask="url(#mask4_130_13)">
                      <path
                        d="M461.649 586.466C483.885 503.01 569.186 453.483 652.173 475.845C735.16 498.207 784.408 583.989 762.172 667.445C739.935 750.901 654.635 800.428 571.648 778.066C488.661 755.704 439.413 669.921 461.649 586.466Z"
                        fill="url(#blob4color)"
                      />
                    </g>
                  </g>
                  <g opacity="0.7" filter="url(#filter5_f_130_13)">
                    <mask
                      id="mask5_130_13"
                      style="mask-type:luminance"
                      maskUnits="userSpaceOnUse"
                      x="292"
                      y="141"
                      width="560"
                      height="556"
                    >
                      <path
                        d="M540.782 696.406L292.42 388.366L602.638 141.747L851 449.788L540.782 696.406Z"
                        fill="white"
                      />
                    </mask>
                    <g mask="url(#mask5_130_13)">
                      <path
                        d="M607.36 321.232C657.082 382.903 647.043 472.922 584.937 522.296C522.83 571.67 432.174 561.701 382.451 500.03C332.728 438.36 342.767 348.34 404.874 298.966C466.981 249.592 557.637 259.561 607.36 321.232Z"
                        fill="url(#blob4color)"
                      />
                    </g>
                  </g>
                  <g opacity="0.7" filter="url(#filter6_f_130_13)">
                    <mask
                      id="mask6_130_13"
                      style="mask-type:luminance"
                      maskUnits="userSpaceOnUse"
                      x="233"
                      y="209"
                      width="359"
                      height="428"
                    >
                      <path
                        d="M591.548 209.47H233.69V636.286H591.548V209.47Z"
                        fill="white"
                      />
                    </mask>
                    <g mask="url(#mask6_130_13)">
                      <path
                        d="M363.414 518.913C435.058 518.913 493.137 449.642 493.137 364.192C493.137 278.742 435.058 209.471 363.414 209.471C291.77 209.471 233.69 278.742 233.69 364.192C233.69 449.642 291.77 518.913 363.414 518.913Z"
                        fill="url(#blob4color)"
                      />
                    </g>
                  </g>
                  <g opacity="0.7" filter="url(#filter7_f_130_13)">
                    <mask
                      id="mask7_130_13"
                      style="mask-type:luminance"
                      maskUnits="userSpaceOnUse"
                      x="396"
                      y="418"
                      width="291"
                      height="337"
                    >
                      <path
                        d="M686.104 754.542L677.205 454.335L396.999 418.638L405.897 718.845L686.104 754.542Z"
                        fill="white"
                      />
                    </mask>
                    <g mask="url(#mask7_130_13)">
                      <path
                        d="M477.283 537.282C479.064 597.385 525.985 651.901 582.083 659.047C638.181 666.194 682.213 623.265 680.432 563.163C678.65 503.061 631.73 448.544 575.632 441.398C519.533 434.251 475.501 477.18 477.283 537.282Z"
                        fill="url(#blob4color)"
                      />
                    </g>
                  </g>
                  <g opacity="0.7" filter="url(#filter8_f_130_13)">
                    <mask
                      id="mask8_130_13"
                      style="mask-type:luminance"
                      maskUnits="userSpaceOnUse"
                      x="117"
                      y="232"
                      width="375"
                      height="378"
                    >
                      <path
                        d="M491.734 440.972L287.509 609.73L117.015 401.065L321.24 232.307L491.734 440.972Z"
                        fill="white"
                      />
                    </mask>
                    <g mask="url(#mask8_130_13)">
                      <path
                        d="M237.931 397.274C278.817 363.488 339.633 369.965 373.766 411.74C407.9 453.515 402.426 514.77 361.539 548.556C320.653 582.341 259.837 575.865 225.704 534.089C191.57 492.314 197.045 431.06 237.931 397.274Z"
                        fill="url(#blob4color)"
                      />
                    </g>
                  </g>
                </g>
                <defs>
                  <radialGradient id="blob4color">
                    {#each divide_genres(entry.genres) as genreType, i}
                      <stop
                        offset="{i *
                          (100 / divide_genres(entry.genres).length)}%"
                        stop-color={genreType}
                      />
                    {/each}
                  </radialGradient>
                  <filter
                    id="filter0_f_130_13"
                    x="0.300072"
                    y="203.231"
                    width="578.146"
                    height="580.28"
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
                      stdDeviation="50.1256"
                      result="effect1_foregroundBlur_130_13"
                    />
                  </filter>
                  <filter
                    id="filter1_f_130_13"
                    x="354.474"
                    y="117.219"
                    width="578.177"
                    height="580.311"
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
                      stdDeviation="50.1256"
                      result="effect1_foregroundBlur_130_13"
                    />
                  </filter>
                  <filter
                    id="filter2_f_130_13"
                    x="81.2467"
                    y="33.2102"
                    width="544.267"
                    height="543.232"
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
                      stdDeviation="50.1256"
                      result="effect1_foregroundBlur_130_13"
                    />
                  </filter>
                  <filter
                    id="filter3_f_130_13"
                    x="320.514"
                    y="97.5249"
                    width="541.545"
                    height="543.472"
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
                      stdDeviation="50.1256"
                      result="effect1_foregroundBlur_130_13"
                    />
                  </filter>
                  <filter
                    id="filter4_f_130_13"
                    x="356.058"
                    y="370.225"
                    width="511.704"
                    height="513.462"
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
                      stdDeviation="50.1256"
                      result="effect1_foregroundBlur_130_13"
                    />
                  </filter>
                  <filter
                    id="filter5_f_130_13"
                    x="250.595"
                    y="167.332"
                    width="488.621"
                    height="486.599"
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
                      stdDeviation="50.1256"
                      result="effect1_foregroundBlur_130_13"
                    />
                  </filter>
                  <filter
                    id="filter6_f_130_13"
                    x="198.69"
                    y="174.471"
                    width="329.447"
                    height="379.441"
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
                      stdDeviation="17.5"
                      result="effect1_foregroundBlur_130_13"
                    />
                  </filter>
                  <filter
                    id="filter7_f_130_13"
                    x="442.23"
                    y="405.62"
                    width="273.254"
                    height="289.204"
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
                      stdDeviation="17.5"
                      result="effect1_foregroundBlur_130_13"
                    />
                  </filter>
                  <filter
                    id="filter8_f_130_13"
                    x="168.294"
                    y="340.629"
                    width="262.882"
                    height="264.571"
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
                      stdDeviation="17.5"
                      result="effect1_foregroundBlur_130_13"
                    />
                  </filter>
                </defs>
              </svg>
            </div>
          {:else if parseInt(rating(entry.favRate)) == 5}
            <div class="data-entry">
              <svg
                width="{episodes(entry.episodes)}px"
                height="{episodes(entry.episodes)}px"
                viewBox="0 0 673 675"
                xmlns="http://www.w3.org/2000/svg"
                fill="url(#blob5color)"
              >
                <g filter="url(#filter0_f_87_1455)">
                  <path
                    d="M261.238 517.05C355.731 517.05 432.333 440.443 432.333 345.943C432.333 251.442 355.731 174.835 261.238 174.835C166.746 174.835 90.144 251.442 90.144 345.943C90.144 440.443 166.746 517.05 261.238 517.05Z"
                    fill="url(#blob5color)"
                  />
                  <path
                    d="M287.798 471.286C273.386 395.468 323.161 322.322 398.973 307.909C474.785 293.497 547.925 343.276 562.336 419.093C576.748 494.911 526.973 568.057 451.161 582.469C375.35 596.882 302.209 547.103 287.798 471.286Z"
                    fill="url(#blob5color)"
                  />
                  <path
                    d="M213.029 110.148C305.828 63.6743 418.729 101.235 465.199 194.042C511.668 286.849 474.111 399.758 381.311 446.231C288.511 492.705 175.611 455.144 129.141 362.338C82.6713 269.531 120.229 156.621 213.029 110.148Z"
                    fill="url(#blob5color)"
                  />
                  <path
                    d="M362.307 269.728C356.242 330.404 400.509 384.509 461.18 390.575C521.852 396.64 575.952 352.37 582.018 291.694C588.083 231.018 543.816 176.913 483.145 170.847C422.473 164.781 368.373 209.052 362.307 269.728Z"
                    fill="url(#blob5color)"
                  />
                </g>
                <defs>
                  <radialGradient id="blob5color">
                    {#each divide_genres(entry.genres) as genreType, i}
                      <stop
                        offset="{i *
                          (100 / divide_genres(entry.genres).length)}%"
                        stop-color={genreType}
                      />
                    {/each}
                  </radialGradient>
                  <filter
                    id="filter0_f_87_1455"
                    x="0.144043"
                    y="0.215332"
                    width="672.429"
                    height="674.738"
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
                      result="effect1_foregroundBlur_87_1455"
                    />
                  </filter>
                </defs>
              </svg>
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
