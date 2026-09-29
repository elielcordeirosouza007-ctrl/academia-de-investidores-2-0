# Academia de Investidores — publicação

## Estrutura
- `frontend/`: aplicação React que será publicada no Vercel.
- `backend/`: API FastAPI, que precisa ficar hospedada em um serviço de backend com Python/MongoDB.

## GitHub
1. Crie um repositório novo no GitHub.
2. Extraia este projeto e envie o conteúdo da pasta `Academia-de-investidores-main`, não o ZIP dentro do repositório.
3. Não envie arquivos `.env` com senhas ou chaves. Os arquivos `.env.example` servem apenas como modelo.

## Vercel
Ao importar o repositório no Vercel:

- **Root Directory:** `frontend`
- **Framework Preset:** Create React App
- **Build Command:** `npm run build`
- **Output Directory:** `build`
- **Install Command:** `npm install --legacy-peer-deps`

Em **Environment Variables**, adicione `REACT_APP_BACKEND_URL` com a URL pública da API FastAPI, sem `/api` no final.

Exemplo: `https://minha-api.exemplo.com`

O `frontend/vercel.json` já está preparado para o React Router funcionar em URLs como `/aprender`, `/simular`, `/perfil` etc.

## Backend
O backend exige MongoDB e estas variáveis:
- `MONGO_URL`
- `DB_NAME`
- `JWT_SECRET`
- `EMERGENT_LLM_KEY`
- `CORS_ORIGINS`

Use `backend/.env.example` como referência. Não coloque esses valores diretamente no GitHub.

## Importante
Publicar apenas o frontend no Vercel não substitui a API. Cadastro, login, progresso, perfil e recursos de IA que usam o backend só funcionarão depois que a API estiver hospedada e `REACT_APP_BACKEND_URL` apontar para ela.
