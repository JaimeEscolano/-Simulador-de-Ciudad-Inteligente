# DOCUMENTACIÓN GENERAL
# Sistema de Cámara – Urban Horizon

**Proyecto:** Urban Horizon  
**Tipo de videojuego:** Simulador de construcción y gestión de ciudades  
**Género:** City Builder / Simulación  
**Motor gráfico:** Unity 3D  
**Lenguaje de programación:** C#  
**Versión del documento:** 1.0.0  
**Estado:** En desarrollo  
**Departamento:** Programación y sistemas gráficos  

---

# ÍNDICE

1. Introducción
2. Descripción general del sistema
3. Objetivos del sistema de cámara
4. Arquitectura del sistema
5. Organización de archivos
6. Inicialización de la cámara
7. Sistema de movimiento
8. Sistema de zoom
9. Sistema de rotación
10. Sistema de inclinación
11. Límites del escenario
12. Seguimiento de objetos
13. Cámara de construcción
14. Cámara de inspección
15. Sistema de selección de elementos
16. Modos de cámara
17. Interfaz de usuario
18. Configuración de controles
19. Optimización y rendimiento
20. Gestión de errores
21. Seguridad y validaciones
22. Integración con otros sistemas
23. Sistema de guardado
24. Configuración avanzada
25. Pruebas y validación
26. Problemas conocidos
27. Mejoras futuras
28. Conclusiones

---

# 1. INTRODUCCIÓN

El presente documento describe el diseño, desarrollo e implementación del sistema de cámaras del videojuego Urban Horizon.

Urban Horizon es un videojuego ficticio de simulación urbana en el que el jugador puede construir, administrar y desarrollar una ciudad desde sus primeras etapas hasta convertirse en una gran metrópolis.

El sistema de cámara constituye uno de los componentes fundamentales del videojuego, ya que permite al usuario observar el entorno, desplazarse por el mapa, seleccionar edificios, inspeccionar infraestructuras y gestionar diferentes elementos urbanos.

El objetivo principal es proporcionar una experiencia de navegación fluida, intuitiva y precisa, permitiendo observar la ciudad desde diferentes perspectivas.

La cámara debe adaptarse tanto a operaciones generales de construcción como a tareas específicas de inspección.

El sistema está diseñado para funcionar sobre Unity 3D utilizando el lenguaje de programación C#.

# 2. DESCRIPCIÓN GENERAL DEL SISTEMA

El sistema de cámara está compuesto por diferentes módulos independientes que trabajan conjuntamente.

Cada módulo se encarga de una responsabilidad específica:

- Movimiento horizontal y vertical.
- Acercamiento y alejamiento.
- Rotación horizontal.
- Inclinación vertical.
- Seguimiento de elementos.
- Limitación de movimiento.
- Selección de objetos.
- Gestión de modos de visualización.

La arquitectura se basa en un controlador principal denominado CameraController, encargado de coordinar los diferentes subsistemas.

El sistema utiliza una cámara perspectiva como configuración predeterminada, aunque contempla la posibilidad de incorporar una cámara ortográfica para determinados modos de visualización.

## 2.1. Comportamiento general

Al iniciar una partida, la cámara aparece situada sobre una zona central del mapa.

El jugador puede utilizar el ratón para controlar el zoom y la rotación, mientras que el teclado permite desplazarse por el escenario.

El movimiento debe ser independiente de la velocidad de fotogramas para garantizar un comportamiento consistente en diferentes equipos.

## 2.2. Características principales

- Movimiento libre sobre el escenario.
- Zoom progresivo.
- Rotación de 360 grados.
- Inclinación configurable.
- Límites geográficos.
- Seguimiento de vehículos y edificios.
- Transiciones suaves entre posiciones.
- Compatibilidad con diferentes resoluciones.
- Configuración personalizable.

# 3. OBJETIVOS DEL SISTEMA DE CÁMARA

Los objetivos principales son:

1. Permitir una navegación intuitiva por toda la ciudad.
2. Facilitar la observación de edificios e infraestructuras.
3. Evitar movimientos bruscos.
4. Mantener una distancia adecuada respecto al terreno.
5. Garantizar un rendimiento estable.
6. Permitir diferentes perspectivas de visualización.
7. Facilitar la integración con los sistemas de construcción.
8. Permitir la selección precisa de objetos.
9. Proporcionar controles configurables.
10. Mantener la cámara dentro de los límites del mapa.

