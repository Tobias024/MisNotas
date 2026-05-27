# Capítulo 3 — Modularidad

> "El 95% del material sobre arquitectura habla de los beneficios de la modularidad, pero casi nada explica cómo lograrla."
>
> — Glenford J. Myers, 1978

---

## ¿De qué trata este capítulo?

De cómo **medir** si un sistema está bien dividido en partes coherentes. No alcanza con decir "esto es modular": existen métricas concretas que permiten evaluarlo, refactorizarlo y defenderlo ante otros.

El capítulo introduce tres herramientas conceptuales que vas a usar el resto del libro y el resto de tu carrera: **cohesión**, **acoplamiento (coupling)** y **connascence**.

---

## ¿Por qué importa la modularidad?

Los sistemas de software son sistemas complejos. Y como cualquier sistema físico complejo, tienden naturalmente al **desorden** (entropía).

Para mantener orden, hay que **invertir energía**. En arquitectura, esa energía se invierte preservando límites claros entre las partes, manteniendo las dependencias controladas y revisando regularmente si la estructura sigue siendo sana.

Si no se invierte esa energía, el sistema se degrada por sí solo. No hay que hacer nada para que un sistema se vuelva un desastre; hay que hacer mucho para que no.

La modularidad es lo que Richards y Ford llaman una **característica implícita**: casi ningún proyecto te pide explícitamente que la cuides en sus requisitos. Pero un código sin modularidad no se sostiene a mediano plazo.

---

## Definición de "módulo"

El diccionario define _módulo_ como "cada una de las partes estandarizadas o unidades independientes que se pueden usar para construir una estructura más compleja".

En el libro, **modularidad** se usa como término genérico para describir **un agrupamiento lógico de código relacionado**. Puede ser:

- Un conjunto de clases en un lenguaje orientado a objetos
- Un conjunto de funciones en un lenguaje funcional o estructurado
- Un `package` en Java, un `namespace` en .NET, un módulo en Python

**Punto clave:** "modularidad" describe una separación **lógica**, no necesariamente física. Dos clases pueden vivir en el mismo binario pero ser conceptualmente módulos distintos.

Esta distinción importa cuando llega el momento de **partir un monolito**: si el código no fue armado con módulos lógicos claros, romperlo en partes físicas separadas resulta carísimo.

---

## Tres herramientas para medirla

Los investigadores desarrollaron varias métricas para evaluar modularidad, agnósticas al lenguaje. El libro se concentra en tres:

1. **Cohesión** — qué tan relacionado está el contenido de un módulo consigo mismo.
2. **Acoplamiento (coupling)** — qué tan conectados están los módulos entre sí.
3. **Connascence** — refinamiento del coupling para lenguajes modernos. Mide qué tipos de cambios obligan a otros cambios.

Las tres se complementan. La primera mira hacia adentro del módulo; la segunda hacia afuera; la tercera describe la naturaleza del vínculo cuando existe.

---

## 1. Cohesión

**Cohesión** mide en qué medida las partes de un módulo **deberían** estar juntas. Es una medida de qué tan relacionadas están entre sí.

Un módulo cohesivo es aquel donde, si lo partís en dos, las dos partes resultantes terminan acoplándose tanto que el split no compensa.

> "Intentar dividir un módulo cohesivo sólo resulta en mayor acoplamiento y menor legibilidad."
>
> — Larry Constantine

### Los siete niveles de cohesión

Existe un rango clásico, ordenado de **mejor a peor**:

