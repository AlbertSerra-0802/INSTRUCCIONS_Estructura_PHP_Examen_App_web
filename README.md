###INSTRUCCIONS_Estructura_PHP_Examen_App_web
Passes per fer el wordpress i php perfecte
PASSES:

ESTRUCTURA ADEQUADA ABANS DE COMENÇAR A FER L’EXAMEN:

PAS 1: Creació de la carpeta del tema

Dins de VS Code, a la columna de l'esquerra, has d'estructurar els fitxers dins de la carpeta src.
Ves a src/wp-content/themes/.
Crea una carpeta nova anomenada asv_examen.

--------------------------------------------------------------------------------------------

PAS 2: PREPARACIÓ DE L'ENTORN (VS CODE I DOCKER) 

Obrir l'entorn: Obrir el Docker Desktop de l'escriptori.

Executar contenidors: Obre la terminal de Visual Studio Code (sempre comprovant que estàs dins la ruta de la teva carpeta del projecte) i executa:

docker-compose down 
docker-compose up -d 

Obre el navegador a localhost:8000. Clica a ¡Vamos a ello! i omple els paràmetres de connexió del teu servidor local. Després crea el teu usuari d'administrador de l'examen. 

--------------------------------------------------------------------------------------------

PAS 3: CONFIGURACIÓ DE FITXERS AL VS CODE
A la ruta src/wp-content/themes/, crea una carpeta anomenada asv_examen i copia-hi dins les teves carpetes d'estils (css) i imatges (img) de l'HTML original. Ara posa exactament aquest codi net a cadascun dels 5 fitxers obligatoris: 


1. functions.php
Deixem només la línia per activar les fotos de portada. Eliminem completament els registres de menús dinàmics complexos que feien embulls.

<?php
function asv_examen_scripts_bootstrap() {


    // Carreguem el CSS de Bootstrap
    wp_enqueue_style('bootstrap-css', 'https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css', array(), '5.3.3');
    
    // Carreguem el teu full d'estils propi (style.css)
    wp_enqueue_style('theme-style', get_stylesheet_uri(), array('bootstrap-css'), '1.0');

    // Carreguem el JS de Bootstrap (per si teniu menús desplegables)
    wp_enqueue_script('bootstrap-js', 'https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js', array(), '5.3.3', true);
}

add_action('wp_enqueue_scripts', 'asv_examen_scripts_bootstrap');


2. Fitxer header.php
Ves al teu index.html original i copia des de la primera línia (<!DOCTYPE html>) fins a on s'acaba el menú (</header>). Enganxa-ho aquí eliminant els enllaços antics de CSS i afegint les claus i el Menú Simple per IDs fixes:

<!DOCTYPE html>
<html <?php language_attributes(); ?>>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <?php wp_head(); ?> 
</head>
<body class="bg-dark text-white">

<header>
<nav class="navbar navbar-expand-lg navbar-dark bg-dark border-bottom border-secondary">
  <div class="container">
  <a class="navbar-brand fw-bold text-light" href="<?php echo home_url(); ?>">To the Moon</a>
  <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
  <span class="navbar-toggler-icon"></span>
  </button>
            
  <div class="collapse navbar-collapse" id="navbarNav">

  <ul class="navbar-nav ms-auto">
  <li class="nav-item">
  <a class="nav-link" href="<?php echo home_url(); ?>">Home</a>
  </li>
  <li class="nav-item">
  <a class="nav-link" href="<?php echo home_url('?page_id=7');?>">Portafolio</a>
  </li>
  <li class="nav-item">
  <a class="nav-link" href="<?php echo home_url('?page_id=9'); ?>">Contacto</a>
  </li>
  </ul>
  </div>
  </div>
  </nav>
</header>

3. Fitxer footer.php
Copia des de on comença el teu <footer> fins al final del fitxer HTML original i enganxa-ho afegint la clau obligatòria de tancament de WordPress:

<footer class="container mt-5 pt-4 border-top border-secondary text-center text-secondary">
    <p class="small mb-0">
        © 2026 Apollo Misión. Todos los derechos reservados.
    </p>
</footer>

<?php wp_footer(); ?> 
</body>
</html>

4. Fitxer index.php (La teva Home Principal amb el loop 5 entrades)
Ajunta les peces bàsiques i enganxa el contingut central del teu index:

**<?php get_header(); ?>
<main class="container my-5 text-white">
    <?php
    if (have_posts()) :
        while (have_posts()) : the_post();
            the_title('<h2>', '</h2>');
            the_content();
        endwhile;
    endif;
    ?>
</main>
<?php get_footer(); ?>


5. Nou fitxer page-portfolio.php (La Plantilla de Portafoli dedicada)
Crea aquest fitxer nou a dins de la carpeta del teu tema per guardar el teu portafoli de 6 caixes:
<?php
/*
Template Name: Portfolio Mision
*/
get_header(); ?>

<?php get_footer(); ?>


