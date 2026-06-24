# Guía de Laboratorio — PS1: Regresión Simple
**Econometría LECO | Dr. Francisco Cabrera**
**Duración:** 1 hora 30 minutos
**Asistente de profesor:** _______________________

---

> **Tu rol en este laboratorio**
> No eres un solucionario. Eres un interlocutor.
> Tu trabajo es hacer que el estudiante piense en voz alta.
> Si un estudiante te pregunta "¿cómo se hace?", responde con una pregunta.
> Si un estudiante dice "no entiendo", responde con "¿qué parte sí entiendes?"
> Solo intervén con una pista directa si el grupo completo está atascado más de 5 minutos.

---

## ⏱ Acto 1 — Arranque (0:00 – 0:20)

### Objetivo
Que todos tengan el repositorio abierto, el `.Rmd` cargado en RStudio, y entiendan la estructura del archivo antes de escribir una sola línea de código.

### Pasos concretos

**0:00 — Bienvenida (2 min)**
- Pide que abran RStudio y GitHub Classroom.
- Pregunta al grupo: *"¿Alguien puede decirme qué es un archivo `.Rmd` en una oración?"*
- No corrijas todavía. Escucha 2-3 respuestas y di: *"Vamos a verlo en acción."*

**0:02 — Clone del repositorio (5 min)**
Proyecta en pantalla mientras haces esto con ellos:
```
1. Abre GitHub Classroom → acepta la tarea PS1
2. Copia la URL del repositorio
3. En RStudio: File → New Project → Version Control → Git → pega la URL
4. Abre ps1_estudiante.Rmd
```
⚠️ **Error frecuente #1:** Estudiantes que abren el `.Rmd` sin crear el proyecto primero.
Pista a dar: *"¿Dónde está guardado ese archivo en tu computadora?"*

**0:07 — Recorrido del archivo (8 min)**
Haz un `Knit → PDF` del archivo **sin modificar nada**.
Pregunta al grupo:
- *"¿Qué ven en el PDF? ¿Qué falta?"*
- *"¿Para qué creen que sirve el bloque de `setup` al inicio?"*
- *"¿Qué significa `eval = TRUE` vs `echo = TRUE`?"* — No respondas tú. Deja que busquen en la ayuda de R (`?knitr::opts_chunk`).

**0:15 — Formación de parejas (5 min)**
- Asigna parejas (no dejes que ellos elijan, mezcla niveles si los hay).
- Recuérdales el **Protocolo de IA** que aparece en el documento.
- Di en voz alta: *"Si en algún momento quieren usar Claude o ChatGPT, primero llenan los tres puntos del protocolo como comentario en su código. Si no pueden llenarlo, aún no están listos para preguntarle a la IA."*

---

## ⏱ Acto 2 — Trabajo colaborativo (0:20 – 1:10)

### Estructura general
Circula constantemente. Cada ~10 minutos haz una **pausa de anclaje** (ver abajo) donde preguntas algo al grupo completo para sincronizar el avance.

---

### Bloque P1 — Derivación OLS (0:20 – 0:30)

**Qué deben hacer:** Derivar el estimador OLS algebraicamente en LaTeX dentro del `.Rmd`.

**Preguntas socráticas si están atascados:**

| Si el estudiante dice... | Tú respondes... |
|--------------------------|-----------------|
| "No sé por dónde empezar" | "¿Cuál es la función que estamos minimizando? Escríbela primero sin derivar nada." |
| "¿Derivo respecto a qué?" | "¿Cuántos parámetros desconocidos tienes en esa función?" |
| "Me da cero pero no sé qué hacer con eso" | "Muy bien. Ahora tienes dos ecuaciones y dos incógnitas. ¿Qué método conoces para resolver eso?" |
| "¿Cómo escribo fracciones en LaTeX?" | "Escribe `help LaTeX fraction` en Google. Tienes 2 minutos." |

**Error conceptual frecuente:** Confundir $\sum(y_i - \bar{y})^2$ (SST) con la función de pérdida a minimizar.
Pista: *"¿Estamos minimizando la variación total de Y, o los errores de nuestra recta?"*

**✅ Señal de que van bien:** Escriben las dos condiciones de primer orden antes de despejar.

---

### Bloque P2 — Datos simulados (0:30 – 0:55)