<svg viewBox="0 0 720 460" xmlns="http://www.w3.org/2000/svg" style="max-width:100%;height:auto;display:block;margin:1em 0;">
  <rect x="0" y="0" width="720" height="460" fill="#FBF7F0"/>
  <text x="360" y="30" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="16" font-weight="bold" fill="#2A2A2A">Niveles de cohesión — de mejor a peor</text>

  <!-- Functional - best -->
  <rect x="60" y="60" width="600" height="44" fill="#D4EDD4" stroke="#3D7F4D" stroke-width="2" rx="4"/>
  <text x="80" y="88" font-family="Verdana, Tahoma, sans-serif" font-size="14" font-weight="bold" fill="#2A2A2A">1. Functional</text>
  <text x="280" y="88" font-family="Verdana, Tahoma, sans-serif" font-size="13" fill="#2A2A2A">Todo en el módulo contribuye a una sola función bien definida.</text>

  <!-- Sequential -->
  <rect x="60" y="112" width="600" height="44" fill="#DCEAD8" stroke="#5C9A5C" stroke-width="2" rx="4"/>
  <text x="80" y="140" font-family="Verdana, Tahoma, sans-serif" font-size="14" font-weight="bold" fill="#2A2A2A">2. Sequential</text>
  <text x="280" y="140" font-family="Verdana, Tahoma, sans-serif" font-size="13" fill="#2A2A2A">La salida de un componente es la entrada del siguiente.</text>

  <!-- Communicational -->
  <rect x="60" y="164" width="600" height="44" fill="#E7EFD4" stroke="#7AA856" stroke-width="2" rx="4"/>
  <text x="80" y="192" font-family="Verdana, Tahoma, sans-serif" font-size="14" font-weight="bold" fill="#2A2A2A">3. Communicational</text>
  <text x="280" y="192" font-family="Verdana, Tahoma, sans-serif" font-size="13" fill="#2A2A2A">Comparten los mismos datos pero hacen cosas distintas con ellos.</text>

  <!-- Procedural -->
  <rect x="60" y="216" width="600" height="44" fill="#F2EED4" stroke="#B5A53D" stroke-width="2" rx="4"/>
  <text x="80" y="244" font-family="Verdana, Tahoma, sans-serif" font-size="14" font-weight="bold" fill="#2A2A2A">4. Procedural</text>
  <text x="280" y="244" font-family="Verdana, Tahoma, sans-serif" font-size="13" fill="#2A2A2A">Deben ejecutarse en un orden específico, sin compartir datos.</text>

  <!-- Temporal -->
  <rect x="60" y="268" width="600" height="44" fill="#F5E6CC" stroke="#C9913D" stroke-width="2" rx="4"/>
  <text x="80" y="296" font-family="Verdana, Tahoma, sans-serif" font-size="14" font-weight="bold" fill="#2A2A2A">5. Temporal</text>
  <text x="280" y="296" font-family="Verdana, Tahoma, sans-serif" font-size="13" fill="#2A2A2A">Relacionados sólo por el momento en que se ejecutan (ej. arranque).</text>

  <!-- Logical -->
  <rect x="60" y="320" width="600" height="44" fill="#F2D9C9" stroke="#C97B2E" stroke-width="2" rx="4"/>
  <text x="80" y="348" font-family="Verdana, Tahoma, sans-serif" font-size="14" font-weight="bold" fill="#2A2A2A">6. Logical</text>
  <text x="280" y="348" font-family="Verdana, Tahoma, sans-serif" font-size="13" fill="#2A2A2A">Categoría lógica pero funcionalmente distintos (StringUtils, helpers).</text>

  <!-- Coincidental - worst -->
  <rect x="60" y="372" width="600" height="44" fill="#EDC7C7" stroke="#A93D3D" stroke-width="2" rx="4"/>
  <text x="80" y="400" font-family="Verdana, Tahoma, sans-serif" font-size="14" font-weight="bold" fill="#2A2A2A">7. Coincidental</text>
  <text x="280" y="400" font-family="Verdana, Tahoma, sans-serif" font-size="13" fill="#2A2A2A">No tienen relación. Están juntos por accidente. La peor forma.</text>

  <!-- Arrow indicating direction -->
  <text x="30" y="80" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#3D7F4D" font-weight="bold">Mejor</text>
  <text x="30" y="400" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#A93D3D" font-weight="bold">Peor</text>
  <line x1="40" y1="100" x2="40" y2="370" stroke="#888" stroke-width="1.5" marker-end="url(#arrow-down)"/>
  <defs>
    <marker id="arrow-down" markerWidth="10" markerHeight="10" refX="5" refY="9" orient="auto">
      <path d="M0,0 L10,0 L5,9 Z" fill="#888"/>
    </marker>
  </defs>

  <text x="360" y="445" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#5A5A5A" font-style="italic">Cuanto más arriba, más justificado está que el código viva en el mismo módulo.</text>
</svg>

### Ejemplos rápidos de cada nivel

**Functional** — Un módulo `CalcularImpuesto` que sólo contiene la lógica para calcular impuestos. Todo lo que está adentro existe para esa única función.

**Sequential** — Un módulo `ImportarCSV` que primero lee, después valida, después transforma y después guarda. Cada paso usa la salida del anterior.

**Communicational** — Un módulo que `agregaRegistroEnBD()` y después `enviaMailDeConfirmacion()`. Las dos cosas operan sobre la misma información.

**Procedural** — Un módulo de "inicialización" donde paso 1 debe correr antes que paso 2 antes que paso 3, pero no comparten datos entre sí.

**Temporal** — Módulos relacionados sólo por el momento en que se ejecutan. El clásico es la rutina de _startup_ del sistema, que arranca logging, conexión a BD, lectura de configuración: cosas no relacionadas que sólo coinciden temporalmente.

**Logical** — El típico `StringUtils` con métodos estáticos sobre `String` que no tienen nada que ver entre sí salvo que operan sobre el mismo tipo de datos.

**Coincidental** — Funciones puestas en el mismo archivo porque sí, sin ningún criterio organizativo. El peor caso.

### Cohesión no es una métrica precisa

Decidir si un grupo de funciones debería ser un solo módulo o dos depende de **contexto y juicio**. El libro da un ejemplo claro:

```
Customer Maintenance
- add customer
- update customer
- get customer
- notify customer
- get customer orders
- cancel customer orders
```

¿Las dos últimas operaciones (orders) deberían estar acá, o vivir en su propio módulo `Order Maintenance`? La respuesta depende de:

1. ¿Esas dos son las únicas operaciones sobre órdenes? Si sí, conviene dejarlas adentro.
2. ¿Se espera que `Customer Maintenance` siga creciendo? Si sí, conviene sacarlas ahora.
3. ¿Las operaciones de orders necesitan tanto conocimiento de customer que separarlas exigiría mucho coupling para funcionar?

**Este tipo de análisis de trade-offs es el corazón del trabajo de arquitecto.**

### La métrica LCOM

Existe una métrica estructural llamada **LCOM** (Lack of Cohesion in Methods). En palabras simples:

> LCOM mide la suma de conjuntos de métodos que **no** comparten ningún campo entre sí.

Es decir: si una clase tiene métodos que tocan distintos subgrupos de campos sin solaparse, la clase probablemente debería ser varias clases distintas.

