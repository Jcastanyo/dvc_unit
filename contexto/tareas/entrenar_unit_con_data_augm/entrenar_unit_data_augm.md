# TAREA ENTRENAR UNIT APLICANDO DATA AUGMENTATION

En el paper entrené UniT para reconstruir las imágenes táctiles del sensor GelSight y vi que tenía buenas prestaciones para reconstruir imágenes con diferentes colores y de otros sensores. 


En esta tarea quiero probar si entrenando UniT aplicando data augmentation, puedo incluso mejorar estos resultados, ya no solo para generalizar entre sensores táctiles de tipo GelSight, sino también para intentar generalizar la reconstrucción a otros sensores como los Digit.

La lista de tareas a realizar serían las siguientes:

1. Tienes que crear un fichero de configuración donde se añada el data augmentation. Quiero que crees este fichero en la carpeta config del repo dvc_unit. El nombre debe seguir el siguiente formato: fecha_modelo_X donde X empieza en 100, aumenta de 10 en 10 si el cambio de hacemos en ese experimento es de hiperparámetros o de 100 en 100 si el cambio es de datos o modelos. 

Para crear este archivo de configuración debes seguir 