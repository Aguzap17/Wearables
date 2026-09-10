---
layout: default
title: Estado del Arte
nav_order: 5
---

# Estado del arte

**Proyecto semestral — Tecnología vestible**

*¿Cómo se está resolviendo actualmente el problema de la rehabilitación en casa sin supervisión?*

---

## Tabla comparativa de soluciones existentes

| Solución / producto | Usuario | ¿Qué resuelve? | ¿Cómo lo hace? | Ventajas | Limitaciones |
| --- | --- | --- | --- | --- | --- |
| **A. Hoja impresa de ejercicios** (estándar de atención actual) | Cualquier paciente ambulatorio | Transmitir la prescripción del terapeuta al hogar | Diagramas e instrucciones en papel | Costo ~cero; universal; no requiere tecnología | Sin retroalimentación, sin motivación, sin registro; es el grupo control que toda la investigación busca vencer [5][10] |
| **B. Video / app de ejercicios** (no vestible) | Paciente ambulatorio con smartphone | Claridad de la instrucción y confianza en la ejecución | Videos demostrativos de cada ejercicio en el teléfono | RCT: adherencia y confianza superiores a la hoja impresa; bajo costo [5] | No mide al paciente: muestra el ejercicio, pero no verifica la ejecución ni registra nada |
| **C. Hinge Health** (plataforma digital, EE.UU.) | Empleados con cobertura de aseguradora/empleador | Supervisión y corrección sin ir a consulta | Visión por computadora que rastrea +100 puntos del cuerpo, mide ángulos articulares y corrige la forma en tiempo real; kit de sensores vestibles [7] | Corrección en tiempo real validada a gran escala; empresa pública (~$4.3 mil M USD) [7] | No se vende al paciente directo; acceso solo vía aseguradoras de EE.UU.; requiere colocarse frente a la cámara del teléfono |
| **D. Sword Health** (plataforma digital, Portugal/EE.UU.) | Empleados con cobertura empresarial | Fisioterapia remota supervisada con datos | Sensores de movimiento + IA con retroalimentación en tiempo real, supervisado por fisioterapeutas licenciados [8] | Combina humano + IA; valuación ~$4 mil M USD valida el mercado [8] | Mismo modelo B2B cerrado; inexistente como opción directa en Latinoamérica |
| **E. RAPAEL Smart Glove** (Neofect, dispositivo gamificado) | Pacientes neurológicos (EVC), principalmente en clínica | Motivación y medición en rehabilitación de mano | Guante con sensor de movimiento de 9 ejes y 5 sensores de flexión; ejercicios como videojuegos | Ensayo clínico: mejoras significativas en escalas Fugl-Meyer y Jebsen-Taylor vs. terapia convencional [9] | Versión clínica de ~$15,000 USD [11]; enfocado a EVC, no a lesión musculoesquelética común |
| **F. Prototipos académicos de biofeedback vestible** | Participantes de estudios (post-quirúrgicos, lumbar) | Saber en el momento si el ejercicio está bien hecho | IMUs, sEMG y modelado biomecánico; en algunos casos sensores integrados a prenda + app con reconocimiento de patrones [12][13] | RCT: biofeedback con sensores supera a la retroalimentación convencional [14]; prototipo en casa logró 79% de adherencia y usabilidad de 90.8/100 [10] | No llegan al mercado; alta heterogeneidad de sensores y métricas, sin estándar consolidado [13] |
| **G. Telerrehabilitación con sensores** | Pacientes con acceso a programas remotos | El puente de datos paciente–terapeuta | Programas guiados a distancia; los sensores intervienen y a la vez miden objetivamente el progreso | Mejoras clínicamente significativas en discapacidad y funcionalidad; sustituye el autorreporte [15] | Depende de infraestructura clínica y disponibilidad del programa; aún no masivo |

---

## Análisis crítico: patrones y contradicciones

**Patrones que se repiten en todos los enfoques:**

1. **La retroalimentación en tiempo real funciona.** Aparece como el mecanismo central en C, D, E y F, y está validada por ensayo controlado [14]. Toda la evidencia converge: decirle al paciente *en el momento* si lo hace bien cambia el resultado.
2. **La gamificación resuelve la adherencia, no es adorno.** El único dispositivo con resultados clínicos publicados frente a terapia convencional (E) es precisamente el que convierte el ejercicio en juego [9].
3. **La medición objetiva sustituye al autorreporte.** C, D, F y G coinciden en generar datos de ejecución para el profesional, atacando la tercera falla del problema [15].