<svg viewBox="0 0 720 320" xmlns="http://www.w3.org/2000/svg" style="max-width:100%;height:auto;display:block;margin:1em 0;">
  <rect x="0" y="0" width="720" height="320" fill="#FBF7F0"/>
  <text x="360" y="30" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="16" font-weight="bold" fill="#2A2A2A">LCOM — Cohesión estructural en tres clases</text>

  <!-- Class X - good cohesion -->
  <rect x="40" y="60" width="200" height="220" fill="#FFFFFF" stroke="#3D7F4D" stroke-width="2" rx="6"/>
  <text x="140" y="85" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="14" font-weight="bold" fill="#3D7F4D">Class X</text>
  <text x="140" y="105" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#3D7F4D">Cohesión alta</text>

  <!-- Fields -->
  <circle cx="80" cy="140" r="14" fill="#FFE9A8" stroke="#B5A53D" stroke-width="1.5"/>
  <text x="80" y="145" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="12" font-weight="bold" fill="#2A2A2A">a</text>
  <circle cx="80" cy="200" r="14" fill="#FFE9A8" stroke="#B5A53D" stroke-width="1.5"/>
  <text x="80" y="205" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="12" font-weight="bold" fill="#2A2A2A">b</text>

  <!-- Methods -->
  <rect x="170" y="125" width="40" height="30" fill="#CFE3F5" stroke="#2C5F9E" stroke-width="1.5" rx="3"/>
  <text x="190" y="145" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#2A2A2A">m1</text>
  <rect x="170" y="185" width="40" height="30" fill="#CFE3F5" stroke="#2C5F9E" stroke-width="1.5" rx="3"/>
  <text x="190" y="205" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#2A2A2A">m2</text>

  <!-- Connections -->
  <line x1="94" y1="140" x2="170" y2="140" stroke="#5A5A5A" stroke-width="1.5"/>
  <line x1="94" y1="200" x2="170" y2="200" stroke="#5A5A5A" stroke-width="1.5"/>
  <line x1="94" y1="148" x2="170" y2="195" stroke="#5A5A5A" stroke-width="1.5"/>
  <line x1="94" y1="192" x2="170" y2="155" stroke="#5A5A5A" stroke-width="1.5"/>

  <text x="140" y="260" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#2A2A2A">Cada método toca todos los campos.</text>

  <!-- Class Y - bad cohesion -->
  <rect x="260" y="60" width="200" height="220" fill="#FFFFFF" stroke="#A93D3D" stroke-width="2" rx="6"/>
  <text x="360" y="85" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="14" font-weight="bold" fill="#A93D3D">Class Y</text>
  <text x="360" y="105" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#A93D3D">Sin cohesión</text>

  <circle cx="300" cy="140" r="14" fill="#FFE9A8" stroke="#B5A53D" stroke-width="1.5"/>
  <text x="300" y="145" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="12" font-weight="bold" fill="#2A2A2A">a</text>
  <circle cx="300" cy="200" r="14" fill="#FFE9A8" stroke="#B5A53D" stroke-width="1.5"/>
  <text x="300" y="205" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="12" font-weight="bold" fill="#2A2A2A">b</text>

  <rect x="390" y="125" width="40" height="30" fill="#CFE3F5" stroke="#2C5F9E" stroke-width="1.5" rx="3"/>
  <text x="410" y="145" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#2A2A2A">m1</text>
  <rect x="390" y="185" width="40" height="30" fill="#CFE3F5" stroke="#2C5F9E" stroke-width="1.5" rx="3"/>
  <text x="410" y="205" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#2A2A2A">m2</text>

  <line x1="314" y1="140" x2="390" y2="140" stroke="#5A5A5A" stroke-width="1.5"/>
  <line x1="314" y1="200" x2="390" y2="200" stroke="#5A5A5A" stroke-width="1.5"/>

  <text x="360" y="260" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#2A2A2A">Cada método toca su propio campo.</text>
  <text x="360" y="275" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#A93D3D" font-style="italic">Podrían ser dos clases distintas.</text>

  <!-- Class Z - mixed -->
  <rect x="480" y="60" width="200" height="220" fill="#FFFFFF" stroke="#C9913D" stroke-width="2" rx="6"/>
  <text x="580" y="85" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="14" font-weight="bold" fill="#C9913D">Class Z</text>
  <text x="580" y="105" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#C9913D">Cohesión mixta</text>

  <circle cx="520" cy="135" r="12" fill="#FFE9A8" stroke="#B5A53D" stroke-width="1.5"/>
  <text x="520" y="140" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" font-weight="bold" fill="#2A2A2A">a</text>
  <circle cx="520" cy="180" r="12" fill="#FFE9A8" stroke="#B5A53D" stroke-width="1.5"/>
  <text x="520" y="185" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" font-weight="bold" fill="#2A2A2A">b</text>
  <circle cx="520" cy="225" r="12" fill="#FFE9A8" stroke="#B5A53D" stroke-width="1.5"/>
  <text x="520" y="230" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" font-weight="bold" fill="#2A2A2A">c</text>

  <rect x="600" y="120" width="36" height="26" fill="#CFE3F5" stroke="#2C5F9E" stroke-width="1.5" rx="3"/>
  <text x="618" y="137" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#2A2A2A">m1</text>
  <rect x="600" y="165" width="36" height="26" fill="#CFE3F5" stroke="#2C5F9E" stroke-width="1.5" rx="3"/>
  <text x="618" y="182" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#2A2A2A">m2</text>
  <rect x="600" y="210" width="36" height="26" fill="#CFE3F5" stroke="#2C5F9E" stroke-width="1.5" rx="3"/>
  <text x="618" y="227" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#2A2A2A">m3</text>

  <line x1="532" y1="135" x2="600" y2="133" stroke="#5A5A5A" stroke-width="1.5"/>
  <line x1="532" y1="142" x2="600" y2="175" stroke="#5A5A5A" stroke-width="1.5"/>
  <line x1="532" y1="180" x2="600" y2="178" stroke="#5A5A5A" stroke-width="1.5"/>
  <line x1="532" y1="225" x2="600" y2="223" stroke="#5A5A5A" stroke-width="1.5"/>

  <text x="580" y="260" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#2A2A2A">Algunos métodos comparten, otros no.</text>
  <text x="580" y="275" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#C9913D" font-style="italic">Refactorizar el método aislado.</text>
