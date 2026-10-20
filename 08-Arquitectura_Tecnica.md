---
layout: default
title: Arquitectura
nav_order: 9
---

# Arquitectura técnica

**Proyecto semestral — Wearables** · Rodillera sensorizada para rehabilitación

Este documento describe la ingeniería del prototipo: sensores, microcontrolador, alimentación, interconexión, integración física y el formato de medición.

---

## 1. Visión general del sistema

El sistema mide el **ángulo de flexión-extensión de la rodilla** en tiempo real con dos unidades inerciales (una en el muslo y una en la espinilla) montadas sobre una rodillera de compresión comercial. Un microcontrolador con WiFi/BLE procesa las lecturas, calcula el ángulo articular y lo transmite para visualización, retroalimentación y registro.

```
 [IMU 1 · muslo] ──┐
                   ├── bus I2C ──> [XIAO ESP32-S3] ──> Serial USB / WiFi ──> Interfaz web
 [IMU 2 · espinilla]┘                    │
                                         ├── [Motor de vibración] (feedback háptico)
                                         └── [LiPo 3.7 V + switch + divisor de medición]
```

| Bloque | Componente | Función |
| --- | --- | --- |
| Sensado | 2 × MPU6050 (breakout GY-521) | Acelerómetro + giroscopio de 6 ejes por segmento de la pierna |
| Procesamiento | Seeed XIAO ESP32-S3 | Lectura I2C, cálculo de ángulo, comunicación |
| Alimentación | LiPo 3.7 V protegida + switch SS-12D | Operación autónoma vestible |
| Retroalimentación | Módulo de vibración (motor de moneda con driver PWM) | Aviso háptico sin mirar pantalla |
| Interfaz | Página web (Serial / WebSocket) | Gráficas en vivo, conteo de repeticiones, registro |

---

## 2. Sensores: unidades inerciales (IMU)

**Componente:** MPU6050 en breakout GY-521 — acelerómetro de 3 ejes + giroscopio de 3 ejes (6 DoF), interfaz I2C.

**Configuración de operación:**

| Parámetro | Valor | Razón |
| --- | --- | --- |
| Rango de acelerómetro | ±4 g | Movimiento humano; mejor resolución que ±8/±16 g |
| Rango de giroscopio | ±500 °/s | Suficiente para velocidades articulares de rehabilitación |
| Filtro pasa-bajas (DLPF) | 21 Hz | Atenúa vibración y ruido muscular |
| Frecuencia de muestreo del sistema | ~20 Hz | Sobrada para movimiento articular; ligera para transmitir |

**Direccionamiento (dos sensores en un mismo bus):** la MPU6050 fija 6 de los 7 bits de su dirección I2C de fábrica; el séptimo lo define el voltaje del pin **AD0**:

- IMU 1 (muslo): AD0 → GND ⇒ dirección **0x68**
- IMU 2 (espinilla): AD0 → 3V3 ⇒ dirección **0x69**

Así ambas comparten las mismas cuatro líneas (bus) y el microcontrolador las interroga por nombre, sin conflicto. Las resistencias pull-up de SDA/SCL vienen integradas en cada breakout.

---

## 3. Microcontrolador

**Componente:** Seeed Studio **XIAO ESP32-S3** — ESP32-S3 de doble núcleo a 240 MHz, WiFi 2.4 GHz + BLE, 21 × 17 mm.

**Razones de la elección:**

- **Tamaño**: formato XIAO (del tamaño de una moneda) apto para vestible.
- **Conectividad integrada**: WiFi para la interfaz web (WebSocket) y BLE como opción futura.
- **Gestión de batería a bordo**: cargador LiPo integrado — se carga por el mismo USB-C que programa la placa, sin módulos externos.
- **Periféricos suficientes**: I2C por hardware, ADC para medición de batería, PWM para el motor háptico.

**Asignación de pines:**

