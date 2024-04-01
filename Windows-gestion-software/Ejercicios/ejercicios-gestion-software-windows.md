# Ejercicios

## 1
Eres el administrador de sistemas de una empresa que cuenta con una red de
computadoras en un entorno de trabajo. La empresa utiliza Windows 10 en todos 
los dispositivos de los empleados. Tu responsabilidad es configurar las 
políticas de actualizaciones de Windows para garantizar la seguridad y 
el rendimiento de los sistemas, al tiempo que minimizas las interrupciones en 
la productividad de los empleados.

### objetivos
- Configurar una política de actualizaciones que equilibre la seguridad y la 
    estabilidad del sistema con la productividad de los empleados.
- Minimizar las interrupciones causadas por las actualizaciones automáticas 
    durante las horas laborales.
- Garantizar que las actualizaciones críticas de seguridad se instalen de manera
    oportuna para proteger los sistemas contra vulnerabilidades conocidas

<!-- ### posible solución (problema abierto)
- Configurar actualizaciones automáticas: Establece las actualizaciones automáticas para que se descarguen e instalen automáticamente fuera del horario laboral, como durante la noche o los fines de semana.
- Definir la hora de reinicio después de las actualizaciones: Configura el tiempo de reinicio automático después de instalar actualizaciones durante horas fuera de horario laboral.
- Permitir posponer actualizaciones: Habilita la opción para que los usuarios puedan posponer las actualizaciones durante un período limitado, permitiendo cierta flexibilidad sin comprometer la seguridad. -->

## 2
Instalar (y desintalar) el intérprete de Python versión 3.11 en el sistema.
Comprobar que funciona.

## 3 
Instalar y comprobar que funciona el JDK (Java Development Kit) en el sistema.
Para comprobar que funciona, hacer lo siguiente:
- crear un fichero con un editor de texto y meter este código:
```java
class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!"); 
    }
}
```

- Guardar el fichero como `HelloWorld.java`.
- desde una terminal, compilar el fichero con `javac HelloWorld.java`
- una vez compilado, ejecutar el programa (desde la terminal) con `java HelloWorld`