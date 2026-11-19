# Práctica de Laboratorio PL2.1: Ordenamiento de estudiantes con Comparable y Comparator

- **Carrera:** Ingeniería en Ciencias de la Computación
- **Asignatura:** Estructura de Datos
- **Estudiantes:** Fernanda Doryan Cueva Maldonado, Melanie Anyanela Barzallo Alejandres

---

## 1. Objetivo
Implementar el orden natural de la clase `Estudiante` usando `Comparable<Estudiante>` y definir criterios externos con `Comparator<Estudiante>` (lambdas, referencias a métodos y criterios compuestos), comprobando desempates, estabilidad, conservación de los 20 registros y preservación de la lista original.

---

## 2. Estructura del proyecto
El proyecto está organizado con dos clases principales dentro de `src` y la documentación de pruebas dentro de `evidencias`:

```text
icc-est-u1-practica1/
├── evidencias/
│   ├── assets/
│   │   ├── c1.png ... c7.png       # Capturas de ejecución de las consultas C1–C7
│   │   ├── secuencias.png          # Captura de secuencias de códigos
│   │   ├── validaciones.png        # Captura de validaciones automáticas
│   │   └── casos_de_prueba.png     # Captura de casos borde y casos propios
│   └── PL2.1.md                    # Evidencias de ejecución y tabla de pruebas
├── src/
│   ├── Estudiante.java             # Modelo inmutable que implementa Comparable<Estudiante>
│   └── App.java                    # Datos base, consultas C1–C7, ejecución y validaciones
└── README.md
```

- **`Estudiante.java`:** Modelo de datos inmutable (`private final`) que encapsula los atributos del alumno e implementa el orden natural por código ascendente mediante `Comparable<Estudiante>`.
- **`App.java`:** Clase ejecutable que construye la lista base de 20 registros, genera copias independientes para cada consulta, aplica los comparadores requeridos y ejecuta las pruebas de verificación (apoyándose en `estaOrdenada()`).

---

## 3. Entorno de desarrollo
- **Versión del JDK:** Java 25 (25.0.4.1)
- **IDE / Editor:** Visual Studio Code
- **Gestión de dependencias:** Ninguna (bibliotecas estándar `java.util.*`)

---

## 4. Compilación y ejecución

### Desde la terminal
Ubicado en la carpeta raíz del proyecto (donde están `src` y `README.md`):

```bash
javac -encoding UTF-8 -d bin src/Estudiante.java src/App.java
java -cp bin App
```

### Desde Visual Studio Code
1. Abrir la carpeta raíz `icc-est-u1-practica1`.
2. Abrir `src/App.java`.
3. Hacer clic en **Run** sobre el método `main` o presionar `F5`.

---

## 5. Consultas implementadas (C1–C7)

| ID | Criterios de ordenamiento | Enfoque técnico | Método que la implementa |
| :--- | :--- | :--- | :--- |
| **C1** | Código ascendente | Orden natural (`Comparable<Estudiante>` + `Collections.sort()`) | `Estudiante.compareTo()` y `App.main()` |
| **C2** | Nombre ascendente; código ascendente | `Comparator` con lambda de dos parámetros en `List.sort()` | `App.main()` |
| **C2 (alt)** | Nombre ascendente; código ascendente | `Comparator.comparing()` + `thenComparing()` y referencias a métodos | `App.main()` |
| **C3** | Nota ascendente, sin desempate | `Comparator.comparingDouble(Estudiante::getNota)` | `App.main()` |
| **C4** | Nota descendente; nombre ascendente | `.reversed()` sobre la nota + `.thenComparing()` por nombre | `App.main()` |
| **C5** | Edad ascendente; nombre descendente | `Comparator.comparingInt()` + sentido inverso solo en el nombre | `App.main()` |
| **C6** | Carrera asc; nota desc; nombre asc; código asc | Composición encadenada de cuatro criterios | `App.main()` |
| **C7** | Nota ajustada desc; nombre asc; código asc | Lambda que calcula `Math.min(10.0, nota + 0.5)` solo en `Computacion` sin alterar la nota original | `App.main()` |

---

## 6. Análisis técnico

### Qué representa el signo de `compareTo()`
En `Estudiante.java`, `compareTo(Estudiante otro)` delega la comparación a `this.codigo.compareTo(otro.getCodigo())`. El signo del valor devuelto indica la posición relativa en orden natural ascendente:
- **Negativo (`< 0`):** el código del estudiante actual va antes que el de `otro`.
- **Cero (`0`):** ambos códigos son iguales.
- **Positivo (`> 0`):** el código del estudiante actual va después que el de `otro`.

### Inversión de comparadores compuestos (C4 y C5)
Si se encadenan primero todos los criterios y se llama a `.reversed()` al final del comparador completo, se invierten tanto el criterio principal como todos los criterios de desempate.
- En **C4** (nota descendente; nombre ascendente), invertir el comparador completo dejaría el nombre descendente por error. Por eso se aplica `.reversed()` únicamente a la nota antes de encadenar el nombre: `Comparator.comparingDouble(Estudiante::getNota).reversed().thenComparing(Estudiante::getNombre)`.
- En **C5** (edad ascendente; nombre descendente), la edad conserva su orden ascendente y la inversión se aplica solo al segundo criterio: `Comparator.comparingInt(Estudiante::getEdad).thenComparing(Comparator.comparing(Estudiante::getNombre).reversed())`.

