# DSPy + LangGraph: Client Brief → Design Brief → Concept Image

A multi-agent AI pipeline that turns a raw client packaging brief into a full design brief and a first-concept packaging image — running 100% locally via Ollama.

## Pipeline Overview

```
Client Brief
     │
     ▼
┌─────────────────┐
│  Trend Analyst  │  Market trends · Consumer insights · Competitor landscape · EU sustainability (PPWR)
└────────┬────────┘
         │
         ▼
┌──────────────────────┐
│  Design Strategist   │  Color palette · Typography · Materials · Structural concept
└──────────┬───────────┘
           │
           ▼
┌──────────────────┐
│  Brief Compiler  │  Executive summary · Full design brief · Next steps
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Visual Draft    │  Image-generation prompt → Concept card (matplotlib)
└──────────────────┘
```

Each agent is a **DSPy ChainOfThought module** with a typed Signature. The four agents are wired together by a **LangGraph StateGraph** with a supervisor router.

## Outputs

| File | Description |
|------|-------------|
| `<client>_design_brief_<timestamp>.json` | Full pipeline output as structured JSON |
| `<client>_design_brief_<timestamp>.pdf` | Client-ready design brief PDF with concept image |
| `packaging_concept_draft.png` | First visual concept card |

## Model Mapping

| Agent | PoC (local) | Production |
|-------|-------------|------------|
| Supervisor | llama3.2:3b | Claude Sonnet |
| Trend Analyst | llama3.2:3b | Manus / Perplexity |
| Design Strategist | llama3.2:3b | Gemini 2.5 Pro |
| Brief Compiler | llama3.2:3b | Claude Sonnet |
| Visual Draft | llama3.2:3b | DALL-E 3 / Flux |

## Requirements

- [Ollama](https://ollama.com) installed and running
- Python 3.10+
- Jupyter / JupyterLab

```bash
pip install dspy langgraph langchain langchain-ollama langchain-core fpdf2
ollama pull llama3.2:3b
```

## Usage

Open `dspy_multi_model_trend_to_brief_poc_1.ipynb` and run all cells top to bottom.

To use your own brief, edit `CLIENT_BRIEF` in the run cell:

```python
CLIENT_BRIEF = {
    "client_name":      "Your Client",
    "product_category": "product type",
    "target_market":    "target market",
    "client_context":   "Full brief text here...",
}
```
