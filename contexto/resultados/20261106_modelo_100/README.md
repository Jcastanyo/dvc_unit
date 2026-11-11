# Resultados modelo_100: UniT `small` con data augmentation de color

- **Config:** `config/20261106_modelo_100.yaml` (dvc_unit, commit `36b96e7`).
- **Código:** rama `feature_color_augmentation` del repo UniT (commit `1ef9046`), basada en `feat/update_code_paper_iee_sensors`.
- **Entrenamiento:** 35 épocas, 8-nov a 10-nov de 2026. Checkpoint final: `checkpoint-epoch=34.ckpt`.
- **Entrada aumentada y objetivo limpio:** brillo, contraste, saturación ±0,3 y tono ±0,1 (BGR).

## Evaluación

Protocolo del paper: 11.503 imágenes de test, SSIM por imagen y FID (torch-fidelity). Test aumentado: `ColorJitter` con magnitudes U(0,1; 0,3), misma semilla para todos los modelos.

| Modelo | Parámetros | SSIM limpio | FID limpio | SSIM aumentado | FID aumentado |
|---|---|---|---|---|---|
| **Nuevo `small` con augmentation** | 25,95 M | **0,9329 ± 0,0349** | 17,63 | **0,9309 ± 0,0353** | **17,46** |
| Base `small` (paper) | 25,95 M | 0,9288 ± 0,0390 | 17,66 | 0,9032 ± 0,0469 | 27,51 |
| Base `medium` (paper) | 35,66 M | 0,9337 ± 0,0358 | 18,11 | 0,9162 ± 0,0406 | 26,92 |

Los modelos base reproducen las cifras del paper (small: 0,9289 / 17,80; medium: 0,9338 / 18,25).

## Imágenes de ejemplo

- `ejemplos_aumentado.png`: columnas = entrada aumentada | nuevo | base `small` | base `medium`.
- `ejemplos_limpio.png`: mismas columnas con entrada limpia.
- `originales.png`: las mismas 6 imágenes originales.
- `results.json`: todas las cifras.

## Cautelas

- Un solo entrenamiento y una sola semilla de augmentation.
- El test aumentado usa la misma familia de transformaciones que el entrenamiento, así que no es independiente.
- No se ha probado en otros sensores reales.
- La pérdida del generador (`aeloss`) se vuelve muy negativa desde la época 5 y la `discloss` se queda en 1: el entrenamiento GAN es anómalo, aunque las métricas son buenas.
