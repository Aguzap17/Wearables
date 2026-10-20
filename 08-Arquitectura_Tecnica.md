---
layout: default
title: Arquitectura
nav_order: 9
---

# Arquitectura técnica

**Proyecto semestral — Wearables** · Rodillera sensorizada para rehabilitación

---

## 1. ¿Cómo funciona el sistema?

El sistema mide el **ángulo de flexión de la rodilla** en tiempo real. Para lograrlo usa dos sensores de movimiento (IMU): uno en el muslo y uno en la espinilla, montados sobre una rodillera de compresión comercial. Cada sensor reporta la inclinación de su segmento de pierna; **la diferencia entre las dos inclinaciones es el ángulo de la rodilla.**

Un microcontrolador lee ambos sensores, calcula el ángulo y lo envía a una interfaz web donde se grafica en vivo, se cuentan repeticiones y se registra el progreso.

```
 [IMU 1 · muslo] ──┐
                   ├── bus I2C ──> [Microcontrolador] ──> USB / WiFi ──> Interfaz web
 [IMU 2 · espinilla]┘                     │
                                   [Batería LiPo + switch]
```

| Bloque | Componente | Función |
| --- | --- | --- |
| Sensado | 2 × MPU6050 (GY-521) | Miden la inclinación de muslo y espinilla |
| Procesamiento | Seeed XIAO ESP32-S3 | Lee los sensores, calcula el ángulo y lo transmite |
| Alimentación | LiPo 3.7 V + switch | Permite usarlo sin cables, todo el día |
| Interfaz | Página web | Gráfica en vivo, repeticiones y registro de sesiones |

---

## 2. Sensores (IMU)

Cada **MPU6050** es una unidad inercial de 6 ejes: acelerómetro (detecta hacia dónde apunta la gravedad, es decir, la inclinación) + giroscopio (detecta qué tan rápido gira). Se comunican por el protocolo **I2C**.

**¿Cómo conviven dos sensores iguales en los mismos cables?** I2C es un bus: todos los dispositivos comparten las mismas dos líneas de datos y cada uno tiene una dirección, como un nombre. La MPU6050 permite elegir entre dos nombres con su pin **AD0**:

- IMU 1 (muslo): AD0 a GND → dirección **0x68**
- IMU 2 (espinilla): AD0 a 3V3 → dirección **0x69**

Así el microcontrolador pregunta por nombre y cada sensor responde solo cuando le toca.

---

## 3. Microcontrolador

**Seeed Studio XIAO ESP32-S3** (21 × 17 mm, del tamaño de una moneda). Se eligió por tres razones:

1. **Tamaño**: cabe en una rodillera sin estorbar.
2. **Conectividad integrada**: WiFi y Bluetooth para hablar con la interfaz web sin cables.
3. **Cargador de batería incluido**: la LiPo se carga con el mismo cable USB-C con el que se programa, sin circuitos extra.

**Conexiones:**

| Pin del XIAO | Va a |
| --- | --- |
| GPIO5 (SDA) y GPIO6 (SCL) | Bus I2C → las dos IMUs |
| GPIO1 (A0) | Medidor de nivel de batería |
| Pads BAT+ / BAT− | Batería (el positivo pasa por el switch) |
| 3V3 y GND | Alimentación de los sensores |

*Para las pruebas de desarrollo se usa un ESP32-S3 DevKit (mismo chip, formato grande); la versión vestible usa el XIAO.*

---

## 4. Alimentación

- **Batería:** LiPo de 3.7 V con circuito de protección integrado — requisito para un dispositivo que va pegado al cuerpo. Se carga conectando el USB-C; la placa cambia sola entre USB y batería.
- **Apagador:** un switch deslizable en serie con el positivo de la batería. Apagado = consumo cero.
- **Medidor de batería:** el voltaje de la LiPo (hasta 4.2 V) es mayor de lo que el microcontrolador puede leer, así que se divide a la mitad con **dos resistencias iguales de 220 kΩ** y esa mitad se lee en el pin A0. En software se multiplica por dos y se convierte a porcentaje (4.2 V = 100 %, 3.3 V = 0 %).
- **Autonomía:** el WiFi solo se enciende durante la sesión de ejercicios (10–15 min); el resto del día el sistema duerme y solo registra actividad básica. Así la batería aguanta la jornada completa.