# 4. ARQUITECTURA DEL SISTEMA

El sistema se divide en cinco componentes principales.

## 4.1. CameraController

Es el controlador principal.

Sus responsabilidades incluyen:

- Recibir las órdenes del usuario.
- Coordinar los módulos de movimiento.
- Aplicar transformaciones.
- Controlar el estado general.
- Gestionar los cambios de modo.

## 4.2. CameraMovement

Gestiona el desplazamiento de la cámara.

Permite mover la vista en las direcciones:

- Norte.
- Sur.
- Este.
- Oeste.

El movimiento se calcula teniendo en cuenta la orientación actual de la cámara.

## 4.3. CameraZoom

Controla la distancia entre la cámara y el punto de referencia.

El sistema debe impedir que la cámara atraviese el terreno o se aleje excesivamente.

## 4.4. CameraRotation

Gestiona el giro horizontal y la inclinación vertical.

La rotación horizontal permite observar la ciudad desde cualquier dirección.

La inclinación vertical permite alternar entre una vista más cenital y otra más cercana al terreno.

## 4.5. CameraBounds

Controla los límites permitidos.

Su función consiste en impedir que la cámara abandone los límites del mapa.

# 5. ORGANIZACIÓN DE ARCHIVOS

La estructura propuesta del proyecto es la siguiente:

    Assets/
    ├── Scripts/
    │   ├── Camera/
    │   │   ├── CameraController.cs
    │   │   ├── CameraMovement.cs
    │   │   ├── CameraZoom.cs
    │   │   ├── CameraRotation.cs
    │   │   ├── CameraBounds.cs
    │   │   ├── CameraFollow.cs
    │   │   ├── CameraSelection.cs
    │   │   └── CameraSettings.cs
    │   │
    │   ├── City/
    │   ├── Buildings/
    │   ├── Vehicles/
    │   ├── Infrastructure/
    │   └── UI/
    │
    ├── Prefabs/
    │   ├── Cameras/
    │   ├── Buildings/
    │   └── Vehicles/
    │
    ├── Materials/
    ├── Scenes/
    └── Resources/

Cada archivo debe mantener una responsabilidad concreta para facilitar el mantenimiento del proyecto.

# 6. INICIALIZACIÓN DE LA CÁMARA

La cámara se inicializa cuando se carga la escena principal.

Durante la inicialización se establecen:

- Posición inicial.
- Rotación inicial.
- Distancia de zoom.
- Velocidad de desplazamiento.
- Sensibilidad del ratón.
- Límites del escenario.

La posición inicial recomendada es una vista elevada que permita observar una parte considerable del terreno.

Los valores iniciales ficticios son:

| Parámetro | Valor |
|---|---:|
| Altura inicial | 120 |
| Distancia inicial | 150 |
| Velocidad de movimiento | 35 |
| Velocidad de zoom | 20 |
| Sensibilidad de rotación | 2 |
| Inclinación inicial | 45° |
| Inclinación mínima | 20° |
| Inclinación máxima | 80° |

Estos valores deberán ajustarse mediante pruebas de jugabilidad.

# 7. SISTEMA DE MOVIMIENTO

El movimiento es uno de los componentes más importantes.

El usuario debe poder desplazarse por toda la superficie del mapa sin experimentar aceleraciones inesperadas.

## 7.1. Movimiento mediante teclado

Se utilizan las teclas WASD o las flechas de dirección.

| Tecla | Acción |
|---|---|
| W | Desplazamiento hacia delante |
| S | Desplazamiento hacia atrás |
| A | Desplazamiento hacia la izquierda |
| D | Desplazamiento hacia la derecha |
| Flecha arriba | Desplazamiento hacia delante |
| Flecha abajo | Desplazamiento hacia atrás |
| Flecha izquierda | Desplazamiento hacia la izquierda |
| Flecha derecha | Desplazamiento hacia la derecha |

## 7.2. Movimiento mediante ratón

El movimiento también puede realizarse manteniendo pulsado el botón central del ratón.

El desplazamiento del cursor determina la dirección de movimiento.

Este sistema resulta especialmente útil cuando el usuario está trabajando en zonas concretas de la ciudad.

## 7.3. Velocidad dinámica

La velocidad de desplazamiento puede variar dependiendo del nivel de zoom.

