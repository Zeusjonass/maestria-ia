# Entrega RAG — pegar esto en el Doc

---

Prototipo de contratos con RAG en AWS (usando S3 como base de datos vectorial)

El presente documento describe el proyecto: qué hace la app, cómo está armado el RAG, con qué se alimenta, y cómo se relaciona con lo que pide el curso. Recientemente anunciaron que en S3 ahora es posible crear bases de datos vectoriales (Amazon S3 Vectors + Amazon Bedrock). Me pareció buena idea experimentar con esa herramienta, aprovechando igualmente el resto del ecosistema en la nube.

El enunciado del curso plantea el mismo ciclo en local: una interfaz (Streamlit), una API (FastAPI con `/health`, `/ingest` y `/query`), un índice persistente (Chroma) y Google AI para embeber y para generar con Gemini. Aquí el ciclo no cambió —incrustar, indexar, recuperar los más cercanos y recién entonces generar—. Cambió el lugar donde vive cada capa.

MiCasa es un prototipo para armar borradores de contratos de vivienda en Yucatán: arrendamiento (renta) o compraventa. La idea sería armar contratos en general; ahora se empieza con este recorte para hacer específico el alcance. Hablas con un agente. El sistema arma o actualiza el documento con lo que vas pidiendo, y también se puede modificar a mano cualquier fragmento, como en Word o Google Docs. Se revisa y se baja en Word o PDF. No es un despacho ni sustituye a un abogado. El texto sale como borrador informativo.

Un modelo grande “sabe” español y algo de derecho mexicano, pero no tiene el Código Civil de Yucatán como fuente de verdad, mezcla el federal con el estatal y a veces cita al revés. El RAG sirve para anclarlo a textos que nosotros subimos: primero busca trozos cercanos a la pregunta, y después el modelo explica o redacta con eso, no de oído. La base vectorial no “entiende” contratos; calcula qué fragmentos se parecen en significado a lo que escribiste. En este prototipo pide los 5 más cercanos (k = 5) y el modelo trabaja con eso.

Hay dos usos, y los dos tocan el RAG, aunque de distinta forma. En el **documento**, dentro de un proyecto, le pides que cree o cambie un borrador: ahí sí se modifica el contrato. Las cláusulas de siempre (comparecencia, renta, depósito, firmas) no las inventa el índice; están en plantillas JSON. El RAG entra cuando hace falta ley, sobre todo si pides un pacto que no está en el catálogo. En el **asistente legal** (el círculo flotante) haces preguntas libres —depósito, desalojo, quién repara— y eso no cambia ningún borrador: solo responde con lo recuperado y muestra de qué archivo salió, con el score de similitud. Las evidencias de este reporte son de esa segunda puerta, porque es la que más se parece a `POST /query`.

La interfaz es React (Vite en la laptop, Amplify en la nube): es el papel de Streamlit. El usuario pregunta y ve la respuesta; la pantalla nunca habla con Bedrock ni con S3, solo con una API HTTP. Esa API es API Gateway más una Lambda (`micasa-chat`), el papel de FastAPI. Expone `GET /health`, `POST /query` (buscar y generar) y `POST /ingest`. El ingest de verdad no ocurre en ese POST: se sube el PDF al bucket y se lanza Sync en la Knowledge Base, que es cuando se vuelven a calcular embeddings. El índice persistente —el papel de Chroma— es la Knowledge Base `Q1UNTTTE8X` sobre `s3://micasa-kb-source/`. Si se reinicia la Lambda, el índice sigue ahí. Los embeddings —el papel de Google AI al vectorizar— los calcula Bedrock al sincronizar, para que pregunta y documentos vivan en el mismo espacio. La generación —el papel de Gemini— es Converse, después de Retrieve, con Kimi K3 y Claude 3.5 Haiku de respaldo. En git va `.env.example` con la URL del API (`VITE_API_URL`); no hay `GOOGLE_API_KEY` porque en AWS eso va por IAM. Región `us-east-1`. Login del prototipo: `admin` / `Admin2026!`. App: https://main.d1hsj6zus1f46.amplifyapp.com. API: `https://j9zrekl9g4.execute-api.us-east-1.amazonaws.com`. El código está en `proyecto_final/`.

