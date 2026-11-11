# TAREA RECONSTRUIR IMÁGENES DE DIGIT AL DOMINIO DEL GELSIGHT MINI

El objetivo del trabajo es que la fase de reconstrucción (UniT) lleve las imágenes táctiles de distintos sensores a una imagen táctil general. Con el modelo entrenado con data augmentation de color (`20261106_modelo_100`) ya se ha visto que generaliza entre variaciones de color del GelSight Mini. Esta tarea comprueba si también sirve con otro sensor, el DIGIT.

## Datos

Dataset de DIGIT en local: `data/digit/force_digit_dataset/` (no se sube a GitHub, `data/` está en el `.gitignore`).

- Tres particiones: `train` (10.246 imágenes), `dev` (1.427) y `test` (1.458).
- Cada una tiene tres subcarpetas: `images` (imagen del sensor), `forces` (fuerza en un `.txt`) y `filt_img_depth` (profundidad).
- Las imágenes son PNG de 240x320 en vertical, con iluminación RGB lateral muy marcada (zonas rojas, verdes y azules), muy distinta a la del GelSight Mini.

## Plan

1. Preprocesar las imágenes de `images`: rotarlas a horizontal (320x240) y redimensionarlas a 160x128, como las imágenes de entrenamiento.
2. Pasarlas por el modelo entrenado (`checkpoint-epoch=34.ckpt` de `20261106_modelo_100`) y compararlas con el modelo `small` base del paper.
3. Revisar visualmente si las reconstrucciones se parecen a una imagen táctil del dominio del GelSight Mini y si conservan el contacto.
4. Si el resultado es razonable, valorar más adelante la estimación de fuerza con las fuerzas del dataset.

## Prueba inicial

Se ha hecho con unas pocas imágenes de `test/images`. Los resultados están en `resultados/digit_prueba_inicial/`.