Cuando la cámara está muy alejada, la velocidad aumenta para facilitar la navegación por grandes extensiones.

Cuando está cerca del suelo, la velocidad disminuye para permitir movimientos más precisos.

La fórmula propuesta es:

    velocidadFinal = velocidadBase * factorZoom

El factorZoom se calcula utilizando la distancia actual respecto al objetivo.

## 7.4. Suavizado

Para evitar movimientos bruscos se utiliza interpolación.

El sistema debe conseguir que la cámara alcance la posición deseada de manera progresiva.

Se recomienda utilizar SmoothDamp para desplazamientos con aceleración y desaceleración.

# 8. SISTEMA DE ZOOM

El zoom permite modificar la distancia de observación.

El usuario puede acercarse para observar edificios individuales o alejarse para contemplar la ciudad completa.

## 8.1. Controles

- Rueda del ratón hacia arriba: acercar.
- Rueda del ratón hacia abajo: alejar.
- Tecla +: acercar.
- Tecla -: alejar.

## 8.2. Límites

El zoom tiene dos restricciones:

**Zoom mínimo:** impide que la cámara atraviese los edificios o el terreno.

**Zoom máximo:** impide que la cámara se aleje tanto que la ciudad resulte demasiado pequeña.

Valores iniciales:

| Parámetro | Valor |
|---|---:|
| Zoom mínimo | 15 |
| Zoom máximo | 500 |
| Velocidad | 20 |
| Suavizado | 0.15 |

## 8.3. Zoom inteligente

Se contempla un sistema de zoom inteligente.

Cuando el usuario selecciona un edificio, la cámara puede acercarse automáticamente hasta una distancia adecuada.

La distancia se calcula considerando el tamaño del objeto seleccionado.

Los edificios grandes requieren una distancia mayor que las construcciones pequeñas.

# 9. SISTEMA DE ROTACIÓN

La rotación permite modificar la orientación horizontal de la cámara.

El sistema debe permitir completar una vuelta de 360 grados.

## 9.1. Controles

La rotación se realiza manteniendo pulsado el botón derecho del ratón y desplazando el cursor horizontalmente.

También se pueden utilizar las teclas Q y E.

| Tecla | Acción |
|---|---|
| Q | Girar hacia la izquierda |
| E | Girar hacia la derecha |

## 9.2. Rotación progresiva

La rotación no debe ejecutarse de forma instantánea.

Se recomienda aplicar interpolación angular para conseguir transiciones suaves.

## 9.3. Rotación alrededor de un objetivo

La cámara debe rotar alrededor de un punto de referencia.

Este punto puede representar:

- El centro de la ciudad.
- Un edificio seleccionado.
- Una carretera.
- Una zona de construcción.

Esta funcionalidad permite inspeccionar estructuras desde diferentes ángulos.

# 10. SISTEMA DE INCLINACIÓN

La inclinación modifica el ángulo vertical de observación.

El sistema debe ofrecer una vista cenital y una vista más próxima al horizonte.

## 10.1. Ángulos permitidos

La inclinación se limita entre 20 y 80 grados.

Estos límites evitan que la cámara adopte posiciones visualmente incómodas.

## 10.2. Modos de inclinación

**Vista cenital**

Permite observar la distribución de calles, parcelas y edificios.

**Vista intermedia**

Representa el modo habitual de construcción.

**Vista cercana**

Permite inspeccionar fachadas, vehículos y elementos urbanos.

# 11. LÍMITES DEL ESCENARIO

El mapa dispone de límites físicos y lógicos.

La cámara no debe desplazarse fuera de ellos.

El sistema CameraBounds comprueba continuamente la posición horizontal del objetivo.

Cuando se alcanza uno de los límites, el movimiento se restringe.

## 11.1. Coordenadas

El escenario utiliza coordenadas X, Y y Z.

- X: posición horizontal.
- Y: altura.
- Z: profundidad.

## 11.2. Límites configurables

Los límites se establecen mediante parámetros:

    minX = -500
    maxX = 500
    minZ = -500
    maxZ = 500

Los valores pueden modificarse dependiendo del tamaño del mapa.

## 11.3. Detección del terreno

La cámara debe evitar atravesar montañas y estructuras.

Para ello se propone utilizar raycasts que comprueben la distancia entre la cámara y las superficies del escenario.

