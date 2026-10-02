# MiCasa

Prototipo de contratos de vivienda en Yucatán, con RAG en AWS. El ciclo es el del curso (incrustar, indexar, top-k, generar), montado con otras herramientas: React en lugar de Streamlit, Lambda en lugar de FastAPI, Knowledge Base / S3 Vectors en lugar de Chroma, embeddings de Bedrock en lugar de Google AI.

**Reporte de entrega:** [`Proyecto Final - Sistema RAG.pdf`](./Proyecto%20Final%20-%20Sistema%20RAG.pdf)

Estructura y equivalencias con el enunciado (mismo contenido, en markdown): `docs/estructura-del-proyecto.md`

## Local

Node 22. El frontend habla por HTTP con el API ya desplegado; no lleva claves de AWS ni de modelos.

```bash
nvm use
npm install
cp .env.example .env.local
npm run dev
```

En `.env.local` pega `VITE_API_URL` (copia `.env.example`). Sin esa variable no hay chat ni RAG. El backend es Python en Lambda (`requirements.txt`). Corpus: `data/README.md`.

Las rutas del API están en `GET /docs` (equivalente a FastAPI) y el spec en `GET /openapi.json`.
