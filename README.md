# PeerResponse

Respuesta a revisores con siete agentes de IA y comprobación por código. Aplicación web de un solo fichero (`index.html`), sin servidor.

**Usar la app:** https://fborrasumh.github.io/peerresponse_ai/

**Idiomas:** español (por defecto), inglés y portugués; selector en la barra superior (o `?lang=en` / `?lang=pt` en la URL). La carta de respuesta se escribe en el idioma que elijas o en el de la revisión.

## Qué hace

1. Lees o pegas el **manuscrito** y la **revisión** (PDF, Word o texto; el texto extraído es editable).
2. Siete agentes trabajan en cadena (los seis primeros usan IA; el séptimo es código):
   - **Analizador**: separa cada comentario de la revisión. Para cada uno da una cita literal (principio y final).
   - **Clasificador**: propone aceptar, aceptar en parte o discrepar, y palabras clave para buscar bibliografía.
   - **Bibliográfico**: busca en Semantic Scholar, PubMed, OpenAlex, arXiv, Crossref y Europe PMC.
   - **Estratega**: plan, referencias a citar y cambio previsto por comentario.
   - **Redactor**: la respuesta de cada comentario, la introducción y el cierre de la carta.
   - **Revisor del manuscrito**: cambios concretos (texto original → texto nuevo).
   - **Control de calidad (código)**: reintenta respuestas que faltan y resume lo que no se pudo comprobar.
3. **Editas** todo: estrategia, texto de cada respuesta, textos nuevos del manuscrito, qué cambios se aplican. Los avisos se recalculan mientras escribes.
4. **Descargas**: carta (Word), manuscrito con cambios marcados (Word), manuscrito limpio (Word), CSV de cambios, ZIP y proyecto reproducible (JSON).

## Qué comprueba el código (y qué no)

| Lo comprueba el código | Cómo |
|---|---|
| Cada comentario existe en la revisión | La cita que da la IA debe aparecer literalmente (sin contar mayúsculas, tildes ni puntuación). El texto completo del comentario se **extrae de la revisión**, no lo escribe la IA. Si no aparece, se marca ⚠. |
| Cobertura de la revisión | Porcentaje de palabras de la revisión cubiertas por comentarios localizados; si es bajo, avisa de que puede faltar alguno (y se puede añadir a mano). |
| Referencias reales | Salen de las bases de datos, no de la IA. Cada DOI se consulta en Crossref y el título debe coincidir; los que no coinciden se descartan. |
| Citas `[N]` de la carta | Toda `[N]` debe existir en la lista; las referencias se renumeran por orden de aparición. |
| Cambios al manuscrito | Solo se aplican los que se localizan literalmente en tu manuscrito. El texto revisado se monta por código sobre el original: el resto no se reescribe. Los solapados no se aplican. |
| Cifras nuevas | Las cifras que aparecen en una respuesta y no están en el manuscrito ni en la revisión se señalan. |
| Datos que faltan | La IA tiene prohibido inventar datos o análisis: deja `[CONFIRMAR: …]` (resaltado en amarillo en Word). Antes de entregar se cuentan los pendientes. |

**No comprueba** que una referencia sea pertinente para el argumento (que exista no significa que sirva), ni que una respuesta sea científicamente correcta, ni que los cambios propuestos sean adecuados. Esa decisión es de los autores.

## Cómo se usa la IA

Con tu propia clave de **OpenAI**, **Google Gemini** o **Anthropic Claude**, o con un **servidor local** (Ollama o LM Studio, en `localhost`). La clave se guarda solo en el navegador. Cada persona paga su propio uso; la pantalla de contexto muestra una estimación orientativa y permite probar con solo los 3 primeros comentarios.

**IA local:** elige «Local (Ollama / LM Studio)» en el botón «IA». El servidor debe permitir el origen de la página (Ollama: variable `OLLAMA_ORIGINS`; LM Studio: activar CORS). Por la política de seguridad de la página, solo se admiten direcciones `localhost` / `127.0.0.1`. Los modelos pequeños tienen poco contexto y fallan más al devolver JSON: la app usa lotes más pequeños, reintenta y marca lo que no puede comprobar, pero la calidad será menor.

## Privacidad

- Documentos y resultados se guardan **solo en el navegador** (IndexedDB).
- Al proveedor de IA va la revisión completa (por partes) y, del manuscrito, **solo los pasajes relacionados con cada comentario**, nunca el texto entero. Correos, DNI/NIE y teléfonos se enmascaran. Antes del primer envío se muestra una muestra y hay que confirmar.
- Con IA local en este equipo no se enmascara ni se pide confirmación, porque no sale nada del ordenador.
- A las bases bibliográficas solo van palabras clave de búsqueda (generadas por la IA a partir de la revisión), nunca tu texto.
- Los nombres propios no se enmascaran: si no quieres que lleguen al proveedor, anonimiza el texto antes.

## Límites

- No hay OCR: un PDF escaneado hay que pegarlo como texto. En PDF de dos columnas el orden del texto puede mezclarse; revísalo en el paso 1.
- Los pasajes del manuscrito se buscan por coincidencia de palabras: si la revisión y el manuscrito están en idiomas distintos, se localiza peor.
- Los comentarios los detecta la IA: puede omitir alguno (el control de cobertura lo señala) o fusionar dos.
- Algunas bases pueden no responder desde el navegador (por ejemplo por CORS); la app lo indica ("respondieron N de 6").
- La IA puede equivocarse: **lo que se envía al editor es responsabilidad de los autores**. Lee la carta entera antes de enviarla.

## Qué se probó

Pruebas automáticas con navegador (Chromium) y proveedores simulados: ejemplo en los tres idiomas, aviso previo y enmascarado, IA que miente (cita inventada, comentario duplicado, referencia inexistente, cifra inventada, respuesta omitida, cambio no localizable, JSON roto, error 429), reanudación, comentario manual, persistencia, descargas (Word, CSV, ZIP, JSON), IA local simulada y móvil de 390 px. **No se probó con claves reales de proveedores ni con un servidor Ollama/LM Studio real**, ni con PDF reales de revistas.

## Autoría

Fernando Borrás Rocher (Universidad Miguel Hernández de Elche).

ORCID: Fernando Borrás Rocher [0000-0002-5519-4573](https://orcid.org/0000-0002-5519-4573)

## Cómo citar

Borrás Rocher, F. (2026). *PeerResponse* (v2.0.0) [Software]. (DOI en trámite)

## Licencia

MIT. Véase [LICENSE](LICENSE).
