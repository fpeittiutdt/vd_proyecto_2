<!-- 

 HECHO POR: nilzon vela;
 FECHA DE CRACION: 2018;
 CUENTA DE CODEPEN: https://codepen.io/nilzon-vela/;
 CUENTA DE FACEBOOK: nilzon vela;

 -->



  <head>
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.1.0/css/all.css"
      integrity="sha384-lKuwvrZot6UHsBSfcMvOkWwlCMgc0TaWr+30HWe3a4ltaBwTZhyTEggF5tJv8tbt" crossorigin="anonymous">
    <link href="https://fonts.googleapis.com/css?family=Open+Sans" rel="stylesheet">
  </head>


  <div id="btn-move" onclick="ba()">
    <i class="fas fa-power-off"></i>
  </div>

  <!-- --------------------- -->
  <!-- ----sistema solar------ -->
  <!-- --------------------- -->

  <div class="sistema-solar">
    <div class="linea-guia linea-guia-1"></div>
    <div class="linea-guia linea-guia-2"></div>
    <div class="linea-guia linea-guia-3"></div>
    <div class="linea-guia linea-guia-4"></div>
    <div class="linea-guia linea-guia-5"></div>
    <div class="linea-guia linea-guia-6"></div>
    <div class="linea-guia linea-guia-7"></div>
    <div class="linea-guia linea-guia-8"></div>
    <div class="linea-guia linea-guia-9"></div>

    <div class="sol objeto" onclick="ap(1)">
    </div>

    <div class="content-mercurio content-objeto" id="co2">
      <div class="mercurio objeto" onclick="ap(2)">
      </div>
    </div>

    <div class="content-venus  content-objeto" id="co3">
      <div class="venus objeto" onclick="ap(3)">
      </div>
    </div>

    <div class="content-tierra content-objeto" id="co4">
      <div class="tierra objeto" onclick="ap(4)">
      </div>
    </div>

    <div class="content-marte content-objeto" id="co5">
      <div class="marte objeto" onclick="ap(5)">
      </div>
    </div>

    <div class="content-jupiter content-objeto" id="co6">
      <div class="jupiter objeto" onclick="ap(6)">
      </div>
    </div>

    <div class="content-saturno content-objeto" id="co7">
      <div class="saturno objeto" onclick="ap(7)">
      </div>
    </div>

    <div class="content-urano content-objeto" id="co8">
      <div class="urano objeto" onclick="ap(8)">
      </div>
    </div>

    <div class="content-neptuno content-objeto" id="co9">
      <div class="neptuno objeto" onclick="ap(9)">
      </div>
    </div>

    <div class="content-pluton content-objeto" id="co10">
      <div class="pluton objeto" onclick="ap(10)">
      </div>
    </div>

  </div>



  <!-- -------------------- -->
  <!-- targetas info de plantas -->
  <!-- -------------------- -->

  <div class="cards-container" id="ccp">

    <div class="card-sol content-card" id="cp1">
      <div class="img-planeta">
        <h2 class="title-planeta">Sol</h2>
      </div>
      <div class="info-sol info-planeta">
        <p>El Sol es una estrella de tipo-G de la secuencia principal y clase de luminosidad V que se encuentra en el
          centro del sistema solar y constituye la mayor fuente de radiación electromagnética de este sistema
          planetario.​</p>
      </div>
      <div class="caja-botones">
        <a href="https://es.wikipedia.org/wiki/Sol" target="_blank">Leer mas</a>
        <div class="btn-planeta" onclick="ct(1)">cerrar</div>
      </div>
    </div>

    <!-- ------ -->
    <div class="card-mercurio content-card" id="cp2">
      <div class="img-planeta">
        <h2 class="title-planeta">Mercurio</h2>
      </div>
      <div class="info-planeta">
        <p>Mercurio es el planeta del sistema solar más próximo al Sol y el más pequeño. Forma parte de los denominados
          planetas interiores o terrestres y carece de satélites naturales al igual que Venus.</p>
      </div>
      <div class="caja-botones">
        <a href="https://es.wikipedia.org/wiki/Mercurio_(planeta)" target="_blank">Leer mas</a>
        <div class="btn-planeta" onclick="ct(2)">cerrar</div></a>
      </div>
    </div>

    <div class="card-venus content-card" id="cp3">
      <div class="img-planeta">
        <h2 class="title-planeta">Venus</h2>
      </div>
      <div class="info-planeta">
        <p>Venus es el segundo planeta del sistema solar en orden de distancia desde el Sol, el sexto en cuanto a
          tamaño, ordenados de mayor a menor. Al igual que Mercurio, carece de satélites naturales. </p>
      </div>
      <div class="caja-botones">
        <a href="https://es.wikipedia.org/8wiki/Venus_(planeta)" target="_blank">Leer mas</a>
        <div class="btn-planeta" onclick="ct(3)">cerrar</div>
      </div>
    </div>

    <div class="card-tierra content-card" id="cp4">
      <div class="img-planeta">
        <h2 class="title-planeta">Tierra</h2>
      </div>
      <div class="info-planeta">
        <p>La Tierra es un planeta del sistema solar que gira alrededor de su estrella —el Sol— en la tercera órbita más
          interna. Es el más denso y el quinto mayor de los ocho planetas del sistema solar. También es el mayor de los
          cuatro terrestres o rocosos.</p>
      </div>
      <div class="caja-botones">
        <a href="https://es.wikipedia.org/wiki/Tierra" target="_blank">Leer mas</a>
        <div class="btn-planeta" onclick="ct(4)">Cerrar</div>
      </div>
    </div>

    <div class="card-marte content-card" id="cp5">
      <div class="img-planeta">
        <h2 class="title-planeta">Marte</h2>
      </div>
      <div class="info-planeta">
        <p>Marte es el cuarto planeta en orden de distancia al Sol y el segundo más pequeño del sistema solar, después
          de Mercurio. Tambien es conocido como el Planeta Rojo.</p>
      </div>
      <div class="caja-botones">
        <a href="https://es.wikipedia.org/wiki/Marte_(planeta)" target="_blank">Leer mas</a>
        <div class="btn-planeta" onclick="ct(5)">Cerrar</div>
      </div>
    </div>

    <div class="card-jupiter content-card" id="cp6">
      <div class="img-planeta">
        <h2 class="title-planeta">Jupiter</h2>
      </div>
      <div class="info-planeta">
        <p>Júpiter es el quinto planeta del sistema solar. Forma parte de los denominados planetas exteriores o
          gaseosos. Recibe su nombre del dios romano Júpiter. Se trata del planeta que ofrece un mayor brillo a lo largo
          del año dependiendo de su fase.</p>
      </div>
      <div class="caja-botones">
        <a href="https://es.wikipedia.org/wiki/J%C3%BApiter_(planeta)" target="_blank">Leer mas</a>
        <div class="btn-planeta" onclick="ct(6)">Cerrar</div>
      </div>
    </div>

    <div class="card-saturno content-card" id="cp7">
      <div class="img-planeta">
        <h2 class="title-planeta">Saturno</h2>
      </div>
      <div class="info-planeta">
        <p>Saturno es el sexto planeta del sistema solar, el segundo en tamaño y masa después de Júpiter y el único con
          un sistema de anillos visible desde nuestro planeta. Su nombre proviene del dios romano Saturno.</p>
      </div>
      <div class="caja-botones">
        <a href="https://es.wikipedia.org/wiki/Saturno_(planeta)" target="_blank">Leer mas</a>
        <div class="btn-planeta" onclick="ct(7)">Cerrar</div>
      </div>
    </div>

    <div class="card-urano content-card" id="cp8">
      <div class="img-planeta">
        <h2 class="title-planeta">Urano</h2>
      </div>
      <div class="info-planeta">
        <p>Urano es el séptimo planeta del sistema solar, el tercero de mayor tamaño, y el cuarto más masivo. Se llama
          así en honor de la divinidad griega del cielo Urano, el padre de Crono y el abuelo de Zeus.</p>
      </div>
      <div class="caja-botones">
        <a href="https://es.wikipedia.org/wiki/Urano_(planeta)" target="_blank">Leer mas</a>
        <div class="btn-planeta" onclick="ct(8)">Cerrar</div>
      </div>
    </div>

    <div class="card-neptuno content-card" id="cp9">
      <div class="img-planeta">
        <h2 class="title-planeta">Neptuno</h2>
      </div>
      <div class="info-planeta">
        <p>Neptuno es el octavo planeta en distancia respecto al Sol y el más lejano del sistema solar. Forma parte de
          los denominados planetas exteriores o gigantes gaseosos, y es el primero que fue descubierto gracias a
          predicciones matemáticas.</p>
      </div>
      <div class="caja-botones">
        <a href="https://es.wikipedia.org/wiki/Neptuno_(planeta)" target="_blank">Leer mas</a>
        <div class="btn-planeta" onclick="ct(9)">Cerrar</div>
      </div>
    </div>

    <div class="card-pluton content-card" id="cp10">
      <div class="img-planeta">
        <h2 class="title-planeta">Pluton</h2>
      </div>
      <div class="info-planeta">
        <p>Plutón, designado 134340 Pluto, es un planeta enano del sistema solar situado a continuación de la órbita de
          Neptuno. Su nombre se debe al dios mitológico romano Plutón.</p>
      </div>
      <div class="caja-botones">
        <a href="https://es.wikipedia.org/wiki/Plut%C3%B3n_(planeta_enano)" target="_blank">Leer mas</a>
        <div class="btn-planeta" onclick="ct(10)">cerrar</div>
      </div>
    </div>

  </div>

  /*

HECHO POR: nilzon vela;
FECHA DE CRACION: 2018;
CUENTA DE CODEPEN: https://codepen.io/nilzon-vela/;
CUENTA DE FACEBOOK: nilzon vela;

*/

