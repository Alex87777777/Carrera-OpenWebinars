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

### Nuevas funcionalidades

Para crear plataformas móviles en nuestro juego, tenemos que crear una clase para ellas que tenga una propiedad de escena, otra de velocidad y una booleana para saber si es horizontal o vertical. También es necesario usar la función setFrictionX() para que el jugador no se caiga de ella al estar encima.
![image](https://github.com/Alex87777777/Carrera-OpenWebinars/assets/160547234/250e61c0-0496-4762-a568-6d06283c02dd)

Es recomendable que los audios que usemos estén en formato ogg ya que ocupan menos.

Cuando cambiemos de escena es recomendable hacer un fadeOut para que la transición sea más suave y agradable, además de detener los sonidos y añadir el HUD cuando la escena lo requiera.

![image](https://github.com/Alex87777777/Carrera-OpenWebinars/assets/160547234/99583de4-fdbd-429d-9cfa-d6ddc06fa886)

__·11/7/2024__

Para añadir un icono y título a nuestro juego para que se visualice en la pestaña del navegador necesitamos añadir las siguientes líneas de código al html principal:

```html
<title>Nombre del Juego</title>
<link rel="icon" type="image/png" href="icono.png">
```
Es importante que el icono sea de 16x16.

### Gestión de niveles

Para gestionar los niveles creamos una clase que se encargue de ello, y que tenga las propiedades nombre, vidas, puntuación, variables relacionadas con el mapa, jugador, enemigos, plataformas móviles, recolectables y sonidos. Además que podemos usar esta clase para no tener que crear clases para cada nivel.

__·12/7/2024__

### Guardado de datos

Para el guardado y cargado de datos tenemos que crear una clase llamada gestorBD que tenga en el constructor el siguiente código:

```typescript
if(JSON.parse(localStorage.getItem(Constantes.BASEDATOS.NOMBRE))) {
    this.datos = JSON.parse(localStorage.getItem(Constantes.BASEDATOS.NOMBRE));
} else {
    this.creaBD();
}

creaBD() {
    let bdinicial = {
        msuica: true,
        efectos: true,
        puntuaciones : {
            nivel1: 0,
            nivel2: 0,
            nivel3: 0
        }
        this.datos = bdinicial;
        localStorage.setItem(Constantes.BASEDATOS.NOMBRE, JSON.stringify(this.datos));
    }
}
```
Esto hace que si no tenemos una base de datos, se cree una con los ajustes por defecto. Cuando necesitemos cambiar uno de los ajustes o puntuación, debemos crear una base de datos en la clase que necesitemos parra luego cambiar sus datos, por ejemplo si queremos desactivar la música haríamos:

```typescript
mibd.datos.musica = !mibd.datos.musica;
```

Luego debemos guardar la base de datos. Para ellos necesitamos una función dentro de la clase gestorBD:

```typescript
grabaBD() {
        localStorage.setItem(Constantes.BASEDATOS.NOMBRE, JSON.stringify(this.datos));
}
```
Si queremos ver si se graban los datos correctamente, tenemos que hacer click derecho en el navegador, ir a inspeccionar, Application y Local Storage.

### Terminando el juego

En el archivo de configuración quitamos el ancho y alto que habiamos establecido anteriormente y lo sustituimos por el siguiente código:

```typescript
scale: {
    mode: Phaser.Scale.FIT,
    autoCenter: Phaser.Scale.CENTER_BOTH,
    width: 854,
    height: 480
},
```
De esta manera hacemos que el juego sea web responsive.

Para habilitar controles táctiles en nuestro juego debemos comprobar en el método create de la clase hud que el dispositivo de juego sea táctil:

```typescript
if(this.speechSynthesis.game.device.input.touch) {
    this.crearControles();
}

crearControles() {
    this.InputDeviceInfo.addPointer(2); // Permite el uso tanto de táctil como de teclado 
    this.controlIzda = this.add.sprite(350, 0, Constantes.CONTROL.IZQUIERDA).setInteractive();
    this.controlDcha = this.add.sprite(350, 0, Constantes.CONTROL.DERECHA).setInteractive();
    this.controlSalto = this.add.sprite(350, 0, Constantes.CONTROL.SALTO).setInteractive(); 
    // Añadimos los sprites de los botones
}
```
Cuando hayamos acabado, ejecutamos en la consola del Visual "npm run build", que nos creará un archivo javascript comprimido, el cual podremos subir a la plataforma que queramos junto con el html y los assets. Además de itch.io, también podemos subir nuestro juego a otras plataformas como gamejolt.

### Conversión a app de Android

Necesitamos tener instalado jdk 1.8, la última versión completa de gradle, android studio y cordova (este último se puede instalar a través de la consola mediante npm install -g cordova). 

Una vez que tengamos la APK debemos firmarlo con Android Studio para generar el ABB, que podemos publicar en Google Play. Para acceder a la consola de google Play debemos crear una cuenta de desarrollador que cuesta 25$.

__·13/7/2024__

## Curso de C#

### Introducción

C# es un lenguaje de programación orientado a objetos desarrollado por Microsoft para la plataforma .NET. Su sintaxis deriva de C y C++ y tiene un modelo de objetos similar al de Java.

### Particularidades con C#

En C# existe "<T>", que es un marcador de posición para un tipo de dato que está sin especificar, que se usa en la programación genérica. Por ejemplo podemos crear una clase genérica que nos permite que el tipo de dato se defina al crear una instancia:

```csharp
public class MyGenericClass<T>
{
    private T genericField;

    public MyGenericClass(T value)
    {
        genericField = value;
    }

    public T GetGenericValue()
    {
        return genericField;
    }
}
```
Esto nos permitiría hacer lo siguiente:

```csharp
var intInstance = new MyGenericClass<int>(10);
Console.WriteLine(intInstance.GetGenericValue()); 

var stringInstance = new MyGenericClass<string>("Hello");
Console.WriteLine(stringInstance.GetGenericValue()); 
```
Los tipos genéricos son útiles para escribir código más flexible y reutilizable.

Stack es una estructura de datos que sigue el principio LIFO (Last In, First Out) lo que significa que el último elemento que se inserta es el primero en retirarse. Se usa principalmente para procesar colecciones en orden inverso y cuenta con las siguientes funciones:

·Push: Inserta un elemento a la pila

·Pop: Devuelve y retira el último elemento que se insertó

·Peek: Igual que el anterior, pero sin retirarlo

·Count: Indica cuantos elementos tiene la pila

Por otra parte tenemos "Queue" que funciona al revés que la pila, con las funciones "enqueue" y "dequeue".

__·14/7/2024__

Linq nos ofrece métodos de extensión para listas que nos pueden ser muy útiles para algunas tareas.

·Where: Devuelve los elementos de una lista que cumplan cierta condición.
```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5 };
var evenNumbers = numbers.Where(n => n % 2 == 0);
```

·SelectMany: Se usa para unir varias colecciones en una sola.
```csharp
var lists = new List<List<int>> {
    new List<int> { 1, 2, 3 },
    new List<int> { 4, 5 },
    new List<int> { 6, 7, 8, 9 }
};
var allNumbers = lists.SelectMany(l => l);
```

·Any: Devuelve true si por lo menos un elemento de la colección cumple con la condición.
```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5 };
bool hasEvenNumbers = numbers.Any(n => n % 2 == 0);
```

·Select: Se usa para convertir cada elemento de la colección a una nueva forma o tipo.
```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5 };
var squaredNumbers = numbers.Select(n => n * n);
```

·First y FirstOrDefault: Devuelven el primer elemento de la colección que cumpla con una condición. Si no encuentra ninguno, First devuelve excepción mientras que FirstOrDefault devuelve null.
```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5 };
var firstEvenNumber = numbers.First(n => n % 2 == 0);
```

·Last y LastOrDefault: Funciona igual que el anterior pero en vez de ser el primer elemento, es el último.

·Take: Devuelve los primeros n elementos de una colección.
```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5 };
var firstThreeNumbers = numbers.Take(3);
```

·Skip: Salta los primeros n elementos de una colección.
```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5 };
var allButFirstTwoNumbers = numbers.Skip(2);
```

·OrderBy y OrderByDescending: Ordena una colección en función de un criterio.
```csharp
var numbers = new List<int> { 5, 3, 1, 4, 2 };
var orderedNumbers = numbers.OrderBy(n => n);

// Usando OrderBy la lista quedaría así {1,2,3,4,5}, en cambio con OrderByDescending quedaría así {5,4,3,2,1}
```

·GroupBy: Crea agrupaciones de elementos basadas en una clave común.
```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };
var groupedNumbers = numbers.GroupBy(n => n % 2);
// groupedNumbers crea dos grupos: uno con los números pares y otro con los impares
foreach (var group in groupedNumbers)
{
    Console.WriteLine($"Key: {group.Key}");
    foreach (var number in group)
    {
        Console.WriteLine(number);
    }
}
// Salida:
// Key: 1
// 1, 3, 5
// Key: 0
// 2, 4, 6
```

·OfType: Obtiene los elementos de un tipo específico de una colección que pueda contener varios.
```csharp
var mixedList = new List<object> { 1, "two", 3, "four", 5 };
var intList = mixedList.OfType<int>();
// intList contiene solo los enteros de la lista
```

·ToList y ToArray: Convierten una colección de tipo IEnumerable en una lista y array respectivamente.
```csharp
var numbers = new int[] { 1, 2, 3, 4, 5 };
var numbersList = numbers.ToList();
var numbersArray = numbers.ToArray();
```

·ToDictionary: Convierte una colección a un Diccionario usando una clave y un valor.
```csharp
var people = new List<(int Id, string Name)>
{
    (1, "Alice"),
    (2, "Bob"),
    (3, "Charlie")
};
var peopleDict = people.ToDictionary(p => p.Id, p => p.Name);
```

Para ejecutar procesos en segundo plano, existe una alternativa a la clase Thread, que es el uso de _async_ y _await_. 
Async sirve para marcar métodos que queramos que sean asíncronos, los cuales deben devolver void, Task o Task<T>. Por otra parte await se usa dentro de un método asíncrono para esperar una tarea sin bloquear el hilo actual.
```csharp
private async static void DownloadSongAsync()
{
    DownloadService ds = new DownloadService();
    byte[] futureSong = await ds.DownloadAsync("cancion");
}
```

__·15/7/2024__

### Introducción a WPF y XAML

XAML es un lenguaje de marcado utilizado principalmente en .NET para definir interfaces de usuario.
Por otra parte WPF es una tecnología usada para el desarrollo de aplicaciones de escritorio, que utiliza XAML para definir las interfaces.
Si ponemos un botón en la interfaz gráfica, por ejemplo, en nuestro archivo XAML tendríamos un código similar al siguiente:
```xaml
<Window x:Class="WpfApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="MainWindow" Height="350" Width="525">
    <Grid>
        <Button Content="Click Me" Width="100" Height="50" Click="Button_Click"/>
    </Grid>
</Window>
```

Algunos controles básicos de XAML son los siguientes:

·Grid: Es similar a una tabla de html

·Grid.RowDefinitions: Representa una fila de la tabla.

·Grid.ColumnDefinitions: Representa una columna de la tabla.

