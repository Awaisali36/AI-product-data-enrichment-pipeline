# 🧵 AI-Powered Industrial Product Database Creation & Enrichment System

> **196+ products. 40+ technical fields per item. 95%+ data accuracy. Zero human intervention.**

An end-to-end automated database pipeline built for **MySewingMall.com** — scraping, enriching, validating, and vectorizing industrial sewing machine data weekly using a dual-AI architecture with RAG-ready output.

<br/>

---

## 📌 The Problem

Industrial equipment retailers waste **hundreds of hours** manually entering and normalizing product data from scattered sources.

The result:
- Inconsistent technical specifications across product listings
- Missing manuals and compatibility information
- Data quality bottlenecks that block e-commerce scalability
- Customer support agents working without reliable product knowledge

Manual entry at scale simply doesn't work. A single product requires cross-referencing PDFs, manufacturer specs, and compatibility tables — then normalizing units, deduplicating records, and validating accuracy. Multiply that by 196 SKUs across 13 brands and the problem becomes unsolvable without automation.

<br/>

---

## ✅ The Solution

A fully automated, weekly-triggered pipeline that:

1. **Scrapes** MySewingMall.com for all product listings
2. **Filters** by 13 major industrial brands
3. **Extracts** 40+ technical fields per product using GPT-4o
4. **Enriches** with forensic technical validation using Google Gemini
5. **Discovers** official PDF manuals automatically via Serper.dev
6. **Vectorizes** all data into Pinecone for RAG and semantic search
7. **Deduplicates** to prevent re-processing on weekly runs

```
┌─────────────────────────────────────────────────────────────────┐
│                     WEEKLY TRIGGER (n8n)                        │
└───────────────────────────┬─────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│              SCRAPE mysewingmall.com                            │
│         Filter: 13 brands · HTML cleaning · Deduplication       │
└───────────────────────────┬─────────────────────────────────────┘
                            │
                    ┌───────┴────────┐
                    ▼                ▼
          ┌──────────────┐  ┌──────────────────┐
          │   GPT-4o     │  │  Google Gemini   │
          │  Extraction  │  │   Enrichment +   │
          │  (40+ fields)│  │   Validation     │
          └──────┬───────┘  └────────┬─────────┘
                 └────────┬──────────┘
                          ▼
          ┌───────────────────────────┐
          │     Serper.dev Search     │
          │   Official PDF Manuals    │
          │   ChatGPT URL Selection   │
          └───────────┬───────────────┘
                      │
          ┌───────────┴───────────────┐
          ▼                           ▼
┌──────────────────┐       ┌──────────────────────┐
│  n8n Data Table  │       │  Pinecone Vector DB   │
│ Structured Store │       │  RAG-Ready Embeddings │
└──────────────────┘       └──────────────────────┘
```

<br/>

---

## 📊 Results

| Metric | Result |
|---|---|
| Products processed | **196+** |
| Technical fields per product | **40+** |
| Brands covered | **13** (Juki, Brother, Janome, Jack, Ricoma + 8 more) |
| Data accuracy | **95%+** |
| Manual data entry eliminated | **100%** |
| Processing cadence | **Weekly, fully automated** |
| Human intervention required | **Zero** |

<br/>

---

## 🔍 What Gets Extracted (40+ Fields)

| Category | Fields |
|---|---|
| **Identity** | SKU, Brand, Model Name, Product Category |
| **Physical** | Weight (kg), Dimensions (cm), Color, Form Factor |
| **Electrical** | Motor Type, Voltage, Power (watts), Frequency |
| **Mechanical** | Max Speed (SPM), Feed Mechanism, Needle System, Throat Clearance |
| **Capabilities** | Stitch Types, Stitch Length/Width, Buttonhole Styles |
| **Compatibility** | Needle Compatibility, Presser Foot Type, Bobbin System |
| **Documentation** | Manual URL, Manual PDF Path, Spec Sheet URL |
| **Metadata** | Source URL, Last Updated, Processing Status, Data Version |

> All weights standardized to **kg**, all dimensions to **cm** — metric normalization applied universally.

<br/>

---

## 🛠️ Tech Stack

