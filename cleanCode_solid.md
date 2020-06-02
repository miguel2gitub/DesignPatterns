# S.O.L.I.D.

### 5 principios sobre POO
  - Permiten un código
    - Más limpio
    - Más mantenible
    - Más extensible
  - Los principios
    1. **SRP**: Single Responsibility Principle
    1. **OCP**: Open/Closed Principle
    1. **LSP**: Liskov substitution Principle
    1. **ISP**: Interface Segregation Principle
    1. **DIP**: Dependency Inversión Principle    

### SRP
  - Principio de responsabilidad única
  - Las clases deben ser pequeñas en responsabilidades
  - Los principios
    - Una clase de tener una única responsabilidad dentro de todo el proyecto
    - Si se cambia algo, solo debería haber una razón    

A primera vista, decir que una clase debe tener una responsabilidad única parece
algo obvio, pero definir de forma clara y sin ambiguedades qué es una responsabilidad única
no lo es tanto.

Lo que marca el número de responsabilidades es el número de actores que va a utilizar una clase.
Una clase que haga muchas cosas puede ser utilizada por distintos perfiles de usuario, un DBA que migra datos, un operador, un gestor que necesita ver informes, y esa no cumpliría con el principio.

#### El SRP en la arquitectura limpia
La arquitectura limpia presentada en el apartado de Clean Code se ajusta muy bien a este principio, ya que todo gira en torno a los casos de uso, y por cada caso de uso hacemos una clase distinta.
Por su parte, las entidades no hacen más que representar datos, así que
en una arquitectura así, el SRP está asegurado.

Este principio está relacionado con la capacidad de hacer que los módulos sean extensibles, sin que su código tenga que ser directmante modificado.

El nombre parece sugerir que se debe apostar por la extensión de una clase para poder modificar su comportamiento, y hace décadas podía ser así; hoy en día se basa en el uso de interfaces.

### OCP
  - Principio de abierto/cerrado
  - El principio
    - Un módulo de software debe estar abierto a extensiones y cerrado a modificaciones
  - Significado
    - Abierto: debería ser fácil cambiar el comportamiento del módulo
    - Cerrado: ese comportamiento se debería poder cambiar sin modificar el módulo
  - Consecuencia
    - Basta con escribir un buen módulo
    - Las modificaciones se basan añadir nuevo código, no en modificar el módulo.
  - Problemas
    - Es difícil en sistemas grandes        

Este principio está relacionado con la capacidad de hacer que los módulos sean extensibles, sin que su código tenga que ser directmante modificado.

El nombre parece sugerir que se debe apostar por la extensión de una clase para poder modificar su comportamiento, y hace décadas podía ser así; hoy en día se basa en el uso de interfaces.

### LSP
  - Principio de sustituión de Liskov
  - El principio
    - Si S es un subtipo de T, los objetos de tipo T en un programa pueden ser intercambiados por objetos de tipo S sin alterar ninguna de sus propiedades.

La programación orientada a objetos también tuvo su burbuja, y hubo un tiempo en el que se esperaban grandes cosas de ella. Con la programación orientada a objetos y con mecanismos como la encapsulación, herencia y polimorfismo, íbamos a ser capaces de desarrollar componentes de software reutilizables, extensibles y más fácilmente mantenibles.

Además, se extendió la idea de que todo podía ser representado utilizando objetos, cuando lo cierto es que no siempre es así, y a veces ocurren paradojas como la del rectángulo y el cuadrado.

A la hora de escribir código, el principio de Liskov requiere que se cumplan las siguientes requerimientos en los métodos:
  - Contravarianza en los argumentos de métodos en el subtipo.
  - Covarianza en el tipo de retorno de los subtipos.
  - No lanzar nuevas excepciones en los métodos de los subtipos.
  - A este principio también se le conoce como diseño por contrato.

### ISP
  - Principio de segregación de interfaces
  - Los principios
    - Los clientes no deberían implementar interfaces que no utilicen
    - Es preferible crear muchos pequeños interfaces que sean implementados por clientes específicos.  

