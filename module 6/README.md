# Module 6 — End-to-End Multimodal RAG

## Overview

This module combines document understanding, computer vision, multimodal embeddings, vector search, and vision-language generation into one **non-agentic Multimodal Retrieval-Augmented Generation (RAG) pipeline**.

The pipeline processes a PDF containing text, tables, and visual content, indexes the extracted information, retrieves relevant context from text-only or text-plus-image queries, and generates an answer grounded in the retrieved evidence.

## Learning objectives

- Ingest PDFs and inspect their page layouts.
- Extract text, tables, figures, and images with document metadata.
- Split extracted text and tables into searchable chunks.
- Generate image captions using BLIP-2.
- Create text and image embeddings using CLIP.
- Build a multimodal FAISS vector index.
- Retrieve relevant content using text-only and text-plus-image queries.
- Generate evidence-based answers with Qwen3-VL.
- Preserve source metadata and check that generated citations refer to retrieved source IDs.

## Notebook

| File | Description |
|---|---|
| `module-6-end-to-end-multimodal-rag.ipynb` | Complete pipeline from PDF ingestion through retrieval and grounded answer generation. |

## Pipeline workflow

1. **Document ingestion and layout parsing** — Upload a PDF, render its pages, and extract layout elements with Unstructured.
2. **Text and table processing** — Convert extracted elements into records and split text and tables into chunks while retaining source information.
3. **Visual content representation** — Extract visual regions, generate captions with BLIP-2, and create CLIP embeddings for text chunks, captions, and images.
4. **Multimodal indexing** — Combine the normalized vectors in a FAISS index and store corresponding metadata.
5. **Multimodal retrieval** — Search the index using a text query or a combined text-and-image query.
6. **Grounded generation** — Pass retrieved text and available images to Qwen3-VL and request an answer with source IDs.
7. **Output and provenance** — Save extracted elements, chunks, captions, the FAISS index, metadata, and the generated answer.

## Models and tools

| Component | Tool / model | Purpose |
|---|---|---|
| PDF parsing | Unstructured | Extract document elements and layout information |
| PDF rendering | PyMuPDF | Render PDF pages as images |
| Text chunking | LangChain text splitters | Split text and tables into manageable chunks |
| Image captioning | BLIP-2 (`Salesforce/blip2-opt-2.7b`) | Generate captions for extracted visual regions |
| Multimodal embeddings | CLIP (`openai/clip-vit-base-patch32`) | Embed text and images in a shared vector space |
| Vector search | FAISS | Index and retrieve multimodal vectors |
| Answer generation | Qwen3-VL (`Qwen/Qwen3-VL-4B-Instruct`) | Generate answers using retrieved text and visual evidence |

## Requirements

- Google Colab is the intended environment.
- A GPU runtime is recommended.
- The notebook installs its Python and system dependencies.
- A PDF containing useful text, tables, and images or figures.
- An image file for the text-plus-image retrieval exercise.

**Memory note:** BLIP-2 OPT-2.7B and Qwen3-VL 4B can require substantial GPU memory. If the runtime runs out of memory, use a higher-memory GPU or reduce the model workload. The notebook does not guarantee that both models will fit on every Colab GPU.

## Running the notebook

1. Open `module-6-end-to-end-multimodal-rag.ipynb` in Google Colab.
2. Select **Runtime → Change runtime type → T4 GPU** (or another available GPU).
3. Run the cells in order, starting with dependency installation.
4. Upload a PDF when prompted.
5. Allow layout extraction, caption generation, embedding, and indexing to complete.
6. Enter or edit the text query and upload an image for the combined query.
7. Review the retrieved records and generated answer.
8. Download the saved FAISS index, metadata, and grounded answer.

## Outputs

The notebook creates a `multimodal_rag_outputs/` directory containing:

- `document_elements.json` — extracted document elements and available metadata.
- `text_chunks.json` — chunked text and table content.
- `image_captions.json` — captions and paths for extracted images.
- `multimodal_faiss.index` — FAISS vector index.
- `index_metadata.json` — metadata aligned with the indexed vectors.
- `grounded_answer.json` — generated answer, question, retrieval mode, source IDs, and retrieved source records.

## Success criteria

- A PDF is parsed into structured text, table, and visual records.
- Text chunks, image captions, and image embeddings are indexed.
- A text-only query retrieves relevant document content.
- A text-plus-image query retrieves relevant multimodal content.
- The VLM produces an answer based on retrieved context and available images.
- The answer's source IDs can be matched to records in the retrieved context.

## Limitations and evaluation notes

- Extracted image records in this notebook do not necessarily have page numbers or bounding boxes. Do not assume those fields are available for every visual result.
- CLIP is used for similarity retrieval; similarity scores are not proof that a result is factually relevant.
- A citation-ID check verifies that cited IDs exist in the retrieved context. It does **not** verify that each answer claim is supported by its cited evidence.
- Review the original document pages and retrieved images when evaluating answer accuracy.
- Compare caption-based retrieval with direct image-embedding retrieval when analysing retrieval quality.
- The pipeline is a non-agentic baseline. It does not include agent planning or tool orchestration.

## Module deliverable

A working end-to-end multimodal RAG baseline that ingests a PDF, indexes text and visual content, supports text-only and text-plus-image retrieval, and generates answers with traceable source IDs. This pipeline serves as the foundation for the agentic workflow in Module 7.
