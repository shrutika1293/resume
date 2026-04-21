FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]


fastapi
uvicorn
langchain
langchain-groq
langchain-community
langchain-chroma
langchain-text-splitters
langchain-core
langchain-huggingface
chromadb
sentence-transformers
python-dotenv

FROM node:20-slim

WORKDIR /app

COPY package*.json .

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]



services:
  backend:
    build: ./backend
    ports:
      - "8000:8000"
    env_file:
      - ./backend/.env
    volumes:
      - ./backend/docs:/app/docs
      - ./backend/chroma_db:/app/chroma_db
    networks:
      - infrachat-network

  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    depends_on:
      - backend
    environment:
      - REACT_APP_API_URL=http://localhost:8000
    networks:
      - infrachat-network

networks:
  infrachat-network:
    driver: bridge




    const res = await fetch(`${process.env.REACT_APP_API_URL || 'http://localhost:8000'}/chat`, {
