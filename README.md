# Carrera-OpenWebinars: Desarrollo de videojuegos 2D

__·6/7/2024__

## Curso de Construct 2

### Introducción

Construct es un motor de juegos 2D usado principalmente para juegos web hecho para gente que no programa.
Es útil principalmente para empezar en la industria del videojuego o para hcer prototipos de juegos más grandes, entre otras cosas
Sus principales ventajas son que no es necesario saber programar, al estar dirigido principalmente para juegos web, es muy fácil de compartir tus proyectos
Algunas desventajas que tiene son que exportar los juegos a consola es más complicado, no está diseñado para hacer juegos 3D, la versión gratuita es muy limitada, y la gente que sepa programar no tendrá tanta libertad como con otros motores.

### Interfaz

La interfaz tiene en el centro un layout que representa una pantalla del juego (menú, nivel, etc) y donde añadiremos los objetos que necesitemos para el juego. Por otro lado está el Event sheet donde desarrollaremos la lógica del juego mediante eventos. A la derecha tenemos el directorio del proyecto con todos sus subdirectorios, además de los objetos, sonidos, fuentes y otros recursos que utilizamos. A la izquierda tenemos el editor de propiedades del elemento que tengamos seleccionado o del propio layout en el caso de que no tengamos nada seleccionado.

__·7/7/2024__

### Puliendo/terminando el juego

Una vez tengamos hecho un prototipo del juego, tenemos que pulir el juego para hacerlo más atractivo, añadiendo efectos y detalles como el movimiento de la cámara o assets (audio, imagenes, etc) para mejorar el game feel.
Una buena página para publicar nuestro juego, una vez que lo tengamos acabado y exportado es itch.io ya que es gratis y muy fácil de usar para dar a conocer nuestro juego.

__·8/7/2024__

## Curso de Phaser

### Introducción a videojuegos HTML5

Los videojuegos HTML5 se ejecutan desde el navegador y no necesitan una instalación previa. Igualmente permite el uso de animaciones, sonido y juego en red entre otras cosas. El código de estos juegos está en Javascript y se ejecutan a través de un contenedor como podría ser WebGL.

### Introducción a Phaser

Phaser es un framework gratis y open source para desarrollo de videojuegos HTML5. Está desarrollado en Javascript moderno, aunque a partir de la versión 4 está hecho con Typescript.

### Introducción a Typescript

Typescript es un superconjunto de Javascript ES5 y ES6, por lo que cuenta con todas las funcionalidades de estes. Su ventaja que tiene sobre los anteriores nombrados es que cuenta con tipado estático como se puede ver en la siguiente imagen:

![image](https://github.com/Alex87777777/Carrera-OpenWebinars/assets/160547234/5627cec6-f050-4aa2-a11c-766335f28403)

También tiene la característica de privacidad para variables y es más eficaz para proyectos grandes. Sin embargo para poder ser ejecutado desde navegador, tendremos que traducir el código a javascript.

### Entorno de desarrollo 

Para trabajar con Typescript necesitamos instalar NodeJS y GitBash y pdoemos usar Visual Studio Code como entorno de desarrollo. Necesitamos crear un archivo JSON en el que estableceremos la configuración de compilado del proyecto:

![image](https://github.com/Alex87777777/Carrera-OpenWebinars/assets/160547234/0d36699a-e94b-462e-bc84-8e5682fa3bef)

Donde "target" es el lenguaje al que queremos traducir, "rootDir" es el directorio del proyecto y "outDir" el directorio destino.

