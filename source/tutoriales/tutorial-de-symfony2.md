---
layout: default

title: Tutorial de Symfony2
---

> Este tutorial de Symfony2 es la adaptación de la unidad 3 del curso
> [Desarrollo de aplicaciones web con Symfony2](https://centrovirtual.educacion.
> es/Symfony14/mentor.php/areasCursosWeb/mostrarInfoCurso/idCurso/958) del
> proyecto [Aula Mentor](http://www.aulamentor.es).

> [En este enlace](http://juandarodriguez.es/cursosf20/) puedes acceder
> al curso completo

El objetivo este tutorial es ofrecer una visión panorámica del framework PHP
para el desarrollo de aplicaciones web _Symfony2_. El desarrollo de una
sencilla aplicación web servirá como elemento vertebrador de este documento.
Para seguir el tutorial necesitarás un entorno con:

  * un servidor web con PHP 5.3.x (x>2)
  * un servidor MySQL 5
  * tu IDE o editor favorito

## Descripción de la aplicación

Vamos a construir una aplicación web para elaborar y consultar un repositorio
de alimentos con datos acerca de sus propiedades dietéticas. Utilizaremos una
base de datos para almacenar dichos datos que consistirá en una sola tabla con
la siguiente información sobre alimentos:

  * El nombre del alimento,
  * la energía en kilocalorías ,
  * la cantidad de proteínas,
  * la cantidad hidratos de carbono en gramos
  * la cantidad de fibra en gramos y
  * la cantidad de grasa en gramos,

todo ello por cada 100 gramos de alimento. Aunque se trata de una aplicación
muy sencilla, cuenta con los elementos suficientes para mostrar las
características básicas de _Symfony2_.

Utilizando algún cliente _MySQL_ (_phpMyAdmin_, por ejemplo) crea la siguiente base
de datos para almacenar los alimentos e introduce algunos registros para probar la
aplicación.


    CREATE TABLE `alimentos` (
      `id` int(11) NOT NULL AUTO_INCREMENT,
      `nombre` varchar(255) NOT NULL,
      `energia` decimal(10,0) NOT NULL,
      `proteina` decimal(10,0) NOT NULL,
      `hidratocarbono` decimal(10,0) NOT NULL,
      `fibra` decimal(10,0) NOT NULL,
      `grasatotal` decimal(10,0) NOT NULL,
      PRIMARY KEY (`id`)
    ) ENGINE=InnoDB  DEFAULT CHARSET=utf8;

## ¿Qué es _Symfony2_?

La respuesta rápida, no por ello menos cierta, es que _Symfony2_ es un
framework _PHP_ para el desarrollo de aplicaciones web. Sin embargo, los
creadores de _Symfony2_ no la darían por buena. Posiblemente tampoco aprobasen
completamente lo que voy a decir a continuación (¡son unos programadores,
además de excelentes, muy “quisquillosos”!).

El objetivo principal de _Symfony2_ no es tanto desarrollar otro framework _PHP_,
como desarrollar un conjunto de componentes estables, independientes, fácilmente
acoplables (que no acoplados) para formar sistemas más complejos, mediante los
cuales se puedan resolver problemas relacionados con el desarrollo de aplicaciones web
en _PHP_.

Obviamente no es necesario utilizar todos los componentes de _Symfony2_; gracias a
su absoluta independencia se pueden utilizar únicamente aquellos que se requieran
para resolver el problema en cuestión. _Symfony2_ se presenta así como una especie
de _Lego_ en el mundo del desarrollo en _PHP_.

Con estos componentes, uno de los problemas que se puede resolver es el de la
creación de un framework de desarrollo web. De hecho, uno de los primeros
productos que se han construido con estos componentes ha sido el framework de
desarrollo conocido como _Distribución Standard de Symfony2_, o más brevemente
_Symfony2_ sin más. Y es precisamente del estudio de esta distribución de lo
que trata este tutorial. Pero hay que tener en cuenta que  el término _Symfony2_
puede referirse tanto a los componentes de _Symfony2_ como a _El framework Symfony2,
o distribución standard de _Symfony2_.

El número de proyectos PHP que usan componentes de Symfony2 crece cada día. Drupal8,
phpBB, laravel, piwik, son algunos ejemplos de proyectos que han optado por usar los
componentes Symfony2 para su desarrollo.

Por último diremos que estos componente son los elementos de bajo nivel del framework
_Symfony2_, por lo que para utilizar dicho framework no es estrictamente necesario
conocerlos. Aunque hay que admitir que ayudaría bastante. De todas formas vamos a
aprovechar este apartado para enumerar los componentes de _Symfony2_, dando una breve
descripción de su funcionalidad. Pero, repetimos: no es necesario conocerlos para utilizar
el framework, que es de lo que trata este tutorial. Damos esta información con el fin de
calmar al curioso y de aclarar las posibles confusiones que se dan al hablar de _Symfony2_
entre los componentes y el framework. Obviamente, el contexto aclarará de que se habla
en cada ocasión.


* [DependencyInjection](http://symfony.com/doc/current/components/dependency_injection.html),
 automaticación de la Inyección de dependencias

* [EventDispatcher](http://symfony.com/doc/current/components/event_dispatcher.html), implementación
 del patrón observer

* [HttpFoundation](http://symfony.com/doc/current/components/http_foundation.html), proporciona una
capa orientada a objetos para el tratamiento del HTTP

* [DomCrawler](http://symfony.com/doc/current/components/dom_crawler.html),para navegar el DOM de un documento HTML

* [ClassLoader](http://symfony.com/doc/current/components/class_loader.html), autoloader que implementa PSR-0 standard (forma standard de autocargar classes
con espacios de nombre)

* [CssSelector](http://symfony.com/doc/current/components/css_selector.html), para convertir un CSS selector en un XPath

* HttpKernel, proporciona la parte dinámica de la especificación HTTP

* BrowserKit, simula el comportamiento de un Browser

* [Templating](http://symfony.com/doc/current/components/templating.html), proporciona elementos para construir sistemas de plantillas

* Translation, soporte para las traducciones

* Serializer, para crear arrays a partir de estructuras complejas como gráficos

* Validator, validaciones basadas en JSR-303 Bean Validation specification

* Security, infraestructura para el tratamiento de la autentificación y la autorización

* [Routing](http://symfony.com/doc/current/components/routing.html), potente sistema para asociar rutas a acciones

* [Console](http://symfony.com/doc/current/components/console.html), para desarrollar herramientas CLI

* [Process](http://symfony.com/doc/current/components/process.html), para ejecutar procesos del sistema

* Config, herramientas para cargar configuraciones de distintas fuentes

* [Finder](http://symfony.com/doc/current/components/finder.html), para encontrar archivos en el sistema de archivos

* [Locale](http://symfony.com/doc/current/components/locale.html), para tratar la localización cuando no está disponible la extensión `intl`

* [Yaml](http://symfony.com/doc/current/components/yaml.html), para la manipulación de archivos de configuración en formato YAML

* Form, herramientas para definir formularios, pintarlos y asociarle datos

Y estas son las “tripas” del monstruo _Symfony2_. No hablaremos más
acerca de estos componentes a lo largo del tutorial. Pero que sepas que existen.
Puede que te ayuden a resolver tu próximo proyecto, y muy probablemente sean
los ladrillos fundamentales con los que se construyan muchas de las
aplicaciones _PHP_ en un futuro no muy lejano.

## Instalación y configuración de _Symfony2_

A partir de este momento, y mientras no lo especifiquemos explicitamente,
cuando hablemos de _Symfony2_ nos estamos refiriendo al framework,
concretamente a la edición estándard. En este apartado vamos a instalar y
configurar _Symfony2_, y lo dejaremos listo para construir la aplicación de
gestión de alimentos sobre él. Bájate de
[http://symfony.com/download](http://symfony.com/download) la última versión
de la rama 2.0 de _Symfony2_. Verás que hay una modalidad normal y otra
_without vendors_. Utiliza la primera.

> Nota: La modalidad normal contiene todas las librerías de terceros (_vendors_)
> necesarias para comenzar a trabajar con el framework, mientras que la
> modalidad _without vendors_, como su nombre indica, viene sin estas librerías,
> razón por lo que hay que instalarlas posteriormente mediante una herramienta
> incluida con _Symfony2_ (`bin/vendors`) que utiliza el sistema de control de
> versiones `git` para bajar las últimas versiones desde el repositorio de
> _github_ [3], donde se encuentra todo el código de _Symfony2_.

Descompríme el archivo descargado en algún directorio accesible al servidor
web, esto es, dentro de su _Document root_. Para que la aplicación funcione,
el servidor web debe poder escribir en los directorios `app/cache` y
`app/logs`. Si estás utilizando un sistema operativo tipo _UNIX_ (_Ubuntu_,
_MacOSX_, etcétera), la forma más fácil de dar dichos permisos es:


    chmod -R 777 app/cache app/logs

Durante toda el tutorial suponemos que has hecho esta operación directamente
 en el _Document root_ del servidor web, de manera que tendrá la siguiente
 estructura de directorios:


    /var/www/    (o donde tengas mapeado tu Document root)
        |
        └── Symfony
           |
           ├── LICENSE
           ├── README.md
           ├── app/
           ├── bin/
           ├── deps
           ├── deps.lock
           ├── src/
           ├── vendor/
           └── web/

 Y que tanto el servidor web como el servidor de MySQL están instalado en la
 máquina local.

A continuación comprobamos que nuestro sistema cumple los requisitos mínimos
ejecutando por la interfaz de comandos la siguiente orden:


    php app/check.php

Si el resultado nos señala algún error, debemos resolverlo antes de continuar.
Una vez que pasemos al menos los requisitos obligatorios (_mandatory
requirements_), podemos ejecutar la demo que viene incorporada en la
distribución standard de _Symfony2_. Para ello apunta con tu navegador a la
siguiente _URL_:


    "http://localhost/Symfony/web/app_dev.php"

¡Y juega un poquito!, Por ejemplo, pica en _Run the demo_ y navega por los
distintos enlaces. Fíjate en la pinta que tienen las _URL’s_. La demo muestra
el código que genera las páginas de la propia demo. Fíjate en él
detenidamente. Verás que muestra dos partes: el del controlador y el de la
plantilla (_template_). Es decir, dos elementos del patrón de diseño MVC
(Modelo - Vista - Controlador).

Ya has visto en acción la primera aplicación construida con _Symfony2_. Ahora
vamos a describir la manera en que _Symfony2_ organiza el código.

_Symfony2_ organiza los archivos en dos grupos: los que deben estar directamente
accesibles al servidor web (_CSS’s_, _Javascript_, imágenes y el controlador frontal) y
los que pueden ser incluidos desde el controlador frontal (librerías _PHP_ y ficheros de
 configuración). Los primeros viven en el directorio `web`, y los segundos, según su funcionalidad,
están repartidos entre los directorios `app` , `src` y `vendor`.

> Nota: En una instalación en un entorno de producción, el **Document root** del
> servidor web (o del **Virtual host** dedicado para la aplicación), debe
> coincidir con el directorio `web`, y el resto de directorios deben ubicarse
> fuera del **Document root**. No obstante, en un entorno de desarrollo podemos
> relajarnos y, para no andar afinando las configuraciones del servidor web, se
> puede ubicar todo el código dentro del **Document root**.

> Para paliar el efecto de posibles despistes o malas prácticas por
> desconocimiento, pereza y otras fatales causas, los directorios `src` y `app`,
> contienen un fichero `.htaccess` que indica al servidor web que no debe
> mostrar su contenido.

Veamos ahora para que se utiliza cada uno de estos directorios.

### El directorio web

Poco hay que decir ya de este directorio, aquí encontraremos el controlador
frontal y todos los _assets_ de la aplicación: _CSS’s_, _Javascipts_,
imágenes, etcétera. Esta es la estructura del directorio:


    web
    ├── app_dev.php
    ├── apple-touch-icon.png
    ├── app.php
    ├── bundles
    │   ├── acmedemo
    │   ├── framework
    │   ├── sensiodistribution
    │   └── webprofiler
    ├── config.php
    ├── favicon.ico
    └── robots.txt

Podemos ver 3 scripts _PHP_:

  * `config.php` es un script que asiste en la configuración del framework. No 
es imprescindible. De hecho cuando uno se siente confortable con _Symfony2_, es
más sencillo realizar la configuración directamente sobre el código fuente. Pero
para empezar puede servir de ayuda. Si lo utilizas ten en cuenta los permisos de
los ficheros del directorio `app/config`, pues este script debe poder escribir 
allí.

  * `app.php` es el controlador frontal de la aplicación, es decir, es el 
script _PHP_ por el que pasan todas las peticiones. Este script decide el flujo 
que debe seguir la aplicación “observando” los parámetros que se hayan pasado en
cada petición (_request_). Un conjunto de parámetros determinado se denomina 
ruta. Veamos algunos ejemplos para aclararlo: en 
`http://tu.servidor.web/app.php/articulo/1`, `app.php`, es el controlador 
frontal y `articulo/1` es una ruta de la aplicación.

  * `app_dev.php` también es el controlador frontal de la aplicación. ¿Cómo?
¿dos controladores frontales? ¡eso no parece encajar con lo que acabamos de 
decir!. Bueno tranquilos, tiene su explicación. Se trata de lo que se denomina 
en _Symfony2_ el controlador frontal de **desarrollo**. En principio pinta lo 
mismo que `app.php`, pero le añade una barra de depuración que ofrece muchísima
información sobre todo lo relacionado con la ejecución del _script_. Puedes ver
la barra de depuración en la demo que has ejecutado hace un momento. Se 
encuentra abajo de la página. Explórala un poco, te asombrarás de la cantidad de
información que te proporciona. Cuando desarrollamos es muy conveniente utilizar
este controlador frontal, pero en producción NUNCA debe utilizarse, pues
daríamos a los usuario de la web información que podría comprometer nuestro 
sistema.

Por otro lado los _assets_ se ubicarán en el directorio `bundles/nombre_bundle`,
donde `nombre_bundle` es el nombre del _bundle_ al que pertenece el _asset_ en 
cuestión. Vale, ¿y que es un _bundle_?, pues por lo pronto quedate con que 
“es la unidad funcional de código que utiliza _Symfony2_“. Algo así como una de 
las piezas del Lego _Symfony2_. En la sección _Los Bundles: Plugins de primera
clase_ hablaremos de estos “personajes” con más detalle.

### El directorio app

La finalidad de este directorio es alojar a a los scripts _PHP_ encargados de
los procesos de carga del framework (lo que se conoce como **bootstraping**) y
a todo lo que tenga que ver con la configuración general de la aplicación. Los
archivos de este directorio son los encargados de **unir** y dar cohesión a
los distintos componentes del framework. Son especialmente importantes los
ficheros `autoload.php` y `AppKernel.php`, ya que hay que tocarlos cada vez
que extendemos el framework con nuevas funcionalidades, es decir cada vez que
incorporamos nuevos _bundles_ (vamos poniendo en circulación a esta palabreja
que usaremos hasta la saciedad).

En `autoload.php` se mapean los espacios de nombres contra los directorios en 
los que residirán las clases pertenecientes a dichos espacios de nombre. De esa
manera el proceso de autocarga de clases sabrá donde tiene que buscar las clases
cuando se _usen_ dichos espacios, sin necesidad de incluir explicitamente
(esto es, usando `include` o `require` ) los archivos donde se definen las
clases.

En `AppKernel.php`, se declaran los _bundles_ que se utilizarán en la 
aplicación. En el directorio `config` se encuentran los archivos de
configuración de la aplicación: `config.yml`, `routing.yml` y `security.yml`. El
sistema de configuración de _Symfony2_ permite trabajar con distintos entornos
de ejecución. Los más típicos son `prod`, para producción y `dev`, para 
desarrollo. Pero se pueden definir tantos entornos como deseemos. 

En el controlador frontal se indica qué entorno deseamos utilizar en la 
ejecución del script. Fíjate en la siguiente línea que aparece en los ficheros
`web/app_dev.php` y `web/app.php`:


    ...
    $kernel = new AppKernel('prod', false);
    ...

El primer argumento decide el entorno de ejecución que se utilizará. ¿Y para
que sirve esto?. _Symfony2_ utiliza este dato para saber qué ficheros de
configuración debe cargar. Supongamos, por ejemplo, que se especifica `dev`
como entorno de ejecución. Entonces, si existe el fichero `config_dev.yml` lo
cargará, y si no es así cargará `config.yml`. Lo mismo ocurre con los ficheros
`routing.yml`, `security.yml` y `services.yml`. Más adelante estudiaremos para
que sirven cada uno de ellos. Por lo pronto nos conformaremos con saber la
dinámica de funcionamiento. 

Los entornos proporcionan mucha flexibilidad a la hora de desarrollar una 
aplicación. Vamos a ilustrar con un ejemplo esta flexibilidad. Un caso que nos
encontramos habitualmente es que la aplicación que estamos construyendo debe
enviar e-mails. Es bastante molesto tener que disponer de cuentas reales y
gestionarlas para que podamos probar la aplicación mientras desarrollamos.

Podemos utilizar este sistema de configuración para indicar al framework que en
el entorno de desarrollo se envíen todos los e-mails a una sola cuenta, o 
incluso que no se envíen. 

Otro ejemplo típico podría ser el definir unos parámetros de conexión a la base
de datos para el entorno de producción y otro para el de desarrollo. 

Una estrategía muy adecuada para tratar con los ficheros de configuración cuando
queremos que haya partes comunes y partes diferentes en cada entorno, es
definir todos los parámetros comunes en el fichero `fichconfig.yml` (donde
`fichconfig` es `config`, `security`, `routing` o `services`), y los
particulares de cada entorno en el fichero `fichconfig_env.yml` (donde `env`
es `dev`, `prod` o cualquier otro nombre de entorno que usemos). Por último
importamos los primeros (comunes) desde los últimos (particulares) de la
siguiente manera: Inicio del fichero `fichconfig_env.yml`


    imports:
     - { resource: fichconfig.yml }
     ...

Puedes comprobar que esta es la estrategia utilizada por la distribución
standard de _Symfony2_ con los ficheros `config.yml`, `config_dev.yml` y
`config_prod.yml`. 

Para acelerar la ejecución de los scripts, la configuración, el enrutamiento y
las plantillas de twig son compiladas y almacenadas en el directorio `cache`.

Por otro lado, los errores y otra información de interés acerca de eventos que
ocurren cuando se ejecuta el framework, son registrados en archivos que se 
almacenan en el directorio `logs`. 

Por eso **los directorios `logs` y `cache` deben tener permisos de escritura 
para el servidor web**. Por último, en este directorio tan “denso”, encontramos 
la navaja suiza de _Symfony2_, la aplicación `app/console`. Prueba a ejecutarla
sin pasarle ningún argumento. Verás una lista con todas las tareas que se
pueden lanzar por línea de comandos.


    # php app/console

### El directorio vendor

Aquí se aloja todo el código funcional que no es tuyo. Es lo que 
tradicionalmente se conoce como librerías de terceros. Entre otras cosas, el
directorio contiene los componentes de _Symfony2_, el ORM _Doctrine2_ y el
sistema de plantillas _twig_. Cuando amplies tu aplicación con nuevos
_bundles_ de terceros instalados automáticamente con la aplicación
`bin/vendors`, será aquí donde se ubique el código.

### El directorio src

Es el directorio donde colocarás tu código. Más concretamente: tus _bundles_.
A base de utilizar este palabro acabarás por asimilarlo antes de que te lo
expliquemos :-).

### El directorio bin

El nombre de este directorio es un clásico en el mundo _UNIX_. En él se
colocan archivos ejecutables. La distribución standard solo trae el ejecutable
`vendors` que se utiliza, en combinación con el fichero `deps` (dependencias),
para instalar componentes de terceros (_vendors_). Y con esto acabamos la
descripción de los directorios de _Symfony2_. Ha llegado el momento de hablar
de los _bundles_, esos grandes desconocidos (¡por ahora!).

## Los Bundles: Plugins de primera clase

Si los creadores de _Symfony2_ hubieran elegido la palabra _plugin_ en lugar
de _bundle_, es probable que te hubieses hecho una idea más concreta de lo que
es un _bundle_. Pues bien, por lo pronto, piensa que un _bundle_ es un
_plugin_, por que no es ni más ni menos que eso.

Cualquier framework que se precie debe ofrecer un mecanismo de extensión que 
permita ampliar la aplicación sin compromenter la escalabilidad. Para ello las 
piezas que se añaden al sistema deben ser bloques prácticamente autónomos y con
una interfaz sencilla para engancharlos (_to plug_, en inglés) al sistema. A 
estos bloques se les conoce a lo largo y ancho de la galaxia con el nombre de
_plugin_.

¿Por qué los creadores de _Symfony2_ han decidido llamarles _bundles_ en su 
lugar? Lo mismo hay alguna razón teórica que se me escapa. Pero de lo que si
estoy seguro es de que hay una razón histórica: El antecesor de _Symfony2_, el
fantástico _symfony 1.x_ organiza el código en _aplicaciones_, que a su vez 
están formadas por _módulos_ con la implementación de las acciones. Además 
ofrece un mecanismo de extensión basado en _plugins_, los cuales también
organizan el código en _módulos_ con sus acciones. Pero a pesar de este 
paralelismo las aplicaciones son “más importantes” que los _plugins_. De hecho, 
las aplicaciones pueden usar módulos de los plugins, pero lo contrario no tiene 
sentido tal y como está concebido _symfony 1.x_. Con el tiempo los 
desarrolladores se dieron cuenta de que era más fácil de mantener y organizar
los plugins, ya que son bloques de código autónomos y fácilmente acoplables a la
aplicación. Este hecho llevó de forma natural a reorganizar la aplicación 
colocando todo el código funcional en los _plugins_. Las aplicaciones se 
quedaban prácticamente vacías de código y tan solo contenían ficheros de 
configuración. Así pues, en _Symfony2_ decidieron olvidarse del concepto de 
aplicación (en el sentido de _symfony 1.x_), y obligar a que todo el código 
funcional se organizase en _plugins_. Es como hacer a los _plugins_ ciudadanos
de primera clase del framework. 

Finalmente, para evitar cualquier confusión y dirimir la diferencia entre 
_plugin_ y aplicación, decidieron usar la palabra _bundle_. Y eso es todo. Si no
conoces _symfony 1.x_, seguro que hubieras preferido llamarle _plugin_. Y si lo
conoces es probable que también. En fin, lo que realmente debes saber:

> **Importante**

> Un _bundle_ no es más que un directorio que aloja todo aquello relativo a una
> funcionalidad determinada. Puede incluir clases _PHP_, plantillas,
> configuraciones, _CSS’s_ y _Javascript_.

## La aplicación _gestión de alimentos_ en _Symfony2_

Y llegó el momento de ponerse a cocinar código.

### Generación de un _Bundle_

La primera idea que debe quedar clara, expresada de manera simplista, es que
_“todo es un bundle”_ en _Symfony2_. Por tanto, si queremos desarrollar una
aplicación necesitaremos, por lo menos, tener un _bundle_ para alojar el
código de la misma. Comencemos por ahí. El siguiente comando de _Symfony2_ nos
ayuda a generar el esqueleto de un bundle de manera interactiva:

    
    # php app/console generate:bundle

A cada pregunta que nos hace le acompaña una pequeña ayuda. En primer lugar
nos pregunta por el espacio de nombre que compartiran las clases del _bundle_.
La recomendación, como se dice en el texto de ayuda del comando, es que
comience por el nombre del fabricante del _bundle_, el nombre del proyecto o
del cliente, seguido, opcionalemente, por una o más categorías, y finalizar
con el nombre del _bundle_ seguido del sufijo _Bundle_. Es decir el nombre
completo del espacio de nombres del _bundle_ debe seguir el siguiente patrón:

    
    Fabricante/categoria1/categoria2/../categoriaN/nombrebundleBundle

Ilustremos esto con varios ejemplos de nombres de _bundles_ válidos:

    
    AulasMentor/AlimentosBundle
    AulasMentor/Tutorial/AlimentosBundle
    AulasMentor/CursoSf2/Tutorial/AlimentosBundle
    Jazzyweb/AulasMentor/AlimentosBundle

Nos quedaremos con el último de los nombres para el _bundle_ que vamos a
construir. Con este nombre se quiere expresar algo así como que el _bundle_
`AlimentosBundle` ha sido creado por `Jazzyweb` (una empresa ficticia) para el
cliente `AulasMentor`. Como ves, cualquier nombre vale siempre que contenga un
nombre de fabricante (_vendor name_) y un nombre de _bundle_. En medio podemos
poner lo que queramos para organizar nuestro trabajo.

Introduce `Jazzyweb/AulasMentor/AlimentosBundle` como espacio de nombres del 
_bundle_. A continuación nos pregunta por el nombre del _bundle_. Y nos ofrece
una recomendación que es el mismo nombre del espacio de nombres anterior pero 
sin los separadores `/`. El nombre del _bundle_ es importante pues, en 
ocasiones, hay que referirse al _bundle_ por este nombre. Presiona `enter` para 
aceptar la sugerencia. 

El próximo paso es asignarle una ubicación en la estructura de directorios del
proyecto. La flexibilidad de _Symfony2_ permite que lo coloques donde quieras.
Pero es muy recomendable que lo coloques en el directorio `src`, ya que está 
pensado para alojar nuestro código. Si lo haces así, te ahorrarás tener que
incluir una línea de código en el fichero `app/autoload.php` para registrar el
espacio de nombre en el sistema de autocarga de clases. Esto último es así 
porque en dicho fichero ya se ha contemplado que todas las clases que se aloje
en `src` sean autocargadas asignándole como espacio de nombre raíz el mismo 
nombre que la estructura de directorios computada desde `src`. Presiona `enter`
para aceptar la sugerencia. 

Cuando termines de generar el _bundle_ verás como se ha creado en `src` el 
directorio `Jazzyweb/AulasMentor/AlimentosBundle`, es decir un directorio que 
tiene la misma estructura que el espacio de nombres que hemos asignado al
_bundle_. Esto es lo que se quiere decir de manera genérica en el párrafo
anterior. 

Los _bundles_ llevarán asociados algo de configuración. Como mínimo será 
necesario configurar las rutas que mapean las _URL’s_ en acciones del _bundle_.
_Symfony2_ admite 4 formas de representar las configuraciones: con ficheros 
_XML_, _YML_ o _PHP_, y mediante anotaciones, que es una manera de expresar 
parámetros de configuración en el propio código funcional aprovechando para ello
los comentarios de _PHP_. Más adelante tendremos ocasión de utilizar las 
anotaciones y las entenderás mejor. 

Llegados a este punto hemos de decir que la elección es una cuestión de gusto; 
discutir con alguien acerca de cual es la mejor opción sería una pérdida de
tiempo. Para el caso de la configuración de los _bundles_ (prácticamente para
definir rutas como veremos después) hemos elegido los fichero _YAML_ como
formato para la configuración. 

Selecciona (escribe) `yml` como formato de configuración. Por último contesta
`yes` a la pregunta de si quieres generar la estructura completa. Esta opción 
generará algunos directorios y archivos extra que siguen las recomendaciones de
_Symfony2_ para alojar código. Es posible que no los utilices, pero no hacen
“daño” y sugieren como debe organizarse el código. No obstante el programador 
tiene bastante libertad a la hora de organizar los archivos del _bundle_ como
quiera. 

Confirma la generación del código. Una vez generado, el asistente te realizará
dos preguntas más. Primera pregunta:

* ¿quieres actualizar automáticamente el Kernel? 

* y segunda pregunta, ¿quieres actualizar directamente el _routing_? 

Contesta a las dos afirmativamente.

Vamos a ver con más detalle las consecuencias de estas actualizaciones
automáticas. Por una parte el _bundle_, como ya hemos explicado, es un bloque
desacoplado y reutilizable de código que agrupa a una serie de
funcionalidades. Si queremos utilizarlo en nuestro proyecto debemos
“notificarlo” al framework. Es decir, hemos de “engancharlo”. Esto se hace
registrándolo en el archivo `app/AppKernel.php`. La primera actualización
automática ha realizado dicho registro. Abre ese archivo y fíjate como al
final del método `registerBundles()` aparece la siguiente línea:
    
    ...
    new Jazzyweb\AulasMentor\AlimentosBundle\JazzywebAulasMentorAlimentosBundle(),
    ...

Dicha línea ha sido insertada automáticamente como consecuencia de haber
respondido afirmativamente a la primera pregunta. El cometido de la línea es
registrar el _bundle_ recien creado en el framework para poder hacer uso del
mismo. La segunda actualización automática “enlaza” la tabla enrutamiento
general de la aplicación con la tabla de enrutamiento particular del _bundle_.
La tabla de enrutamiento es la responsable de indicar al framework como deben
mapearse las _URL’s_ en acciones _PHP_. Para ver como se ha realizado este
enlace mira el fichero `app/config/routing.yml`:

    
    JazzywebAulasMentorAlimentosBundle:
     resource: "@JazzywebAulasMentorAlimentosBundle/Resources/config/routing.yml"
     prefix:   /

Estas líneas han sido introducidas automáticamente como consecuencia de
contestar afirmativamente a la segunda pregunta. Observa que el apartado
_resource_ es la dirección en el sistema de ficheros de la tabla de
enrutamiento propia del _bundle_ que acabamos de crear. _Symfony2_ sabe
convertir `@JazzywebAulasMentorAlimentosBundle` en la ubicación del _bundle_
pues está debidamente registrado. Es importante que conozcas como se acopla un
_bundle_ a la aplicación, pues si falla la actualización automática del
`KernelApp.php` y/o del `routing.yml`, debes realizarlas manualmente. 

Ahora puedes echarle un vistazo al fichero `routing.yml` del _bundle_
(`src/Jazzyweb/AulasMentor/AlimentosBundle/Resources/config/routing.yml`).
Verás que existe una ruta mapeada contra una acción. Después explicaremos los
detalles de la ruta. Esta última ruta sirve para probar el _bundle_. Así que
accede desde tu navegador web a la siguiente _URL_ (que es la que se
corresponde con esta ruta de prueba)

    
    http://localhost/Symfony/web/app_dev.php/hello/alberto

Si todo va bien, obtendrás como respuesta un saludo. Puedes cambiar el nombre
del final de la ruta. Resumiendo: Para desarrollar nuestra aplicación hemos de
contar al menos con un _bundle_ para escribir el código. Según la complejidad
de la aplicación será más o menos adecuado organizar el código en varios
_bundles_. El criterio a seguir es el de agrupar en cada _bundle_
funcionalidades similares o del mismo tipo. Los bundles son bloques
desacoplados y tienen asociado un espacio de nombre. Para acoplar un bundle al
framework hay que :

  1. Registrar el espacio de nombre en el sistema de autocarga (fichero 
`app/autoload.php`. Este paso no es necesario si ubicamos al _bundle_ en el directorio `src`.

  2. Registrar al bundle en el fichero `app/AppKernel.php`. Esta operación se
puede hacer automáticamente a través del generador interactivo de _bundles_, 
pero si fallase por alguna razón (por ejemplo que los permisos de dicho archivo 
no estén bien definidos). Habría que hacerlo a mano.

  3. Importar las tablas de enrutamiento del _bundle_ en la tabla de
enrutamiento de la aplicación.


### Anatomía de un _Bundle_

Si has seguido las indicaciones que hemos dado en este tutorial, debes tener
en tu directorio `src` dos directorios: `Jazzyweb` y `Acme` . El primero se
corresponde con el _bundle_ que acabamos de crear, y el segundo es un ejemplo
que viene de serie con la distribución standard de _Symfony2_ y que contiene
el código de la demo con la que has jugado hace un rato. Vamos a utilizar este
último para realizar la _disección_ de un _bundle_, ya que está más rellenito
de código que nuestro recien horneado y esquelético _bundle_.

    
    Acme/
    └── DemoBundle
        ├── AcmeDemoBundle.php
        ├── Controller
        │   ├── DemoController.php
        │   ├── SecuredController.php
        │   └── WelcomeController.php
        ├── ControllerListener.php
        ├── DependencyInjection
        │   └── AcmeDemoExtension.php
        ├── Form
        │   └── ContactType.php
        ├── Resources
        │   ├── config
        │   │   └── services.xml
        │   ├── public
        │   │   ├── css
        │   │   │   └── demo.css
        │   │   └── images
        │   │       ├── blue-arrow.png
        │   │       ├── field-background.gif
        │   │       ├── logo.gif
        │   │       ├── search.png
        │   │       ├── welcome-configure.gif
        │   │       ├── welcome-demo.gif
        │   │       └── welcome-quick-tour.gif
        │   └── views
        │       ├── Demo
        │       │   ├── contact.html.twig
        │       │   ├── hello.html.twig
        │       │   └── index.html.twig
        │       ├── layout.html.twig
        │       ├── Secured
        │       │   ├── helloadmin.html.twig
        │       │   ├── hello.html.twig
        │       │   ├── layout.html.twig
        │       │   └── login.html.twig
        │       └── Welcome
        │           └── index.html.twig
        ├── Tests
        │   └── Controller
        │       └── DemoControllerTest.php
        └── Twig
            └── Extension
                └── DemoExtension.php

  * `AcmeDemoBundle.php` es una clase que extiende a 
`Symfony\Component\HttpKernel\Bundle\Bundle` y que define al _bundle_. Se 
utiliza en el proceso de registro del mismo (recuerda, en el fichero 
`app/AppKernel.php`). Todos los _bundles_ deben incorporar esta clase (bueno, el
 nombre cambiará según el nombre del _bundle_)

  * `Controller` es el directorio donde se deben colocar los controladores con 
las distintas acciones del _bundle_. Las acciones son las funciones (o métodos).
Lo lógico y recomendado, es crear una clase `Controller` por cada grupo de 
funcionalidades. Pero no es una exigencia, si quieres puedes colocar todas tus
acciones en el mismo controlador. Cuando se genera un _bundle_ se crea el 
controlador _DefaultController_.

  * `Dependency Injection`. Una de las características más sobresaliente de 
_Symfony2_ es el uso intensivo que hace de la _Inyección de Dependencias_, un 
potente patrón de diseño mediante el que se facilita la creación y configuración
de objetos que prestan servicios en una aplicación gracias a la gestión 
automática de sus dependencias. Contribuye a crear un código más desacoplado y 
coherente. Presentaremos este concepto en [este tutorial][di] dedicado en
exclusiva a la Inyección de Dependencias en _Symfony2_. Aunque no es un patrón
complicado, es dificil de explicar con precisión y claridad.
En este directorio se ubican las clases relacionadas encargadas de cargar los 
ficheros con los que se configura el bundle (`services.yml` por defecto) y lo 
que se denomina [_exposición semantica del bundle_][sem], un concepto que no
trataremoa aquí. 

[di]: http://juandarodriguez.es/tutoriales/inyeccion-de-dependencias-en-symfony2
[sem]: http://symfony.com/doc/current/cookbook/bundles/extension.html


  * `Resources`, es decir, recursos. Entendemos por recursos: los ficheros de 
configuración del _bundle_ (directorio `config`), los _assets_ que requiere el 
_bundle_ para enviar en sus respuestas (directorio `public`) y las plantillas 
con las que se _renderizan_ (pintan) el resultado de las acciones de los 
controladores (directorio `view`). Fíjate como en este _bundle_, las plantillas 
están organizadas en tres directorios (`Demo`, `Secured` y `Welcome`) cuyos 
nombres coinciden con los de los controladores.

  * `Test`, es el directorio donde viven las pruebas unitarias y funcionales del
_bundle_.

Estos son los directorios más típicos de cualquier _bundle_, de hecho son los
que se generan automáticamente con el comando `app/console generate:bundle`.
Sin embargo un _bundle_ puede tener muchos más directorios y ficheros,
organizados como su creador crea conveniente. En el caso del _bundle_
`AcmeDemoBundle`, puedes ver los siguientes “extras”:

  * `Form` es el directorio donde se colocarán las clases que definen los 
formularios de la aplicación.

  * `ControllerListener.php` describe un _event listener_ que es un mecanismo 
muy adecuado de extender y alterar el flujo del framework sin tener que tocar el
código original del componente del framework que utiliza dicho sistema. Se trata
de una característica avanzada de _Symfony2_ raramente utilizada cuando uno se 
esta iniciando.

  * `Twig` es un directorio propio de este _bundle_, en el que se ha 
implementado una extensión del sistema de plantillas.

Ahora ya nos encontramos con un mínimo bagaje para emprender el desarrollo del
_bundle_ `JazzywebAulasMentorAlimentosBundle` y, por tanto de la aplicación.

### Flujo básico de creación de páginas en _Symfony2_

La creación de páginas web con _Symfony2_ involucra tres pasos:

  1. Creación de la ruta que mapea la _URL_ de la página en una acción de algún 
controlador. Dicha ruta se registra en el archivo `Resources/config/routing.yml`
del _bundle_, que a su vez debe estar correctamente importado en el archivo de 
rutas general de la aplicación `app/config/routing`.

  2. Creación de dicha acción en el controlador correspondiente. La acción, 
haciendo uso del modelo, realizará las operaciones necesarias y obtendrá los 
datos crudos (raw), es decir sin ningún tipo de formato, que facilitará a una 
plantilla para ser pintados (renderizados). El código de los controladores debe 
ubicarse en el directorio `Controllers` del _bundle_.

  3. Creación de dicha plantilla. Esto se hace en el directorio 
`Resources/view`. Con el fin de organizar bien las plantillas, es recomendable 
crear un directorio con el nombre de cada controlador. También es muy 
recomendable utilizar _Twig_ como sistema de plantillas, aunque también se puede
utilizar _PHP_.

Estos pasos son, por supuesto, una guía general y mínima que debemos seguir en
la creación de las páginas de nuestra aplicación. No obstante, en muchos casos
tendremos que realizar otras operaciones que se salen de este flujo y que
tienen que ver más con la construcción del modelo de la aplicación.

### Definición de las rutas del _bundle_

Ya hemos visto que en _Symfony2_ todas las peticiones a la aplicación se
realizan a través de un script _PHP_ que se llama controlador frontal
(`app.php`). Este script “sabe” lo que debe devolver como respuesta al usuario
“mirando” la _ruta_ que lo acompaña en cada petición. Cada ruta en _Symfony2_
consiste en un conjunto de parámetros separados por el caracter `/`. Ejemplos
de _URL’s_ con rutas de _Symfony2_ serían:

    
    http://tu.servidor/app.php
    http://tu.servidor/app.php/listar
    http://tu.servidor/app.php/ver/4

Las rutas correspondientes serían:
    
    /
    /listar
    /ver/4

Es decir, los parámetros que siguen al controlador frontal. Esta forma de
pasar parámetros a través de la _URL_ mejora en varios aspectos a la clásica
_query string_ del tipo:

    
    ?param1=val1&param2=val2&...&paramN=valN

En primer lugar el usuario que utiliza el navegador “siente” que la _URL_ que
aparece en la barra de direcciones forma parte de la aplicación que está
utilizando. Por tanto, cualquier _URL_ llena de carácteres extraños y
demasiado larga redunda en una degradación estética. En segundo lugar y más
allá de cuestiones estéticas, cuando utilizamos query strings clásicas estamos
dando información innecesaria al usuario, ya que el nombre de los parámetros
(`paramX`) es algo que tiene sentido únicamente para la aplicación en el
servidor. Esta información extra, además de dar lugar a _URL’s_ horribles,
supone un problema de seguridad, ya que el usuario podría utilizarlas para
sacar conclusiones acerca de la aplicación y utilizarla para comprometerla.

No obstante el que usemos el sistema de rutas ĺimpias no significa que no 
podamos pasar valores de parámetros a través de una _query string_ clásica. De
hecho en algunos caso puede sernos de gran ayuda.

El aspecto de las _URL’s_ puede mejorar aún más si utilizamos el módulo `Rewrite`
del servidor web, ya que también podemos eliminar el nombre del controlador
frontal (`app.php`). Así además de mejorar el estilo de la _URL_, ocultamos al
usuario información acerca del lenguaje de programación que estamos utilizando
en el servidor. Nos quedarían _URL’s_ de este tipo:

    
    http://tu.servidor/
    http://tu.servidor/listar
    http://tu.servidor/ver/4

¡Mucho más legibles y elegantes!

En el directorio `web` existe un fichero `.htaccess` con el siguiente
contenido:

    
    <IfModule mod_rewrite.c>
      RewriteEngine On
      RewriteCond %{REQUEST_FILENAME} !-f
      RewriteRule ^(.*)$ app.php [QSA,L]
    </IfModule>

La función de dicho fichero es, precisamente, reescribir las rutas
anteponiendo `app.php`, de manera que no sea necesario especificar el
controlador frontal en la _URL_. Para que esto funcione es necesario que el
servidor web tenga instalado el módulo `Rewrite`, y permita el cambio de
directivas a través de ficheros `.htaccess`.

La siguiente tabla muestra las rutas que definiremos en nuestra aplicación y
la acción que deben disparar.

* `/`, mostrar pantalla inicio

* `/listar`, listar alimentos

* `/insertar`, insertar un alimento

* `/buscar`, buscar alimentos

* `/ver/x`, ver el alimento _x_


En _Symfony2_ las rutas se definen en el archivo `app/config/routing.yml`.
Para que los _bundles_ no pierdan la autonomía que debe caracterizarlos, las
rutas que se mapean en un controlador de un determinado _bundle_ deberían
definirse dentro del propio _bundle_. Concretamente en el archivo
`Resources/config/routing.yml` del _bundle_. Y para hacerlas disponibles a la
aplicación, se importa este último fichero en `app/config/routing.yml`.

> Nota:
>
>Aunque el sitio recomendado para ubicar el fichero `routing.yml` de un
>_bundle_ es `Resources/config`, _Symfony2_ no lo exige, ya que en el archivo
>`app/config/routing.yml`, que es el que realmente define las rutas, puedes
>indicar la ruta concreta de los archivos que se quieren importar.

Abre el archivo
`src/Jazzyweb/AulasMentor/AlimentosBunle/Resources/config/routing.yml` y borra
las siguientes líneas:

    
    JazzywebAulasMentorAlimentosBundle_homepage:
     pattern:  /hello/{name}
     defaults: { _controller: JazzywebAulasMentorAlimentosBundle:Default:index }

Las líneas que acabas de borrar definían la ruta de la acción de ejemplo que
se crea automáticamente al generar el bundle. Fíjate en la estructura de la
definición de una ruta; consisten en un identificador de la ruta
(`JazzywebAulasMentorAlimentosBundle_homepage`), que puede ser cualquiera
siempre que sea único en todo el framework, el patrón de la ruta (`pattern:
/hello/{name}`), que describe la estructura de la ruta, y la declaración del
controlador sobre el que se mapea la ruta (`defaults: { _controller:
JazzywebAulasMentorAlimentosBundle:Default:index }`). Creamos nuestra primera
ruta añadiendo al archivo anterior lo siguiente:

    
    JAMAB_homepage:
     pattern:  /
     defaults: { _controller: JazzywebAulasMentorAlimentosBundle:Default:index }

Como el nombre de la ruta debe ser único en toda la aplicación, es una buena
práctica nombrarlas anteponiendo un prefijo con el nombre del _bundle_, o con
algo que lo identifique. Como el nombre de nuestro _bundle_ es muy largo,
hemos optado por usar como prefijo las siglas `JAMAB`. Una vez definida la
ruta debemos implementar la acción del controlador especificada en la misma,
es decir `JazzywebAulasMentorAlimentosBundle:Default:index`.

> Nota
>
>Fíjate en el patrón que se utiliza para especificar la acción del controlador:
>`JazzywebAulasMentorAlimentosBundle:Default:index`. A esto se le llama en
>_Symfony2_ un nombre lógico. Está compuesto por el nombre del _bundle_, el
>nombre del controlador, y el nombre de la acción separados por el caracter
>`:`. En este caso, el nombre lógico hace referencia a el método
>`indexAction()` de una clase _PHP_ llamada
>`Jazzyweb\AulasMentor\AlimentosBundle\Controller\DefaultController`. Es decir,
>hay que añadir el sufijo `Controller` al nombre del controlador, y el sufijo
>`Action` al nombre de la acción.

### Creación de la acción en el controlador

Editamos el fichero
`src/Jazzyweb/AulasMentor/AlimentosBundle/Controller/DefaultController.php`, y
reescribimos el método `indexAction()`:

    
~~~php
<?php 

namespace Jazzyweb\AulasMentor\AlimentosBundle\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\Controller;

class DefaultController extends Controller
{

   public function indexAction()
   {
       $params = array(
           'mensaje' => 'Bienvenido al curso de Symfony2',
           'fecha' => date('d-m-yy'),
       );

       return $this->render('JazzywebAulasMentorAlimentosBundle:Default:index.html.twig',
       $params);
   }
}
~~~

Analicemos el código anterior. La clase `DefaultController` “vive” en el
espacio de nombres `Jazzyweb\AulasMentor\AlimentosBundle\Controller`, por lo
que su nombre completo es
`Jazzyweb\AulasMentor\AlimentosBundle\Controller\DefaultController`.

La clase extiende de `Symfony\Bundle\FrameworkBundle\Controller\Controller`, la
cual forma parte de _Symfony2_ y, aunque no es necesario que nuestros 
controladores deriven de dicha clase, si lo hacemos nos facilitará mucho la 
vida, ya que esta clase base cuenta con potentes herramientas para trabajar con 
_Symfony2_.

Posiblemente la más útil sea el _Contenedor de Dependencias_ tambien conocido
como _Contenedor de Servicios_, con el que podemos obtener fácilmente
instancias bien configuradas de cualquier servicio del framework, tanto de los
incluidos en la distribución estándard, como de los que nosotros creemos o de
los que se añadan en las extensiones de terceros (_vendors_) que podamos
instalar. 

Quédate tranquilo con esto de los servicios pues será un tema que
abordaremos en [este tutorial][di]. Por lo pronto es suficiente con que sepas que los
servicios son objetos ofrecidos por el framework para realizar determinadas
tareas (como por ejemplo enviar emails o manipular una base de datos).

> Nota:
>
> **Sobre los espacios de nombre de PHP 5.3** Si en el código anterior 
utilizamos únicamente el nombre `Controller` en lugar del nombre completo 
`Symfony\Bundle\FrameworkBundle\Controller\Controller`, es por que previamente, 
hemos indicado mediante el _keyword_ `use`, que se va a utilizar la clase 
`Controller` de dicho espacio de nombre.

El método `indexAction()` es una _acción_ , es decir, un método que está
mapeado en una _URL_ a través de una ruta. Dichas rutas se definen en un
fichero, que utilizarás intensivamente cuando desarrollas aplicaciones con
_Symfony2_, denominado `routing.yml`. La acción `indexAction()` define un
array asociativo con los datos “crudos” (raw) `mensaje` y `fecha`, y se los
pasa a una plantilla para que los pinte. Esto último se hace en la línea 17
utilizando el método `render` de la clase padre
`Symfony\Bundle\FrameworkBundle\Controller\Controller`. 

Este método recibe dos argumentos, el primero es el nombre lógico de la 
plantilla que se desea utilizar, y el segundo es un array asociativo con los
datos. 

Las acciones terminan con la devolución de un objeto `Response`. Precisamente, 
el método `render` convierte una plantilla en un objeto de este tipo. El método
`render` es uno de los servicios disponibles en el framework y accesible desde
cualquier clase que derive de 
`Symfony\Bundle\FrameworkBundle\Controller\Controller`. Es un servicio que
usaremos hasta la saciedad. 

El nombre lógico de una plantilla, es similar al nombre lógico de un 
controlador; está compuesto por el nombre del bundle, el nombre del directorio 
que aloja a la plantilla en el directorio `Resources/view` (que suele coincidir 
con el nombre del controlador, en este caso `Default`), y el nombre del archivo
que implementa la plantilla (en este caso `index.html.twig`). Es decir que el
nombre lógico:
`JazzywebAulasMentorAlimentosBundle:Default:index.html.twig`, hace referencia
al archivo 
`src/Jazzyweb/AulasMentor/AlimentosBundle/Resources/view/Default/index.html.twig`.

### Creación de la plantilla

Siguiendo los pasos para la creación de una página en _Symfony2_, lo siguiente
que tenemos que hacer es crear la plantilla. Edita el fichero 
`src/Jazzyweb/AulasMentor/AlimentosBundle/Resources/view/Default/index.html.twig`
con el siguiente contenido:

~~~html
<h1>Inicio</h1>
<h3> Fecha: {% verbatim %}{{fecha}}{% endverbatim %} </h3>
{% verbatim %}{{mensaje}}{% endverbatim %}
~~~

Aunque _Symfony2_ permite el uso de _PHP_ como sistema de plantillas, en este
tutorial utilizaremos _Twig_, que es lo recomendado oficialmente. El código
anterior es una plantilla _twig_. En _twig_, el contenido dinámico, es decir,
los datos “crudos” que le son pasados desde el controlador (segundo argumento
del método `render` en la acción `indexAction()`), se referencian con dobles
llaves (`{% verbatim %}{{ dato }}{% endverbatim %}`). En el ejemplo anterior 
`{% verbatim %}{{ fecha }}{% endverbatim %}` hace referencia al
elemento `fecha` del array construido en el controlador, y 
`{% verbatim %}{{ mensaje }}{% endverbatim %}`,
como ya has deducido, al elemento `mensaje` de dicho array. Pues con esto
hemos terminado. Vamos a probar lo que acabamos de hacer. Introduce en la
barra de direcciones de tu navegador la _URL_ correspondiente a la ruta que
acabamos de crear. Utiliza el controlador de desarrollo:

    
    http://localhost/Symfony/web/app_dev.php/

¡Vaya! parece que nada de lo que hemos hecho ha funcionado. Vuelve a aparecer
la aplicación demo de _Symfony2_. Ahora prueba con el controlador de
producción:

    
    http://localhost/Symfony/web/app.php/

¡Ahora si! Vemos la pantalla de inicio de nuestro _bundle_. Pero entonces,
¿qué está pansando? las rutas tienen distinto sentido según el controlador
frontal que usemos. ¿Por qué?. La respuesta a este comportamiento se encuentra
en las distintas configuraciones que se cargan en función del entorno de
ejecución. Cuando utilizamos el controlador frontal de desarrollo
`app_dev.php`, se carga el fichero de routing `app/config/routing_dev.yml`. Si
le echas un vistazo al fichero verás que comienza con la siguiente ruta:

    
    _welcome:
        pattern:  /
        defaults: { _controller: AcmeDemoBundle:Welcome:index }

La cual colisiona con la que nosotros hemos creado, ya que el patrón de la
ruta es el mismo: `/`. El sistema de enrutamiento de _Symfony2_ va leyendo
todas las rutas y cuando encuentra una que coincide con la _URL_ que se ha
pedido, ejecuta la acción asociada. No sigue leyendo más rutas. Por eso, si en
un mismo proyecto hay dos rutas, o más precisamente, dos patrones de rutas que
coincidan, se ejecutará la primera que se encuentre. Atención por que no se
producirá ningún error. Esto hay que tenerlo muy en cuenta cuando se
desarrolla con _Symfony2_ para evitarnos algún que otro dolor de cabeza.

En el caso del controlador frontal de producción, el framework lee el fichero
`routing.yml`, ya que no existe `routing_prod.yml`. Mira el fichero y podrás
comprobar que no hay ninguna ruta que colisione con la que nosotros hemos
definido. Por tanto todo está bien y se ejecuta la acción correcta. Una vez
que sabemos las causas del problema, si queremos que el controlador de
desarrollo cargue la ruta de nuestro _bundle_, cualquier solución que
propongamos pasa por evitar la colisión entre rutas. Y para ello podemos hacer
varias cosas:

  1. Deshabilitar el plugin _AcmeDemoBundle_ y sus rutas.
  2. Cambiar el patrón de las rutas del plugin _AcmeDemoBundle_, anteponiendole 
a todas ellas un prefijo (`acme`, por ejemplo)
  3. Cambiar el patrón de las rutas del _Jazzyweb/AulasMentorAlimentosBundle_, 
anteponiendole a todas ellas un prefijo (`alimentos`, por ejemplo)

Con el fin de ilustrar una carácteristica del sistema de routing, hemos optado
por la 3ª solución. Podemos añadir un prefijo a todas las rutas del _bundle_
sin más que cambiar el parámetro `prefix` en la ruta importada en el archivo
`app/config/routing.yml`:

    
    JazzywebAulasMentorAlimentosBundle:
      resource: "@JazzywebAulasMentorAlimentosBundle/Resources/config/routing.yml"
      prefix:   /alimentos

Ahora, para ver la página de inicio de nuestro _bundle_, apuntamos nuestro
navegador a:

    
    http://localhost/app_dev.php/alimentos/

Y ya está! A partir de ahora todas las rutas de nuestro _bundle_ llevarán el
prefijo `alimentos` delante.

Atención

Como hemos cambiado un fichero de configuración, para que el cambio se haga
efectivo en el entorno de producción hay que borrar la caché con el siguiente
comando:

    
    # php app/console cache:clear --env=prod

### Decoración de la plantilla con un layout

Te habrás dado cuenta que hemos pintado un bloque _HTML_ incompleto. Si no te
has percatado de ello mira el código fuente _HTML_ que llega al navegador. Nos
falta someter a la plantilla al proceso de decoración, mediante el cual se le
añade funcionalidad. En el caso de la aplicación de _gestión de alimentos_ hay
que añadir la cabecera con el menú, el pie de página y los estilos.

> **Nota sobre el proceso de decoración:**

> En una aplicación web, muchas de las páginas tienen elementos comunes. Por
> ejemplo, un caso típico es la cabecera (donde se coloca el mensaje de
> bienvenida), el menú y el pie de página. Este hecho, y la aplicación del
> conocido principio de buenas prácticas de programación _DRY_ (_Don’t Repeat
> Yourself_, No Te Repitas), lleva a que cualquier sistema de plantillas que se
> utilice para implementar la vista utilice un conocido patrón de diseño: El
> _Decorator_, o Decorador. Aplicado a la generación de vistas la solución que
> ofrece dicho patrón es la de añadir funcionalidad adicional a las plantillas.
> Por ejemplo, añadir el menú y el pie de página a las plantillas que lo
> requieran, de manera que dichos elementos puedan reutilizarse en distintas
> plantillas. Literalmente se trata de _decorar_ las plantillas con elementos
> adicionales reutilizables.

El sistema de plantillas _twig_, está provisto de un mecanismo de herencia
gracias al cual la decoración de plantillas resulta de una flexibilidad y
versatilidad total. Podemos hacer cualquier cosa que nos imaginemos, como por
ejemplo fragmentar la vista en distintas plantillas organizadas por criterios
funcionales, y combinarlas para producir la vista completa. Podemos colocar en
una un menú, en otra un pie de página, en otra la estructura básica del
documento _HTML_, otra puede pintar un listado de _twits_, etcétera. La
herencia es un mecanismo típico de la programación orientada a objetos
mediante el que un componente software hereda todas las funcionalidades de
otro y puede extenderlas y/o cambiarlas. Es exactamente esto lo que ocurre
cuando una plantilla _twig_ hereda de otra. En _twig_ la herencia se
implementa mediante el concepto de bloque. En las plantillas podemos delimitar
_bloques_ que comienzan con un `{%verbatim%}{% block nombre_bloque %}{%endverbatim%}`
y finalizan con `{%verbatim%}{%endblock %}{%endverbatim%}`. Las plantillas
heredan todas las funcionalidades de las plantillas que extienden y pueden 
cambiar el código de los bloques
heredadados. Como siempre un ejemplo vale más que mil palabras. Fíjate en el
fichero `app/Resources/view/base.html.twig` que viene de serie en la
distribución standard de _Symfony2_: `app/Resources/view/base.html.twig`



* * *

[1]: http://example.com/ Drupal 8, phpBB4, Silex, son el nombre de algunos de los proyectos que han optado por utilizar los componentes de _Symfony2_.


* * *