# 12. SEGUIMIENTO DE OBJETOS

El sistema CameraFollow permite seguir elementos dinámicos.

Los objetos disponibles incluyen:

- Vehículos particulares.
- Autobuses.
- Camiones de mercancías.
- Trenes.
- Tranvías.
- Barcos.
- Aviones.
- Edificios seleccionados.

## 12.1. Seguimiento de vehículos

Cuando el usuario selecciona un vehículo, la cámara puede comenzar a seguirlo.

El objetivo se actualiza continuamente utilizando la posición del vehículo.

La cámara mantiene una distancia configurable.

## 12.2. Seguimiento de edificios

En el caso de edificios, el objetivo corresponde al centro geométrico de la estructura.

El sistema debe considerar su altura para evitar que la cámara enfoque únicamente la base.

## 12.3. Cancelación del seguimiento

El usuario puede cancelar el seguimiento mediante la tecla Escape.

También se cancela automáticamente cuando cambia a otro modo de cámara.

# 13. CÁMARA DE CONSTRUCCIÓN

La cámara de construcción está diseñada para facilitar la creación y modificación de elementos urbanos.

Su principal característica es la precisión.

Durante este modo, la cámara puede acercarse más al terreno y utilizar una inclinación menos pronunciada.

## 13.1. Funciones

- Inspeccionar parcelas.
- Colocar edificios.
- Crear carreteras.
- Construir puentes.
- Diseñar zonas residenciales.
- Diseñar zonas comerciales.
- Diseñar zonas industriales.

## 13.2. Integración con el sistema de construcción

Cuando el usuario selecciona una estructura para colocarla, el sistema muestra una previsualización.

La cámara debe permitir observar la posición de la estructura antes de confirmar su colocación.

## 13.3. Detección de superficies

Se utilizan raycasts desde la posición del cursor.

El sistema identifica el punto de intersección con el terreno.

Este punto se transmite al sistema de construcción.

# 14. CÁMARA DE INSPECCIÓN

La cámara de inspección está orientada a observar elementos específicos.

Permite acercarse a infraestructuras y estudiar su estado.

El jugador puede utilizar este modo para comprobar:

- Estado de las carreteras.
- Congestión del tráfico.
- Funcionamiento de servicios públicos.
- Distribución de edificios.
- Estado de los transportes.
- Zonas afectadas por problemas urbanos.

La cámara de inspección dispone de un zoom más preciso y movimientos más lentos.

# 15. SISTEMA DE SELECCIÓN DE ELEMENTOS

La selección se realiza mediante el ratón.

Cuando el usuario hace clic sobre un objeto, el sistema ejecuta un raycast desde la cámara.

Si el rayo detecta un elemento seleccionable, se almacena su referencia.

## 15.1. Elementos seleccionables

- Edificios.
- Carreteras.
- Vehículos.
- Estaciones.
- Infraestructuras.
- Elementos decorativos.

## 15.2. Información del objeto

Después de seleccionar un objeto, la interfaz puede mostrar:

- Nombre.
- Identificador.
- Categoría.
- Estado.
- Nivel de desarrollo.
- Coste de mantenimiento.
- Información adicional.

## 15.3. Prioridad de selección

Cuando existen varios objetos superpuestos, el sistema selecciona el elemento más cercano a la cámara, salvo que se aplique una prioridad específica.

# 16. MODOS DE CÁMARA

Urban Horizon incorpora diferentes configuraciones de visualización.

| Modo | Descripción |
|---|---|
| Normal | Navegación general |
| Construcción | Colocación de estructuras |
| Inspección | Observación detallada |
| Seguimiento | Seguimiento de vehículos |
| Cenital | Vista superior |
| Cinemático | Movimientos automatizados |

## 16.1. Modo normal

Es el modo predeterminado.

Permite utilizar todas las funciones generales.

## 16.2. Modo construcción

Prioriza la precisión y la estabilidad.

## 16.3. Modo seguimiento

Mantiene un objeto dentro del área central de la pantalla.

## 16.4. Modo cinemático

Permite ejecutar movimientos programados para mostrar la ciudad.

Se utiliza principalmente en presentaciones y secuencias de transición.

# 17. INTERFAZ DE USUARIO

La interfaz debe permitir consultar y modificar la configuración de la cámara.

El menú de configuración incluye:

