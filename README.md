import { useState } from "react";

function App() {
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState("");
  const [loading, setLoading] = useState(false);

  const sendMessage = async () => {
    if (!input.trim()) return;
    const userMsg = { role: "user", text: input };
    setMessages(prev => [...prev, userMsg]);
    setInput("");
    setLoading(true);
    try {
      const res = await fetch("https://YOUR_HF_USERNAME-infrachat.hf.space/chat", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ message: input }),
      });
      const data = await res.json();
      setMessages(prev => [...prev, { role: "bot", text: data.reply }]);
    } catch {
      setMessages(prev => [...prev, { role: "bot", text: "Error — is the backend running?" }]);
    }
    setLoading(false);
  };

  return (
    <div style={styles.container}>
      <div style={styles.header}>
        <h1 style={styles.title}>InfraChat</h1>
        <p style={styles.subtitle}>DevOps AI Assistant</p>
      </div>
      <div style={styles.chatBox}>
        {messages.length === 0 && (
          <p style={styles.placeholder}>
            Ask me anything about Docker, Kubernetes, CI/CD...
          </p>
        )}
        {messages.map((msg, i) => (
          <div key={i} style={msg.role === "user" ? styles.userMsg : styles.botMsg}>
            <strong>{msg.role === "user" ? "You" : "InfraChat"}:</strong> {msg.text}
          </div>
        ))}
        {loading && (
          <div style={styles.botMsg}>
            <em>InfraChat is thinking...</em>
          </div>
        )}
      </div>
      <div style={styles.inputRow}>
        <input
          style={styles.input}
          value={input}
          onChange={e => setInput(e.target.value)}
          onKeyDown={e => e.key === "Enter" && sendMessage()}
          placeholder="Ask a DevOps question..."
        />
        <button style={styles.button} onClick={sendMessage}>
          Send
        </button>
      </div>
    </div>
  );
}

const styles = {
  container: { maxWidth: 700, margin: "0 auto", padding: 20, fontFamily: "sans-serif" },
  header: { textAlign: "center", marginBottom: 20 },
  title: { fontSize: 32, margin: 0, color: "#1a2f58" },
  subtitle: { color: "#64748b", margin: 4 },
  chatBox: {
    border: "1px solid #e2e8f0",
    borderRadius: 12,
    padding: 16,
    minHeight: 400,
    maxHeight: 500,
    overflowY: "auto",
    marginBottom: 16,
    background: "#f8fafc"
  },
  placeholder: { color: "#94a3b8", textAlign: "center", marginTop: 160 },
  userMsg: {
    background: "#1a2f58",
    color: "#fff",
    padding: "10px 14px",
    borderRadius: 10,
    marginBottom: 10,
    maxWidth: "80%",
    marginLeft: "auto"
  },
  botMsg: {
    background: "#fff",
    border: "1px solid #e2e8f0",
    padding: "10px 14px",
    borderRadius: 10,
    marginBottom: 10,
    maxWidth: "80%"
  },
  inputRow: { display: "flex", gap: 10 },
  input: {
    flex: 1,
    padding: "12px 16px",
    borderRadius: 8,
    border: "1px solid #e2e8f0",
    fontSize: 14,
    outline: "none"
  },
  button: {
    padding: "12px 24px",
    background: "#1a2f58",
    color: "#fff",
    border: "none",
    borderRadius: 8,
    cursor: "pointer",
    fontSize: 14
  }
};

export default App;
