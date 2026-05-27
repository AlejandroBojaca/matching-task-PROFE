# matching-task-PROFE

Solución para la subtarea **Matching** de **PROFE 2026** (evaluación de competencia
lingüística en español), dentro de **IberLEF 2026**.

Equipo: **David_** · Contacto: davidbojaca1@gmail.com

## Resumen

Cada ejercicio de *matching* tiene dos conjuntos de textos: hay que asignar a cada
pregunta del **set 2** el texto del **set 1** que mejor le corresponde (el set 1 puede
incluir opciones de más que no se emparejan con nada).

Lo resolvemos como un problema **zero-shot de salida estructurada**: un único modelo
open-weights cuantizado lee el ejercicio y devuelve un JSON que asocia cada pregunta a
una opción. **No hay entrenamiento ni fine-tuning**, solo inferencia.

- **Modelo principal (sistema enviado):** `Qwen2.5-7B-Instruct` en 4 bits NF4
  (bitsandbytes), decodificación *greedy* (N=1). Corre en una sola GPU **T4 de 16 GB**
  (Colab free).
- **Resultado:** **0.6667** de accuracy en el test oculto (+15.6 puntos sobre el
  baseline de ChatGPT, 0.51).
- **Variante multimodal (ablación):** `Qwen2.5-VL-7B-Instruct`. Pasar las imágenes
  **empeoró** el resultado, porque la mayoría son decorativas y no aportan información.
  Conclusión: la información visual hay que filtrarla, no usarla indiscriminadamente.

## Contenido

- `profe2026_matching.ipynb` — notebook de Colab con todo el pipeline (instalación,
  carga del test set, prompt + parsers robustos, modelo solo-texto, detección de
  salidas degeneradas, pase multimodal selectivo y construcción del envío).
- `PROFE26_test_set/` — conjunto de test oficial.
- `example*.json` — ejercicios de ejemplo con gold público.
- `predictions_matching.json` — predicciones del sistema solo-texto (el envío con 0.6667).

## Cómo ejecutarlo

1. Abrir `profe2026_matching.ipynb` en Google Colab (runtime con GPU T4).
2. Ejecutar las celdas en orden. La celda 1 instala las dependencias
   (`transformers`, `accelerate`, `bitsandbytes`, `sentencepiece`, `qwen-vl-utils`).
3. Las predicciones se escriben de forma incremental (un ejercicio a la vez), por lo que
   una ejecución larga puede interrumpirse y reanudarse sin perder trabajo.
4. La salida es un JSON que mapea cada `exerciseID` a un diccionario `{pregunta: opción}`.

## Pipeline del notebook (celdas)

1. Instalación de dependencias.
2. Montar Drive y definir rutas.
3. Carga del test set de *matching*.
4. Construcción del prompt en español + parsers robustos (JSON validado → regex →
   fallback por defecto).
5. Carga del modelo solo-texto `Qwen2.5-7B-Instruct` (4 bits).
6. Detección automática de ejercicios *degenerados* (todas las preguntas con la misma
   letra) para reprocesarlos.
7. Reejecución de los ejercicios pendientes.
8. Liberar el modelo de texto y cargar `Qwen2.5-VL-7B` (multimodal).
9. Pase multimodal **selectivo**: solo en ejercicios cuyo set 1 contiene imágenes.
10. Comprobación final de integridad.
11. Empaquetado del envío (`.zip`).
