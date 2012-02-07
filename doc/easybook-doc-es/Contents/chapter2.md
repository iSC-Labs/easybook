# Publicando tu segundo libro #

En el capítulo anterior se explica cómo crear y publicar fácilmente un libro con **easybook**. No obstante, apenas se mencionan algunas opciones de configuración y no se explican sus características más avanzadas. Este capítulo explica todos los tipos de contenidos disponibles, cómo crear y modificar las ediciones de un libro y cómo controlar su aspecto mediante las plantillas.

## Tipos de contenido ##

Los contenidos del libro se definen en la opción de configuración `contents` del archivo `config.yml`. Al crear un nuevo libro con el comando `new` sus contenidos por defecto son los siguientes:

    book:
        ...
        contents:
            - { element: cover }
            - { element: toc   }
            - { element: chapter, number: 1, content: chapter1.md }
            - { element: chapter, number: 2, content: chapter2.md }

La opción más importante de cada contenido es `element`, que define el tipo de contenido que se trata. **easybook** define actualmente los siguientes once tipos de contenidos, que siempre se indican mediante su palabra en inglés:

  * `acknowledgement` o agradecimientos.
  * `appendix` o apéndice. Parecido a un capítulo, pero normalmente se incluyen al final del libro de forma separada a los capítulos.
  * `author` o información sobre el autor/autores de la obra.
  * `chapter` o capítulo.
  * `cover` o portada.
  * `dedication` o dedicatoria.
  * `edition` o información sobre la edición del libro.
  * `license` o información sobre la licencia del libro.
  * `part` o sección. Se emplea normalmente para agrupar capítulos o apéndices.
  * `title` o portada interior. Se trata de la primera página que se ve en el interior del libro. Normalmente se trata de una versión simplificada de la portada y muestra el título, nombre del autor y edición.
  * `toc` o índice de contenidos.

Salvo `appendix`, `chapter` y `part` el resto de contenidos normalmente no requieren ninguna opción adicional:

    book:
        ...
        contents:
            - { element: cover }
            - { element: title }
            - { element: license }
            - { element: toc }
            - { element: chapter, number: 1, content: chapter1.md }
            - { element: chapter, number: 2, content: chapter2.md }
            - ...
            - { element: author }
            - { element: acknowledgement }

Los contenidos `appendix` y `chapter` admiten las siguientes opciones:

  * `number` o número del capítulo/appendix. Se utiliza para crear las etiquetas que acompañan a cada título de sección (`1.1`, `1.2`, `1.2.1`, `1.2.2`, etc.) **easybook** no restringe su formato, por lo que puedes utilizar números romanos (`I.1`, `I.2`), letras (`A.1`, `A.2`) o cualquier otro símbolo o cadena de texto.
  * `content` o nombre del archivo que tiene los contenidos de este elemento. El nombre del archivo debe incluir su extensión (`.md` en el caso de Markdown). El valor de esta opción se interpreta como la ruta a partir del directorio `Contents/` del libro, por lo que puedes utilizar todos los subdirectorios que quieras.

El contenido `part` admite la siguiente opción:

  * `title` o título de la sección. En un libro impreso, una sección simplemente es una página que separa unos capítulos de otros. En un libro web, la sección sólo se muestra en el índice de contenidos para separar los capítulos entre sí.

Los once tipos de contenidos de **easybook** son suficientes para publicar la mayoría de libros, pero si lo necesitas, el siguiente capítulo explica cómo crear nuevos tipos de contenido.

## Ediciones ##

**easybook** es tan flexible que permite publicar un mismo libro de formas radicalmente diferentes. Esto es posible gracias a las **ediciones**, que definen las características concretas con las que se publica el libro.

Las ediciones se definen bajo la opción `editions` en el archivo `config.yml`. Por defecto los libros creados con el comando `new` disponen de tres ediciones llamadas `print`, `web` y `website` con las siguientes opciones:

    book:
        ...
        editions:
            print:
                format:         pdf
                isbn:           ~
                auto_label:     true
                two_sided:      true
                page_size:      A4
                margin:
                    top:        25mm
                    bottom:     25mm
                    inner:      30mm
                    outter:     20mm
                toc:
                    deep:       2
                    elements:   ['appendix', 'chapter']

            web:
                format:         html
                auto_label:     true
                include_styles: true
                highlight_code: true
                toc:
                    deep:       2
                    elements:   ['appendix', 'chapter']

            website:
                extends:        web
                format:         html_chunked