- Sensibilidad del ratón.
- Velocidad de movimiento.
- Velocidad de zoom.
- Invertir movimiento vertical.
- Activar o desactivar suavizado.
- Restaurar valores predeterminados.

Los cambios se aplican inmediatamente.

# 18. CONFIGURACIÓN DE CONTROLES

La configuración de controles debe ser independiente de la lógica interna.

Esto permite modificar las teclas sin necesidad de alterar los scripts.

Se recomienda utilizar Unity Input System.

Los controles deben poder personalizarse desde el menú de opciones.

# 19. OPTIMIZACIÓN Y RENDIMIENTO

La cámara debe mantener un rendimiento estable incluso cuando la ciudad contiene miles de elementos.

## 19.1. Actualización de movimiento

Las operaciones de movimiento deben ejecutarse en Update o LateUpdate según su responsabilidad.

El movimiento de cámara se recomienda en LateUpdate para evitar conflictos visuales con los objetos que se actualizan durante Update.

## 19.2. Reducción de cálculos

No se deben realizar comprobaciones innecesarias.

Los cálculos de límites y colisiones deben ejecutarse únicamente cuando sea necesario.

## 19.3. Nivel de detalle

El sistema debe trabajar conjuntamente con el sistema LOD.

Los objetos lejanos utilizan modelos simplificados.

Los objetos cercanos pueden mostrar mayor detalle.

## 19.4. Objetivos de rendimiento

| Métrica | Objetivo |
|---|---:|
| FPS objetivo | 60 |
| Tiempo de actualización de cámara | Menos de 2 ms |
| Latencia de entrada | Menos de 50 ms |
| Saltos perceptibles | 0 |
| Actualizaciones innecesarias | Reducidas al mínimo |

Estos valores representan objetivos de diseño y deberán verificarse mediante pruebas reales.

# 20. GESTIÓN DE ERRORES

El sistema debe contemplar diferentes situaciones problemáticas.

## 20.1. Pérdida del objetivo

Si el objeto seguido desaparece, la cámara debe cancelar el seguimiento.

## 20.2. Coordenadas inválidas

Si la posición calculada contiene valores no válidos, se recuperará la última posición segura.

## 20.3. Referencias inexistentes

Si falta una referencia a la cámara principal, el sistema debe registrar un error.

## 20.4. Objetos destruidos

Antes de utilizar un objeto seleccionado o seguido, se debe comprobar que continúa existiendo.

# 21. SEGURIDAD Y VALIDACIONES

Aunque se trata de un sistema local de videojuego, es necesario validar las entradas y referencias.

Las principales validaciones son:

- Límites de zoom.
- Límites de inclinación.
- Límites geográficos.
- Existencia de objetivos.
- Valores numéricos válidos.
- Referencias a componentes.
- Estado del sistema de cámara.

El objetivo es evitar estados inconsistentes durante la ejecución.

# 22. INTEGRACIÓN CON OTROS SISTEMAS

El sistema de cámara se comunica con diferentes módulos del videojuego.

## 22.1. Sistema de construcción

Recibe las posiciones del cursor para determinar dónde colocar edificios.

## 22.2. Sistema de tráfico

Permite seleccionar y seguir vehículos.

## 22.3. Sistema de edificios

Permite inspeccionar estructuras y consultar información.

## 22.4. Sistema de interfaz

Recibe información sobre el objeto seleccionado.

## 22.5. Sistema de terreno

Utiliza información de altura y colisiones para evitar que la cámara atraviese superficies.

# 23. SISTEMA DE GUARDADO

La configuración de cámara debe conservarse entre sesiones.

Los parámetros guardados incluyen:

- Sensibilidad.
- Velocidad de movimiento.
- Velocidad de zoom.
- Distancia de zoom.
- Preferencias de rotación.
- Inversión del eje vertical.

La posición y orientación de la cámara también pueden guardarse dentro de la partida.

Al cargar una partida, se recuperan los valores almacenados.

# 24. CONFIGURACIÓN AVANZADA

Se propone incorporar parámetros adicionales para usuarios avanzados.

## 24.1. Aceleración

Permite modificar cuánto tarda la cámara en alcanzar su velocidad máxima.

## 24.2. Desaceleración

Controla la reducción de velocidad al dejar de pulsar una tecla.

## 24.3. Sensibilidad independiente