6. Nou fitxer page-contacto.php (La Plantilla de Contacte dedicada)
Crea aquest fitxer nou a dins de la carpeta del teu tema per guardar el formulari de contacte fosc:
<?php
/*
Template Name: Contacto Mision
*/
get_header(); ?>

<?php get_footer(); ?>

--------------------------------------------------------------------------------------------

PAS 4: EL PAS A PAS CRONOLÒGIC DIRECTE DENTRE WORDPRESS 

Escrivim (localhost:8000/wp-admin):

Pas 1: Activar el Tema

  Al menú negre lateral de l'esquerra, clica a Apariencia ➔ Temas.

  Busca el quadre del teu tema (asv_examen) i clica al botó blau Personalizar.


Pas 2: Activar el mode d'Enllaços Simple

  Al menú lateral de l'esquerra, clica a Ajustes ➔ Enlaces permanentes.

  Selecciona obligatòriament la primera opció de la llista: 

  Simple (la que té el format ?p=123).

  Baixa al final de la pàgina i clica a Guardar cambios.


Pas 3: Creació de les 5 entrades del Blog

Al menú lateral, fes clic a Entradas ➔ Añadir nueva entrada.

Escriu el títol (Ex: Entrada 1) i una mica de text, i clica a Publicar.

Repeteix el procés fins a tenir un total de 5 entrades creades.


Pas 4: Forçar el límit de les 5 entrades al Loop

Al menú lateral esquerre, ves a Ajustes ➔ Lectura.

Cerca l'apartat Número máximo de entradas a mostrar en el sitio.

Esborra el número que hi hagi i posa-hi un 5.

Clica a Guardar cambios. (Això farà que el Loop d'index.php només pinti les 5 últimes de cop).


Pas 5: Donar d'alta la pàgina de Portfolio

  Al menú lateral, clica a Páginas ➔ Añadir nueva.

  Escriu com a títol de la pàgina: Portfolio i a la barra de la dreta de la teva pantalla (pestanya    "Página"): busca l'apartat Plantilla.

  Obre el desplegable (on posa Plantilla por defecto) i selecciona: Portfolio Mision.

  Clica al botó blau de dalt a la dreta que diu Publicar.


Pas 6: Donar d'alta la pàgina de Contacto

  Torna al menú lateral esquerre i clica a Páginas ➔ Añadir nueva.

  Escriu com a títol de la pàgina: Contacto i a la barra de la dreta de la teva pantalla (pestanya     "Página"): busca l'apartat Plantilla.

  Obre el desplegable (on posa Plantilla por defecto) i selecciona: Contacto.


Pas 5: Bloquejar l'error de Lectura (El pas clau de l'examen)

  Al menú lateral de l'esquerra, clica a Ajustes ➔ Lectura.

  A dalt de tot, a l'opció Tu página de inicio muestra, selecciona la casella: Una página estática.

  Al desplegable de Página de inicio, tria la pàgina que vulguis utilitzar com a principal (per exemple, "Inicio" o "Página de ejemplo").

⚠️ NOTA DE CONTROL: Al desplegable de Página de entradas, obre'l i posa'l a la línia en blanc de dalt de tot que diu - Seleccionar -. Deixa-ho totalment buit per evitar que WordPress trenqui la teva plantilla.

Clica al botó blau de baix de tot: Guardar cambios.

--------------------------------------------------------------------------------------------

PAS 5: Exportació de base de dades SQL desde Mariadb

1. Mira a la columna de l'esquerra de tot (sota el logotip de phpMyAdmin). Fes clic directament sobre la paraula que diu wordpress (que és la base de dades on s'ha guardat tot el teu llistat d'entrades de l'examen).

2. Clicar al menú superior gris d'icones a la pestanya Exportar.

3. Clicar al menú superior gris d'icones a la pestanya Exportar.


Posar imatge devora parragraf

<p>
  <img src="ruta-de-tu-imagen.jpg" alt="Descripción de la imagen" style="float: right; margin-left: 15px; width: 200px;">
  Este es el texto del párrafo. Al usar la propiedad float right, la imagen se posicionará en el lado derecho de este bloque de texto.
</p>


<p>
    <img src="tu-imagen.jpg" alt="Descripción" style="float: right; margin-right: 15px; width: 200px;">
    Este texto aparecerá al lado derecho de la imagen. La propiedad float hace que el contenido envuelva a la imagen.
</p>


<p>
    <img src="tu-imagen.jpg" alt="Descripción" style="float: right; width: 150px; margin-right: 15px;">
    Este es el texto del párrafo. Como la imagen tiene la propiedad float a la izquierda, el texto se ajustará automáticamente por el lado derecho.
</p> 


<p>
    <img src="tu-imagen.jpg" alt="Descripción" style="float: left; width: 150px; margin-right: 15px;">
    Este es el texto del párrafo. Como la imagen tiene la propiedad float a la izquierda, el texto se ajustará automáticamente por el lado derecho.
</p>


