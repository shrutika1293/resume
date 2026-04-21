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