**⏸ Pausa de anclaje a las 0:30:**
Pregunta al grupo: *"Antes de correr cualquier código, díganme: si $\beta_1 = 7$ en la ecuación poblacional, ¿qué esperan obtener cuando estimen la regresión? ¿Exactamente 7?"*
Escucha respuestas. El objetivo es que digan "no exactamente, porque hay error muestral."

---

**P2a — Generación de datos**

⚠️ **Error frecuente #2:** Olvidar `set.seed(123)` o ponerlo después de generar las variables.
Pista: *"¿Para qué sirve `set.seed`? Quita la línea, corre el código dos veces y dime qué cambia."*

⚠️ **Error frecuente #3:** Escribir la ecuación de Y sin los paréntesis correctos.
```r
# MAL — R interpreta esto diferente a lo que quieren
y <- 15 + 7*x + error

# BIEN
y <- 15 + (7 * x) + error
```
No les digas cuál es el error. Diles: *"Imprime las primeras 6 observaciones de Y con `head(y)`. ¿Los valores tienen sentido dado que X es ~N(0,1)?"*

---

**P2b — Scatterplot**

Si no saben cómo hacer el plot:
Pista: *"Escribe `?plot` en la consola. ¿Qué argumentos acepta como mínimo?"*

Si el plot se ve raro (sin títulos, ejes sin nombre):
*"Imagina que le mandas este gráfico al Dr. Cabrera sin contexto. ¿Entendería qué muestra?"*

---

**P2c — Estimación OLS**

**⏸ Pausa de anclaje a las 0:40:**
Cuando la mayoría tenga resultados de `summary(reg)`, para al grupo y pregunta:
*"Alguien que me diga: ¿qué número en este output es $\hat{\beta}_1$? ¿Y cómo se llama esa columna?"*
Luego: *"¿El valor que obtuvieron está cerca del 7 verdadero? ¿Por qué no es exactamente 7?"*

---

**P2e — Valor predicho en $\bar{x}$**

Este es un momento conceptual importante. Si calculan el valor pero no entienden por qué:
*"¿Qué propiedad garantiza que la recta de regresión siempre pasa por $(\bar{x}, \bar{y})$? ¿La vieron en clase?"*
Si no recuerdan: *"Vuelve a la derivación de P1. ¿Qué obtienes cuando evalúas la primera condición de primer orden?"*

---

**P2f — $R^2$ manual**

⚠️ **Error frecuente #4:** Confundir SSE (suma de residuales al cuadrado) con SSR (suma de regresión).

Dibuja esto en la pizarra si hay confusión generalizada:

```
SST  =  SSR  +  SSE
(total)  (explicada)  (residual)

R² = SSR/SST = 1 - SSE/SST
```

Pista si están perdidos: *"¿Cuál es la diferencia entre `fitted(reg)` y `residuals(reg)`? Imprime ambos con `head()`."*

**Verificación incorporada:** El código ya compara el $R^2$ manual con el de `lm()`. Si sale `FALSE`:
*"¿Estás usando los mismos datos en ambos cálculos? ¿Regeneraste x o y en algún bloque intermedio?"*

---

### Bloque P3 — Explorando $R^2$ (0:55 – 1:05)

**⏸ Pausa de anclaje a las 0:55:**
*"Antes de cambiar SD a 15, hagan una predicción: ¿$R^2$ va a subir, bajar, o quedarse igual? ¿Por qué?"*
Pide que lo escriban en su respuesta antes de correr el código.

**El objetivo conceptual de P3c** es que entiendan que $R^2$ bajo ≠ estimador sesgado.
Si un estudiante dice "la segunda regresión es peor porque tiene $R^2$ más bajo":
*"¿Peor en qué sentido? ¿Peor para estimar $\hat{\beta}_1$, o peor para predecir Y? ¿Son lo mismo?"*
*"¿Cambiar la varianza del error viola algún supuesto de Gauss-Markov?"*

---

### Bloque P4 — Wooldridge (1:05 – 1:15)

**⚠️ Error frecuente #5:** No tener instalado el paquete `wooldridge`.
Solución rápida: `install.packages("wooldridge")` — diles que esto va **fuera** del `.Rmd`, en la consola.

**P4d y P4e son los más difíciles.** Si el tiempo aprieta, prioriza que entiendan P4d conceptualmente aunque no completen P4e formalmente.

