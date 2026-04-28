---
# ==========================================
# LEVEL 1: YAML FRONTMATTER (Always Loaded)
# This is the lightweight routing layer. It tells the orchestrator WHEN to load Level 2.
# ==========================================
name: revocable-trust-extractor
description: Extracts Roles, Trust Information, and conditional Distributions from estate planning documents. Trigger this skill when handling a Revocable Trust or when asked to "analyze trust distributions" or "extract estate entities."
---

# ==========================================
# LEVEL 2: SKILL BODY (Loaded on Trigger)
# This is the execution workflow. It instructs the agent on HOW to do the job and WHEN to fetch Level 3 files.
# ==========================================

# Revocable Trust Extractor Workflow

You are an agent responsible for extracting structured data from Revocable Trust documents. You must follow this sequential workflow and utilize your file-reading tools to access necessary reference materials.

## Step 1: Entity Resolution & Context Gathering
Before analyzing distributions, you must identify the key entities (Settlor, Grantor, Trustees). 
- If you encounter ambiguous legal terminology regarding roles, **use your file-reading tool to open `references/legal-definitions.md`** to understand how to resolve them.
- Fetch the "Definitions" or "Preamble" chunks from the trust document first.

## Step 2: Distribution Tracing
Trace the financial routing. You must explicitly separate scenarios based on survivor contingencies.
- If you are unsure how to format or map complex "if X dies first" or "if X dies second" scenarios, **use your file-reading tool to open `references/distribution-contingencies.md`** for domain-specific routing rules.
- Trace all references (e.g., if Article IV references Schedule A, fetch Schedule A).

## Step 3: Schema Mapping and Output
You must output a strictly validated JSON object. 
- **Use your file-reading tool to open `references/schema.json`**. 
- Map your extracted entities and distributions exactly to the keys provided in that schema. Do not invent new keys. 

## Step 4: Iterative Refinement
Review your drafted JSON against the document context:
- Are all contingencies accounted for?
- Are execution dates present?
If verification fails, fetch additional document chunks to fill in the missing data before returning the final payload.