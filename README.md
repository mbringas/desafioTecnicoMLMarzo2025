# Repositorio para el desafío técnico (Marzo 2025)
Quedan a disposición:

- [Código en Google Colab](https://colab.research.google.com/drive/1_PYC2rakKr1WTeHQQVPXGJ29ESY38X4u?usp=sharing)
- [Informe](desafioTecnicoMLMarzo2025/Informe del desafío técnico - ML Marzo 2025.pdf)

También dejo respuesta a tres preguntas presentes en el enunciado
### ¿Qué pasos puedo seguir para intentar asegurar que la performance del modelo en laboratorio será similar a la de producción?
Los modelos de ML fueron entrenados en el esquema de train-validation-test, que pretende utilizar la instancia de test como una exposición del modelo a observaciones no utilizadas para el entrenamiento ni para la selección de hiperparámetros. Idealmente, las métricas logradas sobre este set de testeo deberían mantenerse constantes al incorporar nuevas observaciones.

### Suponiendo que la performance predictiva en producción es muy diferente a la esperada, ¿Cuáles cree que son las causas más probables?
Se me ocurren tres escenarios:
- Nuestro modelo se encuentra subajustado o sobreajustado. La estrategia de division en set de entrenamiento-validación-testeo intenta impedir esto, pero podría haber errores al realizarlo (por ejemplo, data leaking - por la cual la variable objetivo se filtra en los datos de entrenamiento y mejora las métricas de manera espúrea.
- Puede ser que las características o features propias de las distintas clases vayan evolucionando con el tiempo y nuestro modelo no pueda realizar la separación de clases de forma adecuada. Esto se suele llamar Data Drifting (o deriva de datos).

### ¿Qué pasos debería seguir para poner el nuevo modelo en producción?
Para poner este modelo en producción se debe elegir un esquema con el cual evaluar las transacciones que se van realizando 
- Una a una: a medida que se realizan, pasan por el modelo de machine learning y se informa el resultado. Esto es bueno para tener resultados de forma inmediata y poder actuar en consecuencia en caso de sospecha de fraude.
- En batches: se acumula una cantidad determinada de transacciones, se generan los resultados y se estiman métricas del batch. Esto es bueno para saber si la performance varia mucho respecto del momento del entrenamiento.

Luego se debe tener el modelo cargado en una máquina que reciba las features de la transacción, realice tareas de preprocesamiento análogas a las que recibieron los conjuntos de entrenamiento, validacion y testeo y luego genere una predicción. El acceso a estas predicciones puede realizarse 
- interfaseada por una API (application programming interface) donde el dispositivo que registra la transaccion envia los atributos a la máquina donde se corre el modelo y se genera la predicción 
- o teniendo el modelo (en caso de ser pequeño y no requerir demasiados recursos) en el dispositivo que registra la transacción.