</svg>

LCOM es útil sobre todo cuando se está **migrando de un estilo arquitectónico a otro**. Permite detectar clases utilitarias compartidas que en realidad nunca debieron ser una sola clase.

**Limitación.** LCOM sólo encuentra falta de cohesión **estructural**. No puede decirte si algo es lógicamente coherente, sólo si tiene buen "encaje" mecánico de campos y métodos. Por eso es una pista, no una sentencia.

---

## 2. Acoplamiento (Coupling)

El acoplamiento mide qué tan **conectados** están los módulos entre sí. Cuanto más acoplados, más se influencian mutuamente cuando uno cambia.

A diferencia de cohesión, acá tenemos herramientas más precisas, basadas en **teoría de grafos**: como las llamadas entre módulos forman un grafo dirigido, podemos analizarlas matemáticamente.

### Las dos métricas base: afferent y efferent

Edward Yourdon y Larry Constantine, en 1979, definieron dos conceptos que siguen siendo la base de todo análisis de acoplamiento:

<svg viewBox="0 0 720 280" xmlns="http://www.w3.org/2000/svg" style="max-width:100%;height:auto;display:block;margin:1em 0;">
  <rect x="0" y="0" width="720" height="280" fill="#FBF7F0"/>
  <text x="360" y="30" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="16" font-weight="bold" fill="#2A2A2A">Afferent vs. Efferent Coupling</text>

  <!-- Central module -->
  <rect x="290" y="110" width="140" height="80" fill="#CFE3F5" stroke="#2C5F9E" stroke-width="2.5" rx="6"/>
  <text x="360" y="145" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="14" font-weight="bold" fill="#2A2A2A">Mi módulo</text>
  <text x="360" y="165" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#5A5A5A">(componente, clase...)</text>

  <!-- Afferent (incoming) - left side -->
  <text x="60" y="80" font-family="Verdana, Tahoma, sans-serif" font-size="13" font-weight="bold" fill="#3D7F4D">AFFERENT</text>
  <text x="60" y="98" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#3D7F4D">(entrantes, "A" = Antes en alfabeto)</text>

  <circle cx="100" cy="130" r="22" fill="#D4EDD4" stroke="#3D7F4D" stroke-width="2"/>
  <text x="100" y="135" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#2A2A2A">M1</text>
  <circle cx="100" cy="180" r="22" fill="#D4EDD4" stroke="#3D7F4D" stroke-width="2"/>
  <text x="100" y="185" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#2A2A2A">M2</text>
  <circle cx="100" cy="230" r="22" fill="#D4EDD4" stroke="#3D7F4D" stroke-width="2"/>
  <text x="100" y="235" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#2A2A2A">M3</text>

  <defs>
    <marker id="arr-green" markerWidth="10" markerHeight="10" refX="9" refY="5" orient="auto">
      <path d="M0,0 L10,5 L0,10 Z" fill="#3D7F4D"/>
    </marker>
    <marker id="arr-red" markerWidth="10" markerHeight="10" refX="9" refY="5" orient="auto">
      <path d="M0,0 L10,5 L0,10 Z" fill="#A93D3D"/>
    </marker>
  </defs>

  <line x1="122" y1="135" x2="288" y2="140" stroke="#3D7F4D" stroke-width="2" marker-end="url(#arr-green)"/>
  <line x1="122" y1="180" x2="288" y2="155" stroke="#3D7F4D" stroke-width="2" marker-end="url(#arr-green)"/>
  <line x1="122" y1="225" x2="288" y2="170" stroke="#3D7F4D" stroke-width="2" marker-end="url(#arr-green)"/>

  <!-- Efferent (outgoing) - right side -->
  <text x="600" y="80" font-family="Verdana, Tahoma, sans-serif" font-size="13" font-weight="bold" fill="#A93D3D">EFFERENT</text>
  <text x="600" y="98" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#A93D3D">(salientes, "E" = Exit)</text>

  <circle cx="620" cy="130" r="22" fill="#EDC7C7" stroke="#A93D3D" stroke-width="2"/>
  <text x="620" y="135" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#2A2A2A">M4</text>
  <circle cx="620" cy="180" r="22" fill="#EDC7C7" stroke="#A93D3D" stroke-width="2"/>
  <text x="620" y="185" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#2A2A2A">M5</text>
  <circle cx="620" cy="230" r="22" fill="#EDC7C7" stroke="#A93D3D" stroke-width="2"/>
  <text x="620" y="235" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#2A2A2A">M6</text>

  <line x1="430" y1="140" x2="598" y2="132" stroke="#A93D3D" stroke-width="2" marker-end="url(#arr-red)"/>
  <line x1="430" y1="155" x2="598" y2="180" stroke="#A93D3D" stroke-width="2" marker-end="url(#arr-red)"/>
  <line x1="430" y1="170" x2="598" y2="225" stroke="#A93D3D" stroke-width="2" marker-end="url(#arr-red)"/>

  <text x="360" y="260" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#5A5A5A" font-style="italic">Mnemónico: la "a" viene antes que la "e" en el alfabeto → afferent = adentro.</text>
</svg>

**Afferent coupling (entrantes)** — Cuántos otros módulos **dependen de** este módulo. Si subo mucho mi afferent, soy crítico para muchos.

