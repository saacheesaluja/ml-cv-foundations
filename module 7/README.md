# Module 7 --- Agentic Multimodal RAG

## Overview

Module 7 builds an agentic layer on top of the multimodal retrieval
pipeline developed in Module 6. The goal is to move from a mostly fixed
retrieval workflow to a bounded agent that can plan tasks, select tools,
retrieve evidence across modalities, retry weak searches, and produce
answers with traceable sources.

The module emphasizes practical tool orchestration, query planning,
iterative retrieval, text-and-image queries, and evaluation against a
non-agentic baseline.

## Learning objectives

By the end of this module, you should be able to:

-   Explain an agent loop and the role of tools in a ReAct-style
    workflow.
-   Wrap document parsing, retrieval, metadata search, image captioning,
    and visual question answering behind tool interfaces.
-   Decompose complex questions into smaller tasks and route them to
    suitable tools and modalities.
-   Reformulate weak queries, retry retrieval within a bounded loop, and
    verify source provenance.
-   Combine a user-provided image and text query to retrieve related
    document visuals and cross-reference textual evidence.
-   Evaluate retrieval and answer quality using precision, recall, fact
    coverage, citation checks, and human review.
-   Apply guardrails that limit tool calls, discourage unsupported
    claims, and make failures visible.

## Notebook sequence

  ---------------------------------------------------------------------------------------------------
  Day                     Notebook                                            Focus
  ----------------------- --------------------------------------------------- -----------------------
  1                       `day-1-agent-fundamentals-tool-design.ipynb`        Agent fundamentals,
                                                                              tool interfaces, tool
                                                                              registry, and a bounded
                                                                              ReAct-style loop

  2                       `day-2-query-planning-routing.ipynb`                Query decomposition,
                                                                              task planning, modality
                                                                              routing, evidence
                                                                              ledger, and synthesis

  3                       `day-3-multistep-retrieval-self-correction.ipynb`   Iterative retrieval,
                                                                              query reformulation,
                                                                              evidence sufficiency
                                                                              checks, provenance
                                                                              validation, and
                                                                              fallback retrieval

  4                       `day-4-text-image-agentic-queries.ipynb`            CLIP-based image/text
                                                                              retrieval, combined
                                                                              queries, text
                                                                              cross-referencing, and
                                                                              modality-aware evidence

  5                       `day-5-agentic-multimodal-rag-evaluation.ipynb`     Evaluation suite,
                                                                              baseline comparison,
                                                                              retrieval metrics,
                                                                              human review, and saved
                                                                              evaluation outputs
  ---------------------------------------------------------------------------------------------------

## Prerequisites and inputs

-   Python and Google Colab.
-   The Module 6 sample document: `sample_multimodal_invoice.pdf`.
-   Module 6 retrieval output: `day4_retrieval_results.json`.
-   For image-based exercises, the extracted-images ZIP created in
    Module 6 (or the individual extracted image files).
-   A GPU is useful for CLIP workloads in Day 4, but the lightweight Day
    1--3 workflows and Day 5 evaluation harness do not require one.

Upload only the files requested by each notebook. If a notebook accepts
optional artifacts, it can run with the available inputs, but the
resulting evaluation or retrieval coverage may be limited.

## What each day produces

### Day 1 --- Agent fundamentals and tool design

Introduces a small tool interface around document parsing, retrieval,
metadata search, image-caption lookup, and VQA scaffolding. It registers
tools and exercises a bounded deterministic ReAct-style loop, including
a tool trace.

**Important limitation:** The captioning and VQA tools in this
introductory notebook are scaffolds/placeholders. They do not perform
full vision-language-model inference.

### Day 2 --- Query planning and routing

Breaks a complex request into tasks, chooses appropriate tools and
modalities, executes the plan, and collects evidence in a provenance
ledger. It includes a fallback path when the initial retrieval is
insufficient.

**Important limitation:** Image discovery in this notebook can list
candidate image files, but listing a file does not interpret its visual
contents or establish its page association. Visual claims require actual
visual inspection or a vision-language model.

### Day 3 --- Multi-step retrieval and self-correction

Runs retrieval iteratively, reformulates weak queries, checks evidence
sufficiency, validates source references, and records the retrieval
trace. A deliberately constrained first attempt demonstrates a
fallback/recovery path.

**Important limitation:** The evidence-sufficiency check is heuristic
rather than an independent semantic judge. Review the retrieved passages
and source references before treating a result as verified.

### Day 4 --- Text-and-image agentic queries

Uses CLIP embeddings to retrieve document images from a text query, an
image query, or a combined text-and-image query. It then
cross-references retrieved visual candidates with text evidence and
records the retrieval process.

**Important limitation:** CLIP similarity measures embedding similarity;
it does not itself explain or verify the contents of an image. Treat
retrieved images as candidates for visual confirmation. This notebook is
a retrieval workflow, not a complete VLM-based visual reasoning system.

### Day 5 --- Agentic multimodal RAG evaluation

Provides a small evaluation suite covering text-only, image-related, and
multi-hop cases. It computes retrieval-oriented metrics, supports
comparison with a one-pass baseline, records tool-use information, and
includes a human-review rubric for correctness, grounding, citations,
visual relevance, and multi-hop completeness.

**Important limitation:** Some cases require human review and score
entry. Lexical overlap or a retrieval metric alone does not establish
semantic answer correctness. The expected facts in the sample suite
should be checked against the actual document and corpus before scoring.

## Evaluation and success criteria

The module's intended success gate is:

1.  A majority of the text-only, text-and-image, and multi-hop
    evaluation cases are judged correct by human review.
2.  Answers are grounded in retrieved evidence and their
    citations/source references are valid.
3.  At least one case demonstrates recovery from an inadequate first
    retrieval through query reformulation or a fallback attempt.
4.  Tool calls remain bounded and relevant to the query.

Do not mark the module as fully successful until the evaluation cases
have been run, human scores have been entered, and a self-correction
trace has been retained.

## Suggested GitHub folder structure

``` text
module 7/
├── README.md
├── day-1-agent-fundamentals-tool-design.ipynb
├── day-2-query-planning-routing.ipynb
├── day-3-multistep-retrieval-self-correction.ipynb
├── day-4-text-image-agentic-queries.ipynb
└── day-5-agentic-multimodal-rag-evaluation.ipynb
```

Keep generated evaluation JSON/CSV files and trace artifacts in the
repository only if they are useful, reproducible, and do not contain
private data.

## Relationship to Module 6

Module 6 established the non-agentic multimodal RAG baseline: document
ingestion, visual representation, multimodal indexing, retrieval, and
grounded generation. Module 7 builds planning, routing, iterative
retrieval, tool orchestration, and evaluation around those capabilities.

The Day 5 evaluation notebook is designed to compare the agentic
workflow with a one-pass retrieval baseline. Ensure both are evaluated
on comparable queries and evidence before drawing conclusions.

## Final deliverables

-   The five completed notebooks in the `module 7/` GitHub folder.
-   Evaluation outputs (JSON/CSV) from Day 5, where available.
-   At least one saved trace showing a failed or weak first-pass
    retrieval followed by recovery.
-   Human-review scores and a short note explaining remaining
    limitations.
-   A concise architecture summary describing the agent loop, tools,
    routing, evidence flow, and guardrails.

## Key takeaway

Agentic multimodal RAG is not simply retrieval plus an LLM. Its value
comes from selecting the right tools, decomposing a request, gathering
evidence across modalities, recognizing when evidence is weak, retrying
safely, and making the final answer auditable.
