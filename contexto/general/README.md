# dvc_unit

Contexto general del proyecto.

## Artículo de referencia

**Zero-Shot Transfer of Force Map Estimation Across GelSight Mini Sensors**
Julio Castaño-Amoros y Pablo Gil (Universidad de Alicante). IEEE Sensors Letters, vol. 10, no. 10, octubre 2026.
Archivo: `contexto/papers/Zero-Shot_Transfer_of_Force_Map_Estimation_Across_GelSight_Mini_Sensors.pdf`

### Problema
Los sensores táctiles basados en visión (VBTS) como el GelSight Mini se fabrican de forma casi artesanal, por lo que cada unidad presenta variaciones (color de fondo, grosor y rigidez del gel, etc.). Esto obliga a recopilar datos y reentrenar modelos para cada sensor nuevo.

### Método
Dos etapas que permiten generalizar la estimación de mapas de fuerza 3D entre unidades distintas, aunque solo se haya entrenado con imágenes de un único sensor:

1. **Adaptación de dominio (UniT):** una arquitectura tipo VQGAN (VQVAE + discriminador) reconstruye la imagen táctil de cualquier sensor como una imagen táctil general I_r. Su espacio latente discreto (codebook) evita "alucinar" características no vistas. A diferencia del UniT original, trabaja con imágenes sin marcadores, usa unas 115 k imágenes de 55 objetos y reconstruye con más detalle.
2. **Estimación de fuerza (U-Net):** recibe la imagen diferencia I_d (reconstrucción menos fondo) y produce tres mapas de fuerza (Fx, Fy, Fz) de 32×24. La fuerza total se obtiene sumando los mapas y calculando la magnitud.

### Datos
- **Reconstrucción:** 115.042 imágenes de 55 objetos con un solo GelSight Mini.
- **Fuerza:** dataset de Helmut et al. (18.679 muestras, 25 configuraciones de sensor, 24 indentadores impresos en 3D). Se eliminaron los marcadores con inpainting (TELEA) y reconstrucción, quedando 18.175 imágenes sin marcadores.

### Resultados
- **Reconstrucción:** el modelo `medium` (35,66 M parámetros) obtiene el mejor equilibrio, con SSIM 0,9338 ± 0,0358 y FID 18,25. Supera a MAE y pix2pix, sobre todo en el test aumentado (cambios de brillo, contraste, saturación y tono).
- **Fuerza:** con la imagen I_d se obtiene un MAE_F de 1,1294 ± 1,5934 N, comparable al de imágenes con marcadores (0,7250 N).
- **Rendimiento:** inferencia de ≈19,8 ms (RTX 3060), suficiente para tiempo real, ya que el GelSight Mini funciona a 25 Hz.

### Limitaciones y trabajo futuro
Dificultad con fuerzas normales muy bajas (≈0 N) o muy altas (≈40 N), con cizalla superior a 5 N y para preservar la geometría del contacto. Como mejora, se propone añadir una función de pérdida que ayude a la U-Net a conservar la geometría del contacto.

## Posibles líneas de trabajo futuras

### 1. Entrenar la reconstrucción (UniT) con data augmentation de color

Repetir la fase de reconstrucción del paper (modelo `medium`, mismos hiperparámetros) añadiendo data augmentation durante el entrenamiento, para mejorar la generalización a otros sensores.

- **Transformaciones:** brillo, contraste, saturación y tono (referencia: rango [0.1, 0.3] del paper), y posiblemente ruido gaussiano. Evitar transformaciones geométricas, que pueden romper la relación entre la imagen y el mapa de fuerza.
- **Intensidad:** probar dos o tres niveles, porque una augmentation excesiva puede bajar el SSIM en el test original.
- **Evaluación:**
  - SSIM y FID en el test original y en el aumentado, frente al `medium` base.
  - Reconstruir las 25 configuraciones de sensor del dataset de fuerza (Helmut et al.) y medir el MAE_F de la U-Net ya entrenada, sin reentrenarla.
  - Reconstrucción en datasets públicos de otros sensores (SSIM y FID entre la imagen original y la reconstruida, más inspección cualitativa), ordenados por parecido con el GelSight Mini. Hay que preprocesar a la resolución de entrada y eliminar marcadores si los hay. Verificar disponibilidad y licencia antes de descargar.
- **Ablación:** solo color frente a color más ruido.

### 2. Mejorar la forma del mapa de fuerzas con una pérdida que preserve la geometría del contacto

La U-Net tiene dificultades para preservar la geometría del contacto, con fuerzas normales muy bajas (≈0 N) o muy altas (≈40 N) y con cizalla superior a 5 N. Es la mejora que proponen los propios autores del paper.

- **Pérdida:** MSE más un término de forma, como Dice o IoU sobre la máscara de contacto, o una pérdida de gradiente sobre el mapa Fz.
- **Ponderación:** penalizar más los errores en los rangos extremos de fuerza.
- **Evaluación:** MAE_F, errores por eje y fidelidad de la huella, frente al modelo base.
