# AR
AR_STUDY_P
ACLARACION importante, el proyecto aun hace falta de 2 carpetas,falta la carpeta "videos" y "models" debido a su peso original
Documentación Estructurada 
1. Introducción al Proyecto 
AR Mundial 2026 es una aplicación de Realidad Aumentada basada en web que transforma el 
aprendizaje sobre las naciones participantes en una experiencia inmersiva. El sistema utiliza el 
reconocimiento de imágenes para desplegar contenido multimedia, juegos y retos educativos sin 
necesidad de aplicaciones externas. 
2. Guía de Interfaz y Usuario (Manual de Operación) 
La interfaz ha sido diseñada para ser intuitiva y no obstructiva, permitiendo que la Realidad Aumentada 
sea la protagonista. 
● Escaneo de Marcadores: El usuario debe apuntar la cámara hacia las banderas o logos oficiales. 
Al detectar la imagen, el sistema activa el "Target Activo", desplegando un modelo 3D relacionado 
al país. 
● Botón de Información y vídeo: Despliega un panel con datos varios y un reproductor de video 
con un video relacionado al país escaneado. 
● Botón de Trivia: Activa un sistema de preguntas y respuestas. A diferencia de versiones 
anteriores, el usuario ahora enfrenta una secuencia de preguntas por país. 
● Botón de Juego: Inicia un mini-juego de agilidad mental y física donde el usuario debe "atrapar" 
balones que aparecen aleatoriamente en la pantalla. 
3. El "Codex": Arquitectura y Lógica del Código 
Esta sección detalla cómo el código procesa la información para crear la experiencia 
interactiva. 
Módulo 
Lógica Implementada 
Detección (MindAR) 
Se utiliza un archivo compilado (.mind) con 12 
puntos de anclaje. Cada marcador tiene un ID 
numérico que vincula el contenido visual con la 
base de datos interna. 
Sistema de Trivia 
Utiliza una estructura de objetos de arreglos. 
Mediante variables de control como pregunta 
Actual Índice, el código gestiona el flujo de 
preguntas sin refrescar la aplicación, 
manteniendo el puntaje en memoria volátil. 
Motor de Juego 
Basado en eventos de clic y funciones 
Math.random() para el posicionamiento 
dinámico de elementos HTML, creando una 
experiencia de juego "Whack-a-mole" adaptada 
al entorno AR. 
Control Multimedia 
Funciones personalizadas que gestionan el 
play/pause de elementos de video y audio  
Ricardo Linares Renedo - 1975336 - Procesamiento de Imagenes 051 
4. Especificaciones Técnicas 
● Framework AR: A-Frame + MindAR.js. 
● Lenguaje: JavaScript ES6+, HTML5 y CSS3 (Glassmorphism). 
● Compatibilidad: Dispositivos móviles y escritorio con soporte WebGL y acceso a cámara 
vía NGROK. 
3. Análisis Profundo del Código (Lógica del Sistema) 
Esta sección detalla los bloques fundamentales de JavaScript que dan vida a la interactividad 
del proyecto. 
3.1. Gestión de Marcadores y Estados 
El sistema utiliza eventos de escucha (Listeners) de MindAR para detectar cambios en el 
entorno. Al identificar una imagen, el motor asigna un targetIndex que actualiza la variable 
global targetActivo. Esta variable actúa como el "cerebro" de la aplicación, decidiendo qué 
información específica de cada país se debe cargar en los paneles de trivia y multimedia. 
3.2. Arquitectura de la Trivia 
● Estructura de Datos: Se implementó un objeto de arreglos donde cada país posee su 
propio set de preguntas, opciones y la respuesta correcta. 
● Función cargarPregunta(): Esta función es responsable de limpiar el panel actual y 
renderizar dinámicamente la nueva pregunta basada en un índice de progreso. Al 
finalizar el arreglo de preguntas, el sistema calcula y muestra automáticamente el 
puntaje final. 
● Validación de Respuestas: Mediante la función verificarRespuesta(), el código 
compara la selección del usuario con el valor almacenado en el objeto de datos, 
gestionando el avance de la trivia sin necesidad de recargar la interfaz. 
3.3. Lógica del Mini-juego de Agilidad 
El juego de "atrapa el balón" utiliza una combinación de manipulación dinámica del DOM y 
funciones de tiempo: 
● Posicionamiento Aleatorio: Se utilizan funciones Math.random() vinculadas al 
tamaño de la ventana (window.innerWidth/Height) para que cada balón aparezca 
en un punto impredecible. 
● Control de Dificultad: El código incluye un sistema de temporizadores (setTimeout) 
que reduce el tiempo de reacción disponible conforme el usuario aumenta su puntaje, 
creando una curva de dificultad progresiva. 
Ricardo Linares Renedo - 1975336 - Procesamiento de Imagenes 051 
CODEX 
1. Gestión de Modelos 3D y Multimedia 
La aplicación utiliza una integración profunda entre el DOM de HTML y el motor A-Frame para 
gestionar activos pesados de forma eficiente. 
1.1. Carga Dinámica de Assets 
Para optimizar el rendimiento en dispositivos móviles, los modelos 3D (banderas, objetos o 
lugares) y los videos se gestionan mediante el sistema de targetIndex. Cuando MindAR detecta 
un marcador, el código no sólo activa la visibilidad, sino que vincula el ID del marcador con las 
rutas de archivos específicas (MP4 para videos y GLB para modelos), asegurando que el 
contenido sea relevante al país escaneado. 
1.2. Reproducción de Video y Filtros Visuales 
Al presionar el botón de video, se ejecutan dos acciones simultáneas: 
● Activación del Stream: Se busca el elemento <video> correspondiente al targetActivo y 
se dispara el método .play(). 
● Aplicación de Filtros: Mediante la manipulación de propiedades CSS (backdrop-filter) y 
atributos de A-Frame, se aplica un efecto de enfoque o cambio de saturación al entorno 
AR para resaltar la pantalla de video, mejorando la legibilidad en condiciones de mucha 
luz. 
2. Animaciones y Sistemas de Partículas 
La interactividad visual se divide en animaciones de transformación y efectos atmosféricos. 
2.1. Animación Básica de Transformación 
El código utiliza el componente animation de A-Frame. Al activar el botón de "Animación", se 
modifican dinámicamente los atributos de rotation o scale del modelo 3D. Esto se logra 
mediante setAttribute(), permitiendo que los objetos no sean estáticos y reaccionen a la entrada 
del usuario. 
3. Lógica de Interfaz Condicionada 
En documento Previo mencionaba secciones "No Condicionadas". Técnicamente, esto se 
maneja mediante un sistema de visibilidad: 
● Estado de Escaneo: El código escucha los eventos targetFound y targetLost. Mientras 
no haya un objetivo, los botones de "Trivia" e "Info" permanecen bloqueados o invisibles 
para evitar errores de referencia nula. 
● Persistencia de Datos: Las estadísticas del juego y la galería se mantienen en el estado 
global de la sesión, permitiendo al usuario navegar entre diferentes países sin perder su 
progreso acumulado en la sesión actual. 
Ricardo Linares Renedo - 1975336 - Procesamiento de Imagenes 051 
4. Renderizado y Mezcla de Capas 
El desafío técnico de mostrar botones 2D sobre modelos 3D se resuelve mediante un Z-Index 
superior en el contenedor UI y el uso de Glassmorphism en el CSS, lo que permite que el 
usuario mantenga la referencia visual del mundo real (el video de la cámara) mientras 
interactúa con los menús. 
