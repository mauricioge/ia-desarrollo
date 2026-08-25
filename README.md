# Pautas para el uso de IA aplicada al Desarrollo de Software

Cada pauta sigue el mismo formato: **qué hacer** → **por qué importa** → **ejemplo de prompt**.

---

## 1. Planeación

### 1.0 Insumos iniciales (antes de pedir ruta a seguir)
**Qué hacer:** Antes de consultarle a la IA la ruta a seguir (1.1), declarar explícitamente:
1. **Recursos con los que se cuenta** — desarrolladores, DBAs, servicios en la nube, licencias, infraestructura ya disponible.
2. **Recursos que proporcionará el cliente** (si aplica) — por naturaleza del proyecto o por acuerdo contractual: accesos, credenciales, datos, personal de su lado, servicios ya contratados por ellos, etc.
3. **Deadline, si ya está establecido** — y si ese deadline **no está a favor del proyecto** (es decir, es más ajustado de lo que el alcance requiere), identificar qué recursos adicionales serían necesarios para cumplirlo (más desarrolladores, reducir alcance, horas extra, servicios adicionales en la nube, etc.).

**Por qué importa:** La ruta a seguir (1.1), el orden de construcción (1.3) y la estimación de tiempos (1.7) dependen directamente de qué recursos existen realmente y de cuánto tiempo hay. Pedir la ruta sin este insumo produce una recomendación teórica que puede ser inviable con lo que realmente se tiene disponible.

**Ejemplo de prompt:** "Antes de proponer la ruta a seguir, toma en cuenta estos insumos: recursos propios [lista], recursos que proporcionará el cliente [lista o 'ninguno'], y deadline [fecha o 'sin definir']. Si el deadline es ajustado, dime qué recursos adicionales se necesitarían para cumplirlo."

### 1.1 Consultar la ruta a seguir y exigir justificación
**Qué hacer:** Antes de escribir código, pedirle a la IA que proponga la ruta técnica a seguir y que la justifique.

**Por qué importa:** Evita que la IA (o el desarrollador) salte directo a codificar sin haber pensado el enfoque.

**Ejemplo de prompt:** "Antes de escribir código, dime qué arquitectura/enfoque usarías para esto y por qué."

### 1.2 Pensamiento crítico sobre características
**Qué hacer:** Pedir que agregue o elimine características, pero preguntando antes cómo afecta el desarrollo.

**Por qué importa:** Cada feature tiene costo de tiempo, complejidad y mantenimiento; decidir a ciegas infla el alcance.

**Ejemplo de prompt:** "Si agrego [funcionalidad X], ¿cómo afecta el tiempo de desarrollo y la complejidad del sistema? ¿Qué se simplifica si la elimino?"

### 1.3 Orden de construcción por recursos disponibles
**Qué hacer:** Pedir que liste las características en el orden en que deben crearse para aprovechar los recursos disponibles (desarrolladores, DBAs, servicios en la nube, etc.).

**Por qué importa:** Evita cuellos de botella — por ejemplo, que el DBA esté esperando mientras se termina el frontend.

**Ejemplo de prompt:** "Dado que tengo estos recursos disponibles [lista], ordena las características a construir para que ningún recurso quede ocioso."

### 1.4 Identificar riesgos y supuestos antes de codificar
**Qué hacer:** Pedir que liste explícitamente qué está asumiendo (versiones, comportamiento de APIs externas, volumen de datos) y qué se rompe si esos supuestos son falsos.

**Por qué importa:** Evita construir sobre bases que nadie verificó.

**Ejemplo de prompt:** "Antes de implementar esto, dime qué supuestos estás haciendo y qué pasaría si alguno resulta falso."

### 1.5 Pedir alternativas de arquitectura, no una sola
**Qué hacer:** Solicitar 2-3 enfoques distintos con sus trade-offs, no solo la primera solución plausible.

**Por qué importa:** Fuerza comparación real en vez de aceptar la respuesta por defecto.

**Ejemplo de prompt:** "Dame 2 o 3 enfoques distintos para resolver esto, con ventajas y desventajas de cada uno."

### 1.6 Escalabilidad como decisión de arquitectura, no de ajuste posterior
**Qué hacer:** Antes de decidir la arquitectura, preguntar por usuarios/registros esperados a 6 meses y 2 años, si la solución escala horizontal o vertical, y cuál sería el primer cuello de botella.

**Por qué importa:** Corregir una arquitectura que no escala es mucho más caro que diseñarla bien desde el inicio.

**Ejemplo de prompt:** "Con un crecimiento esperado de X usuarios en 6 meses y Y en 2 años, ¿esta arquitectura escala? ¿Cuál sería el primer cuello de botella?"

