# Module 4 — Document Understanding

## Overview

**Pillar:** Document Intelligence  
**Goal:** Convert documents into structured data using two complementary approaches: self-hosted OCR/open-source document understanding and managed cloud document-intelligence services. The module focuses on extracting key-value pairs and tables from invoices and receipts, handling difficult layouts, normalizing outputs into common JSON, and evaluating trade-offs.

## Learning objectives

- Compare Tesseract, EasyOCR, and PaddleOCR for text extraction.
- Apply preprocessing such as deskewing, denoising, and thresholding.
- Explain why layout matters for forms and invoices.
- Explore LayoutLMv3 and Donut.
- Understand managed extraction with Azure AI Document Intelligence and AWS Textract.
- Extract and normalize key-value pairs and table data.
- Evaluate accuracy, latency, cost, setup effort, and privacy.

## Day-by-day work

| Day | Topic | Main work |
|---|---|---|
| Day 1 | OCR landscape and preprocessing | Compare Tesseract and EasyOCR; extract text; test grayscale, denoising, thresholding, and deskewing. |
| Day 2 | Layout-aware document understanding | Study LayoutLMv3 and Donut; run pretrained Donut and inspect structured output; understand layout in invoices and forms. |
| Day 3 | Managed document-intelligence services | Explore Azure AI Document Intelligence (prebuilt invoice/receipt and custom extraction) and AWS Textract (forms and tables). Google Document AI is optional. |
| Day 4 | Key-value and table extraction | Work through merged-header examples, multi-page tables, rotated scans, and varied layouts; normalize outputs into a common JSON schema. |
| Day 5 | Structured evaluation and comparison | Use a small labelled held-out set; measure field-level accuracy, table extraction quality, latency, and cost per page; build a comparison matrix. |

## Deliverables

The intended deliverable is an invoice/receipt extraction pipeline that produces structured JSON containing key-value pairs and line-item/table data using:

1. **One self-hosted OCR/layout-aware approach**, such as an OCR pipeline or suitable layout-aware model.
2. **One managed service**, either Azure AI Document Intelligence or AWS Textract.

Evaluate both approaches on the same held-out documents where possible. Compare field-level accuracy, table extraction quality, latency, cost per page, setup effort, and data privacy/on-premises considerations.

## Trade-offs and limitations

### Self-hosted OCR and open-source models

**Strengths**
- Can run locally or in a controlled environment, giving more control over document handling.
- Avoids per-page managed API charges once the required infrastructure is available.
- Allows flexibility in OCR engines, preprocessing, models, and post-processing.

**Trade-offs**
- Requires environment setup, dependency management, model selection, and maintenance.
- OCR quality is sensitive to image quality, skew, noise, unusual fonts, and layout.
- Raw OCR text does not automatically provide reliable key-value relationships or correctly structured tables.
- A pretrained layout-aware checkpoint should not automatically be treated as a production invoice extractor; task-specific fine-tuning or careful inference/post-processing may be needed.
- Local compute and memory requirements can be significant for larger models.

**Where it can fail**
- Low-resolution, blurred, skewed, or noisy scans.
- Forms where values are far from their labels.
- Tables with merged cells, repeated headers, or multi-page continuation.
- Unfamiliar templates or document types not represented in the model's training data.

### Managed cloud document-intelligence services

**Strengths**
- Provide ready-made document analysis workflows, including invoice/receipt extraction and form/table features.
- Reduce the amount of OCR infrastructure and model-serving code to maintain.
- Can return structured fields and table-related information rather than only raw text.

**Trade-offs**
- Require cloud credentials, permissions, network access, and service-specific setup.
- Usually incur per-page or usage-based charges; check current provider pricing before larger evaluations.
- Documents are sent to an external service, so data residency, contractual terms, retention, and organizational privacy requirements must be reviewed.
- Provider-specific response formats need normalization before results can be compared or passed downstream.

**Where it can fail**
- Unusual layouts or fields that do not match the service's expected schema.
- Merged cells, complex multi-page tables, rotated pages, or ambiguous line-item relationships.
- Poor-quality scans, handwriting, or uncommon content.
- Incorrect field mapping or post-processing can make a useful API response appear incomplete.

## Comparison matrix

Fill this table with actual measurements from your own runs. Do not invent scores or costs.

| Dimension | Self-hosted approach | Managed service |
|---|---|---|
| Accuracy | Measure on labelled held-out documents | Measure on the same held-out documents |
| Table extraction | Record cell/row errors and layout limitations | Record cell/row errors and layout limitations |
| Latency | Record runtime and hardware | Record API request latency |
| Cost per page | Estimate compute/infrastructure cost and state assumptions | Use applicable provider pricing and actual usage |
| Setup and maintenance | Record installation, model, and maintenance effort | Record resource, credentials, permissions, and API setup |
| Privacy / on-prem | Can run in a controlled environment; verify deployment | Documents are sent to the provider; review data handling and residency |

**Evaluation note:** Simple cell-level precision/recall/F1 is useful for a small exercise, but it is not the same as a full table-structure metric such as TEDS. A tiny synthetic held-out set is a learning exercise, not a robust benchmark. State these limitations when presenting results.

## Module status and evidence

Keep the five day notebooks and outputs generated by running them in this repository.

Suggested evidence:
- **Day 1:** OCR text outputs and before/after preprocessing examples.
- **Day 2:** Donut raw output and parsed JSON output.
- **Day 3:** Raw managed-service responses and normalized JSON, if Azure or AWS access is available.
- **Day 4:** Synthetic hard-case PDF/images, common-schema JSON, and normalization summary.
- **Day 5:** Held-out labels, prediction files, evaluation CSVs, comparison matrix, and report.

**Important:** If cloud credentials were unavailable, mark the managed-service exercises as not run. Synthetic examples and schema demonstrations are not managed-service predictions and must not be reported as measured cloud extraction results. A synthetic normalization example is also not evidence that an OCR model extracted those values.

## Connection to later modules

- **Module 5 — VLM comparison:** Document examples and failure cases from this module provide a basis for comparing vision-language models on document understanding.
- **Module 6 — RAG ingestion:** Normalized JSON and extracted text provide structured and unstructured inputs for a document ingestion and retrieval pipeline. Preserve source-document references and metadata so extracted content can be traced back to its origin.

## Completion checklist

- [ ] Day 1: OCR engines and preprocessing experiments run.
- [ ] Day 2: Donut inference and structured output inspected.
- [ ] Day 3: At least one managed service tested, or access limitation documented.
- [ ] Day 4: Hard-case examples and common JSON schema exercises completed.
- [ ] Day 5: Held-out evaluation completed and limitations documented.
- [ ] Comparison matrix filled only with measured or clearly labelled estimated values.
- [ ] Self-hosted and managed-service outputs saved, or unavailable components clearly marked.
- [ ] Trade-offs, failure cases, privacy, and cost considerations documented.

**Estimated effort:** 32–40 hours full-time.