*,*::after,*::before{
 margin: 0;
 padding: 0;
  /* outline: 1px solid white; */
}

/* ------variables imagenes de fondo------ */
:root{
 --z-s: 70vmin;
 /*  -----background contents------  */
 --bgi-galaxia: url('https://d2v9y0dukr6mq2.cloudfront.net/video/thumbnail/moving-through-stars-in-space_-1zccenlb__F0000.png');
 --bgi-card: url('https://lh3.googleusercontent.com/K3UdS0t311DpKIiq614Ix6cRanFYxueEFaLF3T0bPQLGcJtqzw5ps3ClI85nK7jB4ElbKBs8xg=w640-h400-e365');
 
 /*  -------image planetas---------  */
 --sol: url('https://media.giphy.com/media/OWDUUrk8fwYj6/giphy.gif');
 --mercurio: url('http://opcionweb.com/webs/espacio/mercurio.png');
 --venus: url('https://vignette.wikia.nocookie.net/dumbledoresarmyroleplay/images/d/dc/Venus.png/revision/latest?cb=20120527010129');
 --tierra: url('https://vignette.wikia.nocookie.net/tmnt/images/0/05/La_tierra_00.png/revision/latest?cb=20170919151939&path-prefix=es');
 --marte: url('https://cdn.icon-icons.com/icons2/1434/PNG/256/06-mars_98594.png');
 --jupiter: url('http://pluspng.com/img-png/png-jupiter-jupiter-500.png');
 --jupiter2: url('http://www.pngmart.com/files/4/Jupiter-PNG-File.png');
 --saturno: url('https://upload.wikimedia.org/wikipedia/commons/c/c0/3D_Saturn.png');
 --urano: url('https://vignette.wikia.nocookie.net/warframe/images/8/8e/Urano.png/revision/latest?cb=20161018113918&path-prefix=es');
 --urano2: url('https://cdn.icon-icons.com/icons2/1434/PNG/256/09-uranus_98592.png');
 --neptuno: url('https://www.edu.xunta.es/espazoAbalar/sites/espazoAbalar/files/datos/1491394205/contido/img/neptuno_lim.png');
 --neptuno2: url('https://vignette.wikia.nocookie.net/destiny/images/5/5e/Neptuno.png.png/revision/latest?cb=20130625170428&path-prefix=es');
 --pluton: url('https://i.dlpng.com/static/png/480693_thumb.png');
}

