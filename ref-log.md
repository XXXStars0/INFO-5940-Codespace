# ref-log.md
## Reference Sources
- `chat_with_pdf.py` (provided base file)
- `langgraph_chroma_retreiver.ipynb` (reference for retriever setup and code example usage)
- `/data/**` (test document used for retrieval and chat, especially `RAG_source.txt`)

## Referenced Parts
- From `chat_with_pdf.py`:
    - Overall structure for document loading and ChatGPT interface.
    - Functions for PDF parsing and text chunking.
    - Basic Chat class and prompt handling.

- From `langgraph_chroma_retreiver.ipynb`:
    - Multiple useful code examples.
    - Prompt construction logic for RAG-style QA.

## Testing
- Tested on codespace using `/data/RAG_source.txt` for retrieval and question answering.
- Verified chat responses based on the Assignment 1 as prompt by ChatGpt.
- Identified API connection issues (now troubleshooting).