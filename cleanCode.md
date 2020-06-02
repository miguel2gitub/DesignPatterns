# GIT

### Crear un repositorio. Método 1:
~~~
 git init project_name
~~~

### Crear un repositorio. Método 2:
~~~
mkdir project_name
cd project_name
git init
~~~
### Añadir ficheros al repositorio
~~~
git add file_name
~~~
### Añadir ficheros. Wildcards & directorios
~~~
git add *.java
~~~
### Status
~~~
git status
~~~
### Commits
~~~
git commit –m "Commit message"

Commit all. Cambios en ficheros ya añadidos

git commit -a –m "Some changes in File1.java"
~~~
### Revertir un add. Modo interactivo
~~~
git add -i
~~~
### Ignorar ficheros:

Aspecto de .gitignore
~~~
target/
dirs/
*.class
*.dll
*.exe
~~~
¡añade .gitignore al repositorio!
~~~
git add .gitignore
~~~
### Eliminar ficheros
~~~
git rm File4.java
git commit –m “Deleted File4.java”
~~~
### Recuperar eliminados por fallo
~~~
git checkout – file_name
~~~
### Recuperar versiones
ver Hash de cada commit
~~~
git log
~~~
Y luego usarlo:
~~~
git checkout 3ac6f
~~~
Tras recuperar lo que tengamos, estaremos en Detached state

Salir de detached state
~~~
git checkout master
~~~
### Comprobar diferencias. Respecto al actual.
~~~
git diff
~~~
### Comprobar diferencias. Entre commits.
~~~
git diff  e586f  57847
~~~
### Eliminar el repositorio

Simplemente elimina la carpeta .git
~~~
rm -rf .git
~~~
## Branches

### Creación
~~~
git branch brach_name
~~~
O bien
~~~
git checkout branch_name
~~~
### Branches. Branch + checkout
~~~
git checkout –b brach_name
~~~
### Branches. Diferencias entre ramas
~~~
git diff branch1 branch2
~~~
### Branches. Cambiar de rama
~~~
git checkout branch_name
~~~
### Branches. Comprobar ramas
~~~
git branch
~~~
### Branches. Eliminar rama
~~~
git branch –D branch_name
~~~
## Merges

### Merge entre dos ramas
~~~
git checkout branch1
git merge branch2
~~~
### Merges y conflictos. Escenario 2
~~~
git checkout –b conflicting
<Modifica un fichero existente>
git commit –a –m “Saved existing file”
git checkout master
<Modifica el mismo fichero>
git commit –a –m “Saved existing file”
git merge conflicting
~~~
## Remotes

No necesariamente url remotas, el remoto puede ser otro directorio local.
~~~
git clone repository_dir cloned_repository_dir
~~~
Remotes. Agregando un remote (directorio local)
~~~
git remote add our_clone   directory_of_clone_full_path
~~~
## Workflow

Merge con master
~~~
git checkout master
git pull   (por si no está up-to-date)
git merge nombre_de_tu_rama 
git push
~~~
En caso de conflicto, debes resolverlo de forma manual editando los ficheros.

Esto te indicará el estado
~~~
git status
~~~

---

# Clean Code

### Comentarios
- Si necesitas comentar un fragmento del código significa que el código no es limpio
- Comentarios malos:
  - Obvios: los que añaden algunos ides, constructor, etc...
  - Corporativos/Disclaimers/Legales
  - Historial o change log, para eso tenemos git
  - Javadocs o phpdocs, dice que solo para apis públicas
  - Secciones/Ascii Art, típicas rayas para separar código
  - Marcar cierres } // end for
  - Código antiguo
  - Comentarios informativos generales: config, rfc, análisis
- Comentarios aceptables:
  - Ejemplo de expresion regular o del formato fecha
  - Los que aclaran intenciones 
  - Clarifican un resultado  
~~~
// return -1 when character is not found
~~~  
  - Advertencias
  - TODOs

### Nombres

