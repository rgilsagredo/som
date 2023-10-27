# Software
Parte lógica del SI (lo que dicta cómo se tienen que hacer las cosas).
Tipos:

- Base: lo que hace que funcione el hardware: OS, firmware y drivers (entre 
otros)
- De aplicación: cualquier otra cosa que no sea software base, hace que el 
ordenador (SI) haga cosas.

Los datos o ficheros técnicamente son algo lógico, pero no se consideran 
software (un fichero txt no es técnicamente software, pero tampoco es
técnicamente hardware). Se entienden más como "materia prima" para el software.
Técnicamente, el software son datos (son ficheros).

## Programas
Es un conjunto de instrucciones que dicen cómo procesar unos datos (de entrada).
Una app es un conjunto de programas que permiten a un usuario hacer algo.

Un procesador solo sabe interpretar instrucciones numéricas (decirle a un
ordenador que sume es "decirle" algo en binario). Un programa en binario se dice
que están código máquina.

Como es inviable escribir programas en binario, se inventan los lenguajes
de programación, que son "idiomas" que son fáciles de aprender para humanos,
y ahí se pueden dar las instrucciones al ordenador. Luego un compilador/
intérprete traduce eso a lenguaje máquina, y el ordenador lo ejecuta.

### Clasificación de programas por nivel de abstracción
Según "lo cerca" que esté el lenguaje del código máquina se distingue:

- lenguajes de bajo nivel (ensamblador): se asigna un nombre a cada instrucción
numérica del procesador. Permite "agrupar" instrucciones simples bajo un nombre
para crear instrucciones complejas. Un programa en ensamblador es específico
para un procesador (ya que usa sus instrucciones)

- alto nivel: quita al programador los detalles del procesador; se parece a
"hablar". Es portable entre procesadores. Java, Python, JS serían los más 
conocidos

- medio nivel: Siendo de alto nivel, permite acceso a registros y direcciones de
memoria. C/C++ es el ejemplo

### Clasificación de programas por paradigma
El paradigma es la "filosofía" o estrategia a la hora de picar el código.

- Imperativa: dice el cómo hacer las cosas. Subparadigmas
    - Estructurada: con secuencia, condicional, loops y funciones haces todo. 
    Ejemplo: C
    - Modular: es lo anterior, pero dividiendolo en "cachos" (módulos), que
    se encargan de hacer una tarea concreta. En vez de 1 programa, serán varios
    que trabajan juntos; o uno (o varios) divididos en cachitos. Ejemplo: C
    - OOP: Object Oriented Programing. añade a lo anterior la organización
    del código en clases que interactuan entre sí. Es fácil pensar en OOP
    porque puedes traducir literalmente de tu lenguaje a código. Ejemplos:
    C++, Java, Python
- Declarativa: dice el qué se quiere hacer, pero no cómo. Subparadigmas:
    - Funcional: se basa en funcioanes matemáticas. Ejemplo: haskell, ruby, R
    - Lógica: se basa en predicados lógicos. Ejemplo: Prolog

Seguramente el lenguaje declatarivo más conocido sea SQL (dices lo que quieres,
pero dejas al engine que haga esa recuperación de información como quiera).

También, en general los lenguajes no son monoparadigma, si no que combinan
paradigmas según sea necesario.

### Clasificación de los programas según traducción a código máquina
Un lenguaje de programación no es código máquina; el ordenador no lo puede 
ejecutar. Hay que traducir a código máuina. Según sea esta tarducción puedes 
tener:
- compilados: se traduce y almacena en un fichero. ejecutar el programa es
ejecutar el ifichero compilado
    - No necesito herramienta de traducción para ejecutar el programa (el 
    procesador lo entiende)
    - Ejecución más rápida y menor consumo de recursos
    - El fichero compilado solo vale para procesadores con mismas instrucciones
    y mismo OS (misma plataforma). Si quires que corra en otra plataforma, 
    necesitas acceso al source code y hacer otra compilación.
    - Ejemplo: C. "Java"
- interpretados: se pasa a código máuina según se va ejecutando el programa
    - Hay que tener en el ordenador ejecutor instalado un intérprete que 
    traduzca
    - consume más tiempo y recursos
    - El mismo source code vale para cualquier plataorma
    - Ejemplo: shell, "Python"
- Entremedias: 
    - el source code se compila a bytecode, algo entre medias del lenguaje de
    programación y el código máquina
    - El bytecode se interpreta a código máquina
    - Bytecode es independiente de plataforma; por tanto se puede distribuir
    entre distintas plataformas.
    - El intérprete es el que depende de la plataforma
    - Ejemplo: Java, Python


## Licencias
Una licencia es lo que el propietario (del software) permite hacer con su 
propiedad (su software) a otra persona. El propietario legal puede ser una 
persona (el desarrollador) o una empresa (Oracle)

Hay 4 aspectos que el propietario puede conferir al usuario:
- uso: puede usar el software
- Acceso a código: el usuario puede ver el source code, y adaptarlo para sí
- Copia: se permite copia del programa para sí o terceros
- Mejora: se permite *publicar* mejoras/modificaciones del código

Las licencias se distinguen en como manejan estos aspectos:
- sin licencia, o de dominio público
- software libre
- software de código abierto
- software privativo

### Public Domain
Puedes hacer lo que quieras, a tu riesgo

### Libres
Tienes las 4 libertades: puedes usarlo, ver el source code, modificarlo,
publicar modificaciones y copiar

Hay 2 grandes familias que dan licencia de software libre
- tipo BSD (Berkeley Software Distribution). No pone restricciones. Ejemplos: 
    - BSD License
    - MIT License
    - Apache License
    - ISC License
    - Beerware License
- tipo GPL (copyleft). Esta impone que publicaciones de cosas GPL sigan siendo
GPL. Copyleft es la obligación de mantener la licencia. Ejemplos:
    - LGPL: permite usar el software en un proyecto que no sea libre
    - AGPL: obliga a entregar el source code al usuario, aunque no 
    necesariamente una copia. El ejemplo para entender es montarle a un cliente 
    un server: le doy el source code ya en el server, no le doy una copia

### Open source
Es prácticamente lo mismo que libre; se sigue un decálogo de normas para que un
software sea Open Source; a efectos prácticos es lo mismo que software libre

### Privativas
Lo que no cae en las cajas anteriores. Las modalidades de distribución más
usadas son:
- Freeware: distribución gratuita
- Shareware: versiones que se distribuyen para evaluación. suelen llevar 
restricción en tiempo de uso. No se pretende que sea completamente funcional,
es para ver qué tal funciona
- Crippleware: Una versión parcial de un producto. Funcionalidad limitada.
se llaman también versiones *Lite*. Son funcionales, aunque no son el producto
completo.

Podemos clasificar las licencias según destinatario:
- EULA: End User License Agreement. Para un único usuario final
- OEM: Original Equipment Manufacturer. Lo que viene incluido al comprar un 
sistema (por ejemplo un ordenado que te viene con windows, ese windows va con
licencia OEM)
- Corporativas: Se otorgan a usuarios de una empresa o entidad