El nombre de las ediciones debe ser único para un mismo libro y no puede contener espacios en blanco. Este mismo nombre se utiliza como subdirectorio dentro del directorio `Output/` del libro para distinguir los contenidos de cada edición. Puedes crear tantas ediciones como quieras, pero todas pertenecen a alguno de los tres tipos siguientes definidos por **easybook** mediante la opción `format`:

  * `pdf`, el libro se publica como un archivo PDF llamado `book.pdf`
  * `html`, el libro se publica como una página HTML llamada `book.html`
  * `html_chunked`, el libro se publica como un sitio web estático en un directorio llamado `book`.

Cada tipo de edición define sus propias opciones de configuración. Los tipos `html` y `html_chunked` disponen de las mismas opciones:

  * `auto_label`, si vale `true` se añaden *labels* o etiquetas a los títulos de las secciones (`1.1`, `1.2`, `1.2.1`, `1.2.2`, etc.)
  * `highligh_code`, si vale `true` se colorea la sintaxis de los listados de código del libro (esta opción todavía no se tiene en cuenta).
  * `include_styles`, si vale `true` se enlazan los estilos CSS por defecto de **easybook** en las páginas HTML generadas.
  * `toc`, establece las opciones del índice de contenidos. Sólo se tiene en cuenta si el libro incluye un contenido de tipo `toc`. A su vez, dispone de dos subopciones:
    * `deep`, indica el nivel de título máximo que se incluye en el índice (`1` es el valor más pequeño posible y equivale a mostrar sólo los títulos de nivel `<h1>`; el valor más grande es `6` y equivale a mostrar todos los títulos `<h1>`, `<h2>`, `<h3>`, `<h4>`, `<h5>` y `<h6>`).
    * `items`, indica el tipo de contenidos que se incluyen en el índice (por defecto sólo se muestran, si las hay, las secciones y los títulos de apéndices y capítulos).

Por su parte, el tipo de edición `pdf` dispone de las siguientes opciones:

  * `auto_label`, mismo significado y opciones que en las ediciones de tipo `html` y `html_chunked`.
  * `isbn`, idica el código ISBN-10 o ISBN-13 del libro impreso (esta opción todavía no se tiene en cuenta).
  * `include_styles`, si vale `true` el libro PDF se maqueta con los estilos por defecto de **easybook**. Así podrás crear libros bonitos sin esfuerzo. En el próximo capítulo se explica cómo añadir también tus propios estilos.
  * `margin`, permite indicar los cuatro márgenes de las páginas del archivo PDF: superior (`top`), inferior (`bottom`), interior (`inner`) y exterior (`outter`). En los libros impresos a una cara, los márgenes interior y exterior se interpretan respectivamente como margen izquierdo y derecho. Los valores de los márgenes se pueden indicar con cualquier unidad de medida válida en CSS (`25mm`, `2.5cm`, `1in`).
  * `page_size`, indica el tamaño de página del archivo PDF. Por el momento sólo soporta el valor `A4`, que corresponde al tamaño *DIN A-4*.
  * `toc`, mismo significado y opciones que en las ediciones de tipo `html` y `html_chunked`.
  * `two_sided`, si vale `true` el PDF está preparado para imprimirlo a doble cara. Si vale `false`, se prepara para imprimirlo a una cara.

Una última opción de configuración muy útil y disponible en todos los tipos de edición es `extends`. El valor de esta opción indica el nombre de la edición de la que *hereda* o extiende una edición. Cuando una edición *hereda* de otra, se copia el valor de todas sus opciones, que posteriormente se pueden redefinir en la *edición hija*.

Imagina por ejemplo que quieres publicar en PDF un mismo libro modificando ligeramente su aspecto. La versión borrador (`draft`) se publica a doble cara y con unos márgenes muy pequeños para ahorrar papel, la versión normal (`print`) se publica a una cara y con unos márgenes normales. La versión para publicar en el sitio lulu.com (`lulu`) es parecida a la versión normal, pero se publica a doble cara:

    book:
        ...
        editions:
            print:
                format:       pdf
                isbn:         ~
                auto_label:   true
                two_sided:    false
                page_size:    A4
                margin:
                    top:      25mm
                    bottom:   25mm
                    inner:    30mm
                    outter:   20mm
                toc:
                    deep:     2
                    elements: ['appendix', 'chapter']

            draft:
                extends:      print
                two_sided:    true
                margin:
                    top:      15mm
                    bottom:   15mm
                    inner:    20mm
                    outter:   10mm

            lulu:
                extends:      print
                two_sided:    true

La única limitación de la herencia es que sólo puede ser de un nivel, ya que una edición no puede heredar de otra que a su vez herede de una tercera.