**Efferent coupling (salientes)** — Cuántos otros módulos **este módulo necesita**. Si subo mucho mi efferent, dependo de muchos para funcionar.

**Mnemónico oficial del libro:** la _a_ aparece antes de la _e_ en el alfabeto, igual que _entrante_ aparece antes que _saliente_. O bien: _Efferent_ empieza con _e_, igual que _Exit_ (salida).

### Métricas derivadas

A partir de afferent y efferent, Robert Martin definió tres métricas más sofisticadas que ayudan a evaluar la **salud estructural** del código.

**Abstractness** — Ratio entre artefactos abstractos (interfaces, abstract classes) y concretos.

```
A = m_abstractos / (m_abstractos + m_concretos)
```

Un código con `A = 0` es todo implementación, ninguna abstracción. Un `main()` gigante de 5000 líneas.
Un código con `A = 1` es todo abstracción, ninguna implementación.

**Instability** — Qué tan volátil es un módulo, medido por la proporción de coupling saliente sobre el total.

```
I = efferent / (efferent + afferent)
```

Si `I` está cerca de 1, dependo de muchos. Cualquier cambio en mis dependencias me rompe. Soy frágil.
Si `I` está cerca de 0, muchos dependen de mí. Soy estable (y por eso debería cambiar poco).

### Distance from the Main Sequence

Una de las pocas métricas **holísticas** del libro. Combina abstractness e instability.

```
D = | A + I − 1 |
```

La idea: existe una **relación ideal** entre abstracción e inestabilidad.

- Lo que es **muy abstracto** debería ser **muy estable** (las interfaces base no deberían cambiar seguido).
- Lo que es **muy concreto** puede ser **inestable** (el código de implementación cambia con frecuencia).

Cualquier desvío de esa línea es una señal de alarma.

<svg viewBox="0 0 720 460" xmlns="http://www.w3.org/2000/svg" style="max-width:100%;height:auto;display:block;margin:1em 0;">
  <rect x="0" y="0" width="720" height="460" fill="#FBF7F0"/>
  <text x="360" y="30" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="16" font-weight="bold" fill="#2A2A2A">Distance from the Main Sequence</text>

  <!-- Plot area -->
  <rect x="160" y="70" width="320" height="320" fill="#FFFFFF" stroke="#5A5A5A" stroke-width="1.5"/>

  <!-- Axes labels -->
  <text x="320" y="420" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="12" fill="#2A2A2A">Instability (I)</text>
  <text x="155" y="240" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="12" fill="#2A2A2A" transform="rotate(-90 155 240)">Abstractness (A)</text>

  <!-- Tick labels -->
  <text x="160" y="408" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#5A5A5A">0</text>
  <text x="480" y="408" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#5A5A5A">1</text>
  <text x="148" y="394" text-anchor="end" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#5A5A5A">0</text>
  <text x="148" y="74" text-anchor="end" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#5A5A5A">1</text>

  <!-- Zone of Pain (bottom-left) -->
  <circle cx="200" cy="350" r="55" fill="#EDC7C7" opacity="0.6"/>
  <text x="200" y="345" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="12" font-weight="bold" fill="#A93D3D">Zone of</text>
  <text x="200" y="362" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="12" font-weight="bold" fill="#A93D3D">Pain</text>

  <!-- Zone of Uselessness (top-right) -->
  <circle cx="440" cy="110" r="55" fill="#EDC7C7" opacity="0.6"/>
  <text x="440" y="105" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="12" font-weight="bold" fill="#A93D3D">Zone of</text>
  <text x="440" y="122" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="12" font-weight="bold" fill="#A93D3D">Uselessness</text>

  <!-- Main sequence line -->
  <line x1="160" y1="70" x2="480" y2="390" stroke="#3D7F4D" stroke-width="3" stroke-dasharray="0"/>
  <text x="350" y="195" font-family="Verdana, Tahoma, sans-serif" font-size="12" font-weight="bold" fill="#3D7F4D" transform="rotate(45 350 195)">Main Sequence</text>

  <!-- Example dot -->
  <circle cx="280" cy="180" r="6" fill="#2C5F9E" stroke="#1A3F6E" stroke-width="2"/>
  <line x1="280" y1="180" x2="240" y2="220" stroke="#2C5F9E" stroke-width="1.5" stroke-dasharray="4,3"/>
  <text x="295" y="178" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#2C5F9E">Mi clase</text>
  <text x="190" y="240" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#2C5F9E" font-style="italic">D = distancia</text>

  <!-- Right panel - explanations -->
  <rect x="510" y="80" width="200" height="80" fill="#FFE9A8" stroke="#B5A53D" stroke-width="1.5" rx="4"/>
  <text x="610" y="100" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" font-weight="bold" fill="#2A2A2A">Zone of Uselessness</text>
  <text x="520" y="118" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#2A2A2A">Demasiada abstracción.</text>
  <text x="520" y="132" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#2A2A2A">Nadie usa estas clases.</text>
  <text x="520" y="146" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#2A2A2A">Sobreingeniería.</text>

  <rect x="510" y="190" width="200" height="80" fill="#D4EDD4" stroke="#3D7F4D" stroke-width="1.5" rx="4"/>
  <text x="610" y="210" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" font-weight="bold" fill="#2A2A2A">Main Sequence</text>
  <text x="520" y="228" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#2A2A2A">Balance saludable entre</text>
  <text x="520" y="242" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#2A2A2A">abstracción y volatilidad.</text>
  <text x="520" y="256" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#2A2A2A">Objetivo: estar cerca.</text>

  <rect x="510" y="300" width="200" height="80" fill="#EDC7C7" stroke="#A93D3D" stroke-width="1.5" rx="4"/>
  <text x="610" y="320" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" font-weight="bold" fill="#2A2A2A">Zone of Pain</text>
  <text x="520" y="338" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#2A2A2A">Mucha implementación,</text>
  <text x="520" y="352" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#2A2A2A">poca abstracción.</text>
  <text x="520" y="366" font-family="Verdana, Tahoma, sans-serif" font-size="10" fill="#2A2A2A">Código frágil y rígido.</text>
