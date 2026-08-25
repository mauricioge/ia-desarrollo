# Pautas para el uso de IA aplicada al Desarrollo de Software

Cada pauta sigue el mismo formato: **qué hacer** → **por qué importa** → **ejemplo de prompt**.

---

## 1. Planeación

### 1.0 Insumos iniciales (antes de pedir ruta a seguir)
**QUÉ HACER:**\
Antes de consultarle a la IA la ruta a seguir (1.1), declarar explícitamente:
1. **Recursos con los que se cuenta** — desarrolladores, DBAs, código heredado (legacy), servicios en la nube, licencias, infraestructura ya disponible.
2. **Recursos que proporcionará el cliente** (si aplica) — por naturaleza del proyecto o por acuerdo contractual: accesos, credenciales, datos, personal de su lado, servicios ya contratados por ellos, etc.
3. **Deadline, si ya fue establecido** — y si ese deadline **no está a favor del proyecto** (es decir, es más ajustado de lo que el alcance requiere), identificar qué recursos adicionales serían necesarios para cumplirlo (más desarrolladores, reducir alcance, horas extra, servicios adicionales en la nube, etc.).

**POR QUÉ IMPORTA:**\
La ruta a seguir (1.1), el orden de construcción (1.3) y la estimación de tiempos (1.7) dependen directamente de qué recursos existen realmente y de cuánto tiempo se dispone. Pedir la ruta sin este insumo produce una recomendación teórica que puede ser inviable con lo que realmente se tiene disponible.

**EJEMPLO DE PROMPT:**\
"Antes de proponer la ruta a seguir, toma en cuenta estos insumos: recursos propios [lista], recursos que proporcionará el cliente [lista o 'ninguno'], y deadline [fecha o 'sin definir']. Si el deadline es ajustado, dime qué recursos adicionales se necesitarían para cumplirlo."

### 1.1 Consultar la ruta a seguir y exigir justificación
**QUÉ HACER:**\
Antes de escribir código, pedirle a la IA que proponga la ruta técnica a seguir y que la justifique.

**POR QUÉ IMPORTA:**\
Evita que la IA (o el desarrollador) salte directo a codificar sin haber pensado el enfoque.

**EJEMPLO DE PROMPT:**\
"Antes de escribir código, dime qué arquitectura/enfoque usarías para esto y por qué."

### 1.2 Pensamiento crítico sobre características
**QUÉ HACER:**\
Pedir que agregue o elimine características, pero preguntando antes cómo afecta el desarrollo.

**POR QUÉ IMPORTA:**\
Cada feature tiene costo de tiempo, complejidad y mantenimiento; decidir a ciegas infla el alcance.

**EJEMPLO DE PROMPT:**\
"Si agrego [funcionalidad X], ¿cómo afecta el tiempo de desarrollo y la complejidad del sistema? ¿Qué se simplifica si la elimino?"

### 1.3 Orden de construcción por recursos disponibles
**QUÉ HACER:**\
Pedir que liste las características en el orden en que deben crearse para aprovechar los recursos disponibles (desarrolladores, DBAs, servicios en la nube, etc.).

**POR QUÉ IMPORTA:**\
Evita cuellos de botella — por ejemplo, que el DBA esté esperando mientras se termina el frontend.

**EJEMPLO DE PROMPT:**\
"Dado que tengo estos recursos disponibles [lista], ordena las características a construir para que ningún recurso quede ocioso."

### 1.4 Identificar riesgos y supuestos antes de codificar
**QUÉ HACER:**\
Pedir que liste explícitamente qué está asumiendo (versiones, comportamiento de APIs externas, volumen de datos) y qué se rompe si esos supuestos son falsos.

**POR QUÉ IMPORTA:**\
Evita construir sobre bases que nadie verificó.

**EJEMPLO DE PROMPT:**\
"Antes de implementar esto, dime qué supuestos estás haciendo y qué pasaría si alguno resulta falso."

### 1.5 Pedir alternativas de arquitectura, no una sola
**QUÉ HACER:**\
Solicitar 2-3 enfoques distintos con sus trade-offs (ventajas y desventajas), no solo la primera solución plausible.

**POR QUÉ IMPORTA:**\
Fuerza comparación real en vez de aceptar la respuesta por defecto.

**EJEMPLO DE PROMPT:**\
"Dame 2 o 3 enfoques distintos para resolver esto, con ventajas y desventajas de cada uno."

### 1.6 Escalabilidad como decisión de arquitectura, no de ajuste posterior
**QUÉ HACER:**\
Antes de decidir la arquitectura, preguntar por usuarios/registros esperados a 6 meses y 2 años, si la solución escala horizontal o vertical, y cuál sería el primer cuello de botella.

