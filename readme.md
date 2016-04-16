# Juego de Patron de Memoria

La idea de este juego es que se le muestra una secuencia de colores en el recuadro y el usuario la debe seleccionar la secuencia de manera correcta y así va avanzando por niveles hasta llegar el nivel maximo
y salir de el, con la posibilidad de volver a jugar si lo desea.


# Programa final

![](raw.github.com/LeliArevalo/EP01-Trabajo-08/master/img/Inicio.jpg)
![](raw.github.com/LeliArevalo/EP01-Trabajo-08/master/img/Nivel.jpg)
![](raw.github.com/LeliArevalo/EP01-Trabajo-08/master/img/Puntuacion.jpg)
![](raw.github.com/LeliArevalo/EP01-Trabajo-08/master/img/Fin.jpg)

# Programa en javascript y JQuery

El desarrollo se realizo con javascript, JQuery V1.11.1
A continuacion se relacionas las funciones usadas para llear acabo este proyecto.

Declaracion de variables

```
$(function ()
{
  var tabla="";
  var dimension=3;
  var id=0;
  var aleatorio=[];
  var numAleatorio;
  var nivel=1;
  var numId;
  var puntuacion=0;
  var pasoNivel=0;
  var click=0;
  var segundos;

```
En este parte los botonoes inicio y fin para que se muestren en la primera pantalla.
Así tambien cuand se finalice el juego aparezca el boton para poder volver a jugar. Adicional a esto
tambien mide la puntación que va obteniendo el usuario.

```
nom_div("puntuacion").innerHTML=("<h3> Puntuación: "+puntuacion+"</h3>");
$("#fin").hide();
$("#volver").hide();

$("#inicio").click(function () {
  click++;
  nom_div("nivel").innerHTML = "Nivel: "+ nivel;
  escenario(3);
  partida();
  $("#inicio").fadeOut();
})

$("#volver").click(function () {
  location.reload();
})
});

```
Se crea la función escenario para que muestre los cuadros, es decir la tabla que registra al inicio,
```
var escenario =function (dimension)
{
  clearInterval(intervalo);
  tabla="<table id='tabla'>"
  for (var i = 0; i < dimension; i++)
  {
    tabla += "<tr>";
    for (var j = 0; j < dimension; j++)
    {
      id++;
        tabla += "<td>";
        tabla += "<div id= '_"+id+"'class='cuadro'></div>";
        tabla += "</td>";
    }
    tabla += "<tr>";
  }
  tabla += "</table>";
  $("#tablas").append(tabla);
}

```
Las funciones indica que genere un array numero mayor a 10 en el random, recorre toda la matriz y la llena la matriz de manera aleatoria para realizar los patrones en las 2 funciones a continuacion:

```
function aleatorios()
{
  numAleatorio=Math.floor(Math.random()*((dimension*dimension)-1+1))+1;
}

var llenarAleatorio = (function llenarAleatorio(dimension)
{    clearInterval(intervalo);
  for (var b = 0; b < dimension; b++)
  {
    aleatorios();
    if (!igual()) {
      aleatorio.push(numAleatorio);
      console.log(numAleatorio);
    } else {
      b--;
      aleatorios();
    }
}

  return llenarAleatorio;
})(3);

function igual()
{
var igual=false;
for (var i = 0; i < aleatorio.length; i++) {
  if (numAleatorio===aleatorio[i]) {
    igual=true;
  }
}
return igual;
}

```

function nom_div(div)
{
    return document.getElementById(div);
}
function randomColor() {
return '#'+(function lol(m,s,c){return s[m.floor(m.random() * s.length)] +
(c && lol(m,s,c-1));})(Math,'0123456789ABCDEF',4);

//Tomado: http://www.paulirish.com/2009/random-hex-color-code-snippets/
}

Se crea los cuadros que sera el escenario para el ingreso de los patrones o secuencias

```
var contador1=0;var intervalo;
var animacion="flip";
function patron()
{
clearInterval(tiempo);
contador1=0;
if (contador1 <=aleatorio.length)
{
  intervalo= setInterval(function()
   {
      $("#_"+aleatorio[contador1]).css("background-color", randomColor()).addClass(animacion);

      contador1++;
      patron2();
    },1000);
}
}

function patron2()
{
  var contador2=0;
  if (contador2<= aleatorio.length && contador1 === aleatorio.length)
  {
    setInterval(function ()
    {
      $("#_"+aleatorio[contador2]).removeClass("animated"+animacion).css("background-color","rgb(232,232,232)");
      contador2++;
      if (contador2===aleatorio.length)
      {
          clearInterval(intervalo);
          if (click===1 || nivel>1) {
            clearInterval(tiempo);
            segundos=60;
            timer(segundos);
          }
      }
    },1000);

  }
}
```

La funcion partida es la que permite al patron asi como tambien ayudara en la validacion de la respuesta dada por el usuario.

```

function partida() {
  patron();
  //escenario(dimension);
  $(".cuadro").click(function cuadro(e)
  {
    numId=e.target.id;
    numId=numId.split("_");
    console.log(numId+" " +numId[1]);
    if (aleatorio.indexOf(Number(numId[1]))>=0) {
      puntuacion++;
      pasoNivel++;
      nom_div("puntuacion").innerHTML=("<h3> Puntuación: "+puntuacion+"</h3>");
          if (pasoNivel===aleatorio.length) {
          $("#tabla").remove();
          id=0;
          pasoNivel=0;
          nivel++;
          dimension++;
          nom_div("nivel").innerHTML=("<h3> Nivel: "+nivel+"</h3>");
          aleatorio.splice(0,aleatorio.length);
          llenarAleatorio(dimension);
          escenario(dimension);
          partida();
          patron();

        }
      } return cuadro;
  })

}
```

Esta funcion se encarga de que cuando se finalice el juego no se muestre, los botones de inicio, nivel y dimension y así mismo quedar con la posibilidad de volver a jugar.

```
function end(segundos, nivel) {
    if(segundos===0 || nivel >5)
    {
      $("#tablas").fadeOut();
      $("#tiempo").fadeOut();
      $("#nivel").fadeOut();
      $("#fin").fadeIn();
      $("#volver").fadeIn();
    }
}
```
En esta función se calcula el tiempo que tiene el usuario para solucuonar el juego

```
function timer()
{
  segundos=60;
  var tiempo=setInterval(function ()
  {
    segundos--;
    nom_div("tiempo").innerHTML="00:"+ segundos;
    end(segundos, nivel);
    if (segundos <= 0) {
      clearInterval(tiempo);
    }
  },1000);

}
```

Autor: Leidy Arevañp
