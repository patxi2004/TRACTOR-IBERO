# 🦵 Subsistema: Actuador de Acelerador

![Estado](https://img.shields.io/badge/Estado-En%20documentación-yellowgreen)
![Motor](https://img.shields.io/badge/Motor-Oukeda%200K57H18112A--420--8--21B-blue)
![Encoder](https://img.shields.io/badge/Encoder-AS5600%2012bit-purple)
![Fabricación](https://img.shields.io/badge/Estructura-Perfil%20aluminio%20Item-orange)

---

## Descripción funcional

Este subsistema implementa el **control automatizado del pedal del acelerador** de un tractor agrícola convencional con caja de velocidades manual. En lugar de modificar o reemplazar el tractor, se adopta un enfoque de **retrofit no invasivo**: un actuador lineal de husillo construido alrededor de un motor paso a paso **NEMA 27 de la familia Oukeda** se monta sobre una estructura de perfil de aluminio y empuja físicamente el pedal del acelerador, replicando la acción que realizaría el pie del operador.

El actuador tiene **lazo cerrado de posición** gracias al encoder magnético absoluto AS5600, lo que garantiza un posicionamiento preciso y repetible del pedal.

---

## Principio de funcionamiento

El ciclo de control completo es el siguiente:

```
Sistema de control (ROS 2)
         │
         ▼
  Señal de velocidad objetivo
         │
         ▼
  Microcontrolador + Driver paso a paso   🚧 (por definir)
         │
         ▼
  Motor NEMA 27 (200 pasos/rev)
         │  ← Encoder AS5600 retroalimenta posición angular
         ▼
  Engranes rectos (reducción)
         │
         ▼
  Husillo M8 + tuerca Delrin
  (convierte rotación → movimiento lineal)
         │
         ▼
  Carro lineal sobre perfil aluminio Item
         │
         ▼
  Empuje físico sobre pedal acelerador
         │
         ▼
  Incremento/decremento de RPM del tractor
```

El encoder AS5600 lee la posición angular del eje del motor, el controlador calcula la posición lineal del husillo y envía pulsos de paso al NEMA 27 para alcanzar la posición objetivo con precisión.

---

## Especificaciones técnicas

| Parámetro | Valor | Fuente |
|---|---|---|
| Motor | Paso a paso bifásico bipolar — Oukeda **0K57H18112A-420-8-21B** | Etiqueta física del motor |
| Cara de montaje del motor | 68.6 × 68.6 mm | Árbol CAD `nema 27.STEP` — NEMA 27 |
| Ángulo de paso | 1.8° (200 pasos/revolución) | Estándar NEMA 27 |
| Encoder | AS5600 — magnético absoluto 12 bits | Árbol CAD `AS5600` |
| Resolución del encoder | 4096 posiciones/revolución | Datasheet AS5600 |
| Interfaz del encoder | I²C / PWM | Datasheet AS5600 |
| Voltaje encoder | 3.3 – 5 V | Datasheet AS5600 |
| Husillo | Varilla roscada M8 | Árbol CAD `barilla roscada 8 mm` |
| Tuerca anti-juego | Delrin (poliacetal) — 4× pieza `Delrin dual 5×24×11` | Árbol CAD |
| Rodamiento A | 608ZZ — 8×22×7 mm — ×2 | Árbol CAD |
| Rodamiento B | 688ZZ — 8×19×6 mm — ×2 | Árbol CAD |
| Transmisión | Engrane recto métrico ×2 (relación de reducción ⚠️ por confirmar) | Árbol CAD `spur gear_am_Metric` |
| Estructura | Perfil de aluminio con ranura tipo Item/Bosch (`item_64234`) | Árbol CAD |
| Conector | GX16-4 pines — 250 V / 5 A — ×2 (entrada y salida) | Árbol CAD `GX16-4p` |
| Finales de carrera | Endstop mecánico ×2 (límite mínimo y máximo) | Árbol CAD |
| Ventilador de refrigeración | 50×50×11 mm (12 V o 24 V ⚠️ por confirmar) | Árbol CAD |
| Orificio de montaje | M6 (radio 3 mm) | Medición SolidWorks |
| Modelo exacto del motor | **0K57H18112A-420-8-21B** | Etiqueta física del motor |
| Cableado del motor | A+ Rojo · A− Verde · B+ Amarillo · B− Azul | Etiqueta física del motor |

---

## Lista de materiales (BOM)

| # | Componente | Descripción | Cantidad | Especificaciones técnicas | Notas |
|---|---|---|---|---|---|
| 1 | Motor Oukeda 0K57H18112A-420-8-21B | Motor paso a paso NEMA 27 que genera el movimiento rotacional del actuador | 1 | Bifásico bipolar · 1.8°/paso · 200 pasos/rev · Cara 68.6×68.6 mm · A+(Rojo) A−(Verde) B+(Amarillo) B−(Azul) | Modelo confirmado por etiqueta física |
| 2 | Encoder AS5600 | Encoder magnético absoluto para retroalimentación de posición (lazo cerrado) | 1 | 12 bits (4096 pos/rev) · I²C/PWM · 3.3–5 V | Montado sobre el eje del motor |
| 3 | Varilla roscada M8 | Husillo que convierte el movimiento rotacional en lineal | 1 | Métrica M8 · Longitud ⚠️ por confirmar | Acero |
| 4 | Tuerca anti-juego Delrin | Tuerca de poliacetal que minimiza el backlash del husillo | 4 | Modelo `Delrin dual 5×24×11` | Diseño típico en CNC bajo costo |
| 5 | Rodamiento 608ZZ | Rodamiento de apoyo del husillo | 2 | 8×22×7 mm | Estándar de bajo costo |
| 6 | Rodamiento 688ZZ | Rodamiento de apoyo secundario | 2 | 8×19×6 mm | Estándar de bajo costo |
| 7 | Engrane recto métrico | Par de engranes de transmisión entre motor y husillo | 2 | Módulo métrico ⚠️ por confirmar · Relación de reducción ⚠️ por confirmar | Aumenta fuerza a costa de velocidad |
| 8 | Perfil aluminio Item/Bosch | Riel estructural y guía del carro lineal | ⚠️ cant. por confirmar | `item_64234` — ranura estándar 8 mm | Base modular del actuador |
| 9 | Conector GX16-4 pines | Conector circular de aviación para cableado del sistema | 2 | 4 pines · 250 V / 5 A | 1× entrada, 1× salida |
| 10 | Endstop mecánico | Final de carrera para límites de recorrido (seguridad) | 2 | ⚠️ Modelo por confirmar | Límite mínimo y máximo del actuador |
| 11 | Ventilador de refrigeración | Refrigeración del motor para trabajo continuo | 1 | 50×50×11 mm · 12 V o 24 V ⚠️ por confirmar | Indica que el sistema soporta ciclos de trabajo elevados |
| 12 | Driver de motor paso a paso | Módulo de potencia para control del NEMA 27 | 1 | ⚠️ Por definir | Compatible con microcontrolador seleccionado |
| 13 | Microcontrolador | Recibe comandos de ROS 2 y genera señales de paso | 1 | ⚠️ Por definir | Debe soportar micro-ROS o interfaz serial |
| 14 | Bracket de montaje sobre tractor | Soporte de fijación del conjunto actuador al chasis del tractor | ⚠️ | ⚠️ Dimensiones por confirmar | Posiblemente impreso en 3D · Ver `cad/` |
| 15 | Tornillería M6 | Fijación de los puntos de montaje (orificio Ø6 mm confirmado) | ⚠️ | M6 · Acero inoxidable recomendado | Radio del orificio: 3 mm (medición SolidWorks) |
| 16 | Pedal del acelerador (referencia) | Pedal físico original del tractor sobre el que actúa el actuador | — | ⚠️ Modelo de tractor por confirmar | STL disponible en `cad/pedal.stl` |

---

## Archivos CAD

Los archivos de diseño se encuentran en la carpeta [`cad/`](./cad/).

| Archivo | Descripción | Estado |
|---|---|---|
| `../Actuador_2.STEP` | Ensamble completo del actuador Oukeda (raíz del repo) | ✅ Disponible |
| `cad/pedal.stl` | Modelo STL del pedal del acelerador del tractor | ✅ Disponible |
| `cad/bracket-montaje.stl` | Soporte de fijación del actuador al tractor | ⚠️ Por diseñar |
| `cad/nema27.step` | Modelo STEP del motor NEMA 27 | ✅ Del árbol CAD |
| `cad/as5600.step` | Modelo STEP del encoder AS5600 | ✅ Del árbol CAD |

> **Componentes identificados en el árbol de SolidWorks:** `nema 27.STEP`, `barilla roscada 8 mm`, `Delrin dual 5x24x11`, `spur gear_am_Metric`, `item_64234_tubo-pe`, `GX16-4p`, `endstop ×2`, `ventilador 50×50×11 mm`.

---

## Notas de diseño

### ¿Por qué husillo M8 con tuerca Delrin en lugar de husillo de bolas?
El husillo de bolas ofrece mayor eficiencia y precisión, pero su costo es considerablemente mayor. Para este proyecto, la tuerca anti-juego de Delrin (poliacetal) con 4 piezas en tándem proporciona una reducción de backlash suficiente para el control de posición del pedal, manteniendo el costo del subsistema accesible. El diseño es idéntico al utilizado en máquinas CNC de bajo costo.

### ¿Por qué NEMA 27 en lugar de NEMA 23?
El NEMA 23 es el estándar más común en CNC y robótica DIY, pero el NEMA 27 ofrece un torque significativamente mayor gracias a su cara de montaje de 68.6 mm frente a los 56.4 mm del NEMA 23. Esta diferencia es necesaria para vencer la resistencia mecánica del pedal del acelerador del tractor, que opera con un resorte de retorno de fuerza considerable.

### Lazo cerrado de posición con AS5600
La incorporación del encoder magnético absoluto AS5600 eleva el sistema de control abierto (típico en actuadores lineales básicos) a control en lazo cerrado. Esto garantiza que el actuador alcance y mantenga la posición exacta del pedal incluso bajo perturbaciones externas, y permite detectar pérdida de pasos en el motor.

---

## Consideraciones de seguridad

| Elemento | Función de seguridad |
|---|---|
| Endstop mínimo | Evita que el vástago se retraiga más allá del punto de reposo físico del pedal |
| Endstop máximo | Evita que el actuador fuerce el pedal más allá de su carrera máxima, previniendo daños mecánicos |
| Ventilador 50×50 mm | Refrigera el motor durante ciclos de trabajo prolongados; sin ventilación el NEMA 27 puede sobrecalentarse en operación continua |
| Conector GX16 | Permite desconexión rápida del sistema en caso de emergencia |

> ⚠️ **Importante:** El sistema debe incorporar lógica de fallo seguro en el firmware: si se pierde la comunicación con ROS 2, el actuador debe retornar a la posición de ralentí (mínimo).

---

## Instalación mecánica

> ⚠️ **Antes de iniciar:** Asegúrate de que el tractor esté apagado, con freno de mano activado y las llaves retiradas del contacto.

### Paso 1 — Inspección del pedal y zona de montaje

Examina el pedal del acelerador y el espacio disponible bajo el tablero para determinar los puntos de fijación del bracket de montaje. Verifica la disponibilidad de anclajes M6 en la estructura.

![Foto paso 1](images/paso-01.jpg)

---

### Paso 2 — Montaje del bracket de fijación al tractor

Fija el bracket de montaje a la estructura del tractor usando tornillos M6. Verifica que quede alineado con el eje de acción del pedal y que la superficie de apoyo sea plana.

![Foto paso 2](images/paso-02.jpg)

---

### Paso 3 — Instalación del conjunto actuador

Coloca el ensamble del actuador (perfil aluminio + motor + husillo) sobre el bracket y asegúralo. El carro lineal del husillo debe quedar orientado hacia el punto de contacto del pedal.

![Foto paso 3](images/paso-03.jpg)

---

### Paso 4 — Conexión entre carro lineal y pedal

Conecta el extremo del carro del husillo con el pedal del acelerador mediante la articulación correspondiente. Verifica que permita el movimiento libre y alineado sin puntos de roce.

![Foto paso 4](images/paso-04.jpg)

---

### Paso 5 — Verificación de recorrido mecánico

Con el actuador desconectado eléctricamente, mueve el carro manualmente para verificar que:
- El pedal se desplaza sin interferencias durante todo el recorrido.
- Los endstops se activan correctamente en ambos extremos.
- Ningún componente interfiere con estructura, cables o mangueras del tractor.

![Foto paso 5](images/paso-05.jpg)

---

### Paso 6 — Conexión del cableado (conector GX16)

Conecta el cableado del motor y del encoder a través del conector GX16-4 pines. Verifica la orientación del conector antes de insertar.

![Foto paso 6](images/paso-06.jpg)

---

### Paso 7 — Fijación final y verificación

Aplica el torque de apriete final a todos los puntos de fijación M6. Realiza una prueba eléctrica manual antes de conectar al sistema de control completo.

![Foto paso 7](images/paso-07.jpg)

---

## Control electrónico

> ### 🚧 En desarrollo
>
> El esquema de control electrónico se encuentra en fase de definición. Los siguientes componentes están **pendientes de selección**:
>
> - **Microcontrolador:** Debe soportar comunicación con ROS 2 (micro-ROS, serial UART o similar) y generar señales de paso y dirección para el driver del NEMA 27.
> - **Driver de motor paso a paso:** Módulo de potencia compatible con el NEMA 27 seleccionado. Debe soportar el voltaje y corriente del motor.
> - **Lectura del encoder AS5600:** Interfaz I²C entre el microcontrolador y el AS5600 para lazo cerrado de posición.
> - **Gestión de endstops:** Entradas digitales para los dos finales de carrera.
> - **Control del ventilador:** Salida PWM o digital para el ventilador de 50×50 mm.
>
> Esta sección se completará una vez que se definan los componentes electrónicos del sistema.

---

## Integración con ROS 2

> ### 🚧 En desarrollo
>
> El nodo de ROS 2 para este subsistema se definirá una vez que el hardware electrónico esté seleccionado. Se espera que:
>
> - Suscriba a un topic de velocidad objetivo publicado por el módulo de navegación.
> - Traduzca la velocidad objetivo a una posición lineal del husillo (mm).
> - Envíe la posición objetivo al microcontrolador.
> - Publique el estado actual del actuador (posición encoder, estado endstops, errores).

---

## Contribuidores

| Nombre | Rol | Institución |
|---|---|---|
| Oliver Ochoa García | Diseño mecánico · Integración de sistemas | Universidad Iberoamericana Puebla |
| Belinka González Fernández | Diseño mecánico · Integración de sistemas | Universidad Iberoamericana Puebla |

---

## Licencia

Este proyecto está distribuido bajo la licencia **MIT**. Consulta el archivo [`LICENSE`](../LICENSE) para más detalles.

---

*← [Volver al README principal](../README.md)*
