# Ingestão e Busca Semântica com LangChain e PostgreSQL (pgVector)

Este projeto implementa um sistema de RAG (Retrieval-Augmented Generation) capaz de:

- Ingerir um arquivo PDF
- Gerar embeddings dos textos
- Armazenar vetores no PostgreSQL com pgVector
- Realizar busca semântica
- Responder perguntas via CLI utilizando LLM (Gemini)

---

# 🚀 Tecnologias utilizadas

- Python 3.10+
- LangChain
- PostgreSQL + pgVector
- Docker & Docker Compose
- HuggingFace Embeddings (sentence-transformers)
- Google Gemini (LLM)

---

# 📁 Estrutura do projeto

├── docker-compose.yml  
├── requirements.txt  
├── .env  
├── document.pdf  
├── README.md  
└── src/  
&nbsp;&nbsp;&nbsp;&nbsp;├── ingest.py  
&nbsp;&nbsp;&nbsp;&nbsp;├── search.py  
&nbsp;&nbsp;&nbsp;&nbsp;└── chat.py  

---

# ⚙️ Configuração do ambiente

## 1. Criar ambiente virtual

python3 -m venv venv  
source venv/bin/activate  

---

## 2. Instalar dependências

pip install -r requirements.txt  

---

## 3. Configurar variáveis de ambiente

Crie um arquivo `.env` baseado no `.env.example`:

DATABASE_URL=postgresql+psycopg://postgres:postgres@localhost:5432/rag  
PG_VECTOR_COLLECTION_NAME=documents  
PDF_PATH=document.pdf  
GOOGLE_API_KEY=SUA_CHAVE_AQUI  
GOOGLE_EMBEDDING_MODEL=models/gemini-embedding-001  

---

# 🐳 Subindo o banco de dados

docker compose up -d  

Verifique se está rodando:

docker ps  

---

# 🧠 Fluxo do sistema

PDF  
→ Chunking (1000 / 150)  
→ Embeddings (HuggingFace)  
→ PGVector (Postgres)  
→ Busca semântica (k=10)  
→ Gemini LLM  
→ Resposta no CLI  

---

# 📥 1. Executar ingestão

python src/ingest.py  

Saída esperada:

Chunks gerados: 67  
Ingestão concluída!  

---

# 🔎 2. Busca semântica

O arquivo search.py:

- vetoriza a pergunta
- busca os 10 chunks mais relevantes
- monta o contexto para o LLM

---

# 💬 3. Rodar o chat (CLI)

python src/chat.py  

Exemplo:

PERGUNTA: Qual o faturamento da empresa X?  
RESPOSTA: O faturamento foi de R$ 3.5 milhões.  

---

# 🚫 Regras do sistema

- Responder somente com base no contexto recuperado  
- Não usar conhecimento externo  
- Se não houver informação, responder exatamente:

"Não tenho informações necessárias para responder sua pergunta."

---

# 🧪 Exemplos fora do contexto

PERGUNTA: Qual a capital da França?  
RESPOSTA: Não tenho informações necessárias para responder sua pergunta.

PERGUNTA: Quantos clientes temos em 2024?  
RESPOSTA: Não tenho informações necessárias para responder sua pergunta.

---

# 📌 Decisões técnicas

- Embeddings locais (HuggingFace) para evitar rate limits  
- Gemini usado apenas para geração de resposta  
- pgVector como banco vetorial  
- LangChain para orquestração do pipeline RAG  

---

# ▶️ Ordem de execução

docker compose up -d  
python src/ingest.py  
python src/chat.py  

---

# 📦 Resultado esperado

Sistema funcional de:

- ingestão de PDF  
- busca semântica  
- resposta baseada em contexto  
- CLI interativo  

---

# 👨‍💻 Autor

Projeto acadêmico - MBA em Inteligência Artificial