| Tipo                        | Norma           | Ejemplo
| --------------------------- | --------------- | ---------------- |  
| Variable                    | Sustantivo      | float weight;    | 
| Variable booleana           | is + sustantivo | boolean isAlive; | 
| Método                      | Verbo           | close()          | 
| Método con retorno booleano | is + verbo 	    | isClosed()       | 
| Clase                       | Nombre          | Customer         | 

- Habilidades que deben tener
  - Lógica
  - Abstracción
  - PONER BUENOS NOMBRES
    - Deben explicar y aclarar
    - No escatimar en longitud
    - Deben ser coherentes
- Deben ser pronunciables
- Evitar codificaciones, prefijo
- La longitud de una variable debe ser proporcional a su ámbito
- Índices y coordenadas aceptables
~~~
// i, j, k  x, y, z
~~~

### Métodos
- Un método debe hacer solo una cosa
- Evolucion 24 líneas, verse en la pantalla a 5 lineas
- Tamaño reducido: **5 líenas** como mucho
- No deben de tener mas de **3 parametros**
- Evitat variables innecesarias, usar directamente el return ...
- Bloques if, else, while, cuerpo una sola línea con llamada a método
- No usar Parámetros: 
  - booleanos
  - argumentos swicth/case
  - nulos
  - de salida, solo de entrada, su valor no cambia al terminar la llamada
- Evitar programación defensiva:
~~~
if (null != x && null != y && y != 0)
~~~
- Separar comando y consulta, un get no debe cambiar ningún valor
- Afirma, no pidas
~~~
instancia.getA().getB().getC().getD().hazAlgo();
Sería mejor:
instancia.hazAlgo();
Ver: Ley de Demeter
~~~
- break, continue, return
  - Alteran la ejecución normal
  - Si la función es grande, problemáticos
  - En métodos pequeños, aceptables
  - goto: NUNCA
- Evitar códigos de error, Escenarios: ErrorHandling
  que devuelvan varios errores diferentes
- Evitar swicth case  

### Clases
- Deben ser pequeñas, no es en nro de líneas, sino en responsabilidades
- Principio de responsabilidad única
  - 1 clase = 1 responsabilidad
  - Si hay que cambiar la clase, solo debe haber un motivo para cambiarla
- Cohesión
  - Trabajar con otras clases SIN SOLAPARSE
  - Clases cohesivas:
    - Pocos atributos
    - Utilizados por todos los métodos  
- Control de cambios
  - Riesgos: errores, añadir responsabilidades,…
  - Protección de clases
  - Uso de interfaces    


###  Data Structure vs Objects
- Data Structure no tiene métodos, solo propiedades (exponen datos)
- Objectos, solo tiene métodos (exponen funcionalidad)
- DTO y Registros activos
  - Mapean datos de una BD
    - DTO: datos
    - Registros activos: datos con métodos para gestionar registros
- Ley de Demeter
  - Facilitan el Haz, no preguntes
  - Mide la cualidad de encapsulación.
  - El método f de una clase C solo puede llamar a:
    - Métodos de C
    - Objetos creados por f
    - Objetos pasados como argumentos a f
    - Objetos contenidos en atributos de C
      
### Arquitectura
- Características:
  - Elementos fácilmente localizables
  - Retrasamos o posponemos decisiones
    - Deja opciones abiertas hasta el final
    - Maximiza el número de opciones disponibles
  - Si cambiamos algo en una parte de esa arquitectura no afecta al resto
  - Es fácilmente testable
- Casos de uso
  - Lo es **TODO**
  - Debe **exponerse** ese uso
  - No debe mezclarse con UI, BD, ...
  - El valor de sistema está en los casos de uso no en la UI
  - UI -> Casos de uso <- plugins (bd,etc...)
- Particionado
  - Business objects: **Entities**
    - Entidades independientes de la aplicacion
    - Representan elementos del dominio, pero no mapean exactamente la BD
  - Use case: **Controls/Interactor**
    - Son los métodos específicos de la aplicación
  - UI objects: **Boundaries**
    - La frontera entre la arquitectura y el exterior.
    - Son mecanismos de entrega
    - Comunican los casos de uso con: la UI, BD, etc...

![arquitectura](.owb/arquitectura.png)    