#btn-move{
 position: absolute;
 top: .5rem;
 left: .5rem;
 display: block;
 color: white;
 cursor: pointer;
 user-select: none;
 color: gray;
 font-size: 1.5em;
 border-radius: 100%;  
 background-color: black;
}
/* ------------------------------
-------padres prinsipales-------
---------------------------------*/
body{
 margin: 0;
 display: flex;
 align-items: center;
 justify-content: center;
 height: 100vh;
 background-image: var(--bgi-galaxia);
 background-color: #000;
 background-size: cover;
 background-position: center;
 overflow: hidden;
/*   overflow-y: auto; */
}
:root{
}

body::after{
 content: "Preciona los planetas / Click the planets";
 position: absolute;
 bottom: .4rem;
 left: .5rem;
 padding: .3rem .5rem;
 color: white;
 background-color: #000;
 text-transform: uppercase;
 font: .5em sans-serif;
}

.sistema-solar{
 position: relative;
 width: var(--z-s);
 height: var(--z-s);
 display: flex;
 align-items: center;
 justify-content: center;
/*   transform: rotatex(50deg) */
}

.content-objeto{
 position: absolute;
 display: flex;
 align-items: center;
 justify-content: flex-end;
 transform-origin: left;
 left: 50%;
 visibility: hidden;
}
.objeto{
 border-radius: 100%;
 background-repeat: no-repeat;
 background-position: center;
 background-size: cover;
 position: relative;
 visibility: visible;
 cursor: pointer;
}
.objeto:hover{
 outline: 1px solid #5559;
}