**Contradicciones y tensiones detectadas:**

- **Lo validado no es accesible; lo accesible no está validado.** Las soluciones con mayor evidencia y escala (C, D, E) cuestan miles de dólares o están cerradas tras aseguradoras de EE.UU.; las opciones al alcance del paciente (A, B) no miden nada.
- **La visión por computadora contradice la barrera de tiempo.** C y D exigen colocarse frente a una cámara bien posicionada, con espacio y momento dedicados — exactamente lo que el usuario de 35–60 años con jornada saturada no tiene [1][2]. Un vestible no impone ese ritual.
- **El público estudiado no es el público afectado.** La investigación de dispositivos (E, F) se concentra en pacientes neurológicos o de laboratorio, mientras la carga epidemiológica mayor está en trastornos musculoesqueléticos de población trabajadora [6].
- **La academia y el mercado no se tocan.** Los prototipos (F) demuestran adherencia de 79% [10] pero no se comercializan; los productos comerciales no publican con ese nivel de detalle metodológico.

---

## Síntesis: necesidades no resueltas y oportunidades

Qué permanece sin resolver, sin anticipar todavía una solución específica:

1. **Accesibilidad económica y de canal.** No existe una opción de retroalimentación en tiempo real comprable directamente por el paciente a precio de consumo, y ninguna opera en el contexto latinoamericano.
2. **El segmento trabajador de 35–60 años está desatendido.** Ni los dispositivos clínicos (diseñados para EVC) ni las plataformas B2B (empleados de corporativos de EE.UU.) están diseñados para quien hace terapia en huecos de 10 minutos entre trabajo y familia.
3. **Medición sin ritual.** Falta explorar formas de sensado que vivan en el cuerpo (prenda, accesorio) y funcionen en microsesiones, sin montar cámaras ni despejar espacios.
4. **La tríada completa en un solo sistema accesible.** Retroalimentación inmediata + motivación sostenida + datos objetivos para el terapeuta existen por separado (F, E, G respectivamente); su integración a bajo costo es el espacio de exploración abierto.

---

## Referencias

[1] Palazzo, C., et al. (2016). *Barriers to home-based exercise program adherence with chronic low back pain: Patient expectations regarding new technologies*. Ann Phys Rehabil Med, 59(2). https://pubmed.ncbi.nlm.nih.gov/27050664/

[2] Boutevillain, L., et al. (2017). *Facilitators and barriers to physical activity in people with chronic low back pain*. PLOS ONE. https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0179826

[5] *Patient Adherence With At-Home Hand and Wrist Exercises: RCT of Video Versus Handout Format* (2021). HAND. https://pmc.ncbi.nlm.nih.gov/articles/PMC10233647/

[6] Organización Mundial de la Salud (2023). *Low back pain — Fact sheet*. https://www.who.int/news-room/fact-sheets/detail/low-back-pain

[7] Sacra Research (2026). *Hinge Health revenue, funding & growth rate*. https://sacra.com/c/hinge-health/

[8] Contrary Research (2025). *Sword Health Business Breakdown & Founding Story*. https://research.contrary.com/company/sword-health

[9] Neofect. *Smart Glove — Clinical Trial Results* (J NeuroEng Rehabil). https://www.neofect.com/us/smart-glove

[10] *Wearable Sensor-Based Exercise Biofeedback for Orthopaedic Rehabilitation: A Mixed Methods User Evaluation* (2019). PMC. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6359655/

[11] Digital Trends (2017). *Neofect's Rapael Smart Glove*. https://www.digitaltrends.com/health-fitness/neofect-smart-glove-ces-2017/

[12] *Wearable Sensor Array Design for Spine Posture Monitoring During Exercise Incorporating Biofeedback* (2020). PubMed. https://pubmed.ncbi.nlm.nih.gov/32031929/

[13] *Bioengineering Support in the Assessment and Rehabilitation of Low Back Pain* (2025). PMC. https://pmc.ncbi.nlm.nih.gov/articles/PMC12467509/

[14] *Sensor-based postural feedback vs. conventional feedback in chronic low back pain: RCT* (2018). J NeuroEng Rehabil, 15:85.

[15] *Effects of a home-based telerehabilitation exercise program in non-specific chronic low back pain* (2026). PubMed. https://pubmed.ncbi.nlm.nih.gov/41990574/
