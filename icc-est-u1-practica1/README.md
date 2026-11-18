# Práctica de Laboratorio PL2.1: Ordenamiento de estudiantes con Comparable y Comparator

*Carrera:* Ingeniería en Ciencias de la Computación  
*Asignatura:* Estructura de Datos  
*Estudiante:* Fernanda Doryan Cueva Maldonado - Melanie Anyanela Barzallo Alejandres  

---
## 1. Objetivo
Implementar el orden natural de la clase `Estudiante` usando `Comparable<Estudiante>` y definir criterios externos con `Comparator<Estudiante>` (lambdas, referencias a métodos y criterios compuestos), comprobando desempates, estabilidad, conservación de los 20 registros y preservación de la lista original.
---
## 2. Estructura del Proyecto
El proyecto sigue una arquitectura minimalista compuesta exclusivamente por dos clases en el paquete principal:
Estudiante.java: Modelo de datos inmutable que encapsula la información del alumno e implementa la interfaz Comparable<Estudiante>.
Main.java: Clase ejecutable que construye la colección base de 20 registros, genera copias independientes por cada consulta y aplica los distintos criterios de ordenamiento mediante Comparator.
---
## 3. Entorno de Desarrollo
*Versión del JDK:* Java 23
*IDE / Editor:* Visual Studio Code
*Gestión de dependencias:* Ninguna (bibliotecas estándar java.util.*)
---
## 4. Compilación y ejecución

### Desde la terminal
```bash
javac -d bin src/Estudiante.java src/App.java
java -cp bin App
```

### Desde Visual Studio Code
1. Abrir la carpeta raíz `icc-est-u1-practica1`.
2. Abrir `src/App.java`.
3. Hacer clic en **Run** sobre el método `main` o presionar `F5`.
---
## 5. Tabla

## 4. Consultas implementadas (C1–C7)

| ID | Criterios de ordenamiento | Método que la implementa |
| :--- | :--- | :--- |
| **C1** | Código ascendente (orden natural con `Comparable`) | `Estudiante.compareTo()` y `App.consultaC1()` |
| **C2** | Nombre ascendente; código ascendente (lambda de dos parámetros) | `App.consultaC2Lambda()` |
| **C2 (alt)** | Nombre ascendente; código ascendente (`comparing`, `thenComparing` y referencias) | `App.consultaC2Referencias()` y `App.comparadorC2Referencias()` |
| **C3** | Nota ascendente, sin desempate (`comparingDouble` y referencia a método) | `App.consultaC3()` y `App.comparadorC3()` |
| **C4** | Nota descendente; nombre ascendente (`reversed` y `thenComparing`) | `App.consultaC4()` y `App.comparadorC4()` |
| **C5** | Edad ascendente; nombre descendente (`comparingInt` y sentido independiente) | `App.consultaC5()` y `App.comparadorC5()` |
| **C6** | Carrera ascendente; nota descendente; nombre ascendente; código ascendente | `App.consultaC6()` y `App.comparadorC6()` |
| **C7** | Nota ajustada descendente (`+0.5` tope `10.0` en `Computacion`); nombre asc; código asc | `App.consultaC7()`, `App.comparadorC7()` y `App.calcularNotaAjustada()` |

---
## 6. Conclusiones

