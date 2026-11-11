# Prueba inicial: DIGIT reconstruido con el modelo entrenado

Tarea: `contexto/tareas/reconstruir_digit_dominio_gelsight/`.

- **Fecha:** 11/11/2026
- **Imágenes:** 6 de `test/images` del dataset DIGIT (`11000`, `16004`, `3046`, `17790`, `28096`, `7366`, de arriba abajo).
- **Columnas:** imagen DIGIT | modelo nuevo (`20261106_modelo_100`, época 34) | `small` base del paper.
- `digit_rotada.png`: imagen girada a horizontal antes de redimensionar a 160x128.
- `digit_sin_rotar.png`: imagen redimensionada directamente (queda deformada).

## Observaciones

- Los dos modelos convierten el fondo de DIGIT, con luces rojas, verdes y azules, en un fondo verde azulado propio del GelSight Mini, y conservan la forma del contacto (círculo, estrella, hexágono).
- El modelo nuevo da un fondo algo más limpio. El base deja más manchas azules y una banda oscura a la derecha en las imágenes giradas.
- La diferencia entre los dos modelos es pequeña y es una valoración visual con solo 6 imágenes, sin métricas.
- Al no girar, el fondo queda más homogéneo, pero la huella sale deformada y estirada. Girar mantiene las proporciones, por eso es la opción elegida en la tarea.
