# 🚜 Transformación de Tractores Convencionales en Tractores Inteligentes con Geolocalización y Percepción

![Estado del proyecto](https://img.shields.io/badge/Estado-En%20desarrollo-yellow)
![Licencia](https://img.shields.io/badge/Licencia-MIT-green)
![ROS 2](https://img.shields.io/badge/ROS%202-Humble-blue)
![Institución](https://img.shields.io/badge/Institución-UIA%20Puebla-darkred)

---

## Descripción general

Este repositorio contiene el desarrollo de un sistema de **retrofit mecatrónico** para la automatización de tractores agrícolas convencionales, desarrollado como proyecto de investigación en la **Universidad Iberoamericana Puebla**.

El proyecto busca transformar tractores de combustión interna con caja de velocidades manual —ya existentes en el campo— en plataformas inteligentes capaces de operar de forma autónoma o semiautónoma, integrando tecnologías de navegación de alta precisión, percepción del entorno y control automatizado de los mandos físicos del vehículo, sin necesidad de reemplazar la maquinaria existente.

---

## Problemática

El estado de Puebla, México, presenta índices de **mecanización agrícola insuficiente**. Gran parte de los agricultores de pequeña y mediana escala operan con tractores de tecnología antigua, sin acceso a los sistemas de agricultura de precisión disponibles en plataformas modernas. Adquirir maquinaria nueva con estas capacidades representa una inversión inaccesible para la mayoría.

La propuesta de **retrofit** (actualización tecnológica sobre plataforma existente) permite incorporar capacidades de autonomía y percepción a tractores convencionales a un costo significativamente menor, democratizando el acceso a la agricultura de precisión en regiones con recursos limitados.

---

## Objetivo

Diseñar, implementar y validar un sistema modular de automatización para tractores agrícolas convencionales que integre:

- **Navegación RTK** para posicionamiento centimétrico en campo abierto.
- **Percepción del entorno** mediante LiDAR y cámaras.
- **Control físico automatizado** del volante, acelerador y caja de velocidades, mediante actuadores que operan directamente sobre los controles del tractor.

La validación del sistema se realiza mediante simulación en **ROS 2** y **Gazebo** antes de la implementación física.

---

## Arquitectura del sistema

El sistema se divide en tres subsistemas mecatrónicos independientes y un módulo de navegación/percepción central:

```
┌─────────────────────────────────────────────────────────┐
│              Sistema de Control Central                  │
│         (ROS 2 · RTK GNSS · LiDAR · Cámaras)           │
└────────────┬────────────┬─────────────┬─────────────────┘
             │            │             │
    ┌────────▼──┐  ┌──────▼────┐  ┌────▼──────────┐
    │ Actuador  │  │ Actuador  │  │     Caja de   │
    │ Acelerador│  │  Volante  │  │  Velocidades  │
    └───────────┘  └───────────┘  └───────────────┘
```

---

## Estado de los subsistemas

| Subsistema | Carpeta | Estado | Descripción |
|---|---|---|---|
| Actuador de Acelerador | [`actuador-acelerador/`](./actuador-acelerador/) | ✅ En documentación | Actuador lineal Oukeda sobre pedal físico del tractor |
| Actuador de Volante | [`actuador-volante/`](./actuador-volante/) | 🚧 En desarrollo | Control de dirección mediante motor sobre columna de dirección |
| Caja de Velocidades | [`caja-velocidades/`](./caja-velocidades/) | 🚧 En desarrollo | Automatización del cambio de marchas manual |

---

## Tecnologías

| Capa | Tecnología |
|---|---|
| Middleware robótico | ROS 2 Humble |
| Simulación | Gazebo |
| Navegación | RTK GNSS (precisión centimétrica) |
| Percepción | LiDAR · Cámaras estéreo |
| Actuación | Actuadores lineales eléctricos |
| Fabricación | Impresión 3D (brackets y soportes) |

---

## Estructura del repositorio

```
TRACTOR/
├── README.md                   ← Este archivo
├── actuador-acelerador/
│   ├── README.md               ← Documentación del subsistema ✅
│   ├── cad/                    ← Archivos STL y modelos CAD
│   └── images/                 ← Fotografías de instalación
├── actuador-volante/
│   └── README.md               ← 🚧 En desarrollo
└── caja-velocidades/
    └── README.md               ← 🚧 En desarrollo
```

---

## Cómo citar este proyecto

Si utilizas este trabajo en una publicación académica, por favor cítalo de la siguiente manera:

```bibtex
@misc{ochoa_gonzalez_2026,
  author       = {Ochoa García, Oliver and González Fernández, Belinka},
  title        = {Transformación de tractores convencionales en tractores inteligentes
                  con geolocalización y percepción},
  year         = {2026},
  institution  = {Universidad Iberoamericana Puebla},
  howpublished = {\url{https://github.com/[usuario]/[repositorio]}},
  note         = {Proyecto de investigación en desarrollo}
}
```

---

## Contribuidores

| Nombre | Rol | Institución |
|---|---|---|
| Oliver Ochoa García | Investigador / Desarrollador | Universidad Iberoamericana Puebla |
| Belinka González Fernández | Investigadora / Desarrolladora | Universidad Iberoamericana Puebla |

---

## Licencia

Este proyecto está distribuido bajo la licencia **MIT**. Consulta el archivo [`LICENSE`](./LICENSE) para más detalles.

---

*Universidad Iberoamericana Puebla · Departamento de Ingeniería · 2026*