**POR QUÉ IMPORTA:**\
Corregir una arquitectura que no escala es mucho más caro que diseñarla bien desde el inicio.

**EJEMPLO DE PROMPT:**\
"Con un crecimiento esperado de X usuarios en 6 meses y Y en 2 años, ¿esta arquitectura escala? ¿Cuál sería el primer cuello de botella?"

### 1.7 Estimación de tiempos con margen para imprevistos (PERT)
**QUÉ HACER:**\ Pedir tres estimados por tarea (optimista, más probable, pesimista) y aplicar la fórmula PERT: `(O + 4M + P) / 6`. Alternativamente, usar un buffer único al final del proyecto (30-50% del total) en vez de inflar cada tarea individual.

**POR QUÉ IMPORTA:**\
Da un tiempo de entrega realista sin depender de un "colchón" arbitrario, y obliga a la IA a explicitar _dónde ve riesgo_.

**EJEMPLO DE PROMPT:**\
"Para esta tarea, dame estimado optimista, más probable y pesimista, y explica por qué cada uno."

---

## 2. Ejecución

### 2.1 Explicar el "por qué", no solo el "qué"
**QUÉ HACER:**\
Exigir que cada decisión no trivial venga con su justificación (por qué ese patrón, esa librería, ese enfoque).

**POR QUÉ IMPORTA:**\
Permite auditar después, por un humano o por la IA en otra sesión.

**EJEMPLO DE PROMPT:**\
"Explica por qué elegiste este patrón/librería y no otro."

### 2.2 Revisión de código humana obligatoria
**QUÉ HACER:**\
Ningún código se fusiona o despliega sin que una persona lo lea, sin importar qué tan bien luzca el proceso previo.

**POR QUÉ IMPORTA:**\
La IA puede generar código sintácticamente correcto pero funcionalmente equivocado con total confianza.

### 2.3 Casos de prueba, incluyendo casos límite
**QUÉ HACER:**\
Pedir que proponga qué probar — inputs vacíos, valores extremos, fallos de red — no solo el "happy path".

**POR QUÉ IMPORTA:**\
El camino feliz rara vez es donde ocurren los bugs en producción.

**EJEMPLO DE PROMPT:**\
"Antes de implementar, dime qué casos límite y de error debería contemplar."

### 2.4 Pruebas unitarias y de integración como parte del entregable
**QUÉ HACER:**\
Que la IA genere pruebas unitarias y de integración automatizadas junto con el código, con cobertura real de la lógica de negocio — no después, ni superficiales.

**POR QUÉ IMPORTA:**\
Pruebas escritas "después" tienden a no escribirse, o a cubrir solo lo obvio.

**EJEMPLO DE PROMPT:**\
"Junto con esta función, genera las pruebas unitarias que cubran su lógica de negocio y los casos límite ya identificados."

### 2.5 Control de contexto/memoria explícito
**QUÉ HACER:**\
En proyectos largos, definir qué información persiste entre sesiones (specs, decisiones tomadas) y qué no.

**POR QUÉ IMPORTA:**\
Evita que la IA "olvide" una decisión ya tomada o la contradiga en otra sesión.

---

## 3. Seguridad y calidad

### 3.1 Checklist de seguridad obligatorio
**QUÉ HACER:**\
Revisar explícitamente inyección SQL, manejo de secretos/credenciales, validación de inputs y permisos.

**POR QUÉ IMPORTA:**\
La IA no lo revisa por defecto a menos que se le pida.

**EJEMPLO DE PROMPT:**\
"Revisa este código específicamente por vulnerabilidades de seguridad: inyección, manejo de credenciales, validación de inputs, permisos."

### 3.2 Declarar deuda técnica introducida
**QUÉ HACER:**\
Si se toma un atajo por velocidad, que quede declarado y registrado.

**POR QUÉ IMPORTA:**\
La deuda técnica no declarada se vuelve invisible hasta que causa un problema.

---

## 4. Documentación

### 4.1 Documentación como parte del entregable
**QUÉ HACER:**\
Generar documentación técnica (qué se construyó, cómo se configura, cómo se despliega) junto con el código, no como ocurrencia tardía.

**POR QUÉ IMPORTA:**\
No depende de que el mismo desarrollador o la misma sesión de IA recuerde todo después.

---
## 5. Gobernanza (transversal — aplica en todo el proceso)
---

## Resumen de la estructura

| Sección | Pautas |
|---|---|
| 1. Planeación | 1.0 – 1.7 |
| 2. Ejecución | 2.1 – 2.5 |
| 3. Seguridad y calidad | 3.1 – 3.2 |
| 4. Documentación | 4.1 |
| 5. Gobernanza | 5.1 – 5.2 |
