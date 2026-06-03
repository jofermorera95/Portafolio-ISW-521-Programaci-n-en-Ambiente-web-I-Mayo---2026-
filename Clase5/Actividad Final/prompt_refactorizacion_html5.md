# Prompt Técnico de Refactorización — Feria Nacional de Innovación Estudiantil
## Fase 2: Ingeniería de Prompts

---

> **Instrucciones de uso:** Copie todo el contenido del bloque de prompt a continuación y péguelo directamente en el chat de la IA de su preferencia (Claude, ChatGPT, Gemini, etc.), seguido del código HTML del archivo `v1_base_feria_innovacion.html`.

---

## ──────────────────────────────────────────
## PROMPT TÉCNICO — INICIO
## ──────────────────────────────────────────

---

### SECCIÓN A — ROL Y CONTEXTO

Actúa como un Desarrollador Frontend Senior especializado en estándares W3C con 10 años de experiencia en arquitectura de documentos HTML5, semántica web, accesibilidad WCAG 2.1 y optimización de rendimiento según las métricas Core Web Vitals de Google. Has realizado auditorías de código para organismos públicos e instituciones académicas y conoces con precisión el modelo de contenido HTML5, las implicaciones de SEO de la estructura semántica, y los criterios de validación del W3C Markup Validation Service.

Tu tarea es refactorizar completamente el código HTML deficiente que se te proporciona a continuación. El resultado debe ser un documento HTML5 moderno, semántico, accesible y seguro, listo para despliegue en producción.

---

### SECCIÓN B — DESCRIPCIÓN DEL PROBLEMA

Recibirás el código fuente de `v1_base_feria_innovacion.html`, la versión 1 del sitio oficial de la **Feria Nacional de Innovación Estudiantil 2025** del ITCR. Este código fue generado por un desarrollador sin criterio técnico y presenta los siguientes problemas estructurales graves, todos documentados en una auditoría formal previa:

1. **Div-Soup generalizado:** La totalidad de la estructura del documento está construida con `<div>`, sin ninguna etiqueta semántica HTML5 (`<header>`, `<nav>`, `<main>`, `<footer>`, `<section>`, `<article>`, `<aside>`). Esto destruye la jerarquía semántica, perjudica el posicionamiento SEO e impide que los motores de búsqueda y tecnologías asistivas interpreten correctamente el documento.

2. **Formulario sin estructura semántica ni accesibilidad:** Ningún campo del formulario de inscripción tiene una etiqueta `<label>` correctamente vinculada mediante `for`/`id`. Los campos descriptivos son simples `<div>` con texto. No existen agrupaciones con `<fieldset>` y `<legend>`. El campo de carrera usa un `<select>` cerrado cuando debería ser un `<datalist>` que permita entrada libre. Los campos de email y teléfono usan `type="text"` en lugar de sus tipos semánticos correctos. Esto viola WCAG 2.1 nivel A.

3. **Tabla de agenda incompleta:** La tabla carece de las secciones estructurales `<thead>`, `<tbody>` y `<tfoot>`. No existe elemento `<caption>`. Los encabezados `<th>` no tienen atributo `scope`, lo que los hace ininterpretables para tecnología asistiva.

4. **Elemento `<video>` inutilizable:** El video no tiene atributo `poster` (causando Cumulative Layout Shift penalizado en Core Web Vitals), carece del atributo `controls` (el usuario no puede reproducirlo ni controlarlo), y tiene una única fuente en formato MP4 sin alternativas de formato para compatibilidad cruzada.

5. **`<iframe>` inseguro y con mal rendimiento:** El iframe del mapa de Google Maps no tiene atributo `loading="lazy"` (se carga en el arranque aunque el usuario no llegue a verlo), carece de atributo `sandbox` (el contenido embebido tiene acceso irrestricto al contexto de la página padre, representando un riesgo de seguridad), y no tiene atributo `title` (inaccesible para lectores de pantalla).