## Temas ##

Se denomina **tema** al conjunto de plantillas, hojas de estilo y otros recursos que definen el aspecto de los contenidos del libro.  **easybook** ya incluye un tema para cada tipo de edición (`pdf`, `html`, `html_chunked`), por lo que tus libros se verán muy bien sin ningún esfuerzo. 

Los temas por defecto se encuentran en el directorio`app/Resources/Themes/`. Si quieres modificar el aspecto de tus libros, **no** toques los archivos de estos directorios. En el siguiente capítulo se explica cómo redefinir fácilmente para tu libro cualquier plantilla o recurso.

### Contenidos por defecto ###

En la mayoría de libros, los únicos elementos que definen de sus propios contenidos son los capítulos y los apéndices (utilizando la opción `content`). Por eso **easybook** define contenidos por defecto sensatos para algunos tipos de elementos. Así por ejemplo, si en el libro añades una portada interna (contenido de tipo `title`):

    book:
        ...
        contents:
            - ...
            - { element: title }
            - ...

**easybook** utiliza lo siguiente como contenido por defecto de este elemento:

    <h1>{{ book.title }}</h1>
    <h2>{{ book.author }}</h2>
    <h3>{{ book.edition }}</h3>

La portada interna por defecto simplemente muestra el título del libro, el nombre del autor o autores y la edición del libro. Todos estos valores se configuran bajo la opción `book` del archivo `config.yml`.

Los contenidos por defecto dependen tanto del elemento como del tipo de edición. Puedes observar estos contenidos en el directorio `Contents/` del tema. Si no quieres utilizar el contenido por defecto para un determinado elemento, simplemente añade la opción `content` e indica el archivo que define sus contenidos:

    book:
        ...
        contents:
            - ...
            - { element: title, content: portada-interna.md }
            - ...

### Plantillas por defecto ###

