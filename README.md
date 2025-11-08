# 🤖 ai-soc-runbook-qa

AI-assisted runbook Q&A tool for my SOC lab.

This project uses embeddings and a small language model to answer questions about SOC incident response using **my own runbooks and procedures** as context. It is designed to sit on top of:

- `lab-core-network` – on-prem / virtual lab network  
- `soc-alert-automation` – alert pipeline that generates cases  
- `ug-aws-hybrid-soc-lab` – cloud side of the hybrid environment  
- `g-hybrid-soc-security-program` – formal incident-response program this app follows  

The goal is to make it faster to go from **alert → correct steps** by querying runbooks directly.

> This repo holds the code and config for the Q&A app itself.  
> A Hugging Face Space can be wired to this code as the public UI/demo.

---

## 🎯 Purpose

- Turn static SOC runbooks into an **interactive assistant**.
- Let an analyst ask things like:
  - “How do I handle an SSH brute force alert?”
  - “What are the steps for DNS tunneling incidents?”
- Get a step-by-step answer grounded in **my SOC playbooks**, not random internet advice.

---

## 🧱 Project Structure

```text
ai-soc-runbook-qa/
├── README.md
├── app.py                      # Gradio app / main entrypoint
├── requirements.txt
├── config/
│   ├── model-config.yaml       # embedding + LLM model names, params
│   └── app-settings.yaml       # host/port, index path, UI settings
├── data/
│   └── runbooks/
│       ├── ssh-bruteforce.md
│       ├── dns-tunneling.md
│       ├── cloud-credential-leak.md
│       └── _index.yaml         # optional metadata about runbooks (tags, severity, etc.)
├── src/
│   ├── ai_soc_runbook_qa/
│   │   ├── __init__.py
│   │   ├── loader.py           # load and chunk runbooks
│   │   ├── embedder.py         # build / load embeddings index
│   │   ├── retriever.py        # similarity search over chunks
│   │   ├── generator.py        # call LLM to generate answers
│   │   └── prompts.py          # system / user prompt templates
│   └── build_index.py          # script to pre-build embeddings
├── hf-space/
│   └── README-space.md         # notes specific to the Hugging Face Space deployment
├── docs/
│   ├── architecture.md         # high-level design and data flow
│   ├── usage.md                # how analysts should use the tool
│   └── screenshots/
│       ├── question-example.png
│       └── context-view.png
└── tests/
    ├── test_loader.py
    ├── test_retriever.py
    └── test_generator.py