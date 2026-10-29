# Data para presentación: música y salud mental

## Enfoque

**Pregunta guía:** ¿Cómo describen las personas encuestadas su relación cotidiana con la música y su salud mental?

**Motivación sugerida:** elegí esta encuesta porque combina hábitos musicales con valoraciones personales de bienestar, lo que permite explorar si la música se percibe igual para todas las personas. Ajusta esta motivación a tu razón real para elegir el dataset.

**Idea central:** la mayoría dice que la música mejora su estado, pero las respuestas no son iguales para todos; además, los puntajes de ansiedad y depresión tienden a aumentar juntos. La encuesta muestra asociaciones y percepciones, no que escuchar música cause cambios de salud mental.

## El dataset en una diapositiva

- 736 respuestas y 33 columnas; cada fila representa una respuesta de encuesta.
- La edad tiene 735 respuestas válidas: mediana 21 años, promedio 25.2 y rango de 10 a 89. El grupo de 18–24 años representa 334 personas (45.4% de las filas con edad), así que la muestra se concentra en gente joven.
- Hábitos: servicio de streaming, horas de escucha al día, escuchar mientras se trabaja, tocar instrumentos, componer, explorar géneros y escuchar música en otros idiomas.
- Preferencias: género favorito y frecuencia de escucha de 18 géneros.
- Salud autopercibida: Anxiety, Depression, Insomnia y OCD aparecen como puntajes enteros de 0 a 10. Son autovaloraciones del cuestionario, no diagnósticos clínicos.
- `Music effects` registra la percepción de si la música mejora, no afecta o empeora la salud mental.

### Tipos de variables y sorpresas

| Columnas | Qué representan | Tipo real / observación |
|---|---|---|
| `Timestamp` | Momento de respuesta | Fecha y hora; pandas la lee como texto si no se convierte explícitamente |
| `Age` | Edad en años | Numérica discreta; aparece como decimal porque hay un valor faltante |
| `Primary streaming service`, `Fav genre`, `Music effects` | Servicio, género favorito y efecto declarado | Categóricas nominales |
| `Hours per day` | Horas de escucha por día | Numérica; rango observado de 0 a 24 |
| `While working`, `Instrumentalist`, `Composer`, `Exploratory`, `Foreign languages` | Respuestas sí/no | Categóricas binarias, aunque estén guardadas como texto |
| `Frequency [...]` | Frecuencia por género | Categóricas ordinales: Never, Rarely, Sometimes, Very frequently; no son números continuos |
| `Anxiety`, `Depression`, `Insomnia`, `OCD` | Puntajes de autorreporte | Escala discreta acotada de 0 a 10; no interpretar como diagnóstico |
| `BPM` | Tempo indicado por quien responde | Numérica, pero contiene valores faltantes y anomalías que no deben usarse sin revisar |
| `Permissions` | Texto de consentimiento | Categórica / administrativa; no es un indicador de salud |

**Chequeos concretos:** no hay filas completamente duplicadas. Hay 129 celdas vacías repartidas en 120 filas; `BPM` concentra 107 faltantes (14.5% del dataset), seguido de `Music effects` con 8. En `BPM` aparecen tres ceros y valores 624 y 999,999,999, claramente sospechosos para un tempo musical. No imputé ni corregí esos valores para estos hallazgos; excluí BPM de la historia.

## Tres insights y los datos para los gráficos

### 1. La mayoría percibe una mejora, pero la respuesta no es universal

**Gráfico:** barras horizontales con porcentaje y conteo. Usa solo respuestas no vacías de `Music effects`; denominador `n = 728`.

| Efecto declarado | Respuestas | Porcentaje de respuestas válidas |
|---|---:|---:|
| Improve | 542 | 74.5% |
| No effect | 169 | 23.2% |
| Worsen | 17 | 2.3% |

**Qué decir:** “Casi tres de cada cuatro personas que contestaron esta pregunta sienten que la música les ayuda; para cerca de una de cada cuatro no cambia nada. Un grupo pequeño dice que empeora.”

**So what:** no conviene asumir que la música tiene el mismo efecto para todos; una experiencia o recomendación debería permitir preferencias individuales.