El principio establece que los clientes no deberían implementar interfaces que no van a necesitar.
No es solo una cuestión de utilidad, el principio de segregación es ante todo una forma más de encapsular nuestros componentes, exponiendo para cada cliente únicamente aquellos métodos que se necesiten.

Los clientes pueden estar vinculados a distintos roles o distintas funcionalidades del sistema. En el ejemplo presentado en la presentación, se usa como clase principal un supuesto Kernel o núcleo de un sistema, al que acceden toda clase de dispositivos.

Obviamente, esa implementación es un desastre. Cualquier dispositivo tendría acceso total a todas las funcionalidades del kernel.
Lo correcto sería establecer interfaces separados para cada uno de los dispositivos (que son los clientes),
de tal manera que controlamos el acceso al Kernel y lo encapsulamos.
Todos acceden al Kernel

### DIP
  - Principio de inversión de dependencia
  - Elementos de un sistema
    - Módulos de alto nivel, lógica de negocio
    - Módulos de bajo nivel, detalles  
  - D.I.P. dice:
    - Los módulos de alto nivel, deben depender de abstracciones, no de los módulos de bajo nivel    
    - Las abstracciones,  no deben depender de los detalles de bajo nivel. Los módulos de bajo nivel, también deben depender de abstracciones.

Para ilustrar el concepto de inversión de dependencia se utilizan dos proyectos,
uno en el que hay una dependencia y acoplamiento entre dos clases y otro en el que la dependencia se invierte a través de un interface.

La primera vez que se lee la definición forma del principio puede no quedar muy clara:
  - Los módulos de alto nivel no deben depender de los de bajo nivel. Deben depender de abstracciones, no de elementos concretos.
  - Las abstracciones no deberían depender de los detalles. Son los detalles los que deberían depender de las abstracciones.

Pero en realidad, viendo un ejemplo está claro. Para romper esa dependencia, la clase de alto nivel pasa a utilizar un interface, que a fin de cuentas es una abstracción. Y la otra clase no tienen más remedio que implementar ese interface.

#### ¿Cómo metemos la implementación en la clase de alto nivel?
Se puede hacer a través del método set o si tuviera el parámetro necesario, por un métodos constructor. Y eso es algo que lo facilitaría la técnica de inyección de dependencias.

Spring es un ejemplo de framework de inyección de dependencias o dependency injection,
pero también los hay más ligeros como di-guice.

# Principios LEAN    
  - Principio de inversión de dependencia
  - Los principios
    1. Eliminate waste
    1. Amplify learning
    1. Decide as late as posible
    1. Deliver as fast as posible
    1. Empower the team
    1. Build integrity in
    1. See the whole   

### 1. Eliminate waste
- Todo lo que no aporte valor al usuario = WASTE
- Desarrollar una funcionalidad o producto incorrecto
- No gestionar bien el backlog
- Volver a hacer el mismo trabajo
- Desarollar soluciones innecesariamente complejas
- Carga cognitiva
- Estress psicológico
- Espera/Multitarea
- Perdida de comunicación
- Comunicación inefectiva    

### 2. Amplify learning
- Desarrollo = Aprendizaje continuo
- Las ideas se plasma en código
- Reuniones -> aprendizje dominio y necesidades
- Ciclos cortos con refactoring y tests

### 3. Decide as late as posible
- Posponer decisiones críticas
- Ajustar la solución en ciclos
- Probar y evaluar

### 4. Deliver as fast as posible
- Proyectos web y móviles
- Feedback continuo
- El primero en llegar triunfa

### 5. Empower the team
- Sin gestores ni “recursos”
- El equipo asume su gestión
- Tareas asumibles y contacto con cliente
- ¿Cumple el test de Joel? ¡Deja en paz al equipo!

### 6. Build integrity in
- Ofrecer imagen de robustez
- Integridad = flexibilidad + mantenibilidad
- ¿Cómo?
  - Contacto con cliente
  - Ciclos
  - Información en pequeñas dosis
  - Refactoring + Testing + C.I.