Pregunta para P4d: *"Si quisieras estimar el efecto causal de la educación en salarios, ¿podrías hacer un experimento aleatorio? ¿Qué problema tiene no poder hacerlo?"*

Para P4e, si están perdidos con la álgebra:
*"Parte de sustituir $y_i$ en la fórmula de $\hat{\beta}_1$. ¿Puedes separar los términos con $\beta_1$ de los términos con $\mu_i$?"*

---

### Bloque P5 — Basureros (uso libre, si hay tiempo)

Esta pregunta es intuitiva, no requiere código. Si hay parejas que avanzan rápido, mándalas aquí.

Pregunta motivadora: *"¿El municipio pone el basurero en una colonia al azar? ¿O hay patrones en dónde lo ponen?"*

---

## ⏱ Acto 3 — Cierre y commit (1:10 – 1:30)

### Objetivo
Que todos hagan al menos un commit funcional y entiendan qué están guardando.

### Pasos (proyecta en pantalla)

**1:10 — Knit final (5 min)**
- Pide que hagan `Knit → PDF` con todo lo que llevan.
- Si hay errores de compilación, este es el momento de resolverlos.

⚠️ **Error frecuente #6:** Bloques con `# TU CÓDIGO AQUÍ` sin completar — R los evalúa como comentarios y falla si están en medio de una asignación.
Solución: Buscar con `Ctrl+F` → "TU CÓDIGO" para encontrar bloques incompletos.

⚠️ **Error frecuente #7:** LaTeX mal formado en las respuestas escritas.
Pista: El error de compilación siempre dice el número de línea. *"Lee el mensaje de error. ¿En qué línea ocurre?"*

**1:15 — Primer commit (10 min)**
Guíalos paso a paso:
```
En RStudio:
1. Panel "Git" (esquina superior derecha) → aparece lista de archivos modificados
2. Checkbox en ps1_estudiante.Rmd y ps1_estudiante.pdf
3. Botón "Commit"
4. Mensaje de commit: "PS1 avance lab semana X - [sus nombres]"
5. Botón "Push"
```

Después del push, abre el repositorio en GitHub y muéstralo en pantalla.
Di: *"Lo que acaban de hacer es exactamente lo que hacen los equipos de datos profesionalmente. Cada cambio queda registrado con fecha, hora y autor."*

**1:25 — Reflexión de cierre (5 min)**
Pregunta al grupo (no como evaluación, como conversación):
1. *"¿Qué fue lo más difícil de hoy?"*
2. *"¿Alguien usó la IA? ¿Les fue útil o no? ¿Por qué?"*
3. *"¿Qué parte de P1 (la derivación) les ayudó a entender mejor P2?"*

---

## 📋 Errores frecuentes — Referencia rápida

| # | Error | Síntoma visible | Pista a dar (nunca la solución) |
|---|-------|-----------------|--------------------------------|
| 1 | `.Rmd` abierto sin proyecto Git | No aparece panel Git | "¿Dónde creaste el proyecto?" |
| 2 | `set.seed` fuera de lugar | Resultados no reproducibles | "Quita la línea y corre dos veces" |
| 3 | Ecuación de Y mal escrita | Valores de Y sin sentido | "Imprime `head(y)`, ¿tiene sentido?" |
| 4 | SSE ↔ SSR confundidos | $R^2 > 1$ o negativo | Dibuja SST = SSR + SSE en pizarrón |
| 5 | Paquete no instalado | `Error: there is no package called` | `install.packages()` en consola |
| 6 | Bloque incompleto al hacer Knit | Error en línea con `<-` | `Ctrl+F` → "TU CÓDIGO" |
| 7 | LaTeX mal formado | `! LaTeX Error` al compilar | "Lee el número de línea en el error" |

---

## 📝 Notas para el Dr. Cabrera

Al finalizar el laboratorio, completa esto y súbelo como `lab_reporte_semanaX.md` al repositorio del curso:

```
Semana: ___
Asistente: ___
Parejas formadas: ___

Avance general del grupo (1-5): ___
Pregunta donde más se atascaron: ___
Concepto que requiere refuerzo en clase: ___

¿Alguien usó IA? ¿Cómo?: ___

Observaciones adicionales:
___
```

---

*Última actualización: PS1 — Regresión Simple*
*Coordinación: Dr. Francisco Cabrera — Econometría LECO*
