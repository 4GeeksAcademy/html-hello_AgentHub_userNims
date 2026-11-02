# AgentHub - Especificación de interfaz

## Ambigüedades del briefing

Los archivos disponibles describen una plantilla inicial de HTML/CSS, pero no incluyen requisitos, capturas ni contenido de producto para AgentHub. Por ello, no se puede confirmar el público objetivo, el significado exacto de «AgentHub», los nombres de las seis vistas, las acciones permitidas ni una identidad visual existente.

## Supuestos explícitos para esta especificación

- AgentHub es una interfaz de demostración para consultar agentes de IA, sus skills, ejecuciones recientes y fichas de catálogo. No administra agentes reales ni se conecta a servicios externos.
- Las seis vistas son **Resumen**, **Agentes**, **Skills**, **Actividad**, **Catálogo** y **Ajustes**. Se pueden cambiar por los nombres del briefing definitivo sin alterar el alcance descrito.
- Los datos son ficticios, estáticos y coherentes entre vistas. No hay autenticación, backend, formularios de alta funcionales ni persistencia de datos.
- El único estado que se conserva entre recargas es la preferencia de modo claro/oscuro, mediante `localStorage`.
- El idioma de la interfaz es español. «Skill» se conserva como término del producto.

## Stack y límites

- HTML semántico, Tailwind CSS v3 mediante CDN y JavaScript vanilla.
- Sin frameworks, jQuery, proceso de build, backend, CSS personalizado ni atributos `style`.
- Usar utilidades de Tailwind para estructura, espaciado, tipografía, colores, estados y responsive. El modo oscuro usa las variantes `dark:` de Tailwind.
- La interfaz debe funcionar como una página cliente con datos locales de demostración. No simular una conexión real ni mostrar estados de carga de servicios inexistentes.
- Usar controles HTML nativos cuando corresponda; los controles interactivos deben tener nombre accesible, foco visible y estados perceptibles.

## Contenedor exterior

- **Componente:** una aplicación de una sola página con navegación lateral y una región principal; ambas ocupan el alto disponible y no se presentan como tarjetas anidadas.
- **Contenido:** el contenedor principal incluye navegación, header global y el contenido de exactamente una de las seis vistas activas.
- **Comportamiento:** cambiar de vista actualiza el contenido principal y el estado activo de navegación sin recargar la página; la vista inicial es **Resumen**.

## Navegación

- **Componente:** navegación lateral semántica con marca «AgentHub», seis enlaces de vista e indicador visual y accesible de la vista actual.
- **Contenido:** los enlaces se muestran en este orden: Resumen, Agentes, Skills, Actividad, Catálogo y Ajustes. Cada enlace tiene texto visible, además de cualquier icono.
- **Comportamiento:** activar un enlace cambia la vista; en tablet la navegación se reduce a una barra compacta con controles identificables y no tapa el contenido; los enlaces funcionan con teclado.

## Header global

- **Componente:** header de la región principal con título de la vista actual, breadcrumb «AgentHub», menú desplegable de cuenta de demostración y control de tema.
- **Contenido:** mostrar la cuenta ficticia «Equipo Demo» y el título correspondiente a la vista. No presentar notificaciones, perfil real ni datos de sesión.
- **Comportamiento:** el menú de cuenta se abre/cierra mediante su botón; el control de tema conmuta claro/oscuro y conserva la selección en `localStorage`.

## Vistas

### 1. Resumen

- **Componente:** encabezado de vista, tres indicadores compactos y dos regiones de información: agentes destacados y actividad reciente.
- **Contenido:** mostrar 4 agentes registrados, 3 activos, 5 skills disponibles y 128 ejecuciones completadas en las últimas 24 horas. Incluir Atlas, Nara y Lumen entre los agentes destacados y eventos presentes en Actividad.
- **Comportamiento:** seleccionar un agente destacado abre su ficha de solo lectura; «Ver actividad» navega a Actividad. Los indicadores no son controles.

### 2. Agentes

- **Componente:** lista adaptable de cuatro agentes, con nombre, descripción breve, estado, skills asociadas y acción de consulta de ficha; incluir un desplegable de filtro de estado.
- **Contenido:** Atlas (activo; Investigación), Nara (activo; Resumen), Lumen (pausado; Análisis CSV) y Senda (activo; Búsqueda web). El total y estados deben coincidir con Resumen.
- **Comportamiento:** el filtro permite mostrar todos, activos o pausados; la acción de ficha abre un modal de solo lectura. No añadir, editar, iniciar ni eliminar agentes.

### 3. Skills

- **Componente:** lista de cinco skills mediante secciones colapsables; cada encabezado expone nombre, categoría y estado expandido/contraído.
- **Contenido:** Búsqueda web, Resumen, Análisis CSV, Clasificación de texto y Extracción de entidades. Indicar de forma ficticia qué agentes usan cada skill, coherente con la vista Agentes.
- **Comportamiento:** activar el encabezado expande o contrae su detalle; el estado se comunica con `aria-expanded` y el detalle se relaciona con el encabezado. La apertura no cambia datos ni navega fuera de la vista.

### 4. Actividad

- **Componente:** lista cronológica o tabla adaptable de ejecuciones con fecha/hora, agente, skill y resultado; incluir un desplegable para filtrar por resultado.
- **Contenido:** usar al menos cuatro eventos ficticios con agentes y skills existentes. Mostrar estados «Completada» y «Fallida» y fechas relativas claras; el resumen puede indicar 128 completadas en 24 horas sin limitar la lista a esas cuatro.
- **Comportamiento:** el filtro muestra todos, completados o fallidos; activar una fila abre un modal de solo lectura con el mismo evento. No permitir reintentar, cancelar ni borrar ejecuciones.

