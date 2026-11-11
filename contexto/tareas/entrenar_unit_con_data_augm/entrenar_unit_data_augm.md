# TAREA ENTRENAR UNIT APLICANDO DATA AUGMENTATION

En el paper entrené UniT para reconstruir las imágenes táctiles del sensor GelSight y vi que tenía buenas prestaciones para reconstruir imágenes con diferentes colores y de otros sensores. 


En esta tarea quiero probar si entrenando UniT aplicando data augmentation, puedo incluso mejorar estos resultados, ya no solo para generalizar entre sensores táctiles de tipo GelSight, sino también para intentar generalizar la reconstrucción a otros sensores como los Digit.

Para ejecutar el entrenamiento en el servidor, necesitas conectarte con ssh. Accede a la carpeta Julio/REMAIN/ y ahí encontrarás las carpetas forked_UniT y dvc_unit, que son los repos con los que vas a trabajar.

1 - Debes ejecutar los siguientes comandos para lanzar el contenedor de docker:

- docker run --rm -it --gpus all  --volume ./forked_UniT:/UniT_shared  --name unit unit_vae
- conda activate unitenv
- muevete a la carpeta UniT_shared/third_party/diffusion_policies y ejecuta pip install -e .
- muevete para atrás a la carpeta UniT_shared y ejecuta pip install -e .

2 - En este punto ya podría lanzar un entrenamiento ejecutando python train.py y pasando dos argumentos: para --config-dir hay que pasarle la ruta a donde está la config en dvc_unit, y para --config-name hay que pasarle el nombre de la config