| Pin | Señal | Conecta a |
| --- | --- | --- |
| GPIO5 (D4) | SDA | Bus I2C → ambas IMUs |
| GPIO6 (D5) | SCL | Bus I2C → ambas IMUs |
| GPIO1 (A0) | ADC | Nodo central del divisor de batería |
| GPIO digital c/PWM | Señal | Módulo de vibración |
| Pads BAT+ / BAT− | Alimentación | LiPo (BAT+ a través del switch) |
| 3V3 / GND | Riel regulado | IMUs, módulo de vibración, AD0 de IMU 2 |

---

## 4. Alimentación

### 4.1 Batería

**LiPo de 1 celda, 3.7 V nominal (4.2 V a plena carga), con circuito de protección (PCM) integrado** y conector JST. El PCM protege contra sobredescarga y cortocircuito — requisito para un dispositivo en contacto con el cuerpo.

La carga la realiza el circuito integrado del XIAO (~100 mA): basta conectar el USB-C. La placa conmuta automáticamente entre USB y batería sin intervención.

### 4.2 Interruptor de encendido

Un **switch deslizable SS-12D (SPDT)** en serie con el positivo de la batería: LiPo(+) → pin común → pin extremo → pad BAT+ del XIAO. El negativo va directo. Con el switch apagado se desconecta absolutamente todo (incluido el divisor de medición): consumo en apagado = 0.

### 4.3 Medición del nivel de batería

El voltaje de la LiPo (3.3–4.2 V) excede el rango del ADC, por lo que se lee a través de un **divisor resistivo de 2 × 220 kΩ** colocado *después* del switch:

```
V_BAT (tras switch) ──[220 kΩ]──●──[220 kΩ]── GND
                                │
                            A0 (ADC)
```

- El nodo central entrega **V_BAT / 2** (1.65–2.1 V), dentro del rango seguro del pin.
- El valor alto de las resistencias limita el drenaje a ~9 µA.
- En firmware se lee con `analogReadMilliVolts()` (usa la calibración de fábrica del ADC) promediando 16 muestras, se multiplica ×2 y se mapea linealmente: 4.2 V = 100 %, 3.3 V = 0 %.

### 4.4 Estrategia de consumo (diseño para uso diario)

El WiFi es el gran consumidor, así que el firmware opera en dos modos:

- **Modo día**: radio apagado; el microcontrolador duerme y la interrupción de movimiento de la IMU lo despierta para registrar actividad (pasos, flexión de marcha) a baja frecuencia, guardando resúmenes en memoria flash.
- **Modo sesión**: durante los 10–15 min de ejercicios se enciende el WiFi, hay streaming en vivo a la interfaz y retroalimentación en tiempo real; al terminar se sincroniza y vuelve a dormir.

---

## 5. Interconexión y cableado

- **Topología:** bus I2C compartido — cuatro líneas (3V3, GND, SDA, SCL) desde el microcontrolador hacia ambas IMUs, a 100 kHz.
- **Cable:** multifilar de silicona 28–30 AWG (flexible, resistente a fatiga). *Nunca* alambre sólido en tramos que se flexionan.
- **Pares trenzados:** cada señal viaja trenzada con una tierra (SDA+GND, SCL+GND, VCC+GND) para inmunidad al ruido. Las dos señales nunca se trenzan entre sí.
- **Uniones:** soldadas, con termorretráctil individual por conductor más un tubo general como alivio de tensión en cada salida de placa, y anclajes de adhesivo termofusible donde el cable sale de las soldaduras (las uniones no soportan esfuerzo mecánico directamente).
- **Montaje actual:** placa perforada como placa madre del prototipo (XIAO, switch, divisor y puntos de distribución del bus). Evolución prevista: PCB portadora de una cara fresada, con el XIAO montado plano por sus castellations.

---

## 6. Integración física en la rodillera

- **Base:** rodillera de compresión comercial tipo manga — aporta el acoplamiento firme sensor-cuerpo y la función de soporte ya validada del producto base.
- **Ubicación de módulos (cara lateral externa, evitando corva, rótula y cara interna):**
  - **Módulo principal** (XIAO + LiPo + switch): parte alta del muslo, zona plana que no se flexiona.
  - **IMU 1:** cara externa del muslo, 8–10 cm sobre la rodilla.
  - **IMU 2:** cara externa de la espinilla, 8–10 cm bajo la rodilla.
