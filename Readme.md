### Generación de SQL desde Lenguaje Natural

#### Descripción del Proyecto

Este proyecto es una Prueba de Concepto (PoC) que explora la posibilidad de transformar consultas en lenguaje natural a sentencias SQL mediante el ajuste fino (*fine-tuning*) de modelos de lenguaje avanzados, como **T5-Large** y **Facebook/MBART-Large-50**.

El objetivo es evaluar la efectividad de estos modelos para comprender el contexto de bases de datos y generar consultas SQL precisas en función de preguntas formuladas en lenguaje natural.

#### Estructura del Proyecto

```plaintext
📂 KeepIA
│── 📂 datasets        # Dataset generado con preguntas, respuestas y contexto SQL
│   │── sql_dataset       
│   │── sql_text.json     
│
│── 📂 outputs
│   │── models/      # Checkpoints de modelos ajustados
│
│
│── main.ipynb          # Clase principal con el flujo de la PoC
│── data.ipynb          # Preprocesamiento de datos y generación del dataset
│── README.md           # Documentación del proyecto
│── requirements.txt    # Librerías necesarias 
```
### Detalles de los Archivos Principales

####  `data.ipynb` - Preprocesamiento de Datos

Este archivo contiene el proceso de preparación de datos para generar un dataset propio con la siguiente estructura:

```json
{
 'answer': "SELECT AVG(tiempoRespuesta_min) FROM rpt_actual_casos WHERE Estado = 'En Proceso'",
 'question': "¿Cuál es el promedio de tiempo de respuesta en minutos para los casos en estado 'En Proceso'?",
 'context': "CREATE TABLE rpt_actual_casos.."
}
```
###  `main.ipynb` - Flujo de la Prueba de Concepto

Este cuaderno contiene la implementación principal del proyecto. Aquí se realizan los siguientes pasos:

1. **Carga y preprocesamiento de datos**
   - Se utiliza `data.ipynb` para estructurar el dataset con preguntas en lenguaje natural, su respectiva consulta SQL y el esquema de la base de datos.
   
2. **Entrenamiento de los modelos**
   - Se realiza *fine-tuning* de los modelos `T5-Large` y `Facebook/MBART-Large-50` utilizando el dataset generado.
   - Se ajustan los hiperparámetros clave como `learning_rate`, `batch_size` y `num_train_epochs`.
   - Se almacena el modelo ajustado en la carpeta `models/checkpoint/`.

3. **Generación de consultas SQL**
   - Se evalúa la capacidad de los modelos para convertir preguntas en consultas SQL utilizando `generate()` de Hugging Face.
   - Se compara la `model_response` con la `ground_truth` para medir la precisión.

4. **Evaluación del desempeño**
   - Se calcula la `Training Loss` y `Validation Loss` para determinar la convergencia del modelo.
   - Se analiza la calidad de las consultas generadas y los errores más comunes.

---

###  Conclusiones y Próximos Pasos

Los primeros resultados obtenidos indican que **los modelos aún no generan consultas SQL **. Se identificaron algunas áreas de mejora:

- **Uso de datasets más especializados**  
  - Implementar **Spider**, un dataset ampliamente utilizado para entrenamiento en Text-to-SQL.
  - Explorar otros datasets que permitan diversificar los ejemplos de entrenamiento.

- **Refinamiento del preprocesamiento de datos**  
  - Asegurar que la estructura de la base de datos se incluya correctamente en el contexto.
  - Aumentar la cantidad de ejemplos con variaciones en las preguntas para mejorar la generalización.

- **Calibración de hiperparámetros**  
  - Ajustar la tasa de aprendizaje (`learning_rate`).
  - Experimentar con distintas estrategias de generación, como `beam search` o `top-k sampling`.

El archivo JSON `sql_text` se encuentra disponible en **Google Drive** debido a su tamaño.  
Puedes acceder a él a través del siguiente enlace:  

🔗 [Descargar JSON `sql_text`](https://drive.google.com/drive/folders/1qiFGprasKba-LRbovRokvgHHIyPrOiKY?usp=sharing)
