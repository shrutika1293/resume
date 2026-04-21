from langchain_groq import ChatGroq
from langchain_community.vectorstores import Chroma
from langchain_community.embeddings import HuggingFaceEmbeddings
from langchain_community.document_loaders import TextLoader, DirectoryLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.chains import RetrievalQA
from langchain.prompts import PromptTemplate
import os
from dotenv import load_dotenv

load_dotenv()

SYSTEM_PROMPT = """You are InfraChat, an expert DevOps assistant.
Answer only DevOps related questions about Docker, Kubernetes,
CI/CD, and cloud infrastructure. Be concise and practical.
If the question is not related to DevOps, politely decline.

Context: {context}
Question: {question}
Answer:"""

def load_vectorstore():
    embeddings = HuggingFaceEmbeddings(
        model_name="all-MiniLM-L6-v2"
    )
    if os.path.exists("chroma_db"):
        return Chroma(
            persist_directory="chroma_db",
            embedding_function=embeddings
        )
    loader = DirectoryLoader("docs/", glob="**/*.txt",
        loader_cls=TextLoader)
    documents = loader.load()
    splitter = RecursiveCharacterTextSplitter(
        chunk_size=200, chunk_overlap=40)
    chunks = splitter.split_documents(documents)
    vectorstore = Chroma.from_documents(
        chunks, embeddings,
        persist_directory="chroma_db"
    )
    return vectorstore

def get_answer(question: str) -> str:
    llm = ChatGroq(
        api_key=os.getenv("GROQ_API_KEY"),
        model_name="llama3-8b-8192"
    )
    vectorstore = load_vectorstore()
    prompt = PromptTemplate(
        template=SYSTEM_PROMPT,
        input_variables=["context", "question"]
    )
    chain = RetrievalQA.from_chain_type(
        llm=llm,
        retriever=vectorstore.as_retriever(
            search_kwargs={"k": 3}
        ),
        chain_type_kwargs={"prompt": prompt}
    )
    result = chain.invoke({"query": question})
    return result["result"]
