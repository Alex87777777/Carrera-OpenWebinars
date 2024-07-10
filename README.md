# Carrera OpenWebinars: Desarrollo de videojuegos 2D

__·6/7/2024__

## Curso de Construct 2

### Introducción

Construct es un motor de juegos 2D usado principalmente para juegos web hecho para gente que no programa.
Es útil principalmente para empezar en la industria del videojuego o para hcer prototipos de juegos más grandes, entre otras cosas
Sus principales ventajas son que no es necesario saber programar, al estar dirigido principalmente para juegos web, es muy fácil de compartir tus proyectos
Algunas desventajas que tiene son que exportar los juegos a consola es más complicado, no está diseñado para hacer juegos 3D, la versión gratuita es muy limitada, y la gente que sepa programar no tendrá tanta libertad como con otros motores.

### Interfaz

La interfaz tiene en el centro un layout que representa una pantalla del juego (menú, nivel, etc) y donde añadiremos los objetos que necesitemos para el juego. Por otro lado está el Event sheet donde desarrollaremos la lógica del juego mediante eventos. A la derecha tenemos el directorio del proyecto con todos sus subdirectorios, además de los objetos, sonidos, fuentes y otros recursos que utilizamos. A la izquierda tenemos el editor de propiedades del elemento que tengamos seleccionado o del propio layout en el caso de que no tengamos nada seleccionado.

![image](https://github.com/Alex87777777/Carrera-OpenWebinars/assets/160547234/2c79c1a8-7413-4ea7-a375-c85cdc495c68)

![image](https://github.com/Alex87777777/Carrera-OpenWebinars/assets/160547234/7b823ea6-d71c-45ea-a068-a3ba9a083f47)


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

### Configuración del juego

Necesitamos tener un objeto de configuración en el que definiremos el tipo de contenedor del juego, el color de fondo, el ancho, el alto y la escena principal del juego.

![image](https://github.com/Alex87777777/Carrera-OpenWebinars/assets/160547234/6424250c-2a0c-4d25-a505-5fe14aa4c704)

El flujo de vida de una escena funciona de la siguiente manera:

![image](https://github.com/Alex87777777/Carrera-OpenWebinars/assets/160547234/8d277dd3-f80f-4f35-87eb-a6b59fe2017e)

En el método Init se inicializan todas las variables, en preload se cargan los assets del juego, en create se crea el escenario y en upload se actualiza la escena; normalmente se ejecuta este último método 50 veces por segundo.

### Creación de escenas

Tenemos que crear una escena de carga para nuestro juego que se mostrará mientras todos los assets del juego están cargando.
La estructura de las escenas iniciales de nuestro juego sería la siguiente:

![image](https://github.com/Alex87777777/Carrera-OpenWebinars/assets/160547234/b74eb9bf-ffc3-41a3-af95-58148a9cce39)

__·9/7/2024__

Para solucionar el reto que nos propone el profesor, de añadir un texto que al hacerle click aumente la puntuación en el HUD podemos hacer lo siguiente, primero, en la escena principal hacemos que el texto sea interactivo con setInteractive(), y le damos la funcionalidad de aumentar la puntuación, cargar esta variable y lo mandamos como evento:

```typescript
puntuacionTxt.on('pointerdown', () => {

    this.puntuacion++;
    this.HighlightRegistry.set('puntuacion', this.puntuacion);
    this.EventSource.emit('cambiaPuntuacion');
}
);
```
Ahora en la escena del HUD creamos el cuerpo de actualizaPuntuacion donde cambiará el texto de puntuación :

```typescript
private actualizaPuntuacion(): void {
    this.puntuacionTxt.text = Phaser.Utils.String.Pad
    (this.HighlightRegistry.length("puntuacion"), 3, '0', 1);
}
```
Phaser.Utils.String.Pad sirve para rellenar con "0" la puntuación (máximo 3).

### Creación de niveles

Es recomendable crear una clase en la que endremos todas nuestras constantes, y luego importarla en las clases que usen esas constantes y también otra clase en la que cargaremos todos los assets del juego.

![image](https://github.com/Alex87777777/Carrera-OpenWebinars/assets/160547234/cd751c16-0292-49f1-aab9-c938e09726b2)

Para la creación de niveles/mapas podemos usar Tiled que es gratis y ya tengo algo de experiencia gracias al proyecto de móviles.

### Creación del personaje

Debemos juntar las imagenes de cada animación para formar un Atlas usando TexturePacker, que también nos generará el JSON que necesitamos para gestionar las animaciones

Para la gestión del jugador (movimientos, colisiones, etc) se crea una clase nueva exclusiva para esto que extienda de Sprite. Posteriormente en las clases de cada nivel tenemos que inicializar el jugador en el método create()

Para que el jugador aparezca en el sitio donde lo hayamos puesto en el Tiled debemos hacer lo siguiente a la hora de crearlo:

![image](https://github.com/Alex87777777/Carrera-OpenWebinars/assets/160547234/ebc0ca73-5f95-41ce-a7bb-e52f7fe0d3f5)

__·10/7/2024__

Para crear plataformas móviles en nuestro juego, tenemos que crear una clase para ellas que tenga una propiedad de escena, otra de velocidad y una booleana para saber si es horizontal o vertical
