# MiCasa (proyecto final RAG)

Prototipo de contratos de vivienda en Yucatán, con RAG en AWS. El ciclo es el del curso (incrustar, indexar, top-k, generar), con otras herramientas: React en lugar de Streamlit, Lambda en lugar de FastAPI, Knowledge Base / S3 Vectors en lugar de Chroma, embeddings de Bedrock en lugar de Google AI.

Texto para el Doc: `docs/ENTREGA.md`

Login: `admin` / `Admin2026!`  
App: https://main.d1hsj6zus1f46.amplifyapp.com  
API: https://j9zrekl9g4.execute-api.us-east-1.amazonaws.com

## Local

Node 22. Copia `.env.example` a `.env.local`.

```bash
nvm use
npm install
cp .env.example .env.local
npm run dev
```

http://localhost:5173/

El backend es Python en Lambda (`requirements.txt`). Corpus: `data/README.md`.