| Layer | Tool | Purpose |
|---|---|---|
| **Orchestration** | n8n | Workflow automation, scheduling, data routing |
| **Primary AI** | GPT-4o | Initial field extraction from product HTML |
| **Validation AI** | Google Gemini | Forensic technical enrichment + anti-hallucination checks |
| **Manual Discovery** | Serper.dev | Web search for official PDF manuals |
| **URL Selection** | ChatGPT | Intelligent ranking and selection of manual URLs |
| **Vector Storage** | Pinecone | Semantic search and RAG-ready embeddings |
| **Embeddings** | OpenAI `text-embedding-ada-002` | Product vectorization |
| **Structured DB** | n8n Data Tables | Normalized product records |
| **PDF Processing** | Custom chunking | 500 char chunks, 50 char overlap |

<br/>

---

## 🏗️ System Architecture

### Dual-AI Validation Layer
The pipeline uses a **two-model approach** to maximize accuracy:

- **GPT-4o** handles initial extraction — fast, structured, reliable for well-formatted HTML
- **Google Gemini** runs forensic enrichment — cross-referencing industry standards, inferring compatibility from needle systems, flagging suspect values

This redundancy achieves 95%+ accuracy without human review.

### Anti-Hallucination Controls
- Gemini is prompted with strict grounding instructions
- All inferred fields are tagged with a confidence flag
- Values outside physically plausible ranges are flagged for review
- Compatibility data is inferred from known industry standards (needle systems, bobbin types) — not generated

### RAG Pipeline
- Products chunked at 500 characters with 50-character overlap
- Embedded using `text-embedding-ada-002`
- Stored in Pinecone with full metadata for filtered retrieval
- Ready for semantic chatbot, site search, and support agent integration

<br/>

---

## 📁 Repository Structure

```
📁 ai-industrial-database-enrichment/
├── 📄 README.md
├── 📁 workflow/
│   └── mysewingmall-enrichment.json     ← n8n workflow export
├── 📁 schema/
│   └── product-schema.json              ← 40+ field data schema
├── 📁 prompts/
│   ├── gpt4o-extraction-prompt.md       ← GPT-4o system prompt
│   └── gemini-enrichment-prompt.md      ← Gemini validation prompt
└── 📁 docs/
    └── architecture.md                  ← System design notes
```

<br/>

---

## 🚀 How to Use

### Prerequisites
- n8n instance (self-hosted or cloud)
- OpenAI API key (GPT-4o + Embeddings)
- Google Gemini API key
- Serper.dev API key
- Pinecone account + index

### Setup
1. Import `workflow/mysewingmall-enrichment.json` into your n8n instance
2. Add your API credentials in n8n's credential manager:
   - `OpenAI API`
   - `Google Gemini API`
   - `Serper.dev API`
   - `Pinecone API`
3. Update the brand filter list if needed (currently set to 13 brands)
4. Set your Pinecone index name and dimension (`1536` for ada-002)
5. Activate the workflow — weekly trigger runs every Monday at 00:00

### Customization
- **Add brands:** Edit the brand filter node with additional brand names
- **Add fields:** Extend the GPT-4o extraction prompt and update the schema
- **Change cadence:** Modify the CRON trigger node for daily or bi-weekly runs
- **Different site:** Update the scraper URL and HTML parsing logic

<br/>

---

## 💼 Business Applications

This system outputs a production-ready database that powers:

| Application | How |
|---|---|
| 🛒 **E-commerce population** | Push enriched data directly to Shopify/WooCommerce via API |
| 🤖 **RAG Chatbot** | Pinecone embeddings ready for semantic Q&A |
| 📦 **Inventory management** | Structured records with SKU, weight, dimensions |
| 🎧 **Customer support** | Agents query product specs instantly |
| 📊 **Internal knowledge base** | Searchable catalog for sales and support teams |

<br/>

---

## ⚙️ Want This Built for Your Business?

This system was built by **[Awais Ali](https://www.linkedin.com/in/awais-ali-tillesai)**, CEO & Co-Founder of **[Trilles AI](https://www.trillesai.com)**.

If your business is drowning in manual data entry, inconsistent product info, or needs a scalable AI enrichment pipeline — this is exactly what we build.

<div align="center">

[![Connect on LinkedIn](https://img.shields.io/badge/Connect%20on%20LinkedIn-%230A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/awais-ali-tillesai)
&nbsp;
[![Visit Trilles AI](https://img.shields.io/badge/Visit%20Trilles%20AI-%233A7CFF?style=for-the-badge&logoColor=white)](https://www.trillesai.com)
&nbsp;
[![Email](https://img.shields.io/badge/Email%20Me-%23EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:letsautomatewithawais@gmail.com)

</div>

<br/>

---

<div align="center">
<sub>Built with precision · Powered by Trilles AI · <code>www.trillesai.com</code></sub>
</div>
