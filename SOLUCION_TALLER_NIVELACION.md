# Taller de Nivelación Programación I a Programación II

**Institución:** Institución Universitaria Antonio José Camacho (UNIAJC)  
**Asignatura:** Programación II  
**Documento:** Solución del Taller de Nivelación PI a PII  

---

## Tabla de Contenido
1. [Parte Teórica: Markdown](#1-investigar-qué-es-markdown)
2. [Parte Teórica: Git](#git)
   - [Preguntas 1 a 14](#1-qué-es-un-repositorio-en-git-y-cómo-se-diferencia-de-un-proyecto-normal)
3. [Parte Teórica: Programación (Java y POO)](#programación)
   - [Preguntas 15 a 22](#15-cuáles-son-los-tipos-de-datos-primitivos-en-java)
4. [Parte Práctica: Ejercicios de Programación en Java](#parte-práctica)
   - [Ejercicio 1: Calculadora Básica](#ejercicio-1-calculadora-básica)
   - [Ejercicio 2: Contador de Vocales y Consonantes](#ejercicio-2-contador-de-vocales-y-consonantes)
   - [Ejercicio 3: Invertir una Cadena de Texto](#ejercicio-3-invertir-una-cadena-de-texto)

---

# Parte Teórica

## 1. Investigar qué es Markdown

**Markdown** es un lenguaje de marcado ligero creado en 2004 por John Gruber y Aaron Swartz. Su propósito principal es permitir a los desarrolladores y redactores escribir texto con formato fácil de leer y escribir en texto plano, el cual puede convertirse de manera directa y estándar a HTML estructurado y otros formatos.

### Características Principales:
- **Legibilidad:** El formato en texto plano es 100% entendible sin necesidad de ser procesado o renderizado.
- **Portabilidad:** Se almacena en archivos estándar con extensión `.md`, compatibles con cualquier editor de texto.
- **Ecosistema y GitHub (GFM):** Es el estándar universal para la documentación de software (archivos `README.md`), plataformas como GitHub, GitLab, foros técnicos (StackOverflow) y herramientas de gestión del conocimiento (Obsidian, Notion).

---

## Git

### 1. ¿Qué es un repositorio en Git y cómo se diferencia de un proyecto "normal"?
Un **repositorio en Git** es un directorio gestionado por el sistema de control de versiones Git que contiene no solo los archivos y carpetas del proyecto (el *árbol de trabajo*), sino también una base de datos interna oculta alojada en la carpeta `.git/`.

**Diferencias con un proyecto "normal":**
- **Historial completo:** Un proyecto normal solo representa el estado actual de los archivos en disco. Un repositorio Git guarda toda la historia de cambios cronológicos, quién los hizo, cuándo y por qué (mensajes de commit).
- **Control de versiones y ramas:** Permite crear líneas de desarrollo paralelas (ramas), experimentar sin alterar el código estable y fusionar cambios de manera controlada.
- **Capacidad de viaje en el tiempo:** En Git es posible restaurar el proyecto a cualquier punto exacto de su historia sin riesgo de pérdida de información.

---

### 2. ¿Cuáles son las tres áreas principales de Git (working directory, staging area/index y repository) y qué papel cumple cada una?

Git organiza el flujo de trabajo en tres áreas fundamentales:

```
+-------------------+      git add       +----------------------+     git commit     +----------------------+
| Working Directory | -----------------> | Staging Area (Index) | -----------------> | Repository (.git dir)|
| (Modificaciones)  | <----------------- | (Preparación)        | <----------------- | (Historial/Commits)  |
+-------------------+     git checkout   +----------------------+     git checkout   +----------------------+
```

1. **Working Directory (Directorio de Trabajo):**
   - Es el espacio en el sistema de archivos local donde se crean, editan y eliminan archivos directamente. Contiene el estado actual tangible del proyecto.
2. **Staging Area / Index (Área de Preparación):**
   - Es una zona intermedia (un archivo en `.git/index`) que almacena una instantánea de los cambios seleccionados que se incluirán en el próximo commit. Permite armar commits atómicos y organizados seleccionando únicamente lo que se desea guardar.
3. **Repository / Git Directory (Repositorio local / `.git`):**
   - Es la base de datos permanente donde Git almacena de manera comprimida e inmutable los objetos (`blobs`, `trees`, `commits`, `tags`) y las referencias a las ramas.

---

### 3. ¿Cómo representa Git los cambios internamente? (objetos blob, tree, commit y tag)

Git es fundamentalmente un almacén de objetos direccionable por contenido mediante sumas de verificación (hashes SHA-1 / SHA-256). Los cuatro tipos de objetos son:

1. **Blob (Binary Large Object):**
   - Almacena exclusivamente el contenido puro de un archivo (los datos crudos), sin guardar su nombre de archivo, permisos ni fecha.
2. **Tree (Árbol):**
   - Representa la estructura de directorios. Un objeto *tree* contiene una lista con punteros a *blobs* (con sus respectivos nombres de archivo y permisos) y punteros a otros *sub-trees* (subcarpetas).
3. **Commit (Confirmación):**
   - Representa una instantánea (*snapshot*) del proyecto en un momento dado. Apunta al *tree* raíz del proyecto, contiene metadatos (autor, committer, fecha, mensaje) y uno o varios punteros al commit padre (*parent commits*), formando un grafo acíclico dirigido (DAG).
4. **Tag (Anotado):**
   - Es una referencia inmutable a un commit específico que incluye metadatos adicionales (nombre del tagger, fecha, mensaje y firma GPG opcional), usado habitualmente para marcar versiones de lanzamiento (ej. `v1.0.0`).

---

### 4. ¿Cómo se crea un commit y qué información almacena un objeto commit?

**Cómo se crea:**
1. Se modifican archivos en el *Working Directory*.
2. Se pasan los cambios deseados al área de preparación mediante `git add <archivos>`.
3. Se ejecuta el comando `git commit -m "Mensaje descriptivo"`. En ese momento, Git genera los blobs, compone los trees correspondientes y crea el objeto commit.

**Información que almacena el objeto commit:**
- Identificador SHA único del objeto *Tree* raíz.
- Identificador(es) SHA del commit padre (o padres en caso de un merge).
- Datos del Autor (nombre, correo electrónico y timestamp de autoría).
- Datos del Committer (nombre, correo electrónico y timestamp de confirmación).
- Mensaje del commit.

---

### 5. ¿Cuál es la diferencia entre `git pull` y `git fetch`?

| Operación | ¿Qué hace? | ¿Modifica el Working Directory? | Seguridad |
| :--- | :--- | :---: | :--- |
| **`git fetch`** | Descarga todos los commits, ramas y referencias nuevas desde el repositorio remoto hacia las ramas de seguimiento remoto (ej. `origin/main`), pero **no** modifica los archivos locales de trabajo ni mueve la rama actual. | **No** | Muy seguro. Permite inspeccionar los cambios remotos antes de integrarlos. |
| **`git pull`** | Realiza internamente un `git fetch` seguido automáticamente de un `git merge` (o `git rebase`, según configuración) para integrar los cambios remotos en la rama local activa. | **Sí** | Puede generar conflictos de fusión inmediatos en el código de trabajo si hay divergencias. |

$$\text{git pull} = \text{git fetch} + \text{git merge}$$

---

### 6. ¿Qué es un branch (rama) en Git y cómo Git gestiona los punteros a commits?

Un **branch (rama)** en Git es simplemente un **puntero móvil y ligero** (de solo 41 bytes, que contiene el hash SHA-1) hacia un commit específico. 

**Gestión de punteros:**
- Git maneja un puntero especial llamado **`HEAD`**, el cual apunta a la rama local en la que nos encontramos trabajando actualmente.
- Cuando se realiza un nuevo commit, el nuevo commit se crea teniendo como padre al commit al que apuntaba la rama actual.
- Automáticamente, la rama activa avanza para apuntar al nuevo commit, y `HEAD` continúa apuntando a dicha rama.
- Crear o eliminar ramas en Git es una operación casi instantánea porque solo implica escribir o borrar un archivo de texto plano con un hash en el directorio `.git/refs/heads/`.

---

### 7. ¿Cómo se realiza un merge y qué conflictos pueden surgir? ¿Cómo se resuelven?

**Cómo se realiza:**
Se sitúa en la rama destino que recibirá los cambios y se ejecuta la orden de fusión:
```bash
git checkout main
git merge feature-login
```

**Tipos de merge:**
- **Fast-Forward:** Si la rama destino no tiene commits nuevos respecto a la rama origen, Git simplemente mueve el puntero hacia adelante.
- **3-Way Merge (True Merge):** Si ambas ramas divergieron, Git busca el ancestro común más cercano y genera un nuevo *commit de merge*.

**Conflictos:**
Surgen cuando dos ramas modificaron **las mismas líneas** de un mismo archivo de forma diferente, o si una rama eliminó un archivo mientras la otra lo editaba. Git detiene la operación y marca el conflicto en el archivo con delimitadores:
```text
<<<<<<< HEAD (rama actual)
String mensaje = "Hola Mundo";
=======
String mensaje = "Hola compañeros";
>>>>>>> feature-login
```

**Resolución:**
1. Abrir el archivo en conflicto y editar manualmente el código decidiendo qué versión conservar (o combinando ambas) y removiendo los delimitadores `<<<<<<<`, `=======` y `>>>>>>>`.
2. Guardar el archivo y agregarlo al área de staging: `git add <archivo-resuelto>`.
3. Finalizar la fusión completando el commit: `git commit`.

---

### 8. ¿Cómo funciona el área de staging (`git add`) y qué pasa si omito este paso?

El área de staging actúa como un filtro o mesa de preparación donde se ensambla con precisión qué cambios formarán parte del siguiente commit.
- Al ejecutar `git add <archivo>`, Git lee el contenido del archivo en el working directory, genera un nuevo objeto *blob* en la base de datos de objetos y actualiza el archivo índice (`.git/index`).
- **¿Qué pasa si omito este paso?**
  Si se intenta ejecutar `git commit` directamente sin haber añadido archivos al staging:
  - Git indicará `no changes added to commit` y **no creará ningún commit**.
  - Si se utiliza `git commit -a`, Git añadirá automáticamente los archivos modificados previamente rastreados, pero ignorará completamente los archivos nuevos no rastreados (*untracked files*).

---

### 9. ¿Qué es el archivo `.gitignore` y cómo influye en el seguimiento de archivos?

El archivo **`.gitignore`** es un archivo de configuración en texto plano ubicado en el repositorio que especifica patrones de nombres de archivos y carpetas que Git debe ignorar deliberadamente.

**Influencia en el seguimiento:**
- Los archivos que coincidan con las reglas de `.gitignore` no aparecerán como archivos sin seguimiento (*untracked*) en `git status` y no serán añadidos cuando se use `git add .`.
- **Importante:** Solo afecta a archivos que **no** han sido rastreados previamente. Si un archivo ya estaba en el repositorio antes de agregarlo al `.gitignore`, Git continuará rastreándolo a menos que se elimine del índice con `git rm --cached <archivo>`.
- **Casos de uso típicos:** Archivos binarios compilados (`.class`, `.jar`), archivos de configuración local con credenciales (`.env`), carpetas de dependencias y temporales del IDE (`.idea/`, `target/`, `build/`, `.vscode/`).

---

### 10. ¿Cuál es la diferencia entre un "commit amend" (`--amend`) y un nuevo commit?

- **`git commit --amend`:**
  - Modifica el commit más reciente reemplazándolo por uno nuevo.
  - Permite agregar cambios olvidados en el staging al último commit o corregir el mensaje del commit sin alterar el número total de commits en el historial.
  - **Efecto técnico:** Genera un commit completamente nuevo con un nuevo hash SHA que sustituye al anterior. *(Nota: No debe usarse si el commit ya fue subido a una rama pública compartida)*.
- **Nuevo commit (`git commit`):**
  - Crea una nueva confirmación independiente en la cadena histórica, conservando el commit anterior como su padre y extendiendo la línea del tiempo.

---

### 11. ¿Cómo se utiliza `git stash` y en qué escenarios es útil?

`git stash` toma el estado sucio del directorio de trabajo y del área de staging (cambios modificados y preparados) y los guarda en una pila de cambios no confirmados, dejando el directorio de trabajo limpio e idéntico al último commit (`HEAD`).

**Comandos principales:**
```bash
git stash              # Guarda los cambios actuales en la pila
git stash list         # Lista los stashes almacenados
git stash pop          # Aplica los cambios más recientes y los remueve de la pila
git stash apply        # Aplica los cambios pero los conserva en la pila
git stash drop         # Elimina un stash específico
```

**Escenarios útiles:**
- Cambiar urgentemente de rama para corregir un bug prioritario en producción sin tener que hacer un commit incompleto o desordenado de la tarea actual.
- Hacer `git pull` cuando el repositorio remoto tiene cambios que entran en conflicto temporal con modificaciones locales sin terminar.

---

### 12. ¿Qué mecanismos ofrece Git para deshacer cambios (por ejemplo, `git reset`, `git revert`, `git checkout`)?

| Comando | Propósito | Nivel de impacto | ¿Es destructivo? |
| :--- | :--- | :--- | :---: |
| **`git checkout -- <archivo>`** | Descarta modificaciones no guardadas en el Working Directory restaurando el archivo a la versión del commit actual o staging. *(En Git moderno sustituido por `git restore <archivo>`)*. | Archivos individuales | **Sí** (cambios locales no commiteados se pierden) |
| **`git reset`** | Mueve el puntero de la rama hacia un commit anterior. Dispone de tres modos: <br>• `--soft`: Mantiene los cambios en el Staging Area.<br>• `--mixed` (default): Mantiene los cambios en el Working Directory.<br>• `--hard`: Descarta por completo todos los cambios en staging y working directory. | Historial local | **Sí** (en modo `--hard`) |
| **`git revert <commit-hash>`** | Crea un **nuevo commit** que aplica exactamente los cambios inversos al commit especificado, neutralizando su efecto sin reescribir la historia previa. | Historial público y local | **No** (ideal para ramas públicas compartidas) |

---

### 13. ¿Cómo funciona la configuración de remotos (origin, upstream) y qué comandos uso para gestión de forks?

- **`origin`:** Es el alias por defecto que Git asigna al repositorio remoto principal desde el cual se clonó el proyecto (por ejemplo, el fork propio en GitHub).
- **`upstream`:** Es el alias convencional que se le da al repositorio original (matriz) del cual se realizó el fork, utilizado para sincronizar actualizaciones del proyecto principal.

**Comandos para la gestión de forks y remotos:**
```bash
# 1. Ver los remotos configurados actualmente
git remote -v

# 2. Agregar el repositorio original como remoto upstream
git remote add upstream https://github.com/autor-original/proyecto.git

# 3. Traer los cambios más recientes del repositorio matriz
git fetch upstream

# 4. Sincronizar la rama main local con el repositorio matriz
git checkout main
git merge upstream/main

# 5. Subir los cambios actualizados a nuestro propio fork
git push origin main
```

---

### 14. ¿Cómo puedo inspeccionar el historial de commits (por ejemplo, `git log`, `git diff`, `git show`)?

- **`git log`:** Muestra la lista cronológica inversa de commits.
  - `git log --oneline --graph --decorate --all`: Muestra un diagrama visual y compacto de todas las ramas y commits.
  - `git log -n 5`: Muestra los últimos 5 commits.
- **`git diff`:** Muestra las diferencias línea por línea entre estados:
  - `git diff`: Muestra cambios entre el *Working Directory* y el *Staging Area*.
  - `git diff --staged`: Muestra cambios entre el *Staging Area* y el último commit (`HEAD`).
  - `git diff commitA commitB`: Compara dos commits o ramas diferentes.
- **`git show <objeto/commit>`:** Muestra los metadatos completos y el diff detallado de un commit específico, o el contenido de un tag/blob.

---

## Programación

### 15. ¿Cuáles son los tipos de datos primitivos en Java?

Java cuenta con **8 tipos de datos primitivos**, fuertemente tipados y almacenados por valor directamente en la memoria Stack:

| Tipo | Categoría | Tamaño | Rango de Valores / Representación | Valor por defecto |
| :--- | :--- | :---: | :--- | :---: |
| `byte` | Entero | 8 bits (1 byte) | -128 a 127 | `0` |
| `short` | Entero | 16 bits (2 bytes) | -32,768 a 32,767 | `0` |
| `int` | Entero | 32 bits (4 bytes) | $-2^{31}$ a $2^{31}-1$ (aprox. -2,147 millones a +2,147 millones) | `0` |
| `long` | Entero | 64 bits (8 bytes) | $-2^{63}$ a $2^{63}-1$ (sufijo `L`) | `0L` |
| `float` | Decimal (Pto. flotante) | 32 bits (4 bytes) | $\approx \pm 3.40282347 \times 10^{38}$ (precisión simple, sufijo `f`) | `0.0f` |
| `double` | Decimal (Pto. flotante) | 64 bits (8 bytes) | $\approx \pm 1.79769313 \times 10^{308}$ (precisión doble) | `0.0d` |
| `char` | Carácter | 16 bits (2 bytes) | Carácter Unicode (`\u0000` a `\uffff`, ej. `'A'`, `'9'`) | `'\u0000'` |
| `boolean` | Lógico | 1 bit (teórico) | `true` o `false` | `false` |

---

### 16. ¿Cómo funcionan las estructuras de control de flujo como if, else, switch y bucles en Java?

Las estructuras de control dirigen el orden de ejecución de las instrucciones del programa según condiciones lógicas:

1. **Condicionales:**
   - **`if / else if / else`:** Evalúa una expresión booleana. Si es `true`, ejecuta el bloque correspondiente; de lo contrario, pasa a la siguiente rama alternativa.
   - **`switch`:** Evalúa una variable o expresión (enteros, `char`, `String`, `enum`) contra múltiples casos (`case`). Se utiliza la palabra clave `break` para evitar la ejecución en cascada (*fall-through*). En versiones modernas de Java se soportan expresiones lambda (`switch expressions`).
2. **Bucles / Iterativas:**
   - **`for` tradicional:** Utilizado cuando se conoce el número de iteraciones. Consta de inicialización, condición de parada y paso de incremento (`for (int i = 0; i < n; i++)`).
   - **`for-each` (Enhanced for):** Itera directamente sobre elementos de arreglos o colecciones (`for (String s : lista)`).
   - **`while`:** Evalúa la condición antes de cada iteración; si es `false` desde el inicio, el bloque nunca se ejecuta.
   - **`do-while`:** Evalúa la condición al final del bloque, garantizando que el código se ejecute **al menos una vez**.

---

### 17. ¿Por qué es importante usar nombres significativos para variables y métodos?

- **Legibilidad y Auto-documentación:** El código se lee muchas más veces de las que se escribe. Un nombre descriptivo como `calcularSalarioNeto()` o `saldoDisponible` explica la intención del código de inmediato sin requerir comentarios redundantes.
- **Mantenibilidad:** Facilita a otros desarrolladores (y a uno mismo en el futuro) entender, extender o corregir el código con menor riesgo de introducir errores.
- **Reducción de ambigüedades:** Evita nombres crípticos de una sola letra (como `x`, `temp`, `data2`) que dificultan el rastreo y la depuración del flujo de datos.
- **Convenciones estándar en Java:** Favorece el uso de buenas prácticas como *camelCase* para variables y métodos (`nombreUsuario`, `obtenerDatos`), y *PascalCase* para clases (`CalculadoraAvanzada`).

---

### 18. ¿Qué es la Programación Orientada a Objetos (POO)?

La **Programación Orientada a Objetos (POO)** es un paradigma de programación basado en el concepto de **objetos**, los cuales son entidades que agrupan tanto datos y estado (**atributos**) como comportamientos y funcionalidades asociadas (**métodos**). 

Su propósito es modelar problemas del mundo real en el software de manera modular, reutilizable y escalable, dividiendo sistemas complejos en componentes autónomos que interactúan entre sí a través de mensajes y contratos definidos.

---

### 19. ¿Cuáles son los cuatro pilares de la Programación Orientada a Objetos?

1. **Encapsulamiento:**
   - Consiste en ocultar el estado interno y la implementación detallada de un objeto, exponiendo solo una interfaz pública controlada (mediante métodos *getters* y *setters* y modificadores de acceso). Protege la integridad de los datos.
2. **Abstracción:**
   - Consiste en aislar las características esenciales de un objeto ignorando los detalles secundarios o de implementación no relevantes para el contexto. En Java se logra mediante clases abstractas e interfaces.
3. **Herencia:**
   - Mecanismo por el cual una clase secundaria (*subclase/hija*) adquiere los atributos y métodos de una clase principal (*superclase/padre*), permitiendo la reutilización y especialización de código.
4. **Polimorfismo:**
   - Capacidad de un mismo método o interfaz de comportarse de diferentes formas según el objeto que lo invoque. Se manifiesta mediante la **sobreescritura de métodos** (*Overriding* en tiempo de ejecución) y la **sobrecarga de métodos** (*Overloading* en tiempo de compilación).

---

### 20. ¿Qué es la herencia en POO y cómo se utiliza en Java?

La **herencia** es la relación jerárquica de tipo *"es un"* (*is-a*) que permite a una clase derivar de otra, reutilizando y extendiendo sus propiedades y comportamientos.

**Uso en Java:**
- Se implementa mediante la palabra reservada **`extends`**.
- Java **no admite herencia múltiple de clases** (una clase solo puede extender directamente de una única superclase), pero permite implementar múltiples interfaces mediante `implements`.
- Se usa la palabra clave **`super`** para invocar constructores o métodos de la clase padre.
- Todas las clases en Java heredan implícitamente de la clase raíz `java.lang.Object`.

```java
// Clase Padre
public class Vehiculo {
    protected String marca;
    public void arrancar() {
        System.out.println("El vehículo está en marcha");
    }
}

// Clase Hija que hereda de Vehiculo
public class Automovil extends Vehiculo {
    private int numeroPuertas;

    @Override
    public void arrancar() {
        System.out.println("El automóvil enciende con botón de arranque.");
    }
}
```

---

### 21. ¿Qué son los modificadores de acceso y cuáles son los más comunes en Java?

Los **modificadores de acceso** son palabras clave que delimitan el nivel de visibilidad y accesibilidad que tienen otras clases y paquetes sobre una clase, atributo, constructor o método:

| Modificador | Misma Clase | Mismo Paquete | Subclases (Herencia) | Cualquier Lugar (Global) |
| :--- | :---: | :---: | :---: | :---: |
| **`private`** | Sí | No | No | No |
| *(Default / Package-Private)* | Sí | Sí | No | No |
| **`protected`** | Sí | Sí | Sí (incluso en otro paquete) | No |
| **`public`** | Sí | Sí | Sí | Sí |

---

### 22. ¿Qué es una variable de entorno y por qué son importantes para Java o la programación en general?

Una **variable de entorno** es un valor dinámico configurado a nivel del sistema operativo que afecta el comportamiento de los procesos y programas en ejecución dentro del entorno del sistema.

**Importancia en Java y desarrollo de software:**
- **Localización de herramientas (`JAVA_HOME` y `PATH`):** Permite al sistema operativo y a las herramientas de compilación/ejecución (como Maven, Gradle, IDEs o la terminal) saber exactamente en qué directorio está instalado el JDK y encontrar binarios como `javac` y `java`.
- **Configuración desacoplada y seguridad:** Permite suministrar datos sensibles (claves de API, contraseñas de bases de datos, URLs de conexión) sin incluirlas en el código fuente versionado en Git, cumpliendo con principios de seguridad y la metodología *Twelve-Factor App*.
- **Flexibilidad entre entornos:** Facilita que la misma aplicación se ejecute en desarrollo, pruebas (staging) o producción simplemente cambiando los valores de entorno sin modificar el código compilado.

---

# Parte Práctica

A continuación se presentan las soluciones completas en Java para los 3 ejercicios prácticos planteados en el taller. Cada ejercicio incluye su código fuente documentado y ejemplos de ejecución.

---

## Ejercicio 1: Calculadora Básica

### Descripción
Programa que utiliza estructuras de control (`switch`, `if-else`, bucle de control) para realizar operaciones aritméticas de suma, resta, multiplicación y división, incluyendo validación para evitar divisiones entre cero.

### Código Fuente en Java
```java
import java.util.Scanner;

public class CalculadoraBasica {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        boolean continuar = true;

        System.out.println("=========================================");
        System.out.println("        CALCULADORA BÁSICA EN JAVA       ");
        System.out.println("=========================================");

        while (continuar) {
            System.out.print("\nIngrese el primer número: ");
            double num1 = scanner.nextDouble();

            System.out.print("Ingrese el segundo número: ");
            double num2 = scanner.nextDouble();

            System.out.println("\nSeleccione la operación a realizar:");
            System.out.println("1. Suma (+)");
            System.out.println("2. Resta (-)");
            System.out.println("3. Multiplicación (*)");
            System.out.println("4. División (/)");
            System.out.print("Opción (1-4): ");
            int opcion = scanner.nextInt();

            double resultado = 0;
            boolean operacionValida = true;

            switch (opcion) {
                case 1:
                    resultado = num1 + num2;
                    System.out.printf("Resultado: %.2f + %.2f = %.2f\n", num1, num2, resultado);
                    break;
                case 2:
                    resultado = num1 - num2;
                    System.out.printf("Resultado: %.2f - %.2f = %.2f\n", num1, num2, resultado);
                    break;
                case 3:
                    resultado = num1 * num2;
                    System.out.printf("Resultado: %.2f * %.2f = %.2f\n", num1, num2, resultado);
                    break;
                case 4:
                    if (num2 == 0) {
                        System.out.println("Error: No es posible dividir entre cero.");
                        operacionValida = false;
                    } else {
                        resultado = num1 / num2;
                        System.out.printf("Resultado: %.2f / %.2f = %.2f\n", num1, num2, resultado);
                    }
                    break;
                default:
                    System.out.println("Opción inválida. Intente de nuevo.");
                    operacionValida = false;
                    break;
            }

            System.out.print("\n¿Desea realizar otra operación? (s/n): ");
            char respuesta = scanner.next().toLowerCase().charAt(0);
            if (respuesta != 's') {
                continuar = false;
                System.out.println("¡Gracias por usar la calculadora!");
            }
        }

        scanner.close();
    }
}
```

---

## Ejercicio 2: Contador de Vocales y Consonantes

### Descripción
Programa que recibe una palabra (en minúsculas, sin símbolos, números ni acentos) y cuenta de manera precisa cuántas vocales y cuántas consonantes contiene.

### Código Fuente en Java
```java
import java.util.Scanner;

public class ContadorVocalesConsonantes {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("=========================================");
        System.out.println("   CONTADOR DE VOCALES Y CONSONANTES     ");
        System.out.println("=========================================");

        System.out.print("\nIngrese una palabra (en minúsculas y sin acentos): ");
        String palabra = scanner.nextLine().trim();

        int totalVocales = 0;
        int totalConsonantes = 0;

        for (int i = 0; i < palabra.length(); i++) {
            char c = palabra.charAt(i);

            // Verificamos si es vocal
            if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') {
                totalVocales++;
            } 
            // Si es una letra del abecedario entre 'a' y 'z' distinta a las vocales, es consonante
            else if (c >= 'a' && c <= 'z') {
                totalConsonantes++;
            }
        }

        System.out.println("\n--- Resultados ---");
        System.out.println("Palabra analizada: " + palabra);
        System.out.println("Total de caracteres: " + palabra.length());
        System.out.println("Número de vocales: " + totalVocales);
        System.out.println("Número de consonantes: " + totalConsonantes);

        scanner.close();
    }
}
```

---

## Ejercicio 3: Invertir una Cadena de Texto

### Descripción
Programa que solicita al usuario una cadena de texto y muestra en pantalla la cadena invertida (carácter por carácter), implementado tanto con recorrido tradicional como con utilidades estándar (`StringBuilder`).

### Código Fuente en Java
```java
import java.util.Scanner;

public class InvertirCadena {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("=========================================");
        System.out.println("        INVERTIR CADENA DE TEXTO         ");
        System.out.println("=========================================");

        System.out.print("\nIngrese la cadena de texto a invertir: ");
        String original = scanner.nextLine();

        // Método 1: Recorrido inverso con ciclo for
        String invertidaCiclo = "";
        for (int i = original.length() - 1; i >= 0; i--) {
            invertidaCiclo += original.charAt(i);
        }

        // Método 2: Utilizando StringBuilder (Eficiente en memoria)
        String invertidaBuilder = new StringBuilder(original).reverse().toString();

        System.out.println("\n--- Resultados ---");
        System.out.println("Texto original:  \"" + original + "\"");
        System.out.println("Texto invertido: \"" + invertidaCiclo + "\"");

        scanner.close();
    }
}
```

---

## Instrucciones de Compilación y Ejecución en Terminal

Para compilar y ejecutar cualquiera de los programas desde la consola:

1. **Compilar:**
   ```bash
   javac CalculadoraBasica.java
   javac ContadorVocalesConsonantes.java
   javac InvertirCadena.java
   ```
2. **Ejecutar:**
   ```bash
   java CalculadoraBasica
   java ContadorVocalesConsonantes
   java InvertirCadena
   ```
