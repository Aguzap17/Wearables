---
layout: default
title: Analisis de Clusters
nav_order: 7
---

# Análisis de clusters y primera idea de solución

**Proyecto semestral — Tecnología vestible** · Fase: Definir

Tras el ejercicio de post-its (affinity mapping) con insights de entrevistas y estudios publicados, los agrupamientos resultantes fueron seis. A continuación se interpreta cada cluster, la generalidad a la que convergen, y la primera idea de solución derivada de ellos.

---

## Interpretación de los clusters

### 1. "Se ponen sus propios ejercicios" → El paciente se autogestiona a ciegas

Sin guía continua, cada quien improvisa: sustituye la terapia con actividad cotidiana (subir escaleras), simplifica los ejercicios por edad o por estar solo, no busca otra opción después del alta, y usa **el dolor como su único instrumento de medición** ("me guío por el dolor"). No es rebeldía: nadie le dio otro instrumento.

### 2. "Objetivos del usuario" → La meta del paciente no es la salud, es la vida

Quiere volver a sus actividades normales "aunque duela". Por eso se da de alta solo al alcanzar *su* meta funcional (caminar, trabajar, jugar), que casi siempre llega antes que la recuperación clínica completa.

### 3. "Adaptación a la lesión" → El abandono se normaliza en silencio

"Si lo dejo de hacer, nadie se entera" + "si ya no me duele, ya no lo hago" + vendarse la lesión para poder seguir haciendo deporte. El paciente no abandona la recuperación: **negocia con su lesión** y aprende a vivir con el dolor y sus limitaciones. El sistema de salud no detecta esta negociación.

### 4. "Tiempos" → La terapia es rígida en una vida que no lo es

No compite solo contra el trabajo y la familia: compite contra viajes, horarios cambiantes, falta de organización y de motivación. Al no haber flexibilidad, **no se forma el hábito**. El formato de la terapia (sesión fija, larga, en casa) no está diseñado para la vida real.

### 5. "Retos del usuario" → La experiencia de la terapia es hostil

Aburrida, repetitiva y frustrante; son muchos ejercicios; si es muy difícil no se quiere seguir; no hay resultados rápidos y sin avances visibles no hay motivación. Se hace por obligación, no por gusto. El diseño de la experiencia trabaja *en contra* de la adherencia.

### 6. "Interacción con el médico" → El vínculo profesional se rompe entre consultas

El fisioterapeuta da seguridad y confianza… pero solo en consulta ("solo los hago en consulta"). Nadie enseñó la técnica exacta, hay miedo a lastimarse por hacerlo mal, se necesita orientación de repeticiones, intensidad y carga, el médico no conoce bien al paciente, y cuando pregunta si hicieron los ejercicios, mienten. Este cluster también contiene la solución que los propios usuarios señalan: **el monitoreo y seguimiento evitan recaídas, y la educación previa previene lesiones**.

---

## Generalidad: ¿a qué converge todo?

> **El dolor es el único sensor que tiene el paciente — y es un sensor malo.**

- Se guía por el dolor para ejercitarse (cluster 1).
- Abandona cuando el dolor calla, aunque la recuperación no esté completa (cluster 3).
- Su meta es funcional, no clínica: volver a su vida, aunque duela (cluster 2).
- Nadie —ni él ni su médico— tiene datos para corregir el rumbo (cluster 6).
- Y la terapia que debería sustituir a ese mal sensor es rígida (cluster 4) y hostil (cluster 5).

**El paciente no le falla al sistema; el sistema no le da instrumentos.** La incertidumbre sobre la propia ejecución, la invisibilidad del progreso y la ausencia de acompañamiento convierten el periodo entre consultas en una zona ciega donde cada paciente improvisa su propia rehabilitación.

---

## Primera idea de solución

Un **sistema vestible que sustituya al dolor como instrumento del paciente**, con cuatro funciones que responden una a una a los clusters:

| Función | Qué hace | Clusters que ataca |
| --- | --- | --- |
| **1. Guía de técnica en tiempo real** | Le dice al paciente en el momento si el movimiento es correcto y orienta repeticiones, intensidad y carga: el "fisio de bolsillo" que da la seguridad que hoy solo existe en consulta. | 1, 6 |
| **2. Progreso visible en términos de vida, no de clínica** | Muestra el avance hacia *sus* metas funcionales ("ya puedes cargar X", "tu marcha mejoró Y%"), con mecánicas de juego que rompen la monotonía y dan razón para volver mañana. | 2, 5 |
| **3. Flexibilidad por diseño** | Microsesiones que se adaptan a horarios, viajes y días malos; construye hábito en vez de exigir disciplina. | 4 |
| **4. Canal silencioso con el profesional** | El terapeuta ve datos reales de ejecución y detecta el abandono o la "auto-alta" antes de la recaída: se acaba el mentir y el "nadie se entera". | 3, 6 |

**Criterios heredados de la investigación previa:** bajo costo y compra directa (sin aseguradora ni receta), cero configuración ("que se ponga y funcione"), cómodo y no invasivo, y con evidencia demostrable de utilidad ("ver el dato, no la promesa").

> **Nota:** esta es una primera idea derivada de los clusters, no un producto definido. La forma concreta (prenda, accesorio, segmento corporal, mecánica de juego) se definirá en la fase de ideación y prototipado.