- **Sujeción:** bolsillos de tela elástica/neopreno cosidos con solapa de velcro — los módulos se extraen en minutos para lavar la prenda.
- **Ruteo del cable:** canales de tela; el único tramo que cruza la articulación baja por el costado con una **curva de holgura en "S"**, de modo que la flexión estira la holgura y no el cobre.
- El ángulo se obtiene por **diferencia entre las dos IMUs**, lo que hace la medición robusta a la orientación global de la pierna (de pie, sentado, acostado).

---

## 7. Formato de medición

### 7.1 Principio de medición

Cada IMU estima la **inclinación (pitch) de su segmento en el plano sagital** combinando acelerómetro y giroscopio. El ángulo de rodilla es la diferencia entre segmentos:

```
θ_rodilla = pitch_muslo − pitch_espinilla
```

- **Del acelerómetro:** `pitch = atan2(ax, √(ay² + az²))` — estable en reposo, ruidoso en movimiento.
- **Del giroscopio:** integración de velocidad angular — suave en movimiento, deriva en el tiempo.
- **Fusión (filtro complementario):** `ángulo = α·(ángulo + ω·dt) + (1−α)·ángulo_accel`, con α ≈ 0.96–0.98 — toma lo mejor de ambos. (Versión actual del prototipo: solo acelerómetro; el filtro es la siguiente iteración.)

### 7.2 Calibración

Al iniciar sesión, el usuario se coloca de pie con la **pierna extendida** y el sistema registra ese estado como **0°** (offset restado a todas las lecturas siguientes). Esto absorbe tolerancias de montaje y de fábrica de los sensores.

### 7.3 Formato de datos

Cadenas de texto a ~20 Hz, legibles tanto por el Serial Plotter como por la interfaz web:

```
muslo:12.4,espinilla:-78.9,rodilla:91.3
```

| Campo | Unidad | Descripción |
| --- | --- | --- |
| `muslo` | grados (°) | Pitch del segmento femoral |
| `espinilla` | grados (°) | Pitch del segmento tibial |
| `rodilla` | grados (°) | Ángulo articular (0° = pierna extendida; positivo = flexión) |

Sobre esta señal se calculan las métricas de los ejercicios: **repeticiones** (cruces de umbral con histéresis), **flexión máxima por sesión**, **tiempo de sostén**, **velocidad del movimiento** y el **test de sentarse-pararse de 30 s** (estándar clínico).

### 7.4 Canal de comunicación

| Fase | Canal | Uso |
| --- | --- | --- |
| 1 (actual) | USB Serial → Web Serial API | Desarrollo y validación; la página web lee el puerto directamente en el navegador |
| 2 | WiFi + WebSocket | Operación inalámbrica: el ESP32 sirve los datos y la misma interfaz web se conecta a él |
| Futuro | BLE | Opción de app móvil nativa / bajo consumo |

### 7.5 Validación prevista

Comparación del ángulo del sistema contra **goniómetro clínico** en posiciones estáticas (0°–120°), reportando el error medio. Objetivo: error ≤ ±5°, suficiente para retroalimentación de ejercicio terapéutico (no pretende grado de instrumento de diagnóstico).

---

## 8. Resumen de especificaciones del prototipo

| Especificación | Valor |
| --- | --- |
| Magnitud medida | Ángulo de flexión-extensión de rodilla |
| Sensores | 2 × IMU 6-DoF (MPU6050) en bus I2C |
| Procesador | ESP32-S3 doble núcleo, WiFi/BLE |
| Frecuencia de medición | ~20 Hz |
| Alimentación | LiPo 3.7 V protegida, carga por USB-C |
| Retroalimentación | Háptica (vibración) + interfaz web en vivo |
| Forma | Rodillera de compresión con módulos removibles |
| Costo de componentes electrónicos | < $600 MXN |