</svg>

**Zone of Pain** — código con mucha implementación y poca abstracción. Frágil: cualquier cambio se propaga porque no hay capa de aislamiento.

**Zone of Uselessness** — código con muchas abstracciones que nadie usa. Sobreingeniería. Difícil de entender, alta carga cognitiva, beneficio bajo.

**Distance metric** — qué tan lejos está una clase de la línea ideal. Cuanto más cerca de la línea, mejor. Es de las pocas formas de tener un diagnóstico visual de calidad arquitectónica en código existente.

### Limitaciones de las métricas

Las métricas estructurales tienen dos limitaciones grandes:

1. **No distinguen complejidad esencial de accidental.** Si el problema de negocio es complejo, el código puede ser legítimamente complejo. Las métricas no saben.

2. **Necesitan interpretación.** No alcanza con tener un número alto o bajo. Hay que entender por qué, en qué contexto, qué se gana al moverlo.

Las métricas son una pista para arrancar la conversación, no una sentencia.

---

## 3. Connascence

Meilir Page-Jones, en 1996, introdujo un refinamiento del concepto de coupling pensado para lenguajes orientados a objetos. Lo llamó **connascence**.

> "Dos componentes son **connascentes** si un cambio en uno requiere que el otro también cambie para mantener la correctitud del sistema."
>
> — Meilir Page-Jones

Hay dos grandes tipos:

- **Connascence estática** — coupling visible en el código fuente. Se detecta leyendo.
- **Connascence dinámica** — coupling que sólo se manifiesta en tiempo de ejecución. Más difícil de detectar.

### Connascence estática (5 tipos)

**Connascence of Name (CoN)** — Múltiples componentes deben **estar de acuerdo en el nombre** de una entidad.

Es la forma más común y más débil de coupling. Los IDEs modernos hacen trivial renombrar de forma global. Es el tipo de connascence que querés.

**Connascence of Type (CoT)** — Múltiples componentes deben **estar de acuerdo en el tipo** de una entidad.

Si una función recibe `Integer` y vos le pasás `String`, falla. En lenguajes con tipado estático esto lo atrapa el compilador. En tipado dinámico (sin spec), explota en runtime.

**Connascence of Meaning (CoM)** — Múltiples componentes deben **estar de acuerdo en el significado** de valores particulares.

El clásico: hardcodear que `1 = TRUE` y `0 = FALSE`. Si alguien los invierte, todo se rompe en silencio. La solución es refactorizar a constantes con nombre (que es CoN, más débil).

**Connascence of Position (CoP)** — Múltiples componentes deben **estar de acuerdo en el orden** de valores.

Si una función recibe `(nombre, asiento)` y la llamás como `("14D", "Ford, N")`, los tipos pueden ser válidos pero el significado está invertido. El compilador no se da cuenta. Forma de coupling peligrosa, sobre todo cuando hay muchos parámetros del mismo tipo.

**Connascence of Algorithm (CoA)** — Múltiples componentes deben **implementar el mismo algoritmo**.

Ejemplo típico: un hash de seguridad que se calcula del lado del cliente y del lado del servidor. Si cualquiera de los dos cambia un detalle, los hashes ya no coinciden y la autenticación se rompe. Forma de coupling muy alta porque cualquier cambio requiere actualizar todas las implementaciones simultáneamente.

### Connascence dinámica (4 tipos)

**Connascence of Execution (CoE)** — El **orden de ejecución** importa.

```
email = newEmail();
email.send();                    ← se envía vacío
email.setRecipient("foo@x.com"); ← demasiado tarde
email.setSubject("whoops");
```

El código compila pero no funciona. Hay que ejecutar las cosas en un orden específico.

**Connascence of Timing (CoT)** — El **momento exacto** de ejecución importa.

El caso clásico es una _race condition_: dos hilos compiten por el mismo recurso y el resultado depende de quién llegue primero. Muy difícil de detectar y reproducir.

**Connascence of Values (CoV)** — Varios valores deben **cambiar juntos**.

Pensá en un rectángulo definido por cuatro puntos. No podés cambiar uno sin pensar en los otros tres. O más grave: en sistemas distribuidos, un valor que debe actualizarse en varias bases de datos a la vez (o no actualizarse en ninguna). Esto entra en territorio de transacciones distribuidas.

**Connascence of Identity (CoI)** — Múltiples componentes deben **referirse a la misma instancia**.

Ejemplo: dos componentes independientes que comparten una cola distribuida. No alcanza con que sean dos colas con el mismo nombre — tienen que ser **la misma cola** en memoria o el comportamiento es indefinido.

### El espectro de fuerza

Page-Jones ordenó todos los tipos según su **fuerza**. Cuanto más fuerte, más difícil de refactorizar y más invasivo el cambio cuando aparece.