Permite configurar por separado:

- Sensibilidad horizontal.
- Sensibilidad vertical.
- Sensibilidad del zoom.

## 24.4. Inclinación automática

La cámara puede modificar ligeramente su inclinación dependiendo de la distancia de zoom.

## 24.5. Colisiones inteligentes

Cuando la cámara se aproxima demasiado a un edificio, puede modificar automáticamente su posición para evitar atravesarlo.

# 25. PRUEBAS Y VALIDACIÓN

El sistema debe someterse a pruebas funcionales, visuales y de rendimiento.

## 25.1. Pruebas de movimiento

Se comprobará que:

- La cámara se desplaza en todas las direcciones.
- El movimiento es independiente de los FPS.
- No existen aceleraciones inesperadas.
- Los límites funcionan correctamente.

## 25.2. Pruebas de zoom

Se comprobará que:

- El zoom aumenta progresivamente.
- El zoom disminuye progresivamente.
- No se atraviesa el terreno.
- No se superan los límites establecidos.

## 25.3. Pruebas de rotación

Se comprobará que:

- La cámara completa una rotación de 360 grados.
- La inclinación permanece dentro de los límites.
- No se producen inversiones inesperadas.

## 25.4. Pruebas de seguimiento

Se comprobará que:

- Los vehículos pueden seleccionarse.
- El objetivo permanece visible.
- El seguimiento termina correctamente.
- Los objetos destruidos no provocan errores.

## 25.5. Pruebas de rendimiento

Se realizarán pruebas con diferentes tamaños de ciudad.

Escenarios propuestos:

- Ciudad pequeña: 500 edificios.
- Ciudad mediana: 2.500 edificios.
- Ciudad grande: 10.000 edificios.
- Ciudad extrema: 25.000 edificios.

Estos escenarios son configuraciones de prueba ficticias.

# 26. PROBLEMAS CONOCIDOS

Durante el desarrollo se han identificado posibles problemas.

### 26.1. Movimiento demasiado rápido

Puede producirse cuando la cámara está demasiado alejada.

Solución propuesta: ajustar dinámicamente la velocidad según el zoom.

### 26.2. Rotación poco intuitiva

Puede producirse cuando el punto de rotación no coincide con el objetivo esperado.

Solución propuesta: establecer un pivote independiente.

### 26.3. Colisiones con edificios

Puede producirse cuando el usuario intenta acercarse demasiado a una estructura.

Solución propuesta: implementar detección de obstáculos.

### 26.4. Saltos visuales

Pueden aparecer cuando se modifica directamente la posición de la cámara.

Solución propuesta: aplicar interpolación y suavizado.

# 27. MEJORAS FUTURAS

Las siguientes funcionalidades están previstas para futuras versiones.

**Versión 1.1**
- Nuevos controles de movimiento.
- Configuración avanzada del ratón.
- Mejoras de suavizado.

**Versión 1.2**
- Seguimiento inteligente de vehículos.
- Cámara automática para edificios.
- Mejoras en el sistema de selección.

**Versión 1.3**
- Cámara cinemática avanzada.
- Rutas de movimiento programables.
- Transiciones entre puntos de interés.

**Versión 2.0**
- Compatibilidad con cámaras múltiples.
- Sistema de cámaras para multijugador.
- Herramientas de grabación de recorridos.
- Sistema avanzado de colisiones.

# 28. CONCLUSIONES

El sistema de cámara de Urban Horizon constituye uno de los elementos esenciales de la experiencia de usuario.

Su diseño modular permite separar las responsabilidades y facilita futuras ampliaciones.

La combinación de movimiento libre, zoom progresivo, rotación, inclinación y seguimiento proporciona las herramientas necesarias para gestionar una ciudad de grandes dimensiones.

La integración con los sistemas de construcción, tráfico, edificios e interfaz garantiza que la cámara no funcione como un elemento aislado, sino como parte fundamental de la arquitectura general del videojuego.

El desarrollo futuro deberá centrarse en mejorar la precisión de los controles, reducir los costes computacionales y garantizar una experiencia consistente en diferentes configuraciones de hardware.

---

**FIN DEL DOCUMENTO**

**Proyecto:** Urban Horizon  
**Departamento:** Desarrollo de software  
**Área:** Programación de cámara  
**Documento:** Documentación técnica general  
**Versión:** 1.0.0