El contenido de cada elemento se *decora* con una plantilla antes de incluirlo en el libro definitivo. Las plantillas de **easybook** se crean con [Twig](http://twig.sensiolabs.org/), el mejor lenguaje de plantillas para PHP. Puedes ver todas las plantillas por defecto en el directorio `Templates/` del tema.

Observa por ejemplo la plantilla utilizada para decorar cada capítulo de un libro PDF:

    <div class="page:chapter new-page">

    <h1 id="{{ item.slug }}"><span>{{ item.label }}</span> {{ item.title }}</h1>

    {{ item.content }}

    </div>

Los datos del contenido que se decora se encuentran en una variable llamada `item`. A continuación se muestran las propiedades de esta variable:

  * `item.title`, título del elemento. Obtenido a través de su opción de configuración `title` o bien determinado automáticamente mediante los títulos por defecto que define **easybook**.
  * `item.slug`, es una versión el título que no incluye ni espacios en blanco ni otros *caracteres problemáticos* (eñes, acentos, comas, etc.) Su valor se utiliza en URL, como valor de atributos `id` de HTML, etc.
  * `item.label`, etiqueta del título principal del elemento. Normalmente sólo está definida para los capítulos (`Capítulo XX`), secciones (`Sección XX`) y apéndices (`Apéndice XX`).
  * `toc`, array con el índice de contenidos del elemento. Está vacía para la mayoría de elementos (`cover`, `license`, etc.) pero puede ser muy grande si se trata de un capítulo o apéndice complejo.
  * `item.content`, contenido del elemento listo para mostrarlo en el libro (ya se ha convertido desde su original en Markdown).
  * `item.original`, contenido original del elemento sin procesar ni manipular.
  * `item.config`, array con todas las opciones del elemento definidas en el archivo `config.yml`. Sus propiedades internas son `number`, `title`, `content` y `element`. Así por ejemplo, para determinar el tipo de elemento puedes utilizar la expresión `{{ item.config.element }}`.

Además de estas propiedades relativas al elemento que se está decorando, en todas las plantillas dispones de las siguientes tres variables globales:

  * `book`, proporciona acceso directo a todas las opciones de configuración definidas bajo `book` en el archivo `config.yml`. Así por ejemplo, puedes obtener el autor en cualquier plantilla mediante `{{ book.author }}` y la versión de **easybook** mediante `{{ book.generator.version }}`.
  * `edition`, proporciona acceso directo a todas las opciones de configuración de la edición que se está publicando actualmente.
  * `app`, proporciona acceso directo a todas las opciones de configuración y servicios de la propia aplicación **easybook** (definidos en el archivo `Application.php` del directorio `src/DependencyInjection/`).

### Estilos por defecto ###

El aspecto de los libros se controla mediante hojas de estilo CSS, incluso en el caso de las ediciones de tipo `pdf`. Los estilos por defecto se encuentran en el directorio `Templates/` del tema que utiliza la edición. En el caso de las ediciones `html` y `html_chunked`, el único límite es la versión de CSS que puedas utilizar en tu sitio web (CSS 2.1, CSS 3, CSS 4, etc.)

En el caso de la edición de tipo `pdf` el único límite es la imaginación. Los libros PDF se generan mediante la aplicación [PrinceXML](http://www.princexml.com/) convirtiendo un documento HTML en un archivo PDF con ayuda de una hoja de estilos CSS. Lo mejor de PrinceXML es que define decenas de nuevas propiedades CSS que no existen en el estándar y que permiten hacer cosas que simplemente parecen imposibles. Te recomendamos que eches un vistazo al archivo `styles.css.twig` del tema `Pdf/` para aprender algunas de las características más avanzadas. ¡Vas a alucinar! Y no olvides echar un vistazo también a la [ayuda de PrinceXML](http://www.princexml.com/doc/8.0/).

### Fuentes por defecto ###

Para que los libros publicados tengan un aspecto profesional, **easybook** incluye y utiliza varias fuentes libres y gratuitas de alta calidad. Puedes ver las fuentes y consultar la licencia de cada una en el directorio `app/Resources/Fonts/`.

### Títulos y etiquetas por defecto ###

**easybook** requiere para su funcionamiento que cada contenido disponga de su propio título. En el caso de los capítulos y apéndices, el título se incluye dentro de su propio contenido. En otros casos como el de las secciones, el título se define mediante la opción `title` en el archivo `config.yml`. Al resto de contenidos que no definen o incluyen un título, **easybook** les asigna un título por defecto que varía en función del idioma en el que está escrito el libro. Puedes ver los títulos que se aplican a los libros escritos en español en el archivo `app/Resources/Translations/titles.es.yml`.

Igualmente, si el libro incluye la opción `auto_labels`, se añaden etiquetas a los títulos de sección de los elementos `cahpter`, `appendix` y `part`. Los valores por defecto de las etiquetas en español se encuentran en el archivo `app/Resources/Translations/labels.es.yml`.

A diferencia de los títulos, las etiquetas contienen partes variables, como por ejemplo el número de apéndice o capítulo. Por ello, **easybook** permite definir cada etiqueta mediante una pequeña plantilla Twig:

    label:
        figure: 'Figura {{ item.number }}.{{ counter }} '
        part:   'Sección {{ item.number }} '
        table:  'Tabla {{ item.number }}.{{ counter }} '

La opción `item.number` es el número (o letra) incluido en la opción `number` del elemento dentro del archivo `config.yml`. En el caso de los apéndices y capítulos, es necesario definir seis etiquetas, cada una perteneciente a uno de los seis niveles de título (`<h1>`, `<h2>`, `<h3>`, `<h4>`, `<h5>` y `<h6>`,). El siguiente ejemplo hace que los apéndices sólo muestren una etiqueta en su título, por lo que dejan vacíos los cinco últimos niveles:

    label:
        appendix: ['Apéndice {{ item.number }} ', '', '', '', '', '']

Además de `item.number`, la plantilla de cada etiqueta tiene acceso a una variable especial llamada `counters` que es un array con los contadores de todos los niveles de título. Así, para hacer que las secciones de segundo nivel se muestren como `1.1`, `1.2`, ..., `7.1`, `7.2` puedes utilizar la siguiente expresión:

    label:
        chapter:
            - 'Capítulo {{ item.number }} '
            - '{{ counters[1] }}.{{ counters[2] }}'
            - ...

Generalizando el ejemplo anterior, puedes utilizar el siguiente código Twig para hacer que las etiquetas de los capítulos sean de tipo `1.1`, `1.1.1`, `1.1.1.1`, etc.:
            
    label:
        chapter:
            - 'Capítulo {{ item.number }} '
            - '{{ counters[0:2]|join(".") }}'  # 1.1
            - '{{ counters[0:3]|join(".") }}'  # 1.1.1
            - '{{ counters[0:4]|join(".") }}'  # 1.1.1.1
            - '{{ counters[0:5]|join(".") }}'  # 1.1.1.1.1
            - '{{ counters[0:6]|join(".") }}'  # 1.1.1.1.1.1

Este último ejemplo da una buena idea de todo lo que puedes conseguir aunando la flexibilidad de **easybook** y la potencia de Twig.