<svg viewBox="0 0 720 540" xmlns="http://www.w3.org/2000/svg" style="max-width:100%;height:auto;display:block;margin:1em 0;">
  <rect x="0" y="0" width="720" height="540" fill="#FBF7F0"/>
  <text x="360" y="30" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="16" font-weight="bold" fill="#2A2A2A">Connascence — espectro de fuerza</text>

  <!-- Static -->
  <text x="120" y="65" font-family="Verdana, Tahoma, sans-serif" font-size="13" font-weight="bold" fill="#3D7F4D">Estática (en el código)</text>

  <rect x="120" y="80" width="480" height="38" fill="#D4EDD4" stroke="#3D7F4D" stroke-width="2" rx="4"/>
  <text x="135" y="104" font-family="Verdana, Tahoma, sans-serif" font-size="13" font-weight="bold" fill="#2A2A2A">CoN — Name</text>
  <text x="320" y="104" font-family="Verdana, Tahoma, sans-serif" font-size="12" fill="#2A2A2A">Acordar nombres. La más débil = la mejor.</text>

  <rect x="120" y="124" width="480" height="38" fill="#DCEAD8" stroke="#5C9A5C" stroke-width="2" rx="4"/>
  <text x="135" y="148" font-family="Verdana, Tahoma, sans-serif" font-size="13" font-weight="bold" fill="#2A2A2A">CoT — Type</text>
  <text x="320" y="148" font-family="Verdana, Tahoma, sans-serif" font-size="12" fill="#2A2A2A">Acordar tipos. Bien soportada por compiladores.</text>

  <rect x="120" y="168" width="480" height="38" fill="#E7EFD4" stroke="#7AA856" stroke-width="2" rx="4"/>
  <text x="135" y="192" font-family="Verdana, Tahoma, sans-serif" font-size="13" font-weight="bold" fill="#2A2A2A">CoM — Meaning</text>
  <text x="320" y="192" font-family="Verdana, Tahoma, sans-serif" font-size="12" fill="#2A2A2A">Acordar significado de valores (magic numbers).</text>

  <rect x="120" y="212" width="480" height="38" fill="#F2EED4" stroke="#B5A53D" stroke-width="2" rx="4"/>
  <text x="135" y="236" font-family="Verdana, Tahoma, sans-serif" font-size="13" font-weight="bold" fill="#2A2A2A">CoP — Position</text>
  <text x="320" y="236" font-family="Verdana, Tahoma, sans-serif" font-size="12" fill="#2A2A2A">Acordar orden de parámetros.</text>

  <rect x="120" y="256" width="480" height="38" fill="#F5E6CC" stroke="#C9913D" stroke-width="2" rx="4"/>
  <text x="135" y="280" font-family="Verdana, Tahoma, sans-serif" font-size="13" font-weight="bold" fill="#2A2A2A">CoA — Algorithm</text>
  <text x="320" y="280" font-family="Verdana, Tahoma, sans-serif" font-size="12" fill="#2A2A2A">Acordar implementación de un algoritmo.</text>

  <!-- Divider -->
  <line x1="120" y1="310" x2="600" y2="310" stroke="#5A5A5A" stroke-width="2" stroke-dasharray="6,4"/>
  <text x="360" y="325" text-anchor="middle" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#5A5A5A" font-style="italic">Frontera estática / dinámica</text>

  <!-- Dynamic -->
  <text x="120" y="350" font-family="Verdana, Tahoma, sans-serif" font-size="13" font-weight="bold" fill="#A93D3D">Dinámica (en tiempo de ejecución)</text>

  <rect x="120" y="362" width="480" height="38" fill="#F2D9C9" stroke="#C97B2E" stroke-width="2" rx="4"/>
  <text x="135" y="386" font-family="Verdana, Tahoma, sans-serif" font-size="13" font-weight="bold" fill="#2A2A2A">CoE — Execution</text>
  <text x="320" y="386" font-family="Verdana, Tahoma, sans-serif" font-size="12" fill="#2A2A2A">Orden de ejecución importa.</text>

  <rect x="120" y="406" width="480" height="38" fill="#E8C7B0" stroke="#B05E2D" stroke-width="2" rx="4"/>
  <text x="135" y="430" font-family="Verdana, Tahoma, sans-serif" font-size="13" font-weight="bold" fill="#2A2A2A">CoT — Timing</text>
  <text x="320" y="430" font-family="Verdana, Tahoma, sans-serif" font-size="12" fill="#2A2A2A">Momento exacto importa (race conditions).</text>

  <rect x="120" y="450" width="480" height="38" fill="#EDC7C7" stroke="#A93D3D" stroke-width="2" rx="4"/>
  <text x="135" y="474" font-family="Verdana, Tahoma, sans-serif" font-size="13" font-weight="bold" fill="#2A2A2A">CoV — Values</text>
  <text x="320" y="474" font-family="Verdana, Tahoma, sans-serif" font-size="12" fill="#2A2A2A">Varios valores deben cambiar juntos.</text>

  <rect x="120" y="494" width="480" height="38" fill="#E0A8A8" stroke="#8A2828" stroke-width="2" rx="4"/>
  <text x="135" y="518" font-family="Verdana, Tahoma, sans-serif" font-size="13" font-weight="bold" fill="#2A2A2A">CoI — Identity</text>
  <text x="320" y="518" font-family="Verdana, Tahoma, sans-serif" font-size="12" fill="#2A2A2A">Misma instancia compartida. La más fuerte = la peor.</text>

  <!-- Arrow indicating direction -->
  <text x="640" y="100" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#3D7F4D" font-weight="bold">Débil</text>
  <text x="640" y="520" font-family="Verdana, Tahoma, sans-serif" font-size="11" fill="#A93D3D" font-weight="bold">Fuerte</text>

  <defs>
    <marker id="arr-grad" markerWidth="10" markerHeight="10" refX="5" refY="9" orient="auto">
      <path d="M0,0 L10,0 L5,9 Z" fill="#888"/>
    </marker>
  </defs>
  <line x1="650" y1="115" x2="650" y2="500" stroke="#888" stroke-width="1.5" marker-end="url(#arr-grad)"/>
