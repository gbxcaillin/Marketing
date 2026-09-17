---
name: marketing-audit
description: "Audit any company's marketing collateral against a 24-skill framework, then turn it into operating systems. Use this skill whenever someone uploads marketing materials (PDFs, slides, docs, training decks, course content, SOPs) and wants them analysed, audited, summarised, or gap-checked — even if they just say 'look through these' or 'what are we missing.' Also use when someone asks to build training material, a playbook, SOPs, or supplementary content from existing marketing assets, or wants a Zeely-style pipeline (product link → creatives → campaign launch → sales page → performance loop) built from their own frameworks. Works for agencies, course creators, membership programs, SaaS, e-commerce — any business with marketing collateral to assess."
---

# Marketing Audit

Turn a pile of marketing collateral into four structured deliverables:
1. **Playbook** — everything the material teaches, organised by topic
2. **Gap Analysis** — what's covered and what's missing against a 24-skill marketing framework
3. **Training Supplement** — ready-to-use training modules for every gap
4. **Execution Systems** — nine repeatable operating systems (modelled on Zeely's product-link-to-campaign pipeline) built from the company's own frameworks

## When to use this skill

- Someone uploads marketing PDFs, slide decks, training docs, or course material
- Someone asks "what are we missing" or "audit our marketing"
- Someone wants existing material turned into a structured playbook
- Someone needs training material built for skill gaps
- Someone says "ingest," "summarise," "gap analysis," or "what skills do we cover"
- Someone wants their marketing knowledge turned into SOPs, systems, or a launch pipeline ("like Zeely," "so anyone on the team can run it")

## The 24-Skill Marketing Framework

This is the constant. Every audit maps collateral against these 24 skills, grouped into seven pipeline stages. Read `references/framework.md` for the full framework with definitions of what "strong," "partial," and "missing" mean for each skill.

**Pipeline stages:**
1. **Who** → Customer Avatar & Psychographics
2. **What to say** → Ad Content Creation, Copywriting, Creative Diversity, Neuromarketing & Persuasion
3. **Where to run it** → Ad Targeting & Audiences, Campaign Structure & Account Setup, Google Search Campaigns, Retargeting
4. **Convert** → Conversion Funnels, Landing Page Optimisation, Landing Page Copywriting
5. **Nurture & close** → Nurture Sequences, Show Rates & Sales Process, CRM & Pipeline, A/B Testing
6. **Measure** → Lead Quality Tracking, Reporting & KPI Dashboards, Budget & Unit Economics
7. **Grow** → Offer Creation & Positioning, Post-Sale & Referral Loop, Email Deliverability, Ad Policy & Compliance, Video & Image Production

## Workflow

### Phase 1: Ingest

1. **Find the collateral.** Scan the working directory (or user-specified path) for PDFs, PPTX, DOCX, and other document files. List what you found and confirm with the user.

2. **Read everything.** Read each document systematically. For PDFs, read in page batches (e.g., pages 1-10, 11-20). For large files, don't skip pages — the gap analysis depends on comprehensive coverage.

3. **Deduplicate.** Compare title slides and first-page content across documents. Flag duplicates (same content, different filename) and skip them. Tell the user which files are duplicates.

4. **Extract per document:**
   - Title and author/presenter (if identifiable)
   - Topic area (map to the 24-skill framework)
   - Key frameworks, models, or processes taught
   - Actionable takeaways (specific things someone could implement)
   - Any formulas, checklists, or step-by-step methods

### Phase 2: Playbook

Build a single HTML artifact that organises everything the collateral teaches. This is the reference document — if someone only reads this, they should get 80% of the value of the original material.

**Structure:**
- Group content by topic, not by source document
- Each topic section gets: a summary paragraph, key frameworks in cards/callouts, and actionable takeaways as bullet points
- Credit the source (speaker name, document title) where identifiable
- Include a sticky table of contents for navigation
- Footer showing how many documents were ingested

**Design guidance:** Load the `artifact-design` skill before building. The playbook is a reference document — it should be polished and readable but not over-designed. Warm, professional palette. Readable serif or sans-serif body type. Card-based layout for frameworks. Dark/light theme support.

Publish as an artifact and save to the repo as `playbook.html`.

### Phase 3: Gap Analysis

Map every skill in the 24-skill framework against what the collateral actually covers. Read `references/framework.md` for the detailed criteria.

**For each skill, determine coverage:**
- **Strong** — the collateral teaches this skill with frameworks, examples, and actionable steps
- **Partial** — the collateral touches on this skill but lacks depth, misses key subtopics, or only covers one angle
- **Missing** — the collateral doesn't address this skill at all

**Build the gap analysis artifact:**
- Visual pipeline showing the 7 stages and which skills sit where
- Stats summary (X strong, Y partial, Z missing)
- Each skill listed with its coverage rating and a 1-2 sentence explanation of what is or isn't covered
- For partial skills, specify exactly what's missing
- A summary note identifying the biggest gaps and why they matter

**Design guidance:** Load `artifact-design`. This is an analytical document — use a distinct palette from the playbook so the two artifacts feel like companions, not copies. Colour-code coverage levels (e.g., green/amber/red or similar). Consider a dark-ground design for contrast with the likely light-ground playbook.

Publish as an artifact and save to the repo as `gap-analysis.html`.

### Phase 4: Training Supplement

Build training modules for every skill rated **Missing** or **Partial**. These are the modules that fill the gaps — if someone reads the playbook and then reads the supplement, they have complete coverage of all 24 skills.

Read `references/training-patterns.md` for the module structure and content guidance for each skill area.

**Each module contains:**
1. **Why this matters** — connects the gap back to the pipeline (what breaks without this skill)
2. **Core framework** — the main model or process for this skill (named, structured, memorable)
3. **Practical detail** — formulas, checklists, tables, or step-by-step instructions
4. **Common mistakes** — what people get wrong (drawn from the gap, not generic)
5. **Exercise** — a hands-on task the reader can complete with their own business

**Design guidance:** Load `artifact-design`. The supplement is educational material — optimise for readability. Numbered modules, clear section breaks, callout boxes for frameworks and warnings. A distinct design from both the playbook and gap analysis.

Publish as an artifact and save to the repo as `training-supplement.html`.

### Phase 5: Execution Systems

Knowledge doesn't run campaigns. This phase converts the playbook and supplement into nine operating systems a team runs the same way every time, modelled on Zeely's pipeline: product link in, creatives and copy generated in batch, campaign launched from one screen with a calculated budget, compliance checked, sales page generated, performance managed on a fixed cadence. Zeely's value is that every operator decision is pre-decided by a system; that's what to reproduce, using the company's own frameworks instead of generic AI output.

Read `references/execution-systems.md` for the nine systems, their SOP structure, decision rules, the 24-skill mapping, and the artifact spec.

**How to fill each system:**
- Where the playbook already has a framework for a step (their hook formula, their targeting hierarchy, their testimonial structure), name it and use it. The system should feel like *theirs*, codified.
- Where the gap analysis rated a skill missing or partial, use the training supplement's framework for that step and say so.
- Decision rules are the product. Every "it depends" the operator would face gets a pre-made answer (thresholds, minimums, kill rules, cadences).

**Tool hooks:** If the AdWhispr ad connector (or similar ad-platform tooling) is available in the session, note in each system which tool performs the step (competitor discovery, brief generation, campaign launch, budget changes, performance pulls). If not, write the manual equivalent. The SOP works either way.

Publish as an artifact and save to the repo as `execution-systems.html`.

### Phase 6: Commit & Deliver

1. Save all four HTML files to the repo
2. Commit with a descriptive message
3. Push to the current branch
4. Summarise what was found: how many documents ingested, coverage stats, the biggest gaps, and which systems are ready to run on day one

## Adapting to the Company

The framework is universal but the training content should be contextual. When writing training supplement modules:

- **Use the company's language.** If their collateral talks about "discovery calls" not "sales calls," use their term.
- **Reference their existing strengths.** "Your nurture sequence framework is strong — this budget module shows how to calculate whether those nurtured leads are profitable."
- **Match their business model.** A SaaS company needs different offer-creation guidance than a coaching business. An e-commerce brand needs different campaign structure than a lead-gen agency. Adapt the frameworks in each module to fit.
- **Don't repeat what they already cover well.** If their collateral has excellent copywriting training, the supplement shouldn't re-teach copywriting basics — it should only fill the specific gaps identified.

## If the user asks for just one phase

Sometimes people only want part of the workflow:
- "Summarise these" → Phase 1 + 2 only (ingest and playbook)
- "What are we missing" → Phase 1 + 3 only (ingest and gap analysis)
- "Build training for our gaps" → Assumes Phase 1-3 are done; do Phase 4 only
- "Turn this into systems / SOPs / a launch pipeline" → Assumes Phase 1-2 are done (needs the playbook's frameworks); do Phase 5 only
- "Full audit" → All phases
