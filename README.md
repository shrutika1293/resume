from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware
from pydantic import BaseModel
from dotenv import load_dotenv
import os

load_dotenv()

app = FastAPI()

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_methods=["*"],
    allow_headers=["*"],
)

class Message(BaseModel):
    message: str

@app.get("/")
def root():
    return {"status": "InfraChat is running"}

@app.get("/health")
def health():
    return {"status": "InfraChat is running"}

@app.post("/chat")
async def chat(msg: Message):
    from rag import get_answer
    reply = get_answer(msg.message)
    return {"reply": reply}
