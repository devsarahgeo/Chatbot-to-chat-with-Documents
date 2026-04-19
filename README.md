<h2>Tech Stack:</h2>

AI Tools:   Ollama(Claude Code alternate), Github Copilot
Storage:    Can shift to another DB like SQLite or some other if large records

| Storage       | Used For                  | Why JSON / Format                     |
|---------------|---------------------------|--------------------------------------|
| Raw HTML      | Archive original filings  | Raw data, never modify               |
| Chunks JSON   | Structured SEC text       | Easy to read, debug, portable        |
| FAISS Index   | Vector search             | Binary format, fast similarity search|                                                       
 



  | Tool         | Purpose           | Why                                                                 |
|--------------|------------------|----------------------------------------------------------------------|
| WhatsApp     | User interface   | Everyone has it, no app install needed                              |
| Twilio       | WhatsApp API     | Connects WhatsApp to backend                                        |
| ngrok        | Tunnel           | Exposes local server to internet                                    |
| FastAPI      | Web server       | Receives webhook calls from Twilio                                  |
| Uvicorn      | ASGI server      | Runs FastAPI efficiently                                            |
| LangChain    | LLM orchestration| Chains retrieval + prompts + LLM                                    |
| Groq         | LLM              | Llama 3 (fast, low latency, cost-effective)                         |
| FAISS        | Vector DB        | Fast similarity search for SEC chunks                               |
| HuggingFace  | Embeddings       | Converts text to vectors (e.g., all-MiniLM-L6-v2)                   |
| SEC EDGAR    | Data source      | Free SEC filings, no API key required                               |


<h2>ARCHITECTURE</h2>

<img width="1509" height="2145" alt="mermaid-diagram" src="https://github.com/user-attachments/assets/82ed241b-8a7d-472f-aa54-1cb931d6c481" />

                                                                                   
                                                                                                                                                                          
                                ┌─────────┐                                                                                                                               
                                │ WHATSAPP│
                                │  USER   │
                                └────┬────┘
                                     │ send message                                                                                                                       
                                ┌────▼────┐
                                │ TWILIO  │                                                                                                                               
                                │ WEBHOOK │
                                └────┬────┘
                                     │ HTTPS
                                ┌────▼────┐                                                                                                                               
                                │ NGROK   │
                                │ :5000   │                                                                                                                               
                                └────┬────┘
                                     │ proxy
                                ┌────▼────┐
                                │ UVICORN │                                                                                                                               
                                │ :5000   │
                                └────┬────┘                                                                                                                               
                                     │
                           ┌─────────┼─────────┐
                           │                   │
                     ┌─────▼─────┐       ┌─────▼─────┐                                                                                                                    
                     │   MENU    │       │   QUERY   │
                     │  HANDLER  │       │  HANDLER  │                                                                                                                    
                     │  (1-5)    │       │     │                                                                                                                    
                     └─────┬─────┘       └─────┬─────┘
                           │                   │                                                                                                                          
                           └─────────┬─────────┘
                                     │                                                                                                                                    
                            ┌────────▼────────┐
                            │    LANGCHAIN 
                               ORCHESTRATOR                                                                                                                                  
                            │  (ticker/year   │
                            │   extraction)   │                                                                                                                           
                            └────────┬────────┘
                                     │                                                                                                                                    
                ┌─────────────────────┼─────────────────────┐
                │                     │                     │                                                                                                             
         ┌──────▼──────┐       ┌──────▼──────┐       ┌──────▼──────┐
         │   GROQ      │       │   GROQ      │       │   GROQ       │                                                                                                     
         │   Llama     │       │   Llama     │       │   Llama      │
         │   3.3 70B   │       │   3.3 70B   │       │   3.3 70B    │                                                                                                     
         │             │       │             │       │              │                                                                                                     
         │ (risk_chain)│       │(financials) │       │(valuation)   │                                                                                                     
         └──────┬──────┘       └──────┬──────┘       └──────┬──────┘                                                                                                      
                │                     │                     │
                │            ┌─────────┴─────────┐          │                                                                                                             
                │            │                   │          │                                                                                                             
         ┌──────▼──────┐ ┌───▼────┐       ┌──────▼──────┐   │
         │   FAISS     │ │  JSON  │       │   FAISS     │   │                                                                                                             
         │   (risk)    │ │(metric)│       │   (mda)     │   │
         └─────────────┘ │        │       └─────────────┘   │                                                                                                             
                         └────────┘                                   

                
                                                           
