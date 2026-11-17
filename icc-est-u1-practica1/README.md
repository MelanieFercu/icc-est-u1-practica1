# Práctica de Laboratorio PL2.1: Ordenamiento de estudiantes con Comparable y Comparator

*Carrera:* Ingeniería en Ciencias de la Computación  
*Asignatura:* Estructura de Datos  
*Estudiante:* Fernanda Doryan Cueva Maldonado - Melanie Anyanela Barzallo Alejandres  

---

## 1. Entorno de Desarrollo
*Versión del JDK:* Java 21 (ajusta a JDK 21 o JDK 25 según lo que te muestre java -version en tu terminal)
*IDE / Editor:* NetBeans IDE 23 / Visual Studio Code
*Gestión de dependencias:* Ninguna (bibliotecas estándar java.util.*)

---

## 2. Estructura del Proyecto
El proyecto sigue una arquitectura minimalista compuesta exclusivamente por dos clases en el paquete principal:
Estudiante.java: Modelo de datos inmutable que encapsula la información del alumno e implementa la interfaz Comparable<Estudiante>.
Main.java: Clase ejecutable que construye la colección base de 20 registros, genera copias independientes por cada consulta y aplica los distintos criterios de ordenamiento mediante Comparator.

### Compilación y Ejecución
Desde la terminal, ubicado en la carpeta raíz del código fuente (src):

```bash
javac Estudiante.java Main.java
java Main