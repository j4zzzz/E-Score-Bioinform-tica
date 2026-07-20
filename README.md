# Análisis y Replicación: Puntuación de Alineamientos mediante Similitud de Vectores de Incrustación (E-Score)

**Autor:** Jazmín Pérez Villasante  
**Institución:** Universidad Nacional de San Agustín de Arequipa (UNSA) - Escuela Profesional de Ciencia de la Computación  

## Descripción del Proyecto

Este repositorio contiene la implementación empírica, replicación y análisis comparativo del artículo científico *"Scoring alignments by embedding vector similarity"*. El objetivo principal es evaluar la precisión de los alineamientos de secuencias biológicas (proteínas) contrastando algoritmos clásicos basados en matrices de sustitución estáticas frente a enfoques modernos de Inteligencia Artificial que utilizan modelos de lenguaje grandes (LLMs) y representaciones vectoriales contextuales.

El marco de trabajo compara el algoritmo clásico de Needleman-Wunsch utilizando la matriz BLOSUM62, contra la métrica de puntuación E-score, la cual se basa en la similitud del coseno de *embeddings* generados por redes neuronales *Transformer*.

## Referencias Principales

*   **Artículo Original:** [Scoring alignments by embedding vector similarity](https://pmc.ncbi.nlm.nih.gov/articles/PMC11063651/)
*   **Repositorio Original:** [lucian-ilie/E-score](https://github.com/lucian-ilie/E-score)
*   **Servidor Web de los Autores:** [http://e-score.csd.uwo.ca](http://e-score.csd.uwo.ca)

## Motores de Embedding (Transformers)

El cálculo del E-score depende de la extracción de características mediante modelos de lenguaje pre-entrenados con secuencias de proteínas. En este proyecto se prioriza el uso de **Ankh** por su balance entre eficiencia computacional y precisión, sin embargo, la metodología es compatible con los siguientes motores:

*   **Ankh (Modelo por defecto):** [ElnaggarLab/ankh-base](https://huggingface.co/ElnaggarLab/ankh-base/tree/main)
*   **ProtT5-XL (Half-precision):** [Rostlab/prot_t5_xl_half_uniref50-enc](https://huggingface.co/Rostlab/prot_t5_xl_half_uniref50-enc)
*   **ProtT5-XL (ONNX - versión ligera):** [Rostlab/prot-t5-xl-uniref50-enc-onnx](https://huggingface.co/Rostlab/prot-t5-xl-uniref50-enc-onnx)
*   **ProtBert:** [Rostlab/prot_bert](https://huggingface.co/Rostlab/prot_bert)
*   **ProtAlbert:** [Rostlab/prot_albert](https://huggingface.co/Rostlab/prot_albert/tree/main)
*   **ESM2 (Modelo ligero de 8M):** [facebook/esm2_t6_8M_UR50D](https://huggingface.co/facebook/esm2_t6_8M_UR50D)

## Conjunto de Datos (Datasets)

Los datos utilizados para la validación y el benchmark consisten en pares de secuencias de proteínas extraídas de la base de datos CDD (Conserved Domain Database) con alineamientos curados manualmente que sirven como *ground truth*. 

Los subconjuntos de prueba se dividen en:
*   [Alineamientos Globales (Ejemplos)](https://github.com/lucian-ilie/E-score/tree/main/example_data_global)
*   [Alineamientos Locales (Ejemplos)](https://github.com/lucian-ilie/E-score/tree/main/example_data_local)

## Metodología y Pipeline

La ejecución del código en este repositorio sigue una ruta dual para permitir la comparación directa:

1.  **Enfoque Clásico (Baseline):** 
    *   Implementación del algoritmo Needleman-Wunsch.
    *   Uso de la matriz de puntuación BLOSUM62 (obtenida mediante la librería `Biopython`).
    *   Aplicación de penalidad de hueco afín estándar.
2.  **Enfoque con Inteligencia Artificial (E-score):**
    *   Despliegue del LLM (Ankh).
    *   Tokenización y proyección de las secuencias al espacio latente.
    *   Generación de las matrices de puntuación dinámicas mediante similitud coseno utilizando los scripts nativos del repositorio E-score.
3.  **Evaluación de Métricas:**
    *   El rendimiento de ambas aproximaciones se evalúa calculando la distancia matemática entre los alineamientos generados y los alineamientos de referencia. Se emplean las métricas de comparación espacial provistas en los scripts originales de los autores.

## Requisitos y Dependencias

Para reproducir los experimentos, se requiere un entorno de Python 3.8 o superior con las siguientes librerías:

*   `biopython` (Para la matriz BLOSUM62)
*   `torch` (PyTorch para la inferencia de modelos tensoriales)
*   `transformers` (Para la carga del modelo Ankh desde HuggingFace)
*   `pandas` y `numpy` (Manipulación de datasets)
*   `matplotlib` (Visualización de resultados comparativos)

## Ejecución del Proyecto

1. Clone este repositorio:
   ```bash
   git clone [https://github.com/usuario/nombre-del-repo.git](https://github.com/usuario/nombre-del-repo.git)
   cd nombre-del-repo