/* ----sol---- */
.sol{
 height: calc(var(--z-s) / 15);
 width: calc(var(--z-s) / 15);
 background-image: var(--sol);
/*   background-size: cover; */
 background-size: 126%;
 position: absolute;
 z-index: 10;  
}

/* ------------------------------
-------planetas-------
---------------------------------*/

/* ----mercuio---- */
.content-mercurio{
 width: calc(var(--z-s) / 10);
}
.mercurio{
 width: calc(var(--z-s) / 38);
 height: calc(var(--z-s) / 38);
 background-image: var(--mercurio);
}

/* ----venus---- */
.content-venus{
 width: calc(var(--z-s) / 6.5);
}
.venus{
 height: calc(var(--z-s) / 30);
 width: calc(var(--z-s) / 30);
 background-image: var(--venus);
}

/* ----tierra---- */
.content-tierra{
 width: calc(var(--z-s) / 4.5);
}
.tierra{
 height: calc(var(--z-s) / 23);
 width: calc(var(--z-s) / 23);
 background-image: var(--tierra);
 background-size: cover;
}

/* ----marte---- */
.content-marte{
 width: calc(var(--z-s) / 3.49);
}
.marte{
 height: calc(var(--z-s) / 25.5);
 width: calc(var(--z-s) / 25.5);
 background-image: var(--marte);
 background-size: 110%;
}

/* ----jupiter---- */
.content-jupiter{
 width: calc(var(--z-s) / 2.5);
}
.jupiter{
 width: calc(var(--z-s) / 11.5);
 height: calc(var(--z-s) / 11.5);
 background-image: var(--jupiter);
 background-size: 110%;
}

/* ----saturno---- */
.content-saturno{
 width: calc(var(--z-s) / 1.91);
}
.saturno{
 width: calc(var(--z-s) / 10);
 height: calc(var(--z-s) / 20);
 background-image: var(--saturno);
 background-size: 105%;
 z-index: 100;
}

/* ----urano---- */
.content-urano{
 width: calc(var(--z-s) / 1.66);
}
.urano{
 height: calc(var(--z-s) / 17);
 width: calc(var(--z-s) / 17);
 background-image: var(--urano);
 background-size: 75%;
}

/* ----neptuno--- */
.content-neptuno{
 width: calc(var(--z-s) / 1.53);
}
.neptuno{
 height: calc(var(--z-s) / 30);
 width: calc(var(--z-s) / 30);
 background-image: var(--neptuno);
 background-size: 110%;
}

/* ----pluton---- */
.content-pluton{
 width: calc(var(--z-s) / 1.43);
}
.pluton{
 height: calc(var(--z-s) / 35);
 width: calc(var(--z-s) / 35);
 background-image: var(--pluton);
}


/* ---------------------- */
/* ------linea guia----- */
/* ---------------------- */

.linea-guia{
 position: absolute;
 border-radius: 100%;
 border: 1px dashed rgba(255,255,255,.15);
 z-index: -20;
}
.linea-guia-1{  
 width: calc(var(--z-s) / 6);
 height: calc(var(--z-s) / 6);
}
.linea-guia-2{  
 width: calc(var(--z-s) / 3.6);
 height: calc(var(--z-s) / 3.6);
}
.linea-guia-3{  
 width: calc(var(--z-s) / 2.5);
 height: calc(var(--z-s) / 2.5);
}
.linea-guia-4{  
 width: calc(var(--z-s) / 1.9);
 height: calc(var(--z-s) / 1.9);
}
.linea-guia-5{  
 width: calc(var(--z-s) / 1.4);
 height: calc(var(--z-s) / 1.4);
}
.linea-guia-6{  
 width: calc(var(--z-s) / 1.06);
 height: calc(var(--z-s) / 1.06);
}
.linea-guia-7{  
 width: calc(var(--z-s) / .87);
 height: calc(var(--z-s) / .87);
}
.linea-guia-8{  
 width: calc(var(--z-s) / .79);
 height: calc(var(--z-s) / .79);
}
.linea-guia-9{  
 width: calc(var(--z-s) / .73);
 height: calc(var(--z-s) / .73);
}



