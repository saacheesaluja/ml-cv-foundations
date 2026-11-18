# Module 5 — Vision-Language Models (VLMs)

## Overview

**Pillar:** Vision + Generative AI  
**Goal:** Explore modern vision-language models (VLMs) for visual question answering (VQA), natural-language explanations of computer-vision results, image captioning, and image-text retrieval.

This module combines visual information with language models and introduces CLIP embeddings for searching images with text and comparing images with text descriptions.

## Learning outcomes

By the end of this module, I should be able to:

- Explain the basic architecture of a VLM, including its image encoder and language model.
- Use an open-weight VLM for visual question answering.
- Ask questions about invoice images and compare VLM answers with self-hosted OCR output.
- Generate natural-language explanations of object-detection results.
- Request structured JSON from a VLM and validate the response against a schema.
- Generate image captions.
- Create CLIP image and text embeddings.
- Retrieve images using text queries and rank text descriptions against an image.

## Daily work

| Day | Topic | Main work |
|---|---|---|
| 1 | How VLMs work — Qwen3-VL | Review VLM architecture and visual instruction tuning; load Qwen3-VL and ask questions about an image. |
| 2 | Visual question answering | Ask questions about an invoice image, compare VLM answers with Tesseract OCR, and record extraction observations. |
| 3 | Natural-language explanations and schema-valid JSON | Pass computer-vision detection results and an image to a VLM; generate an explanation and validate structured output using a schema. |
| 4 | Image captioning — BLIP-2 | Generate general and guided captions and consider the difference between captions and image embeddings. |
| 5 | Multimodal embeddings — CLIP | Generate image/text embeddings and perform text-to-image and image-to-text retrieval. |

## Tools and models

- **Qwen3-VL-4B-Instruct:** visual question answering and explanations.
- **Tesseract OCR:** self-hosted OCR baseline for invoice text.
- **Ultralytics YOLO:** object-detection results used in the explanation exercise.
- **Pydantic:** validation of VLM-generated structured JSON.
- **BLIP-2:** image captioning.
- **OpenAI CLIP (`clip-vit-base-patch32`):** shared image/text embedding space and retrieval.
- **Google Colab:** notebook environment; a GPU runtime is recommended for model inference.

## Deliverables

- A working VLM workflow for visual question answering.
- Natural-language explanations of computer-vision outputs.
- Schema-validated JSON responses.
- Image-captioning utility.
- CLIP image and text embedding utilities.
- A working text-to-image and image-to-text retrieval demonstration.
- A comparison of VLM extraction with self-hosted OCR on invoice images.

## Evaluation and observations

For invoice VQA, inspect whether the model extracts the invoice number, date, supplier, customer, and grand total correctly. Check currency symbols and numerical values separately. Compare the VLM's contextual answers with the raw text returned by Tesseract.

Include Azure AI Document Intelligence or AWS Textract results only if the service was actually run and its output was saved. If no managed service was used, mark that comparison as **not run**. Do not present manually created or synthetic values as managed-service output.

For schema-based explanations, record whether the response parses and passes validation. For captioning and retrieval, inspect whether captions describe the image accurately and whether retrieved images match the text query.

## Important limitations

- VLMs can hallucinate or misread details, especially invoice numbers, dates, and financial amounts.
- A plausible answer is not necessarily a verified answer. Validate important fields against the source image.
- A correct numerical amount can still have an incorrect currency symbol.
- OCR returns recognized text; it does not automatically understand the meaning or role of each field.
- Image captions are text descriptions, while CLIP embeddings represent images and text as vectors in a shared space. They support different retrieval approaches.
- GPU memory can limit the model size and image resolution that can be used in Colab. Quantization and smaller image inputs may help, but do not guarantee that every runtime can load the model.

## Module completion checklist

- [ ] Day 1: Run Qwen3-VL on a clearly visible sample image and record observations.
- [ ] Day 2: Complete invoice VQA and compare answers with Tesseract OCR.
- [ ] Day 3: Generate a natural-language explanation and validate the structured JSON output.
- [ ] Day 4: Generate captions for sample images.
- [ ] Day 5: Generate CLIP embeddings and demonstrate text-to-image and image-to-text retrieval.
- [ ] Save useful outputs and note any failures or limitations.
- [ ] Complete the managed-service comparison later only if Azure or AWS output becomes available.

## Connection to the next module

The captioning and embedding utilities developed here can be reused in Module 6 for multimodal retrieval and ingestion. Captions can provide searchable text descriptions, while CLIP embeddings can support image-based similarity search.
