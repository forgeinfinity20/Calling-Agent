from fastapi import FastAPI
from pydantic import BaseModel
from main_agent import compiled, initial_state
import uvicorn

app = FastAPI()

class CallRequest(BaseModel):
    contact: str
    input: str

@app.post("/run")
def run_agent(req: CallRequest):
    state = dict(initial_state)
    state["contact"] = req.contact
    state["input"] = req.input
    result = compiled.invoke(state)
    return {"final_state": result}

@app.get("/")
def home():
    return {"status": "AI Calling Agent API is running"}

if __name__ == "__main__":
    uvicorn.run(app, host="0.0.0.0", port=8000)
