---
layout: default
title: Ideas Propuestas
nav_order: 2
---


# Ideas propuestas

Dos conceptos de tecnología vestible para rehabilitación física. Ambos parten del mismo principio: **convertir el ejercicio prescrito en la mecánica de un videojuego y registrar objetivamente lo que hoy se estima por autorreporte del paciente.**

El dispositivo mide el movimiento que el paciente hace por sí mismo; no lo asiste ni lo sustituye.

---

## El problema común

La necesidad de rehabilitación afecta a **2 410 millones de personas** en el mundo, y en países de ingresos bajos y medios más de la mitad de quienes la requieren no reciben servicios. Por eso el ejercicio domiciliario es la única opción realista para la mayoría.

Pero ahí falla el sistema: las estimaciones de **no adherencia llegan al 50 %**, y no existe un estándar de oro para medirla, porque casi todo depende del autorreporte del paciente.

El problema no es solo la falta de terapia. Es que la terapia prescrita no se hace, y nadie sabe que no se hizo.

---

## Opción A — NÚCLEO

**Prenda de torso para ejercicio lumbar gamificado**

Prenda sensorizada donde la posición de la pelvis controla el videojuego. Los ejercicios de estabilización lumbar (puente de glúteo, bird-dog, dead bug) mueven la partida, y solo se puntúa manteniendo la posición pélvica correcta.

**La idea clave:** estos ejercicios dependen de una posición pélvica neutra que el paciente no puede ver ni percibir. Un fisioterapeuta lo corrige con la mano sobre la pelvis; en casa no hay nadie. Un sensor inercial en el sacro entrega información a la que el usuario no tiene acceso perceptivo.

### ODS

**3.4 — Salud y bienestar (principal).** El dolor lumbar es la condición líder que genera necesidad de rehabilitación en **134 de 204 países**. Afectó a 619 millones de personas en 2020 y se proyectan 843 millones para 2050. Es la principal causa de discapacidad del mundo.

**8.8 — Seguridad laboral (secundario).** Es la principal razón de salida prematura de la fuerza laboral en adultos.

> El problema de rehabilitación más grande del mundo es también uno de los más ignorados: no tiene mención específica en la agenda de enfermedades no transmisibles de la OMS.

### Estado del arte y brecha

Existen sensores de postura de consumo (miden postura estática, no calidad de ejercicio), plataformas de telerrehabilitación (dependen de video o autoevaluación) y exotrajes lumbares pasivos (asisten, no entrenan ni miden).

**Lo que falta:** la zona de mayor necesidad global es la menos atendida por dispositivos gamificados; nadie ha resuelto la calidad de ejecución del ejercicio lumbar; y ninguna solución existente es una prenda.

### Reparto por disciplinas

- **Mecatrónica** — IMUs en sacro y torso, fusión sensorial para ángulo pélvico, calibración del neutro individual, detección de compensación, retroalimentación háptica.
- **Diseño industrial** — encapsulado plano que no moleste boca arriba, anclaje que no rote respecto a la pelvis, módulo desmontable para lavado.
- **Diseño textil** — patronaje con zonas de compresión que fijan los sensores sobre referencias óseas. El corte garantiza la colocación repetible: **el textil resuelve el problema de calibración.**

### MVP en 16 semanas

Validación del ángulo pélvico con error menor a 10°, detección de compensación superior al 85 %, y estudio de adherencia con 12–15 sujetos midiendo tiempo voluntario de ejercicio con juego frente a instrucciones convencionales.

---

## Opción B — REHAB-PLAY

**Guante sensorizado como control de videojuego**

Guante que mide flexión de dedos y movimiento de muñeca, y convierte los ejercicios de terapia de mano en el control del juego.

**Decisión de diseño:** un exoesqueleto que mueva la mano implica actuación, control de fuerza y seguridad que no se resuelven en un semestre. Un guante que mide el movimiento activo ataca el problema real —la adherencia— con una fracción de la complejidad y mejor fundamento clínico.

### ODS

**3.4 — Salud y bienestar (principal).** La necesidad de rehabilitación creció 63 % desde 1990. En Latinoamérica, solo entre el 2 y el 3 % de los niños y adolescentes con discapacidad accede a programas de rehabilitación. En México, 7.1 millones de personas viven con discapacidad, siendo las dificultades de movilidad las más reportadas.

**3.8 — Cobertura sanitaria universal (secundario).** Los dispositivos existentes con evidencia clínica cuestan miles de dólares o exigen renta mensual y receta médica.

### Estado del arte y brecha

**Sí existe algo similar, y con ensayos clínicos publicados.** Declararlo de frente es parte del argumento.

- **Neofect Smart Glove** — sensores de flexión y acelerómetro, dificultad adaptativa por IA, ensayo publicado en *J NeuroEng Rehabil*.
- **MusicGlove** — dos ensayos controlados aleatorizados en UC Irvine; usado en 400+ hospitales y 41 países.
- **GripAble** — dispositivo de mano de Imperial College London, validado contra dinamómetro Jamar.
- **Prototipos académicos Arduino** — abundantes, pero solo ingeniería: sin diseño industrial, sin textil, sin estudio de adherencia.

**Lo que falta:** costo y acceso real en Latinoamérica; integración textil (todos son carcasas plásticas, ninguno es prenda); inclusión de pacientes con movilidad mínima (MusicGlove exige poder tocar el pulgar con un dedo); y colocación con una sola mano.

### Reparto por disciplinas

- **Mecatrónica** — sensores de flexión, IMU dorsal, calibración al rango actual del usuario, métricas de calidad de movimiento, conexión HID o BLE.
- **Diseño industrial** — colocación con una sola mano (reto central), ajuste a distintos tamaños, encapsulado desmontable.
- **Diseño textil** — canales de sensor sobre las articulaciones correctas, elasticidad direccional, cero costuras en palma. **Tesis de diseño:** que se lea como accesorio de gaming y no como aparato médico, porque el paciente lo usa en casa frente a su familia.

### MVP en 16 semanas

Validación del sensado y repetibilidad de colocación, sistema completo con dificultad adaptativa, y estudio de adherencia con 12–15 sujetos.

---

## Comparación

| | **A — NÚCLEO** | **B — REHAB-PLAY** |
|---|---|---|
| Magnitud del problema | Primer lugar mundial | Menor en comparación |
| Saturación del mercado | Baja | Alta |
| Peso del diseño textil | Muy alto: la prenda es el producto | Medio |
| Dificultad técnica | Media (2–3 IMUs) | Media (5 flexores + IMU) |
| Acceso a sujetos de prueba | Muy alto | Medio |
| Riesgo de antecedente comercial | Bajo | Alto |
| ODS secundario | 8.8 seguridad laboral | 3.8 cobertura sanitaria |

---

## Recomendación

**Opción A como propuesta principal, Opción B como alternativa.**

A tiene mejor argumento de impacto por magnitud pura, enfrenta menos competencia directa, y es donde el diseño textil deja de ser empaque para convertirse en el dispositivo.

B es una propuesta sólida y más vistosa en demostración, pero exige posicionarse con humildad frente a productos que ya existen con evidencia clínica publicada.
