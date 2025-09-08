# Laboratorio 6 — Análisis de redes sociales

**Dataset principal del informe:** `tioberny` (con opción de correr todo en `traficogt` cambiando `DATASET`).  
**Notebook base:** `main.ipynb` · **Carpeta de salidas:** `outputs/`

---

## Cómo correrlo rápido

1. En `main.ipynb`, arriba cambia `DATASET = "tioberny"` o `"traficogt"` y ejecuta todo.
2. La carga usa lector **JSONL robusto** (omite líneas corruptas y detecta UTF-16).
3. El pipeline crea el grafo de interacciones (menciones/respuestas), detecta comunidades (Louvain), calcula centralidades, extrae tópicos (LDA) y sentimiento (VADER-ES).
4. Al final, ejecuta `exportar_resultados(...)` para generar CSVs y un **resumen ejecutivo `.md`** en `outputs/`.

---

## Preguntas guía (EDA) y respuestas

### ¿Quién domina la conversación?

La mayor parte de la interacción se organiza alrededor de la cuenta del presidente; su nodo concentra menciones y respuestas, y aparece como el hub más grande de la red.

### ¿Cómo se expresa la crítica?

Existe una comunidad muy activa con foco en prensa y discusión pública; ahí el tono es más confrontativo y se habla de “netcenters”, lo que empuja la conversación a temas de accountability.

### ¿Qué papel juega lo institucional?

Otra comunidad agrupa cuentas oficiales y medios públicos que publican actualizaciones y anuncios; aquí el tono es más neutro e informativo y ayuda a estabilizar la conversación.

---

## Métricas de red (lectura rápida)

La red es dispersa (baja densidad) con un componente grande donde se dan casi todas las interacciones. El diámetro y la cercanía sugieren que los mensajes llegan relativamente rápido a los nodos visibles gracias a unos pocos hubs y conectores.

---

## Comunidades (Top-3)

### Comunidad 5 — conversación masiva alrededor del presidente

Es la más grande y gira en torno al presidente; conviven mensajes de apoyo y de crítica, con muchas cuentas individuales aportando pocos tuits cada una.

### Comunidad 9 — prensa, crítica y “netcenter”

Agrupa cuentas periodísticas y perfiles muy activos que cuestionan; el tono es más duro y hay conectores que mueven temas entre grupos.

### Comunidad 0 — bloque institucional/medios públicos

Reúne cuentas oficiales y medios estatales; el discurso es informativo sobre gestión y territorio, con menos polémica y más boletín.

---

## Influencers

Los nodos con mayor grado y betweenness marcan la pauta: el presidente como hub central, cuentas institucionales como amplificadores y algunos conectores (periodistas/activistas) que enlazan comunidades.

---

## Componentes pequeños

Aparecen microgrupos de 2–3 nodos con menciones aisladas; no mueven volumen, pero explican interacciones puntuales y actividad esporádica de cuentas.

---

## Tópicos (LDA)

Los temas se separan por comunidad: institucional habla de programas y lugares; el bloque crítico gira sobre prensa, preguntas y corrupción; el cluster masivo mezcla gobierno, semilla y salud, reflejando la mezcla de apoyo y crítica.

---

## Sentimiento

El promedio global tiende a lo crítico por la comunidad de discusión pública; el bloque institucional se mantiene más neutro y el cluster masivo combina picos de apoyo con crítica fuerte.

---

## Conclusiones

La conversación se estructura en torno a un hub presidencial, un bloque crítico-mediador y un carril institucional. Los conectores facilitan que los temas crucen de un grupo a otro; en conjunto el tono es más crítico, aunque coexiste con mensajes de apoyo y la capa informativa.

---

## Reproducibilidad

- **Selector:** `DATASET = "tioberny" | "traficogt"`.
- **Carga robusta:** `cargar_jsonl_seguro(...)` (UTF-16 + omisión de líneas corruptas).
- **Limpieza:** `clean_text`, normalización de `user/id_str` y columnas opcionales con helpers.
- **Red:** `edges` → `G (DiGraph)` → métricas + `community_louvain`.
- **Contenido:** LDA (CountVectorizer) + VADER con léxico ampliado en español.
- **Exports:** `exportar_resultados(...)` genera CSVs y `*_resumen_ejecutivo.md`.

---

## Archivos que se generan en `outputs/`

- `{DATASET}_comm_sizes.csv`
- `{DATASET}_top3_resumen_tamanos.csv`
- `{DATASET}_top_influencers.csv`
- `{DATASET}_sentimiento_promedio_top3.csv`
- `{DATASET}_sentimiento_distrib_top3.csv`
- `{DATASET}_componentes_pequenos.csv`
- `{DATASET}_lda_topics_comm{X}.csv` y `{DATASET}_lda_topics_combined.csv`

---
