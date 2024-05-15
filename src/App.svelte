<script>
  import * as d3 from "d3";
  import { onMount } from "svelte";
  import Accordion from "./Accordion.svelte";

  /* Array donde guardaremos la data */
  let series = [];

  let circles_entry = {};

  let circles_entry2 = {};

  let data_by_semester = {};

  let data_by_month = [];

  let dataByZodiac = [];

  let valoresZodiac = [
    "Sagitario",
    "Piscis",
    "Aries",
    "Tauro",
    "Géminis",
    "Escorpio",
    "Virgo",
    "Libra",
    "Cáncer",
    "Leo",
    "Acuario",
    "Capricornio",
  ];

  let genreList = [
    "Drama",
    "Comedia",
    "Misterio",
    "Romance",
    "Ciencia Ficción",
    "Otros",
  ];

  let plataformsList = [
    "Netflix",
    "Disney+",
    "Star+",
    "HBO",
    "Amazon Prime",
    "Otros",
  ];

  let blob_size = 80;

  let diference_from_each_circle = 50;

  let circle_entry_size = 200;

  let initial_size = 700;

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
      "rgb(255, 120, 209, 0.8)",
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

      series.sort((valor1, valor2) => {
        if (valor1.fav < valor2.fav) {
          return -1;
        }
        if (valor1.fav > valor2.fav) {
          return 1;
        }
        return 0;
      });

      console.log(series);

      dataByZodiac = fillDataByZodiac(series, null);
      data_by_semester = fill_data_by_time_interval_dict(series, 3);
      data_by_month = fill_data_month(series, 2);
      fill_circles_entry(circles_entry, data_by_semester);
      //fill_circles_entry(circles_entry2, data_by_month);
    });
  });

  function fill_circles_entry(a_llenar, data) {
    for (var clave in data) {
      a_llenar[clave] = transformar(data[clave].length, initial_size / 2);
    }
  }

  function mapZodiac(date) {
    let splittedDate = date.split("/");
    let zodiac = "";
    if (splittedDate[1] == "12") {
      if (Number(splittedDate[0]) < 22) zodiac = "Sagitario";
      else zodiac = "Acuario";
    } else if (splittedDate[1] == "01") {
      if (Number(splittedDate[0]) < 20) zodiac = "Capricornio";
      else zodiac = "Acuario";
    } else if (splittedDate[1] == "02") {
      if (Number(splittedDate[0]) < 19) zodiac = "Acuario";
      else zodiac = "Piscis";
    } else if (splittedDate[1] == "03") {
      if (Number(splittedDate[0]) < 21) zodiac = "Piscis";
      else zodiac = "Aries";
    } else if (splittedDate[1] == "04") {
      if (Number(splittedDate[0]) < 20) zodiac = "Aries";
      else zodiac = "Tauro";
    } else if (splittedDate[1] == "05") {
      if (Number(splittedDate[0]) < 21) zodiac = "Tauro";
      else zodiac = "Géminis";
    } else if (splittedDate[1] == "06") {
      if (Number(splittedDate[0]) < 21) zodiac = "Géminis";
      else zodiac = "Cáncer";
    } else if (splittedDate[1] == "07") {
      if (Number(splittedDate[0]) < 23) zodiac = "Cáncer";
      else zodiac = "Leo";
    } else if (splittedDate[1] == "08") {
      if (Number(splittedDate[0]) < 23) zodiac = "Leo";
      else zodiac = "Virgo";
    } else if (splittedDate[1] == "09") {
      if (Number(splittedDate[0]) < 23) zodiac = "Virgo";
      else zodiac = "Libra";
    } else if (splittedDate[1] == "10") {
      if (Number(splittedDate[0]) < 23) zodiac = "Libra";
      else zodiac = "Escorpio";
    } else if (splittedDate[1] == "11") {
      if (Number(splittedDate[0]) < 22) zodiac = "Escorpio";
      else zodiac = "Sagitario";
    }

    return zodiac;
  }

  function fillDataByZodiac(listData, filtro) {
    let res = [];
    for (let i = 0; i < listData.length; i++) {
      if (listData[i].watch == "Si") {
        let zodiac = mapZodiac(listData[i].birthDate);
        if (filtro != null) {
          console.log(zodiac == "Géminis");
          if (zodiac === filtro) {
            res.push(listData[i]);
          }
        } else {
          res.push(listData[i]);
        }
      }
    }
    return res;
  }

  function fill_data_month(list_data, value_month) {
    let res = [];
    for (let i = 0; i < list_data.length; i++) {
      if (list_data[i].watch == "Si") {
        let cumple = list_data[i].birthDate.split("/")[1];
        if (cumple == value_month) {
          res.push(list_data[i]);
        }
      }
    }
    return res;
  }

  function fill_data_by_time_interval_dict(list_data, time_interval) {
    let res = {};
    for (let i = 0; i < list_data.length; i++) {
      if (list_data[i].watch == "Si") {
        let month_of_data_i = Math.ceil(
          list_data[i].birthDate.split("/")[1] / time_interval
        );
        if (!(month_of_data_i in res)) {
          res[month_of_data_i] = [list_data[i]];
        } else {
          res[month_of_data_i].push(list_data[i]);
        }
      } else {
        if (!(list_data.length in res)) {
          res[list_data.length] = [list_data[i]];
        } else {
          res[list_data.length].push(list_data[i]);
        }
      }
    }
    return res;
  }

  function divide_platforms(s) {
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

  let ultimo_id = null;

  function changeFilter(filtro, id) {
    // Obtener todos los botones
    const botones = document.querySelectorAll(".botones-filtro");
    const boton_id = document.getElementById(id);
    if (ultimo_id != null) {
      let boton_ultimo = document.getElementById("activado");
      boton_ultimo.id = ultimo_id;
    }
    ultimo_id = boton_id.id;
    boton_id.id = "activado";
    console.log(boton_id.id);
    console.log("aaaaa");
    console.log(series);
    dataByZodiac = fillDataByZodiac(series, filtro);
    console.log(dataByZodiac);
  }

  document.addEventListener("DOMContentLoaded", function () {
    var switch_button = document.getElementById("switch");
    function changeMode() {
      let container1 = document.querySelector(".container1");
      let container2 = document.querySelector(".container2");
      if (switch_button.checked) {
        container1.style.display = "none";
        container2.style.display = "flex";
        console.log(circles_entry);
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
      <p class="bajada">Explorando la clase a través de datos</p>
    </h3>

    <div class="switch-container">
      <input id="switch" type="checkbox" />
      <label for="switch" class="label-switch"></label>
    </div>
  </div>

  <Accordion open={false}>
    <button slot="head" class="accordion-button">Lenguaje visual</button>
    <div slot="details" style="font-size: x-large;">
      <p>
        A continuación se detallan las distintas variables que entran en juego<br
        />
        en la generación de cada observación para una persona cualquiera.
      </p>
      <p class="quality">Forma del núcleo</p>
      <p>Si no mira series, se observa un núcleo genérico sin estilizar.</p>
      <div
        style="display: flex;
      justify-content: center;"
      >
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
      <div>
        <p>
          Si mira series, la forma del núcleo dependerá
          <br />del rating de su serie favorita en
          <a href="https://www.imdb.com/?ref_=nv_home">IMDb</a>.
        </p>
      </div>
      <div
        style="display:flex; 
        justify-content:  space-evenly;"
      >
        <div
          style="display: flex; align-items: center; flex-direction: column;"
        >
          <svg
            viewBox="0 0 500 500"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            width="{blob_size}%"
            id="blobSvg"
          >
            <path id="blob1" fill="black">
              <animate
                attributeName="d"
                repeatCount="indefinite"
                dur="5000ms"
                values="
                M394,270Q372,290,397,332.5Q422,375,396,395.5Q370,416,336.5,415Q303,414,276.5,400.5Q250,387,218.5,415Q187,443,170,413Q153,383,149,354.5Q145,326,91,322.5Q37,319,46,284.5Q55,250,81,227Q107,204,83.5,158Q60,112,115,126Q170,140,181.5,108Q193,76,221.5,87Q250,98,270.5,111.5Q291,125,332.5,102Q374,79,397,103Q420,127,436,155.5Q452,184,434,217Q416,250,394,270Z;
                M471,287.5Q479,325,465,360.5Q451,396,412,408Q373,420,338.5,417.5Q304,415,277,421.5Q250,428,223,421.5Q196,415,172,402.5Q148,390,116,380.5Q84,371,51.5,348Q19,325,15.5,287.5Q12,250,52,224.5Q92,199,74.5,154.5Q57,110,111.5,122.5Q166,135,184,118Q202,101,226,76.5Q250,52,285.5,41Q321,30,341,63.5Q361,97,368,128Q375,159,424,168.5Q473,178,468,214Q463,250,471,287.5Z;
                M393,271Q380,292,393.5,328Q407,364,394,397Q381,430,348,439.5Q315,449,282.5,429Q250,409,223.5,411Q197,413,183.5,386.5Q170,360,127,365.5Q84,371,59.5,345.5Q35,320,46,285Q57,250,77,225Q97,200,96,168.5Q95,137,117,117.5Q139,98,158,62.5Q177,27,213.5,75.5Q250,124,270,125.5Q290,127,322,117Q354,107,396,110.5Q438,114,417.5,158Q397,202,401.5,226Q406,250,393,271Z;
                M394,270Q372,290,397,332.5Q422,375,396,395.5Q370,416,336.5,415Q303,414,276.5,400.5Q250,387,218.5,415Q187,443,170,413Q153,383,149,354.5Q145,326,91,322.5Q37,319,46,284.5Q55,250,81,227Q107,204,83.5,158Q60,112,115,126Q170,140,181.5,108Q193,76,221.5,87Q250,98,270.5,111.5Q291,125,332.5,102Q374,79,397,103Q420,127,436,155.5Q452,184,434,217Q416,250,394,270Z;
                "
              ></animate>
            </path>
          </svg>
          <p>Entre 6 y 7.5</p>
        </div>

        <div
          style="display: flex; align-items: center; flex-direction: column;"
        >
          <svg
            viewBox="0 0 500 500"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            width="{blob_size}%"
            id="blobSvg"
          >
            <defs> </defs>
            <path id="blob2" fill="black">
              <animate
                attributeName="d"
                repeatCount="indefinite"
                dur="5000ms"
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
          <p>Entre 7.51 y 8.5</p>
        </div>
        <div
          style="display: flex; align-items: center; flex-direction: column;"
        >
          <svg
            viewBox="0 0 500 500"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            width="{blob_size}%"
            id="blobSvg"
          >
            <path id="blob3" fill="black">
              <animate
                attributeName="d"
                repeatCount="indefinite"
                dur="5000ms"
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
          <p>Entre 8.51 y 9.5</p>
        </div>
        <div
          style="display: flex; align-items: center; flex-direction: column;"
        >
          <svg
            viewBox="0 0 500 500"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            width="{blob_size}%"
            id="blobSvg"
          >
            <path id="blob4" fill="black">
              <animate
                attributeName="d"
                repeatCount="indefinite"
                dur="5000ms"
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
          <p>Entre 9.51 y 10</p>
        </div>
      </div>
      <p class="quality">Colores</p>
      <p>
        Se arma una combinación para el color del núcleo<br /> según sus géneros
        favoritos.
      </p>
      <div
        style="display: flex;
      justify-content: space-evenly;"
      >
        {#each genreList as genre}
          <div
            style="display: flex; align-items: center; flex-direction: column;"
          >
            <div
              class="colores-lenguaje"
              style="background-color: {genreColor(genre)};"
            ></div>
            <p style="font-weight: bold;">
              {genre === "Misterio" ? "Misterio/Crimen" : genre}
            </p>
          </div>
        {/each}
      </div>

      <p class="quality">Orbitales</p>
      <p>
        Cada orbital indica que la persona usa alguna/s <br />de las siguientes
        plataformas para ver series.
      </p>
      <div style="display: flex; justify-content: space-evenly;">
        {#each plataformsList as platform}
          <div
            style="display: flex; align-items: center; flex-direction: column;"
          >
            <div class="small-circle" style="position: relative;">
              <img
                src="/images/{platforms(platform)}.svg"
                alt="plataforma"
                class="platform-img"
              />
            </div>
            <p style="font-weight: bold;">{platform}</p>
          </div>
        {/each}
      </div>
      <p class="quality">Velocidad de ondulación del núcleo</p>
      <p>
        En función de cuántos episodios ve a la semana, <br />el núcleo cambia
        de forma más rápidamente, por ejemplo:
      </p>
      <div style="display:flex; justify-content: space-evenly;">
        <div
          style="display: flex; align-items: center; flex-direction: column;"
        >
          <svg
            viewBox="0 0 500 500"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            width="{blob_size}%"
            id="blobSvg"
          >
            <path id="blob4" fill="black">
              <animate
                attributeName="d"
                repeatCount="indefinite"
                dur="{Math.abs(episodes(1))}ms"
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
          <p style="font-weight: bold;">1 episodio a la semana</p>
        </div>
        <div
          style="display: flex; align-items: center; flex-direction: column;"
        >
          <svg
            viewBox="0 0 500 500"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            width="{blob_size}%"
            id="blobSvg"
          >
            <path id="blob4" fill="black">
              <animate
                attributeName="d"
                repeatCount="indefinite"
                dur="{Math.abs(episodes(7))}ms"
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
          <p style="font-weight: bold;">7 episodios a la semana</p>
        </div>
        <div
          style="display: flex; align-items: center; flex-direction: column;"
        >
          <svg
            viewBox="0 0 500 500"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            width="{blob_size}%"
            id="blobSvg"
          >
            <path id="blob4" fill="black">
              <animate
                attributeName="d"
                repeatCount="indefinite"
                dur="{Math.abs(episodes(14))}ms"
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
          <p style="font-weight: bold;">14 episodios a la semana</p>
        </div>
      </div>
      <p class="quality">Ejemplo</p>
      <p>
        Para una persona que mira 11 episodios a la semana, <br />que le gustan
        los géneros de drama, comedia y misterio, <br />que usa las plataformas
        Apple TV, Disney+, Star+ y Amazon Prime,<br /> y cuya serie favorita es "Jamie's
        Quick & Easy Food" (rating de IMDb: 8.2):
      </p>
      <div
        style="display: flex; justify-content:space-evenly; margin-bottom: 50px;"
      >
        <div class="circle-entry" style="animation:none; margin-top:20px;">
          {#each transformar(divide_platforms("Disney+, Star+, Amazon Prime, Apple TV").length, 100) as { x, y }, i}
            <div
              class="small-circle"
              style="transform: translate({x}px, {y}px);"
            >
              <img
                src="/images/{divide_platforms(
                  'Disney+, Star+, Amazon Prime, Apple TV'
                )[i]}.svg"
                alt="plataforma"
                class="platform-img"
              />
            </div>
            <!-- Agregar más si necesitas múltiples círculos -->
          {/each}
          <svg
            viewBox="0 0 500 500"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            width="{blob_size}%"
            id="blobSvg"
          >
            <defs>
              <linearGradient
                id="gradient-example"
                gradientTransform="rotate(150, 0.5, 0.5)"
                x1="50%"
                y1="0%"
                x2="50%"
                y2="100%"
              >
                {#each divide_genres("Drama, Comedia, Misterio") as genreType, i}
                  <stop
                    offset="{calculateOffset(
                      divide_genres('Drama, Comedia, Misterio').length,
                      i
                    )}%"
                    stop-color={genreType}
                    stop-opacity="1"
                  />
                {/each}
              </linearGradient>
            </defs>
            <path id="blob2" fill="url(#gradient-example)">
              <animate
                attributeName="d"
                dur="{Math.abs(episodes(11))}ms"
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
          <p class="label-entry">Jamie's Quick & Easy Food</p>
        </div>
      </div>
    </div>
  </Accordion>

  <div class="container1">
    {#each Object.entries(data_by_semester) as [month, list_data]}
      <p style="font-weight: bold; font-size: 30px">
        {month != 31
          ? "Cumpleaños en el " + month + " ° cuatrimestre del año"
          : "No ven series"}
      </p>
      <div style="display: flex; align-items: center; gap: 50px;">
        {#each list_data as entry, id}
          {#if watch_series(entry.watch)}
            <div class="circle-entry">
              {#each transformar(divide_platforms(entry.platforms).length, 100) as { x, y }, i}
                <div
                  class="small-circle"
                  style="transform: translate({x}px, {y}px);"
                >
                  <img
                    src="/images/{divide_platforms(entry.platforms)[i]}.svg"
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
                        id="gradient{id}_{month}"
                        x1="0%"
                        y1="0%"
                        x2="0%"
                        y2="100%"
                      >
                        {#each divide_genres(entry.genres, entry) as genreType, i}
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
                    <path id="blob1" fill="url(#gradient{id}_{month})">
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
                        id="gradient{id}_{month}"
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
                    <path id="blob2" fill="url(#gradient{id}_{month})">
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
                        id="gradient{id}_{month}"
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
                    <path id="blob3" fill="url(#gradient{id}_{month})">
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
                        id="gradient{id}_{month}"
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
                    <path id="blob4" fill="url(#gradient{id}_{month})">
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
            </div>
          {:else}
            <div class="circle-entry-no">
              <div class="data-entry-0">
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
            </div>
          {/if}

          <!--<div class="small-circle">
            <img src="/images/minus.svg" alt="minus" class="minus">
          </div>
          
          <div class="small-circle">
            <img src="/images/minus.svg" alt="minus" class="minus">
          </div> -->
        {/each}
      </div>
    {/each}
  </div>

  <div class="container2">
    <div class="fila-botones" style="margin-bottom: 30px;">
      {#each valoresZodiac as signoZodiac, i}
        <input
          class="botones-filtro"
          id={i}
          type="button"
          value={signoZodiac}
          on:click={() => changeFilter(signoZodiac, i)}
        />
      {/each}
    </div>
    <div>
      <div
        class="big-circle"
        style="height: {initial_size}px; width: {initial_size}px;"
      >
        {#each dataByZodiac as entry, id}
          <div class="circle-container">
            <div
              class="circle-entry"
              style="height: {circle_entry_size}px; width: {circle_entry_size}px; transform: translate({transformar(
                dataByZodiac.length,
                initial_size / 2
              )[id].x}px, {transformar(dataByZodiac.length, initial_size / 2)[
                id
              ].y}px); animation: none;"
            >
              {#if watch_series(entry.watch)}
                {#each transformar(divide_platforms(entry.platforms).length, circle_entry_size / 2) as { x, y }, i}
                  <div
                    class="small-circle"
                    style="transform: translate({x}px, {y}px);"
                  >
                    <img
                      src="/images/{divide_platforms(entry.platforms)[i]}.svg"
                      alt="plataforma"
                      class="platform-img"
                    />
                  </div>
                  <!-- Agregar más si necesitas múltiples círculos -->
                {/each}

                {#if parseInt(rating(entry.favRate)) == 1}
                  <div class="data-entry-0">
                    <svg
                      viewBox="0 0 500 500"
                      xmlns="http://www.w3.org/2000/svg"
                      xmlns:xlink="http://www.w3.org/1999/xlink"
                      width="{blob_size}%"
                      id="blobSvg"
                    >
                      <defs>
                        <linearGradient
                          id="gradient{id}_{2}_2"
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
                      <path id="blob1" fill="url(#gradient{id}_{2}_2)">
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
                  </div>
                {:else if parseInt(rating(entry.favRate)) == 2}
                  <div class="-0">
                    <svg
                      viewBox="0 0 500 500"
                      xmlns="http://www.w3.org/2000/svg"
                      xmlns:xlink="http://www.w3.org/1999/xlink"
                      width="{blob_size}%"
                      id="blobSvg"
                    >
                      <defs>
                        <linearGradient
                          id="gradient{id}_{2}_2"
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
                      <path id="blob2" fill="url(#gradient{id}_{2}_2)">
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
                  </div>
                {:else if parseInt(rating(entry.favRate)) == 3}
                  <div class="data-entry-0">
                    <svg
                      viewBox="0 0 500 500"
                      xmlns="http://www.w3.org/2000/svg"
                      xmlns:xlink="http://www.w3.org/1999/xlink"
                      width="{blob_size}%"
                      id="blobSvg"
                    >
                      <defs>
                        <linearGradient
                          id="gradient{id}_{2}_2"
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
                      <path id="blob3" fill="url(#gradient{id}_{2}_2)">
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
                  </div>
                {:else if parseInt(rating(entry.favRate)) == 4}
                  <div class="data-entry-0">
                    <svg
                      viewBox="0 0 500 500"
                      xmlns="http://www.w3.org/2000/svg"
                      xmlns:xlink="http://www.w3.org/1999/xlink"
                      width="{blob_size}%"
                      id="blobSvg"
                    >
                      <defs>
                        <linearGradient
                          id="gradient{id}_{2}_2"
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
                      <path id="blob4" fill="url(#gradient{id}_{2}_2)">
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
                  </div>
                {/if}
              {:else}
                <div class="data-entry-0">
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
            </div>
          </div>
        {/each}
      </div>
    </div>
  </div>
</main>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins&display=swap");
  @import url("https://fonts.googleapis.com/css2?family=Poppins:wght@700&display=swap");

  :global(body) {
    font-family: "Poppins", sans-serif;
    background-image: linear-gradient(#ff2e00, #3919fb);
    zoom: 60%;
  }

  .colores-lenguaje {
    width: 75%;
    font-size: 22px;
    font-weight: medium;
  }

  .botones-filtro {
    flex-direction: row;
    width: 15%;
    font-weight: bold;
    border: solid 7px black;
    height: 60px;
    margin-bottom: 40px;
    margin-right: 10px;
    border-radius: 10px;
    font-size: x-large;
    font-weight: bold;
  }

  #activado {
    background-color: #ff2e00;
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

  /*  .big-circle-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: relative;
    top: 150px;
    gap: 250px;
  }*/

  .label-switch::after {
    content: "";
    display: block;
    width: 25px;
    height: 25px;
    background-color: white;
    border-radius: 100px;
    position: absolute;
    top: 4px;
    left: 3px;
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

  .data-entry-0 {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .label-entry {
    opacity: 0;
    position: absolute;
    text-align: center;
    width: 80%;
    font-size: 22px;
    font-weight: medium;
  }

  .data-entry:hover svg {
    opacity: 0;
  }

  .data-entry:hover .label-entry {
    opacity: 1;
  }

  .big-circle {
    border-radius: 50%;
    display: flex;
    position: relative;
    margin-bottom: 200px;
    margin-top: 50px;
    justify-content: center;
    align-items: center;
    -webkit-animation: spin 6s linear infinite;
    -moz-animation: spin 6s linear infinite;
    animation: spin 6s linear infinite;
    border: solid 5px black;
  }

  .circle-container {
    position: absolute;
  }

  .circle-entry {
    border: 1.5px white solid;
    position: relative;
    height: 200px;
    width: 200px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 50%;
    /*margin-right: 50px;
    margin-bottom: 50px;*/
    animation: float 10s ease-in-out infinite;
  }

  .circle-entry-no {
    position: relative;
    height: 200px;
    width: 200px;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 50px;
    animation: float 10s ease-in-out infinite;
  }

  .header {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }
  .headline {
    font-size: 50px;
    line-height: 1.2;
    font-weight: normal;
    text-align: center;
    margin: 20px;
  }
  .bajada {
    font-size: 30px;
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
    width: 90%;
    flex-wrap: wrap;
    gap: 50px;
    margin-bottom: 100px;
  }

  .container2 {
    display: none;
    flex-direction: column;
    justify-content: start;
    align-items: center;
    /*margin: auto;*/
    flex-wrap: nowrap;
    /*max-width: 1000px;*/
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

  .quality {
    font-weight: 700;
    font-size: xx-large;
  }

  .accordion-button {
    text-align: center;
    background-color: white;
    width: 100%;
    border-color: black;
    border-width: 7px;
    color: black;
    font-weight: bold;
    font-size: x-large;
    border-radius: 15px;
  }

  .fila-botones {
    flex: 1;
    justify-content: center;
  }
</style>