**Cautela:** las 17 respuestas “Worsen” son pocas y no representan necesariamente a una población más amplia.

### 2. Los puntajes de ansiedad y depresión tienden a moverse juntos

**Gráfico:** dispersión con transparencia o mapa de densidad de `Anxiety` frente a `Depression`; ambos ejes de 0 a 10. Los puntos se superponen porque las escalas son discretas.

| Resumen | Valor |
|---|---:|
| Correlación de Pearson entre Anxiety y Depression | 0.52 |
| Respuestas incluidas | 736 |

**Qué decir:** “En esta muestra, quienes reportan puntajes más altos de ansiedad suelen reportar también puntajes más altos de depresión. La relación es visible, aunque hay variación entre personas.”

**So what:** al describir bienestar autopercibido, puede ser más útil mirar varias dimensiones juntas que tratar cada puntaje como una historia aislada.

**Cautela:** correlación no demuestra causalidad; los puntajes son autoinformados y no clínicos.

### 3. Más horas de escucha coinciden con puntajes de insomnio algo mayores, pero la relación es débil

**Gráfico:** barras o puntos de promedio de `Insomnia` por tramo de horas; añade `n` por grupo. Los tramos son descriptivos y el máximo de 24 horas pertenece al último grupo.

| Horas de música al día | n | Promedio de horas observado | Promedio de Insomnia (0–10) |
|---|---:|---:|---:|
| 0–1 h | 148 | 0.87 | 3.21 |
| Más de 1–3 h | 316 | 2.36 | 3.58 |
| Más de 3–6 h | 185 | 4.80 | 4.02 |
| Más de 6–24 h | 87 | 9.95 | 4.63 |

La correlación lineal entre horas y puntaje de insomnio es **r = 0.142**, una asociación débil. El promedio pasa de 3.21 en el grupo de 0–1 hora a 4.63 en el de más de 6 horas, pero esto no permite concluir que escuchar más música empeore el sueño.

**So what:** la duración por sí sola no explica el bienestar; una investigación posterior debería considerar contexto, hora de escucha y otros factores, sin convertir este patrón en recomendación clínica.

## Guion sugerido: 5–7 minutos, presentación individual

| Diapositiva | Tiempo | Contenido |
|---|---:|---|
| 1. Pregunta y motivación | 30–45 s | Presenta la pregunta guía y por qué te interesó la relación entre hábitos musicales y bienestar autopercibido. |
| 2. Qué contiene la encuesta | 60–75 s | Explica 736 respuestas, 33 columnas, las familias de variables y que los puntajes son autorreportados de 0 a 10. |
| 3. La música no se percibe igual para todos | 60–75 s | Muestra el gráfico 1, explica el denominador 728 y destaca el grupo pequeño que reporta empeoramiento. |
| 4. Dos dimensiones de bienestar relacionadas | 60–75 s | Muestra Anxiety vs Depression, explica r = 0.52 y aclara que es asociación, no causalidad. |
| 5. Horas y sueño | 60–75 s | Muestra el gráfico 3 con tamaños de grupo; interpreta la relación como débil y no causal. |
| 6. Qué aprendí / límites | 30–45 s | Menciona BPM anómalo, faltantes, concentración de edades jóvenes y el “so what” general. |

## Frase de cierre

“En esta encuesta, la música se percibe como una ayuda para muchas personas, pero no para todas. Las respuestas también muestran que ansiedad y depresión suelen aparecer juntas; la relación entre horas de escucha e insomnio es mucho más débil. Esto sirve para generar preguntas y diseñar análisis siguientes, no para afirmar que la música cause un resultado de salud.”

## Preparación pendiente del repositorio

El notebook existente analiza resultados de Fórmula 1 y no genera estos gráficos. Para que el código del repositorio corresponda a la presentación, hace falta crear y subir un notebook específico para `mxmh_survey_results.csv` que replique estas tablas y visualizaciones. También conviene añadir la fuente original del dataset y aclarar cómo se reclutaron las personas encuestadas; esos metadatos no aparecen en el CSV.