6. **Anidamiento incorrecto del modelo de contenido HTML5:** En la sección de contacto, elementos de bloque (`<div>`, `<h3>`) están anidados dentro de `<span>`, un elemento de línea. También hay un `<div>` anidado dentro de un `<a>`. Esto viola el modelo de contenido HTML5 y produce un DOM reparado automáticamente por el navegador de forma impredecible, causando comportamientos de layout diferentes entre motores de renderizado.

---

### SECCIÓN C — RESTRICCIONES TÉCNICAS EXPLÍCITAS

Debes cumplir TODAS las siguientes restricciones sin excepción. No se acepta cumplimiento parcial.

**C.1 — Estructura semántica del documento (Div-Soup)**
- DEBES reemplazar el `<div class="encabezado">` por `<header>`.
- DEBES reemplazar el `<div class="menu">` por `<nav>` con un elemento `<ul>` interno donde cada enlace esté dentro de un `<li>`.
- DEBES reemplazar el `<div class="contenido">` por `<main>`.
- DEBES reemplazar el `<div class="pie">` por `<footer>`.
- DEBES envolver cada sección temática del contenido principal (`#inicio`, `#proyectos`, `#agenda`, `#inscripcion`, `#sede`, contacto) dentro de un elemento `<section>` con su atributo `id` correspondiente.
- DEBES envolver cada tarjeta de proyecto finalista (EcoSensor Pro, BioFiltro Urbano, AgroBot CR) dentro de un elemento `<article>`, ya que cada proyecto es contenido autónomo y reutilizable.
- NO DEBE quedar ningún `<div>` en posición donde corresponde una etiqueta semántica estructural. Los `<div>` solo son válidos para agrupaciones sin significado semántico propio (por ejemplo, wrappers de layout puramente presentacional).
- DEBES añadir `lang="es"` al elemento `<html>` de apertura.
- DEBES añadir una etiqueta `<meta name="description">` en el `<head>` con una descripción representativa del evento.

**C.2 — Formulario de inscripción**
- DEBES añadir una etiqueta `<label>` para CADA campo del formulario, con atributo `for` cuyo valor coincida exactamente con el atributo `id` del campo correspondiente.
- DEBES asignar un atributo `id` único y un atributo `name` descriptivo a CADA campo `<input>`, `<select>`, `<textarea>` y `<datalist>` del formulario.
- DEBES agrupar los campos del formulario en exactamente dos `<fieldset>`:
  - **Primero:** "Datos del Equipo" — debe contener: nombre del proyecto, nombre del responsable, correo, teléfono, institución y carrera.
  - **Segundo:** "Detalles del Proyecto" — debe contener: categoría, descripción del proyecto, requerimientos especiales y las casillas de verificación.
  - Cada `<fieldset>` debe tener su `<legend>` como primer elemento hijo.
- DEBES cambiar el `<select>` del campo "Carrera o programa académico" por un `<input type="text">` vinculado a un `<datalist>` que contenga exactamente las mismas opciones que tenía el `<select>` original, excepto la opción placeholder "Seleccione su carrera".
- DEBES cambiar el campo de correo de `type="text"` a `type="email"`.
- DEBES cambiar el campo de teléfono de `type="text"` a `type="tel"`.
- DEBES añadir el atributo `required` a los campos obligatorios: nombre del equipo, nombre del responsable, correo, institución, carrera, categoría y descripción del proyecto.
- DEBES vincular cada `<input type="checkbox">` a su propio `<label>` con atributos `for`/`id` correctos.

