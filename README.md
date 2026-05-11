# Laboratorio 9 — Redes Neuronales Artificiales

**CC3074 — Minería de Datos** · UVG · Semestre I 2026

**Integrantes:** Jose Sanchez, Roberto Najera, Andre Pivaral

---

## Contexto

Séptima entrega de la consultoría para **SmartStay Advisors**, firma que
intermedia propiedades de Airbnb. Se aplicaron **Redes Neuronales Artificiales**
al conjunto `listings.RData` para clasificar propiedades en categorías de
precio (Barata / Media / Cara) y predecir su precio numérico, comparando los
resultados contra los algoritmos de entregas anteriores.

## Archivos

- `markdown lab9.Rmd` — Código y análisis completo.
- `markdown-lab9.html` — Reporte renderizado.

---

## Conclusiones del reporte

### Clasificación del precio (Barata / Media / Cara)

- Se entrenaron **dos topologías de RNA** distintas (sigmoide y tanh) y luego
  se tuneó la mejor con validación cruzada de 5 folds.
- La RNA tuneada quedó en una posición **competitiva pero no dominante** dentro
  del ranking de los 7 algoritmos.
- **Random Forest** y **SVM con kernel radial** resultaron los más exactos para
  esta tarea, capturando bien las no linealidades entre variables como
  `accommodates`, `bedrooms` y `room_type`.
- **Naive Bayes** quedó al final del ranking porque su supuesto de
  independencia entre predictores se viola claramente en estos datos.

### Predicción del precio numérico

- También se entrenaron **dos RNA de regresión** (`nnet` y `neuralnet`) con
  topologías y activaciones distintas, seguidas de tuning del mejor modelo.
- En regresión las diferencias entre algoritmos son **más marcadas** que en
  clasificación, ya que el RMSE no está acotado.
- **Random Forest** y **SVR** lideraron en RMSE y R², con la RNA tuneada
  quedando cerca pero sin superarlos.
- La regresión lineal funcionó como baseline interpretable; al ser superada
  por los modelos no lineales se confirma que el precio depende de relaciones
  complejas entre los predictores.

### Sobre las Redes Neuronales en este conjunto

Las RNA demostraron ser una **alternativa viable pero no óptima** para este
problema. Con ~2 000 observaciones y ~12 predictores numéricos, los
**ensembles de árboles** (Random Forest) y los **modelos de kernel** (SVM/SVR)
ofrecen mejor desempeño con menos esfuerzo de tuning y normalización. El
valor real de las RNA aparecería al incorporar datos textuales (descripciones,
reseñas) o de imagen (fotos de los listings).

### Sobre el sobreajuste

El sobreajuste se controló mediante:

- **Regularización** (`decay` en `nnet`).
- **Validación cruzada** de 5 folds en el tuning.
- **Diagnóstico train-test** aplicado a los 7 algoritmos para detectar brechas
  problemáticas.

Los modelos finales presentan brechas entrenamiento-prueba en rangos
aceptables, lo que sugiere generalización adecuada a listings nuevos.

---

## Recomendaciones de negocio

1. Usar el mejor modelo de **clasificación** para etiquetar nuevos listings y
   filtrarlos contra el presupuesto del cliente corporativo.
2. Usar el mejor modelo de **regresión** para sugerir precios competitivos a
   propiedades con baja ocupación.
3. **Re-entrenar trimestralmente** para mantener vigencia ante cambios
   estacionales del mercado.
