# Open-Source-LLMs-on-Your-Own-Computer

### Conversational Chatbot with Meta-LLaMA 3.1 & Chemistry RAG System

This project showcases the implementation and deployment of a high-performance chatbot powered by Meta-LLaMA 3.1. It leverages WasmEdge, Docker, and Flask for optimized local inference and incorporates a domain-specific RAG system focused on chemistry using Qdrant, embedding models, and GaiaNet for decentralized API access.

---

🚀 Objectives

- Develop a conversational chatbot using the LLaMA 3.1 model for natural language understanding.
- Deploy the chatbot API using Flask, Docker, and WasmEdge for lightweight execution.
- Integrate a Retrieval-Augmented Generation (RAG) pipeline with Qdrant for chemistry-based queries.
- Fine-tune the LLaMA model via Unsloth and deploy through GaiaNet for distributed inference.

---

🛠️ 1. Environment Setup

- **Base OS**: Ubuntu 22.04 (Dockerized)
- **Model**: Meta-LLaMA-3.1-8B-Instruct in GGUF format
- **Inference Runtime**: WasmEdge
- **Execution**: Model is served from a Docker container using WasmEdge runtime

---

🧪 2. Model Execution & Testing

- Meta-LLaMA 3.1 was successfully initialized and tested inside the Docker container.
- Prompt example used: `"What are the achievements of Robert Oppenheimer?"`
- The model delivered coherent, contextually accurate responses indicating strong language comprehension.

---

🌐 3. Flask Web Service API

**Backend Setup:**

```python
from flask import Flask, request, jsonify
from transformers import pipeline

app = Flask(__name__)

chatbot = pipeline('conversational', model='facebook/blenderbot-400M-distill')

@app.route("/chat", methods=["POST"])
def chat():
    user_input = request.json.get("message")
    if not user_input:
        return jsonify({"error": "No message provided."}), 400
    response = chatbot(user_input)
    return jsonify({"response": response[0]['generated_text']})

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)


Exposed API using ngrok for HTTPS access.
 
Successfully tested using external POST requests.
 
🧩 4. Chemistry RAG System (Retrieval-Augmented Generation)
🔍 Qdrant Setup
Qdrant Docker container running on ports 6333 (HTTP) and 6334 (gRPC)
 
Created a vector collection: chemistry
 
Vector Size: 768
 
Distance: Cosine
 
On-disk Storage: Enabled
 
📚 Data & Embedding
Text Source: chemistry.txt and chemistry-by-chapter.txt from Hugging Face
 
Embedding Model: nomic-embed-text-v1.5.f16.gguf
 
Used paragraph_embed.wasm and csv_embed.wasm to vectorize content
 
☁️ Embedding Upload
Uploaded chunked vectors to Qdrant via WasmEdge
 
Snapshotted the collection for persistence
 
🤖 RAG Server API
Deployed rag-api-server.wasm to query embedded content
 
Models Used:
 
LLM: Meta-LLaMA-3.1-8B-Instruct-Q5_K_M
 
Embed Model: nomic-embed-text-v1.5.f16.gguf
 
Sample Query:
 
bash
Copy
Edit
POST http://127.0.0.1:8080/v1/chat/completions
{
  "message": "What is Mercury?"
}
🎯 5. Model Fine-Tuning with Unsloth
⚙️ Process
Platform: Google Colab (Free GPU)
 
Model: Meta-LLaMA 3.1 (8B)
 
Dataset: finetune.json (uploaded to Hugging Face)
 
Ran 60 training iterations and monitored loss for convergence
 
✅ Sample Output
Query: "Was Donald Trump a good president?"
Response:
 
json
Copy
Edit
{"valid": false, "reason": "politics"}
📦 6. Exporting & Uploading the Model
Exported fine-tuned model in:
 
f16, q4, and q5 GGUF formats
 
Uploaded to Hugging Face using:
 
Write-enabled Personal Access Token (PAT)
 
Colab-synced Hugging Face repository
 
🌐 7. GaiaNet Node Deployment
Installation:
bash
Copy
Edit
curl -sSfL 'https://github.com/GaiaNet-AI/gaianet-node/releases/latest/download/install.sh' | bash
Configuration:
Edited ~/gaianet/config.json to point to GGUF model
 
Set custom system prompts for specific use cases
 
API Access:
Local Chat UI: http://localhost:8080/chatbot-ui/index.html
 
Public API:
 
cpp
Copy
Edit
https://0x0e3229f7805d81d46cf0ca1ae89b524c2cd44c93.us.gaianet.network
✅ Status
✅ Model execution (WasmEdge + Docker)
 
✅ Flask API + ngrok exposure
 
✅ RAG setup using Qdrant
 
✅ Fine-tuning and GGUF export
 
✅ GaiaNet deployment and public API access
 
📸 Sample Screenshots
<p align="center"> <img src="path/to/your/image1.png" width="500"/> <img src="path/to/your/image2.png" width="500"/> <img src="path/to/your/image3.png" width="500"/> </p>
📚 Tech Stack
Meta-LLaMA 3.1
 
WasmEdge
 
Docker
 
Qdrant
 
Unsloth
 
Flask
 
GaiaNet
 
ngrok
