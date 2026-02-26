# 🦵 Subsistema: Actuador de Acelerador

![Estado](https://img.shields.io/badge/Estado-En%20documentación-yellowgreen)
![Hardware](https://img.shields.io/badge/Hardware-Actuador%20Lineal%20Oukeda-blue)
![Fabricación](https://img.shields.io/badge/Soportes-Impresión%203D-orange)

---

## Descripción funcional

Este subsistema implementa el **control automatizado del pedal del acelerador** de un tractor agrícola convencional con caja de velocidades manual. En lugar de modificar o reemplazar el tractor, se adopta un enfoque de **retrofit no invasivo**: un actuador lineal eléctrico de la marca **Oukeda** se monta sobre una estructura de soporte y empuja físicamente el pedal del acelerador, replicando la acción que realizaría el pie del operador.

### Principio de operación

```
Sistema de control (ROS 2)
         │
         ▼
  Señal de velocidad objetivo
         │
         ▼
  Driver de motor + microcontrolador   🚧 (por definir)
         │
         ▼
  Actuador lineal Oukeda
         │
         ▼
  Empuje físico sobre pedal acelerador
         │
         ▼
  Incremento/decremento de RPM del tractor
```

El sistema permite controlar la velocidad del motor del tractor de forma programática, lo que es necesario para la ejecución de trayectorias autónomas definidas por el módulo de navegación RTK.

---

## Lista de materiales (BOM)

| # | Componente | Descripción | Cantidad | Especificaciones técnicas | Notas |
|---|---|---|---|---|---|
| 1 | Actuador lineal Oukeda | Actuador lineal eléctrico de corriente continua que ejerce la fuerza de empuje sobre el pedal | 1 | ⚠️ Modelo por confirmar · Carrera, fuerza y voltaje por confirmar | Ver datasheet del fabricante cuando se tenga el modelo exacto |
| 2 | Bracket de montaje superior | Pieza de soporte que fija el cuerpo del actuador a la estructura del tractor | 1 | ⚠️ Material y dimensiones por confirmar | Posiblemente impreso en 3D · Ver carpeta `cad/` |
| 3 | Bracket de montaje inferior / articulación | Pieza que conecta el vástago del actuador con el pedal del acelerador | 1 | ⚠️ Material y dimensiones por confirmar | Posiblemente impreso en 3D · Ver carpeta `cad/` |
| 4 | Pedal del acelerador (referencia) | Pedal físico original del tractor sobre el que actúa el actuador | — | ⚠️ Modelo de tractor por confirmar | STL de referencia disponible en `cad/pedal.stl` |
| 5 | Driver de motor | Módulo electrónico para controlar dirección y velocidad del actuador | 1 | ⚠️ Por definir | Depende del microcontrolador y voltaje del actuador |
| 6 | Microcontrolador | Unidad de procesamiento que recibe comandos de ROS 2 y genera señal de control | 1 | ⚠️ Por definir | — |
| 7 | Fuente de alimentación | Suministro de energía para el actuador y la electrónica | 1 | ⚠️ Voltaje y corriente por confirmar | Depende del actuador seleccionado |
| 8 | Tornillería y fijaciones | Tornillos, tuercas y arandelas para el montaje de los brackets | — | ⚠️ Tamaños por confirmar | Acero inoxidable recomendado para ambiente agrícola |

---

## Archivos CAD

Los archivos de diseño se encuentran en la carpeta [`cad/`](./cad/).

| Archivo | Descripción | Estado |
|---|---|---|
| `cad/pedal.stl` | Modelo STL del pedal del acelerador del tractor | ✅ Disponible |
| `cad/bracket-superior.stl` | Soporte de montaje del cuerpo del actuador | ⚠️ Por diseñar/confirmar |
| `cad/bracket-inferior.stl` | Articulación entre vástago y pedal | ⚠️ Por diseñar/confirmar |
| `cad/ensamble-completo.step` | Ensamble completo del subsistema | ⚠️ Por generar |

> **Nota:** El archivo `Actuador_2.STEP` en la raíz del repositorio corresponde al modelo CAD del actuador Oukeda para referencia de ensamble.

---

## Instalación mecánica

> ⚠️ **Antes de iniciar:** Asegúrate de que el tractor esté apagado, con freno de mano activado y las llaves retiradas del contacto.

### Paso 1 — Inspección del pedal y zona de montaje

Examina el pedal del acelerador y el espacio disponible bajo el tablero para determinar los puntos de fijación de los brackets.

![Foto paso 1](images/paso-01.jpg)

---

### Paso 2 — Montaje del bracket superior

Fija el bracket superior a la estructura del tractor en el punto de anclaje identificado en el paso anterior. Verifica que quede alineado con el eje de acción del pedal.

![Foto paso 2](images/paso-02.jpg)

---

### Paso 3 — Instalación del actuador lineal

Inserta el cuerpo del actuador Oukeda en el bracket superior y asegúralo con la tornillería correspondiente. El vástago del actuador debe quedar orientado hacia el pedal.

![Foto paso 3](images/paso-03.jpg)

---

### Paso 4 — Montaje del bracket inferior / articulación

Conecta el bracket inferior entre el extremo del vástago del actuador y el punto de contacto del pedal. Verifica que la articulación permita el movimiento libre y alineado del pedal.

![Foto paso 4](images/paso-04.jpg)

---

### Paso 5 — Verificación de recorrido mecánico

Con el actuador desconectado eléctricamente, extiende y retrae el vástago manualmente para verificar que:
- El pedal se desplaza sin interferencias.
- El recorrido cubre el rango completo de operación del acelerador (ralentí → máximo).
- Ningún componente roza con estructura, cables o mangueras del tractor.

![Foto paso 5](images/paso-05.jpg)

---

### Paso 6 — Fijación final y torque de apriete

Aplica el torque de apriete final a todos los puntos de fijación según las especificaciones de la tornillería utilizada.

![Foto paso 6](images/paso-06.jpg)

---

## Control electrónico

> ### 🚧 En desarrollo
>
> El esquema de control electrónico se encuentra en fase de definición. Los siguientes componentes están **pendientes de selección**:
>
> - **Microcontrolador:** Plataforma de procesamiento que ejecutará el nodo de ROS 2 o recibirá comandos vía interfaz serial/CAN.
> - **Driver de motor:** Módulo de potencia para controlar el actuador lineal (dirección y velocidad).
> - **Esquema de comunicación con ROS 2:** Protocolo de comunicación entre el nodo de control y el microcontrolador (serial UART, micro-ROS, CAN, etc.).
>
> Esta sección se completará una vez que se definan los componentes electrónicos del sistema.

---

## Integración con ROS 2

> ### 🚧 En desarrollo
>
> El nodo de ROS 2 para este subsistema se definirá una vez que el hardware electrónico esté seleccionado. Se espera que:
>
> - Suscriba a un topic de velocidad objetivo publicado por el módulo de navegación.
> - Traduzca la velocidad objetivo a una posición del actuador lineal.
> - Publique el estado actual del actuador (posición, corriente, errores).

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