El mismo cableado serviría después con otro código, otro estado u otro tipo de documento. Cambiaría el contenido del bucket y, si hace falta, las plantillas. S3 → índice → Retrieve → modelo se sostiene.

Sobre el corpus: el dominio es derecho civil inmobiliario de Yucatán, sobre todo arrendamiento y compraventa de vivienda. Alimenté el índice con cinco documentos distintos, no con un párrafo repetido. El principal es el Código Civil del Estado (~3.4 MB; arrendamiento alrededor de los arts. 1564–1650, compraventa 1397–1473). Junto a ese van un formato de arrendamiento, un contrato de estacionamiento, un modelo CANADEVI de preventa y un contrato de adhesión de PROFECO. Todo está en `s3://micasa-kb-source/`. El Código Civil se puede bajar de https://www.yucatan.gob.mx/docs/pot/secogey/12_DAJYSP/2022/Fraccion_I/Codigo_Civil_del_Estado_de_Yucatan.pdf (también está en el sitio del Congreso). Los embeddings, como dije, salen del Sync de la Knowledge Base; para generar uso Kimi K3 (`us.moonshotai.kimi-k3`). Lista y enlaces: `data/README.md`.

En cuanto a cómo se parte el texto: no corrí un `chunk.py` en mi laptop. Cuando la Knowledge Base sincroniza los PDFs, Bedrock los fragmenta (tamaño fijo, con overlap). Lo dejé administrado a propósito. Si uno corta a ciegas por número de caracteres, un artículo del Código queda a la mitad, Retrieve trae trozos sueltos y el modelo alucina. Pedir k = 5 es suficiente para traer el artículo y un poco de lo que tiene al lado, sin llenar el contexto de ruido.

Para abstenerse: el asistente tiene orden de usar solo el contexto recuperado. Si ese contexto no alcanza para responder, lo dice y no inventa artículos ni cifras. Eso importa porque una palabra puede aparecer en dos títulos distintos. “Depósito”, por ejemplo, en el Código de Yucatán es la fianza de una renta (arts. 1619-1620) y también el contrato de guarda de bienes; si la pregunta es de vivienda, se tiran los chunks del otro tema. Cuando Retrieve no trae nada útil, la respuesta cae a que no se pudo con las fuentes (`abstained` en `/query`). No se rellena con lo que el modelo “recuerda”. La figura 3 es una pregunta imposible (Bitcoin, día 32) para mostrar ese caso.

Por último, para no mezclar papeles: en el diseño del curso, Google AI hace dos cosas distintas —embeber los documentos y generar la respuesta con Gemini— y Chroma es el cajón donde viven los vectores y se hace el k-NN. En este prototipo esos papeles están, con otras herramientas. Embeber lo hace el Sync de la Knowledge Base sobre los PDFs de S3. El cajón y el k-NN los hace Retrieve: persistente, top-5, con score de similitud y el nombre del archivo. Generar lo hace Converse después de buscar, en español, anclado a esos trozos. No se concatenan los chunks como si fueran la respuesta, ni se deja que Bedrock escriba el contrato entero (por eso no uso RetrieveAndGenerate). Buscar primero, generar después: igual que en el enunciado.

**Figura 1.** Interfaz (el papel de Streamlit): una respuesta del asistente con citas y scores. Pregunta: *En un contrato de renta de casa habitación en Yucatán, ¿cuánto depósito o fianza se puede pedir?* Debe hablar del depósito en garantía (art. 1619), no de depositario de bienes.

**Figura 2.** La misma pregunta contra la API (el papel de FastAPI / curl):

```bash
API=https://j9zrekl9g4.execute-api.us-east-1.amazonaws.com
curl -s "$API/health"
curl -s -X POST "$API/query" -H "Content-Type: application/json" \
  -d '{"question":"En un contrato de renta de casa habitación en Yucatán, ¿cuánto depósito o fianza se puede pedir?"}'
```

**Figura 3.** Fuera de dominio: *Según este material, ¿en qué artículo se obliga al inquilino a pagar la renta en Bitcoin el día 32 de cada mes?* El sistema debe abstenerse.