### 1.7 Estimación de tiempos con margen para imprevistos (PERT)
**Qué hacer:** Pedir tres estimados por tarea (optimista, más probable, pesimista) y aplicar la fórmula PERT: `(O + 4M + P) / 6`. Alternativamente, usar un buffer único al final del proyecto (30-50% del total) en vez de inflar cada tarea individual.

**Por qué importa:** Da un tiempo de entrega realista sin depender de un "colchón" arbitrario, y obliga a la IA a explicitar dónde ve riesgo.

**Ejemplo de prompt:** "Para esta tarea, dame estimado optimista, más probable y pesimista, y explica por qué cada uno."

---

## 2. Ejecución

### 2.1 Explicar el "por qué", no solo el "qué"
**Qué hacer:** Exigir que cada decisión no trivial venga con su justificación (por qué ese patrón, esa librería, ese enfoque).

**Por qué importa:** Permite auditar después, por un humano o por la IA en otra sesión.

**Ejemplo de prompt:** "Explica por qué elegiste este patrón/librería y no otro."

### 2.2 Revisión de código humana obligatoria
**Qué hacer:** Ningún código se fusiona o despliega sin que una persona lo lea, sin importar qué tan bien luzca el proceso previo.

**Por qué importa:** La IA puede generar código sintácticamente correcto pero funcionalmente equivocado con total confianza.

### 2.3 Casos de prueba, incluyendo casos límite
**Qué hacer:** Pedir que proponga qué probar — inputs vacíos, valores extremos, fallos de red — no solo el "happy path".

**Por qué importa:** El camino feliz rara vez es donde ocurren los bugs en producción.

**Ejemplo de prompt:** "Antes de implementar, dime qué casos límite y de error debería contemplar."

### 2.4 Pruebas unitarias como parte del entregable
**Qué hacer:** Que la IA genere pruebas unitarias automatizadas junto con el código, con cobertura real de la lógica de negocio — no después, ni superficiales.

**Por qué importa:** Pruebas escritas "después" tienden a no escribirse, o a cubrir solo lo obvio.

**Ejemplo de prompt:** "Junto con esta función, genera las pruebas unitarias que cubran su lógica de negocio y los casos límite ya identificados."

### 2.5 Control de contexto/memoria explícito
**Qué hacer:** En proyectos largos, definir qué información persiste entre sesiones (specs, decisiones tomadas) y qué no.

**Por qué importa:** Evita que la IA "olvide" una decisión ya tomada o la contradiga en otra sesión.

---

## 3. Seguridad y calidad

### 3.1 Checklist de seguridad obligatorio
**Qué hacer:** Revisar explícitamente inyección SQL, manejo de secretos/credenciales, validación de inputs y permisos.

**Por qué importa:** La IA no lo revisa por defecto a menos que se le pida.

**Ejemplo de prompt:** "Revisa este código específicamente por vulnerabilidades de seguridad: inyección, manejo de credenciales, validación de inputs, permisos."

### 3.2 Declarar deuda técnica introducida
**Qué hacer:** Si se toma un atajo por velocidad, que quede declarado y registrado.

**Por qué importa:** La deuda técnica no declarada se vuelve invisible hasta que causa un problema.

---

## 4. Documentación

### 4.1 Documentación como parte del entregable
**Qué hacer:** Generar documentación técnica (qué se construyó, cómo se configura, cómo se despliega) junto con el código, no como ocurrencia tardía.

**Por qué importa:** No depende de que el mismo desarrollador o la misma sesión de IA recuerde todo después.

---

## 5. Gobernanza (transversal — aplica en todo el proceso)

### 5.1 Definir qué decisiones puede tomar la IA sola
**Qué hacer:** Establecer explícitamente qué puede decidir la IA sin aprobación (ej. nombres de variables) y qué requiere aprobación humana (ej. cambios de esquema en producción).

**Por qué importa:** Sin esta línea clara, se corre el riesgo de cambios no supervisados en zonas críticas.

### 5.2 Versionado y capacidad de reversión
**Qué hacer:** Ningún cambio sugerido por la IA se aplica directo a producción sin un punto de rollback claro.

**Por qué importa:** Es la red de seguridad final cuando algo de lo anterior falla.

---

## Resumen de la estructura

| Sección | Pautas |
|---|---|
| 1. Planeación | 1.0 – 1.7 |
| 2. Ejecución | 2.1 – 2.5 |
| 3. Seguridad y calidad | 3.1 – 3.2 |
| 4. Documentación | 4.1 |
| 5. Gobernanza | 5.1 – 5.2 |