</svg>

### Tres propiedades para evaluar connascence

No alcanza con saber qué tipo es. Hay tres propiedades que definen su impacto real:

**Fuerza (Strength)** — Qué tan difícil es refactorizar este tipo de coupling. Las estáticas son más débiles, las dinámicas son más fuertes.

**Localidad (Locality)** — Qué tan cerca están los módulos acoplados. Una connascence fuerte dentro del mismo módulo molesta menos que una connascence débil cruzando límites de módulos.

**Grado (Degree)** — Cuántos elementos están afectados. Una connascence de alta fuerza pero baja localidad y bajo grado puede ser tolerable. La misma connascence repetida en 30 lugares es un problema serio.

**La regla práctica:** fuerza y localidad se evalúan **juntas**. Una connascence fuerte concentrada en pocas líneas adyacentes es manejable. La misma connascence dispersa por el sistema es deuda técnica grave.

### Las reglas de Weirich

Jim Weirich popularizó dos reglas operativas para usar connascence en la práctica:

**Regla del Grado:** convertir formas fuertes de connascence en formas débiles.

Ejemplo concreto: si tenés _connascence of meaning_ (un `0` que significa "inactivo"), refactorizá a _connascence of name_ (una constante `ESTADO_INACTIVO`). Es un cambio mecánico y todos los usos siguen funcionando.

**Regla de la Localidad:** cuanto más lejos están los elementos en el código, más débil debería ser la connascence entre ellos.

Dos clases en el mismo módulo pueden compartir una _connascence of meaning_ sin que sea drama. Dos componentes en repos distintos compartiendo lo mismo es una bomba de tiempo.

### Las tres pautas de Page-Jones

Page-Jones propuso tres lineamientos generales para mejorar la modularidad usando connascence:

1. **Minimizar la connascence total** dividiendo el sistema en elementos encapsulados.
2. **Minimizar la connascence que cruza los límites** de la encapsulación.
3. **Maximizar la connascence dentro de un mismo límite** de encapsulación.

Es decir: que la mayor parte del coupling viva adentro de los módulos, no entre ellos.

---

## Unificando los tres conceptos

Coupling y connascence son dos vistas del mismo fenómeno, desarrolladas en épocas distintas.

- Coupling (de los 70/80) sólo mide **dirección**: incoming u outgoing.
- Connascence (de los 90) describe **el tipo y la fuerza** del vínculo.

Una llamada a un método es coupling. Connascence te dice: ¿es CoN (apenas un nombre compartido), CoP (depende del orden de parámetros), CoA (depende de un algoritmo)?

Las dos vistas no compiten: se complementan.

### Limitaciones de connascence en arquitectura moderna

El libro nota dos problemas con connascence vista clásicamente:

1. **Es a nivel de código, no de arquitectura.** Connascence mira detalles finos. Pero los arquitectos suelen importarles más el **cómo** (sincrónico vs asincrónico) que el grado fino del coupling.

2. **No habla de comunicación entre servicios.** Connascence original no trata la decisión clave de arquitecturas distribuidas: sincrónico vs asincrónico, request/response vs eventos.

Los autores anticipan que en capítulos posteriores (sobre quanta arquitectónico y arquitecturas modernas) van a extender estos conceptos.

---

## Resumen práctico

| Concepto      | ¿Qué mide?                       | ¿Cuándo usarlo?                              |
|---------------|----------------------------------|-----------------------------------------------|
| Cohesión      | Qué tan relacionado está el contenido interno de un módulo | Al decidir si un módulo debe partirse o fusionarse |
| LCOM          | Falta de cohesión estructural    | Al analizar código legacy o pre-refactor      |
| Afferent      | Cuántos dependen de mí           | Al ver qué módulos son críticos del sistema   |
| Efferent      | De cuántos dependo               | Al ver qué módulos son frágiles               |
| Abstractness  | Ratio de abstracción/implementación | Al evaluar si el código está sobreingenierizado |
| Instability   | Qué tan volátil es el módulo     | Al planificar dónde poner las abstracciones   |
| Distance from main sequence | Salud arquitectónica global | Al revisar código existente para refactor |
| Connascence estática | Tipo y fuerza del coupling visible | Al diseñar y al refactorizar |
| Connascence dinámica | Coupling de runtime | Al diseñar sistemas concurrentes o distribuidos |
| Fuerza · Localidad · Grado | Impacto real de cada connascence | Al priorizar deuda técnica |

### Las cuatro reglas operativas

1. **Maximizar cohesión** — cosas que cambian juntas viven juntas.
2. **Minimizar coupling** — cosas que cambian por razones distintas viven separadas.
3. **Convertir connascence fuerte en débil** — refactor mecánico cuando sea posible.
4. **Mantener connascence fuerte local** — si no se puede debilitar, al menos contenerla.

---

## De módulos a componentes

El capítulo cierra anticipando que **componentes** (no sólo módulos) son las unidades reales con las que trabaja el arquitecto. Pero antes de discutir cómo derivar componentes de un dominio de problema (Capítulo 8), hace falta hablar de algo más fundamental: las **características arquitectónicas** y su alcance.

Eso es lo que viene en los próximos capítulos.

---

## Apuntes propios

> Espacio para tus notas. Usá `> ` para citas tuyas, **[NOTA]** para apuntes, o agregá secciones nuevas debajo.

**[NOTA]**
