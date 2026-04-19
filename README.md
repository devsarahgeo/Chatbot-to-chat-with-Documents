<h2>Tech Stack:</h2>

LLM:        Groq Llama 3.3 70B                                                                                                                 
Embeddings: HuggingFace all-MiniLM-L6-v2                                                                                                         
Vector DB:  FAISS                   
Storage:    JSON files      
RETRIEVER:  LangChain Retriever
Server:     FastAPI + uvicorn                   
WhatsApp:   Twilio                                                                                                                                  
Tunnel:     ngrok  


<h2>ARCHITECTURE</h2>
                                                                                   
                                                                                                                                                                          
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

                
                                                           