### Empates por nota con códigos distintos
Al comparar únicamente por nota (como en **C3**), dos estudiantes diferentes con la misma calificación —por ejemplo `E014` y `E007`, ambos con `8.5`— devuelven `0`. Ese `0` significa equivalencia solo respecto a la nota, no igualdad entre los registros. Como `List.sort()` utiliza *TimSort* (un algoritmo estable), al encontrar empates conserva el orden relativo que esos registros tenían en la lista de entrada.

---

## 7. Resultados obtenidos y evidencias

Las capturas de consola, salidas verificadas y la tabla completa de pruebas se encuentran en [evidencias/PL2.1.md](evidencias/PL2.1.md).

### Secuencias completas de códigos
- **C1:** `E001, E002, E003, E004, E005, E006, E007, E008, E009, E010, E011, E012, E013, E014, E015, E016, E017, E018, E019, E020`
- **C2 (lambda y referencias):** `E005, E010, E014, E017, E015, E008, E020, E004, E012, E002, E019, E009, E003, E013, E018, E016, E011, E007, E006, E001` *(equivalencia verificada: `true`)*
- **C3:** `E012, E006, E016, E015, E003, E020, E013, E002, E019, E014, E007, E005, E008, E010, E018, E009, E004, E001, E011, E017`
- **C4:** `E017, E011, E001, E004, E009, E018, E014, E005, E010, E008, E007, E002, E019, E020, E003, E013, E015, E016, E012, E006`
- **C5:** `E001, E016, E004, E006, E003, E013, E017, E005, E007, E009, E019, E014, E010, E011, E018, E020, E008, E002, E012, E015`
- **C6:** `E001, E018, E005, E010, E014, E015, E016, E006, E017, E011, E004, E009, E003, E013, E012, E008, E007, E002, E019, E020`
- **C7:** `E001, E018, E017, E011, E004, E009, E005, E010, E014, E008, E007, E002, E019, E020, E003, E013, E015, E016, E006, E012`

### Coincidencias entre consultas y comprobaciones clave
- **`E001` (Sofia) ocupa el primer lugar en C1, C5, C6 y C7:** en **C1** por tener el código menor (`E001`); en **C5** por tener la edad mínima (`18`) y el nombre más alto en orden descendente (`Sofia` antes que `Mateo` y `Diana`); en **C6** por pertenecer a la primera carrera alfabética (`Computacion`) con la nota más alta (`9.5`); y en **C7** porque su nota ajustada alcanza el tope de `10.0`.
- **`E018` (Maria) coincide en el segundo puesto en C6 y C7:** en **C6** por ser la segunda nota más alta de `Computacion` (`9.2`) y en **C7** porque su nota ajustada (`9.7`) supera el `9.5` de los estudiantes de `Electronica`.
- **Registros de las tres `Ana` (`E005`, `E010`, `E014`):** aparecen ordenados por código ascendente en **C2**, **C6** y **C7** porque esas consultas incluyen el código como desempate explícito; en **C4** salen como `E014, E005, E010` al no desempatar por código y conservarse su orden de entrada por estabilidad.
- **Equivalencia de C2 y preservación de notas en C7:** la verificación automática entre ambas implementaciones de `C2` dio `true`. En `C7`, la salida muestra tanto `notaOriginal` como `notaAjustada` (`E001`: `9.5` -> `10.0`; `E018`: `9.2` -> `9.7`), confirmando que los objetos `Estudiante` no fueron modificados.

---

## 8. Conclusiones

1. Dejar el orden natural por código dentro de `Estudiante.compareTo()` funcionó bien para **C1**, porque el código es único y sirve como orden base del grupo. Para las demás consultas (**C2** a **C7**) fue mucho más práctico usar `Comparator` en `App.java`, ya que pudimos ordenar por nombre, nota, edad o carrera sin modificar la clase `Estudiante` ni cambiar su comportamiento original.
2. En las consultas con varios criterios comprobamos que tanto los desempates como el lugar donde se coloca `.reversed()` cambian por completo la salida. En **C4** invertimos solo la nota antes de encadenar el nombre para obtener primero a `E017`, `E011` y `E001`, mientras que en **C5** aplicamos el sentido descendente únicamente al nombre para mantener la edad ascendente. Además, en **C2**, **C6** y **C7** las tres estudiantes llamadas "Ana" aparecen como `E005, E010, E014` por el desempate explícito por código, a diferencia de **C4** donde salen como `E014, E005, E010`.
3. La estabilidad de `List.sort()` quedó demostrada en **C3** con los cinco estudiantes que tienen nota `8.5`: al no definir desempate, el resultado fue `E014, E007, E005, E008, E010`, que es exactamente el mismo orden relativo que tenían en la lista de entrada (lo mismo pasó en la prueba de **C4** con `[E014, E005, E010]`). Por último, crear una copia `ArrayList` nueva en cada consulta y declarar los atributos de `Estudiante` como `final` permitió que la lista base terminara intacta (`Original intacta: true`) y que en **C7** las notas reales de `E001` (`9.5`) y `E018` (`9.2`) no se alteraran al calcular el ajuste.  