**C.3 — Tabla de agenda**
- DEBES añadir un elemento `<caption>` como primer hijo de `<table>`, con el texto: *"Agenda del evento — Feria Nacional de Innovación Estudiantil 2025, ITCR Campus Cartago"*.
- DEBES envolver la fila de encabezados (Hora, Miércoles 22 Oct, Jueves 23 Oct, Viernes 24 Oct) dentro de un elemento `<thead>`.
- DEBES envolver todas las filas de datos del horario (de 7:30 a 15:30-17:00) dentro de un elemento `<tbody>`.
- DEBES envolver la fila de "Total horas" dentro de un elemento `<tfoot>`.
- DEBES añadir el atributo `scope="col"` a los cuatro `<th>` de la fila de encabezados.
- El `<th>` de la columna "Hora" en cada fila de datos debe convertirse en un `<th scope="row">` si se añade encabezado de fila, o mantenerse como `<td>` si la fila no requiere encabezado semántico propio.

**C.4 — Elemento `<video>`**
- DEBES añadir el atributo `poster="img/poster_feria.webp"` al elemento `<video>`.
- DEBES añadir el atributo `controls` al elemento `<video>`.
- DEBES añadir un segundo `<source>` con `src="video/feria_presentacion.webm"` y `type="video/webm"`, colocado ANTES del source MP4 existente (el navegador selecciona el primero que puede reproducir; WebM ofrece mejor compresión).
- DEBES mantener el `<source>` MP4 original como segunda opción de fallback.
- DEBES añadir un texto de fallback dentro del `<video>` (después de los `<source>`) para navegadores que no soporten HTML5 video: un párrafo con un enlace de descarga directa del archivo MP4.

**C.5 — Elemento `<iframe>` del mapa**
- DEBES añadir `loading="lazy"` al `<iframe>`.
- DEBES añadir `sandbox="allow-scripts allow-same-origin allow-popups"` al `<iframe>`.
- DEBES añadir `title="Mapa de ubicación del ITCR Campus Central Cartago, Costa Rica"` al `<iframe>`.
- DEBES mantener el atributo `allowfullscreen` existente.

**C.6 — Corrección de anidamiento incorrecto**
- DEBES corregir la sección de contacto donde `<div>` y `<h3>` están anidados dentro de `<span>`. Reemplaza el `<span>` contenedor por un elemento de bloque apropiado (`<div>` o `<address>`).
- DEBES corregir el enlace al portal del ITCR donde un `<div>` está anidado dentro de `<a>`. Mantén el `<a>` como enlace pero estructura su contenido sin violar el modelo de contenido (puedes usar `<a>` como elemento de bloque en HTML5 si no contiene elementos interactivos, o reestructurar el contenido interno con elementos de línea).
- NO DEBE existir ningún elemento de bloque (`<div>`, `<p>`, `<h1>`–`<h6>`, `<ul>`, `<ol>`, `<table>`, etc.) anidado dentro de un elemento de línea puro (`<span>`, `<em>`, `<strong>`, `<small>`, `<cite>`, etc.).

**C.7 — Reglas generales de calidad**
- NO DEBES alterar el contenido textual visible del documento (nombres, fechas, descripciones, links). Solo modifica la estructura HTML.
- NO DEBES eliminar ni añadir estilos CSS existentes. Puedes actualizar únicamente los selectores CSS que sean necesarios para reflejar los cambios de etiquetas (por ejemplo, si `<div class="encabezado">` pasa a `<header>`, el selector CSS puede adaptarse).
- DEBES mantener todos los atributos `id` de ancla existentes para que los enlaces de navegación (`#inicio`, `#agenda`, etc.) sigan funcionando.
- El documento refactorizado DEBE pasar la validación del W3C Markup Validation Service sin errores de estructura o modelo de contenido.

---

### SECCIÓN D — CRITERIOS DE VALIDACIÓN INTERNA

Antes de producir tu respuesta final, realiza internamente la siguiente lista de verificación. Solo entrega el resultado si TODOS los puntos están satisfechos:

**D.1 Semántica estructural**
- [ ] El documento tiene `<html lang="es">`, `<header>`, `<nav>`, `<main>`, `<footer>` correctamente ubicados.
- [ ] Dentro de `<main>`, cada sección usa `<section>` con su `id`.
- [ ] Cada proyecto finalista está dentro de un `<article>`.
- [ ] No existe ningún `<div class="encabezado">`, `<div class="menu">`, `<div class="contenido">` ni `<div class="pie">` en el resultado.

