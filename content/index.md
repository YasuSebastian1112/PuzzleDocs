---
title: BIENVENIDO A PUZZLE ENGINE
---
tag: #Home
Puzzle Engine es un motor de videojuegos y simulaciones 2D/3D desarrollado en C++ sobre el pipeline gráfico de OpenGL, creado por Yatagarasu (Sebastián A. S.) y Godotman (Efraín Contreras).   Nuestro objetivo es construir un entorno modular, ágil y de fácil adopción arquitectónica. El diseño toma como inspiración los fundamentos estructurales de Godot Engine (jerarquía nodal, servidores de bajo nivel y separación modular), implementándolos desde cero para ofrecer un control directo sobre la memoria, maximizar el rendimiento computacional y superar las restricciones tradicionales de los motores de propósito general.

Este portal recopila toda la documentación técnica de **Puzzle Engine**, estructurada en Obsidian y publicada de forma continua a través de Quartz 5. Aquí se detallan los módulos del núcleo, el pipeline de renderizado y el diseño de los múltiples sistemas que se desarrollan.

### Fundamentos Técnicos y Filosofía de Diseño

**Puzzle Engine** está estructurado bajo una arquitectura modular de bajo nivel desacoplada, donde cada subsistema opera de forma independiente para garantizar un control determinista sobre el ciclo de vida de la aplicación, el uso de memoria en el heap y el aprovechamiento del hardware multinúcleo.

A diferencia de los motores convencionales basados en capas opacas de abstracción, Puzzle Engine adopta un diseño orientado a servidores y servicios globales:

- **Separación entre Lógica y Presentación:** El pipeline gráfico delega las operaciones a dispositivos de renderizado especializados (`RenderingDevice`), permitiendo procesar geometría y simulaciones numéricas sin acoplarse rígidamente al bucle de eventos de la ventana.
    
- **Computación Concurrente Nativa:** Integra un sistema de despacho de tareas (`JobSystem`) basado en hilos persistentes y sincronización por variables de condición, diseñado para resolver algoritmos paralelos masivos sin la sobrecarga de recrear contextos de ejecución.
    
- **Estructura Modular Extensible:** Los componentes del motor se aíslan en módulos autocontenidos, lo que permite compilar y enlazar únicamente los subsistemas requeridos por cada proyecto, manteniendo un consumo de memoria base mínimo.
    

Para organizar el código fuente y facilitar el mantenimiento de sus componentes, el motor se divide en las siguientes áreas arquitectónicas:
- [[Classes (Clases)]]
-  [[Core]]
-  [[Editor]]
- [[External]]
- [[Globals]]
- [[Modules]]
- [[Platform]]
- [[Script]]