### 5. Catálogo

- **Componente:** cuadrícula adaptable de tres fichas de plantillas de demostración, cada una con nombre, descripción, tipo y acción «Ver ficha».
- **Contenido:** incluir plantillas ficticias «Analista de investigación», «Asistente de resúmenes» y «Revisor de datos CSV»; describir las skills asociadas usando los nombres de la vista Skills.
- **Comportamiento:** «Ver ficha» abre un modal informativo; no hay instalación, compra, publicación, búsqueda remota ni alta de agentes desde el catálogo.

### 6. Ajustes

- **Componente:** sección sencilla de preferencias de interfaz con control de tema claro/oscuro y estado actual legible.
- **Contenido:** explicar únicamente que el tema afecta a la interfaz de AgentHub; no incluir credenciales, facturación, permisos, integraciones ni configuración de servicios.
- **Comportamiento:** el control ofrece claro y oscuro, actualiza inmediatamente toda la interfaz y mantiene la selección tras recargar. El control del header y el de esta vista siempre reflejan el mismo estado.

## Responsive: desktop y tablet

- **Desktop (desde 1024 px):** navegación lateral persistente; área principal fluida con ancho de lectura controlado; listas y catálogos usan el espacio disponible sin estirar excesivamente el texto.
- **Tablet (768–1023 px):** navegación compacta, header con controles sin solaparse y contenido en una columna cuando dos columnas no permitan lectura cómoda.
- **Ambos tamaños:** no debe existir desplazamiento horizontal de página; tablas o listas densas deben reorganizar columnas como filas etiquetadas o permitir desplazamiento dentro de su propia región. Modales permanecen dentro del viewport.
- **Teclado y tacto:** objetivos de interacción con área suficiente, foco visible y orden de tabulación lógico; los estados no dependen exclusivamente del color.

## Datos ficticios de referencia

| Tipo | Datos | Regla de coherencia |
| --- | --- | --- |
| Agentes | Atlas (activo), Nara (activo), Lumen (pausado), Senda (activo) | 4 registrados; 3 activos y 1 pausado |
| Skills | Búsqueda web, Resumen, Análisis CSV, Clasificación de texto, Extracción de entidades | 5 en total; las asociaciones referencian agentes existentes |
| Actividad | Al menos 4 eventos recientes, completados o fallidos | Agentes y skills deben existir en sus vistas; Resumen puede agregar 128 ejecuciones completadas en 24 horas |
| Catálogo | Analista de investigación, Asistente de resúmenes, Revisor de datos CSV | Las fichas describen combinaciones de skills existentes; no se convierten en agentes |
| Cuenta | Equipo Demo | Etiqueta ilustrativa, sin sesión ni identidad real |

## Inventario de componentes

- **Estructura:** aplicación, navegación lateral, header, región principal y encabezado de vista.
- **Navegación y estado:** enlace de vista, indicador de vista actual, breadcrumb y control de tema compartido.
- **Datos:** indicador de resumen, fila/tarjeta de agente, ficha de catálogo, fila de actividad y sección colapsable de skill.
- **Controles:** botón, desplegable de cuenta, desplegable de filtro, conmutador de tema y acción de abrir ficha.
- **Superposición:** modal informativo con título, contenido, botón de cierre y fondo superpuesto.
- Reutilizar componentes visuales entre vistas; no introducir componentes que representen operaciones fuera del alcance de demostración.

## Criterios de aceptación verificables

1. **Vistas:** la navegación muestra exactamente seis vistas; cada enlace activa solo su vista correspondiente y actualiza el indicador de navegación y el título del header.
2. **Desplegables:** cada desplegable se abre y cierra con su botón, cierra al pulsar `Escape` o al hacer clic fuera, y no queda abierto al cambiar de vista. El botón expone su estado con `aria-expanded`.
3. **Modal:** las acciones de ficha indicadas abren un modal con información del elemento seleccionado; el modal se cierra con su botón, `Escape` y clic en el fondo, mantiene el foco dentro mientras está abierto y devuelve el foco al control que lo abrió al cerrarse.
4. **Skills colapsables:** cada encabezado expande y contrae su detalle al activarlo; `aria-expanded` coincide con el estado visual y el panel se puede recorrer con teclado. No se pierde el estado de otros encabezados al alternarlos.
5. **Modo oscuro:** el control del header y el de Ajustes cambian entre claro y oscuro, ambos muestran el mismo estado, el contraste del texto sigue siendo legible y la elección se restaura tras recargar la página.
6. **Responsive:** a 1024 px o más se presenta navegación lateral persistente; entre 768 y 1023 px se presenta navegación compacta; en ambos rangos no hay solapamientos ni desplazamiento horizontal de página.
7. **Coherencia de datos:** Resumen muestra 4 agentes, 3 activos y 5 skills; nombres, estados, skills y relaciones de las demás vistas coinciden con la tabla de datos de referencia.
8. **Alcance de demostración:** ninguna acción crea, modifica, elimina, instala, ejecuta o sincroniza datos; las únicas persistencias son la selección local del tema y el contenido ficticio estático.
9. **Restricciones técnicas:** la interfaz se implementa con HTML semántico, Tailwind CSS v3 por CDN y JavaScript vanilla; no contiene frameworks, jQuery, build, backend, CSS personalizado ni atributos `style`.