---

## 5. Cableado e integración física

**Cableado:**
- Cable **multifilar de silicona** (28–30 AWG): flexible y resistente a la fatiga — en un dispositivo que se dobla miles de veces al día, el alambre rígido se rompe.
- Cada señal viaja **trenzada con una tierra** para protegerla del ruido.
- Uniones soldadas con termorretráctil y punto de silicón como refuerzo, para que los jalones los absorba el refuerzo y no la soldadura.

**Montaje en la rodillera** (todo en la cara lateral externa de la pierna, evitando la corva y la rótula):
- **Módulo principal** (XIAO + batería + switch): parte alta del muslo, zona que no se flexiona.
- **IMU 1:** cara externa del muslo, 8–10 cm arriba de la rodilla.
- **IMU 2:** cara externa de la espinilla, 8–10 cm abajo.
- Los módulos van en **bolsillos con velcro**: se sacan en minutos para lavar la rodillera.
- El único cable que cruza la rodilla baja por el costado con una **curva de holgura en "S"**: al flexionar se estira la holgura, no el cobre.

---

## 6. Formato de medición

**Principio:** el acelerómetro de cada IMU detecta hacia dónde apunta la gravedad, y de ahí se calcula la inclinación de su segmento. El ángulo de rodilla es la resta:

```
ángulo de rodilla = inclinación del muslo − inclinación de la espinilla
```

Esto hace la medición independiente de la postura global: funciona igual de pie, sentado o acostado, porque lo que importa es el ángulo *entre* los dos segmentos.

**Calibración:** al iniciar, el usuario extiende la pierna y presiona "Calibrar": esa posición se registra como **0°**. Con eso se absorben las pequeñas diferencias de montaje de los sensores.

**Datos:** el sistema envía ~20 mediciones por segundo en texto simple:

```
muslo:12.4,espinilla:-78.9,rodilla:91.3
```

| Campo | Significado |
| --- | --- |
| `muslo` | Inclinación del muslo (grados) |
| `espinilla` | Inclinación de la espinilla (grados) |
| `rodilla` | Ángulo de la rodilla: 0° = pierna extendida, positivo = flexión |

**Sobre esa señal se calculan las métricas de los ejercicios:** repeticiones, flexión máxima de la sesión, tiempo de sostén y el test de sentarse-pararse de 30 segundos (una prueba clínica estándar).

**Comunicación por fases:**

| Fase | Canal |
| --- | --- |
| 1 (actual) | Cable USB → la página web lee el puerto directamente (Web Serial) |
| 2 | WiFi: el microcontrolador transmite y la misma página se conecta sin cable |

**Validación:** el ángulo del sistema se comparará contra un **goniómetro clínico** (el instrumento con el que los fisioterapeutas miden ángulos), con objetivo de error menor a ±5° — suficiente para guiar ejercicio; el dispositivo no pretende ser instrumento de diagnóstico.

---

## 7. Especificaciones del prototipo

| Especificación | Valor |
| --- | --- |
| Magnitud medida | Ángulo de flexión-extensión de rodilla |
| Sensores | 2 × IMU MPU6050 en bus I2C |
| Procesador | ESP32-S3 (WiFi + Bluetooth) |
| Frecuencia de medición | ~20 Hz |
| Alimentación | LiPo 3.7 V protegida, carga por USB-C |
| Interfaz | Página web con gráfica en vivo y conteo de repeticiones |
| Forma | Rodillera de compresión con módulos removibles |
| Costo de electrónica | < $600 MXN |
