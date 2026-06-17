# Análisis de reseñas — Back Market (backmarket.co.uk)

Caso práctico de Deep Learning / NLP: análisis de sentimiento y extracción de topics sobre las reseñas de Trustpilot de **Back Market**, marketplace de electrónica reacondicionada, comparado con su competencia en el sector *Electronics & Technology*.

## Objetivo

A partir de las reseñas de Trustpilot, responder mediante técnicas de NLP:

1. ¿La mayoría de las reseñas son positivas o negativas? ¿Y en la competencia?
2. ¿Qué temas tratan estas reseñas?
3. ¿El sentimiento por tema es positivo o negativo? ¿En qué somos mejores o peores que la competencia?
4. ¿Qué áreas de mejora se pueden identificar para el equipo de Customer Experience?

## Dataset

`trustpilot-reviews-123k.csv` — 123.181 reseñas de 1.680 empresas de 22 sectores. Cada reseña incluye `category`, `company`, `description`, `title`, `review` y `stars`.

> **Nota:** las 558 empresas con 100 reseñas tienen exactamente 20 reseñas de cada nota (1 a 5) — es un diseño artificial del dataset. Por eso el análisis de sentimiento se basa íntegramente en el texto, no en `stars`.

## Empresa elegida

**Back Market**, categoría *Electronics & Technology* (69 empresas en el sector, 35 de ellas con 100 reseñas como Back Market).

## Metodología

- **Limpieza de texto** en dos versiones: ligera (para sentimiento, conserva estructura de frase) y agresiva (para topics, minúsculas/sin stopwords/sin puntuación).
- **Sentimiento**: [VADER](https://github.com/cjhutto/vaderSentiment) (modelo léxico, sin necesidad de descargar pesos de un modelo externo).
- **Topics**: TF-IDF + NMF (8 topics), ajustado sobre las reseñas de Back Market y proyectado sobre la competencia con el mismo vectorizador/modelo para mantener los topics alineados.
- **Comparación con competencia**: por volumen de reseñas, por sentimiento global, por sentimiento dentro de cada topic, y por ranking de % positivo entre las empresas del sector.

## Resultados principales

| Métrica | Back Market | Competencia (sector) |
|---|---|---|
| % reseñas positivas | 54.0% | 60.2% |
| % reseñas negativas | 44.0% | 33.7% |
| Posición en el ranking del sector | 54 / 69 | — |

Mayores brechas negativas frente a la competencia: **atención al cliente y seguimiento de envíos** (25% positivo vs 57%) y **garantía y reparaciones** (30% vs 48%). Fortalezas relativas: **calidad del producto** (68.8% vs 55.6%) y **duración de batería** (60.0% vs 57.3%).

## Estructura del repositorio

```
.
├── analisis_backmarket.ipynb   # Notebook con el análisis completo (código + resultados)
├── trustpilot-reviews-123k.csv # Dataset (opcional, ver nota de tamaño abajo)
└── README.md
```

## Cómo ejecutar

```bash
pip install pandas numpy scikit-learn matplotlib seaborn wordcloud vaderSentiment jupyter
jupyter notebook analisis_backmarket.ipynb
```

El CSV original pesa ~124 MB; si tu repositorio tiene límite de tamaño, puedes omitirlo del repo y dejar solo el notebook ya ejecutado (los resultados y gráficos quedan guardados como salida de las celdas).

## Limitaciones

- Muestra pequeña por empresa (100 reseñas), y algunos topics tienen apenas 10-12 reseñas dentro de ese total, lo que limita la robustez estadística de algunas comparaciones.
- VADER es un modelo léxico: pierde matices en reseñas mixtas (p. ej. "buen producto, mal envío").
- El modelo de topics se ajustó sobre un corpus pequeño (100 documentos), lo que puede afectar a su estabilidad ante cambios de semilla o número de topics.

## Curso

Caso práctico de Deep Learning — análisis NLP de reseñas de Trustpilot.
