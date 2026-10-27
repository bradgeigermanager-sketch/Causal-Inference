Here’s a new set of causal inference scenarios, written entirely in Spanish, focused on two distinct real-world domains: *Turismo Sostenible* (Sustainable Tourism) and *Logística de Última Milla* (Last-Mile Logistics), analyzed using Pearl’s SCM and Rubin’s Potential Outcomes framework.

---

Escenario 31: Turismo Sostenible y Conservación de Arrecifes

1. Título del Escenario: Impacto de los Programas de Turismo Responsable en la Degradación de Arrecifes de Coral

2. Variables Clave:
*   Tratamiento (X): Participación en el "Programa de Turismo Responsable" (Binario: 1 = Participó, 0 = Turismo Convencional).
*   Resultado (Y): Tasa de Degradación del Arrecife (Continua: % de cobertura de coral perdida por año). Valores más bajos son mejores.
*   Factor Confundidor (Z): Densidad Anual de Visitantes (Continua: Número de turistas por km²). Las zonas con mayor afluencia turística son las que más probablemente implementan programas de turismo responsable ($X$) para mitigar daños, y estas mismas zonas ya tienen una presión ambiental intrínsecamente mayor ($Y$).

3. Mecanismo Causal y DAG ASCII:
La densidad de visitantes ($Z$) influye tanto en la adopción del programa ($X$) como en la degradación base del arrecife ($Y$). El programa de turismo responsable ($X$) reduce directamente la degradación ($Y$) al regular las actividades de buceo y anclaje.

```text
      Z (Densidad de Visitantes)
     / \
    v   v
X (Turismo Responsable) --> Y (Degradación del Arrecife)
```

4. Enfoque SCM de Pearl (Bloqueo por Grafo):

*   Paso 1: Objetivo: Estimar la reducción promedio de degradación causada por el programa: $E[Y | do(X=1)] - E[Y | do(X=0)]$.
*   Paso 2: Verificación Backdoor: El camino trasero $X \leftarrow Z \rightarrow Y$ es una ruta confundidora. Condicionar sobre la Densidad de Visitantes ($Z$) bloquea esta ruta.
*   Paso 3: Expresión de Ajuste:
    $$ E[Y | do(X=x)] = \int_{z} E[Y | X=x, Z=z] f_Z(z) dz $$
*   Paso 4: Mecanismo: En análisis de conservación marina, comparamos tasas de degradación entre zonas con y sin programa, estratificadas por niveles de afluencia turística. Esto aísla el efecto del programa de la presión ambiental inherente de la zona. Luego agregamos los efectos ponderados por la distribución de densidades en la región.

5. Enfoque Resultados Potenciales de Rubin (Balanceo de Datos):

*   Paso 1: Definir Resultados Potenciales: $Y_i(1)$ es la degradación del arrecife $i$ si contara con el programa; $Y_i(0)$ sin programa.
*   Paso 2: Cálculo de Puntuación de Propensión: Modelar la probabilidad de tener el programa dada la densidad: $e(z) = P(X=1 | Z=z).$ Zonas de alta afluencia tienen mayor propensión.
*   Paso 3: Aplicación de Pesos IPW:
    $$ w_i = \frac{X_i}{e(Z_i)} + \frac{1-X_i}{1-e(Z_i)} $$
*   Paso 4: Mecanismo: Los pesos crean una pseudo-población donde la distribución de densidad de visitantes está balanceada entre grupos con y sin programa. Zonas de baja afluencia que tienen el programa (raras) se sobreponderan, mientras que zonas de alta afluencia con programa (comunes) se subponderan. Esto permite una comparación imparcial de la degradación media como si la implementación del programa fuera aleatoria respecto a la afluencia.

---

Escenario 32: Logística de Última Milla y Emisiones de Carbono

1. Título del Escenario: Impacto de las Flotas de Reparto Eléctricas en Emisiones Operativas

2. Variables Clave:
*   Tratamiento (X): Uso de "Vehículo Eléctrico de Reparto" (Binario: 1 = Eléctrico, 0 = Combustión Interna).
*   Resultado (Y): Emisiones de CO₂ por Entrega (Continua: kg CO₂ por paquete). Valores más bajos son mejores.
*   Factor Confundidor (Z): Distancia Media de Ruta (Continua: km por entrega). Las empresas suelen asignar vehículos eléctricos a rutas urbanas cortas ($X$) para maximizar la autonomía, y estas rutas cortas ya generan menos emisiones base que rutas interurbanas largas ($Y$).

3. Mecanismo Causal y DAG ASCII:
La distancia media de ruta ($Z$) influye tanto en la asignación del vehículo eléctrico ($X$) como en las emisiones base ($Y$). El vehículo eléctrico ($X$) reduce directamente las emisiones ($Y$) al eliminar el escape de combustible fósil.

```text
      Z (Distancia Media de Ruta)
     / \
    v   v
X (Vehículo Eléctrico) --> Y (Emisiones CO₂)
```

4. Enfoque SCM de Pearl (Bloqueo por Grafo):

*   Paso 1: Objetivo: Estimar la reducción promedio de emisiones atribuible a la flota eléctrica: $E[Y | do(X=1)] - E[Y | do(X=0)]$.
*   Paso 2: Verificación Backdoor: El camino trasero $X \leftarrow Z \rightarrow Y$ es una ruta confundidora. Condicionar sobre la Distancia Media de Ruta ($Z$) bloquea esta ruta.
*   Paso 3: Expresión de Ajuste:
    $$ E[Y | do(X=x)] = \int_{z} E[Y | X=x, Z=z] f_Z(z) dz $$
*   Paso 4: Mecanismo: En análisis de sostenibilidad logística, comparamos emisiones por entrega entre vehículos eléctricos y de combustión, estratificados por tramos de distancia de ruta. Esto aísla el beneficio tecnológico del vehículo eléctrico de la reducción natural de emisiones en rutas cortas. Luego agregamos los efectos ponderados por la distribución de distancias en la red de reparto.

5. Enfoque Resultados Potenciales de Rubin (Balanceo de Datos):

*   Paso 1: Definir Resultados Potenciales: $Y_i(1)$ son las emisiones de la ruta $i$ si se usa vehículo eléctrico; $Y_i(0)$ si se usa combustión.
*   Paso 2: Cálculo de Puntuación de Propensión: Estimar $e(z) = P(X=1 | Z=z)$. Rutas cortas tienen mayor propensión a recibir vehículo eléctrico.
*   Paso 3: Aplicación de Pesos IPW:
    $$ \hat{ATE} = \frac{1}{N} \sum_{i=1}^{N} \left[ \frac{X_i Y_i}{e(Z_i)} - \frac{(1-X_i) Y_i}{1-e(Z_i)} \right] $$
*   Paso 4: Mecanismo: Los pesos crean una pseudo-población donde la distribución de distancias de ruta está balanceada entre grupos eléctricos y de combustión. Rutas largas que usan vehículo eléctrico (raras) se sobreponderan, mientras que rutas cortas con vehículo eléctrico (comunes) se subponderan. Esto permite una estimación imparcial del impacto real de la electrificación sobre las emisiones.



---
¿Te gustaría que genere una simulación en Python para uno de estos escenarios en español, con comentarios en el mismo idioma para que sea más fácil de seguir?<br><br>百度AI生成，内容仅供参考