/* -------------------------- */
/* ----estilos de targetas planetarias */
/* -------------------------- */

#ccp{
  display: none;
}
.cards-container{
 position: fixed;
 width: 100vw;
 height: 100vh;
 align-items: center;
 justify-content: center;
 z-index: 100;
 background-color: rgba(0,0,0,.6);
}
.content-card{
  display: none;
 position: absolute;
 max-width: 280px;
 min-height: 200px;
 max-height: 500px;
 flex-direction: column;
 background-color: white;
 overflow: auto;
 animation: aparecer .2s ease-in-out
}

@keyframes aparecer{
 0%{
   transform: scale(0)
 }
 100%{
   transform: scale(1)
 }
}

.img-planeta{
 height: 155px;
 background-size: contain,cover;
 background-position: center;
 background-repeat: no-repeat;
 background-origin: content-box,border-box;
 margin-bottom: .4rem;
 padding: .7rem;
 position: relative;
}
.title-planeta{
 color: white;
 font-family: calibri;
 font-size: 0.9em;
 text-transform: uppercase;
 position: absolute;
 right: .5rem;
 top: .5rem;
 padding-left: .3rem;
 user-select: none;
 background-color: rgb(255, 8, 8);
 padding: .2rem .5rem;
 border-radius: 1rem 0 0 1rem
}

.info-planeta{
 padding: .5rem 1rem;
}
.info-planeta p{
 font-size: .9em;
 text-align: justify;
 line-height: 1.4;
 color: hsl(0,0%,50%);
 padding-left: .8rem;
 position: relative;
 font-family: 'Open Sans';
 font-weight: 900;
}
.info-planeta p::before{
 content:"";
 position: absolute;
 top: 0;
 left: 0;
 bottom: 0;
 width: 6px;
 background-color: #f00;
 background-image:
   linear-gradient(
     #fc9201,
     #d6723c,
     #9f5f1b,
     #2a376d,
     #d55333,
     #b48a5c,
     #4480a3,
     #4676fa,
     #ecba95
   );
}

.caja-botones{
 display: flex;
 justify-content: space-between;
 padding: 1rem;
}
.caja-botones a,
.caja-botones .btn-planeta
{
 text-decoration: none;
 padding: .5rem .7rem;
 font-size: .70em;
 text-transform: uppercase;
 background-color: dodgerblue;
 font-family: 'Open Sans',sans-serif;
 cursor: pointer;
 transition: .2s;
 font-weight: 900;
 color: black;
 user-select: none;
 color: white;
 border-radius: 1rem;
}
.caja-botones a:hover,
.caja-botones .btn-planeta:hover{
 background-color: #06f;
 color: white;
}
.caja-botones a:active,
.caja-botones .btn-planeta:active{
 transform: scale(.9)
}



/* imagenes de cards planetas */
.card-sol .img-planeta{
 background-image: 
   url('http://galacticmagazine.torodgn.com/viaje/images/elements/compartido/sol.png'),
   var(--bgi-card);
 background-size: 90%,cover;
}

.card-mercurio .img-planeta{
 background-image: 
   var(--mercurio),
   var(--bgi-card);
}

.card-venus .img-planeta{
 background-image: 
   var(--venus),
   var(--bgi-card);
}

.card-tierra .img-planeta{
 background-image: 
   var(--tierra),
   var(--bgi-card);
}

.card-marte .img-planeta{
 background-image: 
   var(--marte),
   var(--bgi-card);
}

.card-jupiter .img-planeta{
 background-image: 
   var(--jupiter2),
   var(--bgi-card);
}

.card-saturno .img-planeta{
 background-image: 
   var(--saturno),
   var(--bgi-card);
}

.card-urano .img-planeta{
 background-image: 
   var(--urano2),
   var(--bgi-card);
}

.card-neptuno .img-planeta{
 background-image: 
   var(--neptuno2),
   var(--bgi-card);
}

.card-pluton .img-planeta{
 background-image:
   var(--pluton),
   var(--bgi-card);
}


