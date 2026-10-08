# Infinite Pizza - Optimized Configuration Patch

Este repositorio contiene un parche de optimización (archivos de configuración `.ini` modificados) para el juego original *Infinite Pizza* (Unreal Engine 4). 

El objetivo principal de esta configuración es permitir que el juego original alcance un rendimiento fluido (hasta 144 FPS) en hardware de gama de entrada y procesadores con gráficos integrados (APUs), como el AMD Ryzen 5 5600G (Radeon Vega 7), solucionando además los problemas de sensibilidad extrema de la cámara que venían por defecto.

## 🛠️ ¿Qué cambió? (Antes vs. Ahora)

Tomamos el juego real y reconfiguramos los parámetros del motor gráfico y de los controles ("Input"). El juego original venía con efectos de postprocesamiento masivos ocultos y una lectura de ratón que rompía el movimiento vertical.

### 1. Optimización Gráfica (`Engine.ini`)
Se modificó la configuración del motor para desactivar procesos pesados que asfixiaban las tarjetas gráficas integradas.
* **Antes:** Niebla volumétrica activa, reflejos de espacio de pantalla (SSR), oclusión ambiental (SSAO) y profundidad de campo (DoF) habilitados por defecto, distancia de dibujado máxima.
* **Ahora:** 
  - `r.VolumetricFog=0` (Desactivado para liberar un gran porcentaje de la GPU).
  - `r.ViewDistanceScale=0.65` (Reducida la distancia geométrica sin afectar la jugabilidad rápida).
  - `r.Shadow.CSM.MaxCascades=1` (Sombras simplificadas al mínimo).
  - `r.AmbientOcclusionLevels=0`, `r.SSR.Quality=0`, `r.DepthOfFieldQuality=0` (Postprocesamiento pesado desactivado).

### 2. Arreglo de Sensibilidad y Cámara (`Input.ini`)
* **Antes:** Mirar hacia arriba o hacia abajo (LookUp) rompía el juego. Un ligero movimiento del ratón hacía que la cámara girara violentamente hacia el techo, haciendo imposible mirar el suelo o apuntar.
* **Ahora:** Se recalibró el `AxisMappings` de `LookUp` a una escala de `Scale=-0.15` (reduciendo la sensibilidad vertical en un 85%). Ahora el movimiento del ratón es natural, controlable y permite apuntar al suelo sin romper la física del personaje.

## 🚀 Resultados de Rendimiento

Con estos ajustes aplicados al juego base, un sistema con un **AMD Ryzen 5 5600G (Vega 7 APU)** pasa de sufrir cuellos de botella severos en la GPU y tirones de cámara, a mantener un flujo constante de **144 FPS**, haciendo que la experiencia de alta velocidad del juego sea verdaderamente jugable.

## ⚠️ Problemas Conocidos y Advertencias (Hardware / Drivers)

Debido a cómo está programado *Infinite Pizza* en Unreal Engine 4, el juego es "egoísta" con los recursos del PC.

* **El problema:** Al cerrar el juego o hacer Alt+Tab para ir al escritorio, podrías notar que las ventanas de Windows (como el Administrador de Tareas) se mueven con retraso, inercia o tirones.
* **La causa:** El juego no devuelve correctamente los recursos de renderizado a Windows (conflicto de Multi-Plane Overlay / MPO) y sigue consumiendo la GPU en segundo plano. Esto asfixia temporalmente a las APUs que comparten memoria RAM.
* **La solución:** Este "lag" en Windows desaparece por completo al reiniciar la PC. No corrompe tus archivos, no daña tu hardware y no es un virus; es un simple atasco temporal de la tarjeta de video con Windows 10/11. Usuarios con gráficas dedicadas (como RTX o RX) sufrirán mucho menos este efecto por tener memoria VRAM independiente.