**D.2 Formulario**
- [ ] Cada `<input>`, `<select>`, `<textarea>` e `<input list>` tiene un `id` único.
- [ ] Cada `id` de campo tiene exactamente un `<label for="...">` que lo referencia.
- [ ] El formulario tiene exactamente dos `<fieldset>` con `<legend>` como primer hijo de cada uno.
- [ ] El campo de carrera es `<input type="text" list="...">` + `<datalist id="...">` con las opciones.
- [ ] El correo usa `type="email"` y el teléfono usa `type="tel"`.
- [ ] Los campos obligatorios tienen el atributo `required`.

**D.3 Tabla**
- [ ] La tabla tiene `<caption>` como primer hijo.
- [ ] La tabla tiene `<thead>`, `<tbody>` y `<tfoot>`.
- [ ] Los cuatro `<th>` del encabezado tienen `scope="col"`.

**D.4 Video**
- [ ] El `<video>` tiene los atributos `poster`, `controls` y `width`.
- [ ] El `<video>` tiene al menos dos `<source>`: WebM primero, MP4 segundo.
- [ ] El `<video>` tiene texto de fallback con enlace de descarga.

**D.5 iFrame**
- [ ] El `<iframe>` tiene `loading="lazy"`.
- [ ] El `<iframe>` tiene `sandbox="allow-scripts allow-same-origin allow-popups"`.
- [ ] El `<iframe>` tiene `title` descriptivo.

**D.6 Anidamiento**
- [ ] No existe ningún `<div>` o `<h*>` anidado dentro de `<span>`.
- [ ] No existe ningún elemento de bloque dentro de un elemento de línea puro.

---

### SECCIÓN E — FORMATO DEL RESULTADO

Entrega ÚNICAMENTE el bloque de código HTML completo, refactorizado según todas las restricciones anteriores.

- El resultado debe comenzar exactamente con `<!DOCTYPE html>` y terminar exactamente con `</html>`.
- NO incluyas ninguna explicación, comentario introductorio, lista de cambios realizados, ni texto fuera del bloque de código.
- NO uses bloques de código Markdown con triple backtick (` ``` `). Entrega el HTML directamente como texto plano.
- El archivo resultante debe poder guardarse directamente con el nombre `v2_feria_innovacion.html` y abrirse en un navegador sin errores de consola relacionados con la estructura del documento.
- Los comentarios HTML internos (`<!-- -->`) son opcionales pero bienvenidos si ayudan a identificar las secciones principales del documento refactorizado.

---

### CÓDIGO FUENTE A REFACTORIZAR:

[PEGAR AQUÍ EL CONTENIDO COMPLETO DE v1_base_feria_innovacion.html]

## ──────────────────────────────────────────
## PROMPT TÉCNICO — FIN
## ──────────────────────────────────────────

---

## Notas de uso para el equipo Tech Lead

**¿Por qué este prompt produce mejores resultados que uno genérico?**

Un prompt como "arregla este HTML" delega todas las decisiones técnicas a la IA sin criterio de validación. Este prompt actúa como un **pliego de especificaciones técnicas**: define el rol del agente, describe el problema con precisión, enumera restricciones no negociables, exige autoverificación antes de responder, y prescribe el formato exacto del entregable.

La Sección D (criterios de validación interna) es especialmente poderosa porque obliga a la IA a ejecutar un proceso de revisión antes de responder, reduciendo drásticamente la tasa de errores por omisión.

**Compatibilidad:** Este prompt ha sido diseñado para funcionar con Claude 3.5+, GPT-4o, Gemini 1.5 Pro y modelos equivalentes. Para modelos más pequeños o de menor contexto, puede ser necesario dividir las secciones B y C en llamadas separadas.