/* ------------------------------
-------pseudoelementos-------
---------------------------------*/
.sol::before{content: "Sol"}
.mercurio::before{content: "Mercurio"}
.venus::before{content: "Venus";}
.tierra::before{content: "Tierra";}
.marte::before{content: "Marte";}
.jupiter::before{content: "Jupiter";}
.saturno::before{content: "Saturno";}
.urano::before{content: "Urano";}
.neptuno::before{content: "neptuno";} 
.pluton::before{content: "pluton"}

.objeto::before{
 position: absolute;
 color: #fff;
 font-family: monospace;  
 right: 140%;
 top: 0;
 bottom: 0;
 height: 20px;
 margin: auto;
 padding: 0 .5rem .15rem;
 display: flex;
 align-items: center;
 justify-content: center;
 display: none;
 transition: .5s;
 animation: girar-pseudo 4s infinite linear;
 background-color: #0008;
}
.mercurio::before,.venus::before{
  right: auto;
  left: 140%
}
.objeto:hover::before{
 display: block;
}

@keyframes girar-pseudo{
 100%{transform: rotate(1turn)}
}

/* animacioness y mas  */

.content-objeto{
  animation: infinite linear both;
  --dm: 10s;
}
.sol{
  animation-duration: 1s;
}
.content-mercurio{  
 animation-duration: calc(var(--dm) * 1);
 animation-name: mercurio;
} 
.content-venus{
 animation-duration: calc(var(--dm) * 1.1);
 animation-name: venus;
}
.content-tierra{ 
 animation-duration: calc(var(--dm) * 1.17);
 animation-name: tierra; 
}
.content-marte{  
 animation-duration: calc(var(--dm) * 1.25);
 animation-name: marte;
}
.content-jupiter{
 animation-duration: calc(var(--dm) * 1.35);
 animation-name: jupiter;
}
.content-saturno{  
 animation-duration: calc(var(--dm) * 1.43);
 animation-name: saturno;
}
.content-urano{  
 animation-duration: calc(var(--dm) * 1.49);
 animation-name: urano;
}
.content-neptuno{  
 animation-duration: calc(var(--dm) * 1.56);
 animation-name: neptuno;
}
.content-pluton{  
 animation-duration: calc(var(--dm) * 1.63);
 animation-name: pluton;
}
.content-objeto{  
 animation-play-state: paused;
}


/* ------------------------------
-------animaciones y 
           transformciones-------
---------------------------------*/

/* @keyframes pluton{
  to{
    transform: rotate(1turn)
  }
} */


@keyframes sol{
  from{transform: rotate(0)}
  to{transform: rotate(360deg)}
 }
 
 @keyframes mercurio{
  from{transform: rotate(-30deg);}
  to{transform: rotate(330deg)}
 }
 
 
 @keyframes venus{
  from{transform: rotate(60deg);}
  to{transform: rotate(420deg)}
 }
 
 @keyframes tierra{
  from{transform: rotate(200deg)}
  to{transform: rotate(560deg)}
 }
 
 @keyframes marte{
  from{transform: rotate(-100deg)}
  to{transform: rotate(260deg)}
 }
 
 @keyframes jupiter{
  from{transform: rotate(130deg)}
  to{transform: rotate(490deg)}
 }
 
 @keyframes saturno{
  from{transform: rotate(3);}
  to{transform: rotate(360deg)}
 }
 
 @keyframes urano{
  from{transform: rotate(-55deg)}
  to{transform: rotate(305deg)}
 }
 
 @keyframes neptuno{
  from{transform: rotate(220deg)}
  to{transform: rotate(580deg)}
 }
 
 @keyframes pluton{
  from{transform: rotate(40deg)}
  to{transform: rotate(400deg)}
 }
 

/* ----------------------------------------- */
/* ------estados activar descativar-------- */
/* -------------------------------------- */

/* ----girar-planetas--- */




// funcion para abrir una trageta
function ap(a){
 document.getElementById('ccp').style.display = 'flex';
 document.getElementById(`cp${a}`).style.display = 'block';
}

// funcion cierra un targeta
function ct(b){
 document.getElementById('ccp').style.display = 'none';
 document.getElementById(`cp${b}`).style.display = 'none';
}

// funcion activar animacion
let num = 1;
function ba(){
 if(num == 1){
  for (i = 2;i <= 10;i++){
   document.getElementById(`co${i}`).style.animationPlayState = 'running';
   document.getElementById('btn-move').style.color = 'white'
  }
  num--;
 } else{
  for (i = 2;i <= 10;i++){
   document.getElementById(`co${i}`).style.animationPlayState = 'paused';document.getElementById('btn-move').style.color = 'gray'
  }
  num++;
 }
}