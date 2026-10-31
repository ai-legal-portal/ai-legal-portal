**AI-POWERED LEGAL**  
**INFORMATION PORTAL**

FYP Proposal \+ Software Requirements \+ Technical Design \+ Implementation Roadmap

Prepared for: Final Year Project (FYP)

Domain: Pakistani legal information and document assistance

Proposed stack: Next.js, FastAPI, PostgreSQL, Python ML/NLP

Team size: 3 members

Target implementation period: 8 weeks

Planning budget: PKR 10,000 (subject to validation)

Version 1.1 \| Planning draft \| 22 October 2026

# **Document Control**

| Field | Details |
| :---- | :---- |
| Document purpose | Single master document with separately labeled sections for academic proposal, functional requirements, technical design, safety, testing, and implementation. |
| Audience | FYP supervisor, project team, evaluators, and approved legal-domain reviewers. |
| Status | Draft for review; confirm scope and academic format with the supervisor. |
| Important limitation | This is a project-planning document, not legal advice and not proof that the proposed features have already been implemented. |
| Revision history | v1.0 (16 October 2026): initial planning draft. v1.1 (22 October 2026): added F-11 voice input and read-aloud output and F-12 legal issue category and forum guidance, and upgraded F-09 to detailed, optional, step-by-step lawyer profiles. Requirements, design, data, AI/ML, testing, roadmap, budget, and risk sections were updated to match. |

# **How to Use This Document**

The document is organized into independent sections so readers can jump directly to the proposal, requirements, architecture, schedule, or evaluation plan. The proposed system is an academic prototype. Features marked 'Future/Stretch' are not commitments for the eight-week MVP.

# **Table of Contents**

* 1\. Executive Summary  
* 2\. Project Proposal  
* 3\. Problem Statement and Motivation  
* 4\. Aim, Objectives, and Research Questions  
* 5\. Scope and Feature Catalogue  
* 6\. Stakeholders and User Roles  
* 7\. Functional Requirements  
* 8\. Non-Functional Requirements  
* 9\. Use Cases and User Journeys  
* 10\. Proposed System Architecture  
* 11\. Technology Stack  
* 12\. Data Strategy and Governance  
* 13\. AI/ML Methodology  
* 14\. Database Design (Initial ERD Description)  
* 15\. API Design (Initial Specification)  
* 16\. UI/UX Screen Plan  
* 17\. Security, Privacy, and Legal Safety  
* 18\. Testing and Evaluation Plan  
* 19\. Eight-Week Implementation Roadmap  
* 20\. Team Responsibilities  
* 21\. Budget and Resource Plan  
* 22\. Risks and Mitigations  
* 23\. Deliverables and Definition of Done  
* 24\. Future Enhancements  
* 25\. Conclusion  
* Appendix A: Supervisor Decisions  
* Appendix B: Starter Reference List and Research Tasks

In Microsoft Word, an automatic table of contents can be added after finalizing heading styles: References → Table of Contents. Page numbers may change as the document is edited.

# **1\. Executive Summary**

The proposed AI-Powered Legal Information Portal is a web application intended to help users find and understand Pakistani legal information through searchable legal documents, a retrieval-augmented generation (RAG) chatbot with source citations, and document summarization. A lawyer directory with detailed, optional, step-by-step profiles may help users discover legal professionals, while any verification status will be shown only when supported by an actual verification process.

The core design principle is evidence-grounded assistance: answers should be generated from retrieved legal sources, show the relevant document and passage, and clearly state when reliable supporting material is unavailable. The portal is not intended to replace a qualified lawyer or make definitive predictions about case outcomes.

Users will be able to type or speak their questions in Urdu or English and listen to answers read aloud. When a user describes a problem, the portal will indicate which area of law it most likely belongs to (for example family, property, banking, or criminal) and which court or department usually handles it, citing a reviewed forum guide.

The eight-week MVP prioritizes document ingestion, search, a cited chatbot with text and voice input and read-aloud output, legal issue category and forum guidance, basic summarization, detailed lawyer profiles, access controls, and a demonstrable user interface. Case-outcome prediction, payments, advanced OCR, and production-scale confidential case-file processing are outside the committed MVP unless time, data, and supervision permit.

# **2\. Project Proposal**

## **2.1 Introduction**

Legal information is often spread across lengthy statutes, judgments, rules, and other documents. Non-specialist users may struggle to locate relevant passages, understand terminology, or compare related decisions. A search-and-assistance portal can make source material easier to discover and navigate, provided it preserves source context and avoids presenting generated text as authoritative legal advice.

## **2.2 Proposed Solution**

Develop a responsive web portal that indexes approved legal documents, retrieves relevant passages for a user query, and produces a concise answer with traceable citations. The system will also provide document search, basic summarization, a lawyer directory with optional step-by-step detailed profiles, voice input and read-aloud output for the chatbot, and guidance on the likely legal category and forum for a problem described by the user. The MVP will use a small, curated and permissioned dataset suitable for academic evaluation.

## **2.3 Proposed Novelty / Contribution**

* A practical RAG workflow for Pakistani legal documents with visible source-level citations.  
* A workflow that distinguishes public legal sources from confidential lawyer-provided case files.  
* A measured evaluation of retrieval and citation quality rather than relying only on subjective demonstrations.  
* Bilingual (Urdu/English) voice access to cited legal information, lowering the barrier for users who find typing or reading difficult.  
* Problem-to-forum guidance that maps a user's described issue to a legal category and the court or department that usually handles it, grounded in a reviewed forum guide.  
* A baseline legal-category classifier only if a sufficiently clean, labeled dataset is available.

## **2.4 Expected Outcomes**

* A working web prototype with document search and a cited legal-information chatbot.  
* A documented data-ingestion and retrieval pipeline.  
* Basic document summarization and a lawyer directory with detailed, optional, step-by-step profiles.  
* Voice input (speech-to-text) and read-aloud output (text-to-speech) for the chatbot in Urdu and English.  
* Legal issue category and forum guidance for problems described by users.  
* A test report covering extraction, retrieval, citations, safety behavior, and system usability.  
* A final FYP report, demonstration, and limitations/future-work discussion.

# **3\. Problem Statement and Motivation**

Users may find it difficult to locate the most relevant legal material in large collections of documents and to identify which passage supports a statement. Keyword search alone may miss semantically related passages, while a general-purpose chatbot may produce plausible but unsupported claims. Legal PDFs may also contain formatting problems, scans, Urdu text, and inconsistent metadata.

The project addresses these challenges by combining document processing, search, retrieval-augmented generation, and source citations. It also explicitly addresses the risks of hallucination, outdated material, OCR errors, privacy exposure, and overconfidence in automated legal predictions.

## **3.1 Problem Definition**

How can a small, affordable, and testable web system help users retrieve and understand approved Pakistani legal documents while making the evidence for generated answers visible and protecting confidential material?

## **3.2 Motivation**

The project provides an applied setting for information retrieval, NLP, software engineering, responsible AI, and human-centered design. Its academic value depends on a measurable evaluation and transparent limitations, not on claiming that AI can replace lawyers.

# **4\. Aim, Objectives, and Research Questions**

## **4.1 Aim**

To design, implement, and evaluate a web-based legal information portal that retrieves relevant Pakistani legal documents and generates source-grounded responses with citations.

## **4.2 Objectives**

1. Collect and organize a small set of approved public legal documents with source, date, jurisdiction, and document-type metadata.  
2. Implement PDF text extraction for text-based PDFs and report extraction failures clearly.  
3. Build document chunking, indexing, and retrieval for legal queries.  
4. Implement a RAG chatbot that displays citations and abstains when evidence is insufficient.  
5. Provide basic document summarization and a searchable document interface.  
6. Add voice input (speech-to-text) and read-aloud output (text-to-speech) to the chatbot for Urdu and English.  
7. Identify the likely legal category of a problem described by the user and the court or department that usually handles it, using a reviewed forum guide.  
8. Implement optional, step-by-step lawyer profiles with a completion indicator and an administrator verification workflow.  
9. Implement authentication/access boundaries appropriate to the prototype and separate public documents from restricted case files.  
10. Evaluate retrieval relevance, citation correctness, answer groundedness, extraction success, category-detection accuracy, speech-recognition accuracy, latency, and user feedback.  
11. Document limitations, risks, ethical considerations, and future work.

## **4.3 Research Questions**

* RQ1: How relevant are the passages retrieved for a set of representative legal queries?  
* RQ2: How often do chatbot claims have valid support in the cited source passages?  
* RQ3: How successfully does the ingestion pipeline extract usable text from the selected PDF dataset?  
* RQ4: What are the trade-offs among response quality, latency, and operating cost for the selected MVP architecture?  
* RQ5: How accurately does the system identify the legal category and usual forum for representative problem descriptions, and how does an LLM-based approach compare with a trained baseline classifier?  
* RQ6: How accurately are Urdu and English spoken questions converted to text, and how usable is read-aloud output for answers?

## **4.4 Success Criteria**

Thresholds must be agreed with the supervisor after a pilot test. Report actual results, dataset size, and evaluation procedure. Do not invent performance figures or present a small academic test as proof of legal reliability.

# **5\. Scope and Feature Catalogue**

## **5.1 Core MVP Features**

| ID | Feature | Description | Priority |
| :---- | :---- | :---- | :---- |
| F-01 | Legal document catalogue | Store approved documents with title, type, jurisdiction, date, source, and access classification. | Must |
| F-02 | PDF ingestion | Upload/ingest text-based PDFs, extract text, retain metadata, and flag extraction failures. | Must |
| F-03 | Document search | Search indexed documents and show title, metadata, and relevant snippets. | Must |
| F-04 | RAG chatbot | Retrieve relevant passages and generate an answer constrained by those passages. | Must |
| F-05 | Citations | Show document title and page/section or chunk reference for supporting passages. | Must |
| F-06 | Insufficient-evidence fallback | Say when sources do not support a confident answer; encourage professional advice for case-specific decisions. | Must |
| F-07 | Basic summarization | Generate a short summary of an approved document or selected text, with a notice to verify against the original. | Should |
| F-08 | Basic user access | Separate public access from authenticated or restricted functions as required by the prototype. | Must |
| F-09 | Lawyer directory | Detailed but optional lawyer profiles completed step by step (Upwork-style): only basic information is required; other sections are optional, with a completion indicator, per-field public/private choice, and a Verified label only after an administrator check (see 5.4). | Should |
| F-10 | Evaluation logging | Record non-sensitive evaluation outcomes, errors, and latency; avoid logging confidential content unnecessarily. | Should |
| F-11 | Voice input and read-aloud output | Users can type or speak questions in Urdu or English; speech is converted to text and shown for editing before sending. Each answer has a speaker button that reads it aloud, with play/pause/stop and speed control. | Should |
| F-12 | Legal issue category and forum guidance | From a described problem, identify the likely legal category and sub-issue, the court or department that usually handles it, and first steps, citing the reviewed forum guide; ask clarifying questions when unclear; link to matching lawyers (see 5.5). | Should |

## **5.2 Future / Stretch Features**

* Advanced OCR for scanned Urdu/English PDFs, with human verification of extracted text.  
* Transformer-based legal semantic reranking or fine-tuning, if data and compute permit.  
* Trained case-category classifier (TF-IDF \+ Logistic Regression) for comparison with the LLM-based category detection in F-12, if a labeled dataset is available.  
* Similar-case recommendations with transparent similarity signals.  
* Entity extraction for parties, dates, courts, statutes, and citations.  
* Consultation booking, reminders, payments, and mobile application.  
* Case complexity or urgency estimation after expert-defined labels and validation.  
* Case-outcome prediction only as a separate research experiment with adequate, representative, labeled data and explicit limitations.  
* Backend speech services (for example self-hosted Whisper for speech-to-text and neural Urdu text-to-speech) if browser speech quality is insufficient.  
* Lawyer profile Steps 4, 5, 6, and 8 (bio, education, experience, contact links) if they are not completed in the MVP.  
* Moderated client reviews on lawyer profiles.

## **5.3 Out of Scope for the Eight-Week MVP**

* Automated legal representation, filing, or advice that claims to be a lawyer.  
* Guaranteed case-win predictions or automated decisions about legal rights.  
* Ingestion of confidential client files without written permission, access control, and a clear data-handling protocol.  
* Claiming that a lawyer is verified without completing the stated verification process.  
* Public win rates, client names, or other unverifiable success claims on lawyer profiles.  
* Production-grade coverage of all Pakistani laws and all courts.

## **5.4 Lawyer Profile Completion Steps (F-09)**

Lawyer profiles follow a step-by-step completion model similar to freelance platforms such as Upwork. Only Step 1 is required; the remaining steps are optional and help lawyers present more detail if they wish.

| Step | Fields | Visibility | Weight |
| :---- | :---- | :---- | :---- |
| 1\. Basic information (required) | Full name, profile photo (optional), city/district, primary practice area(s), account email and phone | Name, photo, city, and practice areas public; account email and phone private | 20% |
| 2\. Professional credentials | Bar Council enrollment number, enrollment year, court level (Lower Courts / High Court / Supreme Court), Bar Association membership, licence or Bar card upload | Public once entered; Verified only after administrator check; uploads visible to administrators only | 20% |
| 3\. Practice details | Specializations (sub-areas), courts regularly appeared in, years of experience, languages spoken, practice type and firm name | Public | 15% |
| 4\. About and bio | Professional headline, short bio, approach to clients | Public | 10% |
| 5\. Education | Law degree(s), university, year; other certifications (for example mediation or arbitration) | Public | 10% |
| 6\. Experience | Previous positions, notable already-public reported judgments (citations only), publications or teaching | Public | 10% |
| 7\. Services and consultation | Services offered, consultation mode (in person, phone, online), office address, availability, fee range or contact for fees, pro bono availability | Public; office address at the lawyer's choice | 10% |
| 8\. Contact and links | Public phone, WhatsApp or email, website, LinkedIn | Lawyer's choice | 5% |

* Only Step 1 is required; every other step can be skipped, saved part-way, and completed later in any order.  
* A progress bar shows the completion percentage and suggests the next step (for example, Add your education to reach 75%).  
* Credential fields show Self-reported until an administrator checks them, then Verified with the date of the check.  
* Empty sections are hidden on the public profile, and the directory can be filtered by city, practice area, court level, language, consultation mode, pro bono, and verified only.  
* MVP: Steps 1, 2, 3, and 7\. Steps 4, 5, 6, and 8 are stretch items if time is short.

Fields intentionally excluded:

* Win rate or number of cases won: unverifiable, misleading, and restricted by Bar Council rules on advertising.  
* Client names or private case details: these would breach client confidentiality.  
* Public star ratings or reviews: risk of fake or defamatory content; a moderated version is future work.  
* CNIC number on the public profile: sensitive personal data; if needed for verification, keep it private and administrator-only.  
* Best lawyer or guaranteed-result claims: misleading, and the portal must not imply endorsement.

## **5.5 Legal Category and Forum Catalogue (F-12)**

Category detection chooses only from the fixed list below. The forum shown to the user comes from the reviewed forum guide (Section 12.5), not from model memory, and is presented as general information rather than legal advice.

| Category | Typical issues | Usual forum(s) |  |  |
| :---- | :---- | :---- | :---- | :---- |
| 1\. Family, marriage and custody | Khula/divorce, dower (Mehar), maintenance, custody, guardianship, visitation | Family Court; Union Council (Arbitration Council) for divorce notices and reconciliation |  |  |
| 2\. Property, land and rent | Urban property, illegal dispossession, partition, sale agreements, agricultural land, mutation (Intiqal), landlord | – | tenant disputes | Civil Court; Revenue officers/courts; Rent Controller |
| 3\. Crime, accidents and personal safety | Theft, assault, harassment, murder, bail | Police (FIR); Magistrate; Sessions Court |  |  |
| 4\. Corporate, business and banking | Company registration and compliance, shareholder disputes, loan recovery and default, anti-competitive practices | SECP (regulator); Banking Courts / High Court; Competition Commission of Pakistan (regulator) |  |  |
| 5\. Labour and employment | Wrongful termination, unpaid wages, union matters, civil-servant service matters | Labour Court; Federal/Provincial Service Tribunals; NIRC |  |  |
| 6\. Consumer rights | Defective products, poor services, misleading advertisements | Consumer Court (provincial) |  |  |
| 7\. Tax and customs | Tax notices and assessments, customs seizures and valuation | FBR / provincial revenue authorities; Appellate Tribunal Inland Revenue; Customs Appellate Tribunal |  |  |
| 8\. Cybercrime and electronic fraud | Online harassment, identity theft, banking/ATM scams, hacking | Cybercrime investigation agency (NCCIA, formerly FIA Cyber Crime Wing); PECA courts |  |  |
| 9\. Civil, contract and money recovery | Breach of contract, recovery of money, injunctions, declarations | Civil Court |  |  |
| 10\. Inheritance and succession | Succession certificates, distribution of estate, inheritance disputes | Civil Court; NADRA succession facilitation where available |  |  |
| 11\. Constitutional and government matters | Unlawful action by public bodies, fundamental rights | High Court (writ petitions) |  |  |
| 12\. Ombudsman complaints | Maladministration by federal agencies, banking service complaints, insurance complaints, workplace harassment | Wafaqi Mohtasib; Banking Mohtasib; Insurance Ombudsman; Federal/Provincial Ombudsperson for harassment |  |  |
| 13\. Other / unclear | Not enough detail to classify | Ask clarifying questions; suggest consulting a lawyer |  |  |

Points to verify with the supervisor or a legal reviewer before the forum guide is indexed:

* Arbitration Councils for divorce and reconciliation operate under the Muslim Family Laws Ordinance, 1961; Family Courts operate under the Family Courts Act, 1964\.  
* Rent matters are heard by the Rent Controller under the applicable provincial rent law.  
* Cybercrime investigation functions previously handled by the FIA Cyber Crime Wing have been assigned to the National Cyber Crime Investigation Agency (NCCIA); confirm the current position.  
* SECP and the Competition Commission of Pakistan are regulatory bodies, not courts.  
* Whether a banking matter goes to a Banking Court or the High Court depends on the claim amount; confirm the current thresholds.  
* Forums and procedures can differ by province; record the province for each entry.

# **6\. Stakeholders and User Roles**

| Role | Needs | Permitted MVP actions |
| :---- | :---- | :---- |
| Visitor / public user | Find general legal information and documents. | Search public documents; ask questions by text or voice against the public corpus if enabled; listen to answers; receive category and forum guidance. |
| Registered user | Save/use permitted portal functions. | Use account-bound features; no access to restricted files by default. |
| Lawyer / approved contributor | Share authorized information or profile details. | Build own profile step by step (only basic information required), choose field visibility, and submit credentials for verification; upload only material explicitly authorized for this purpose. |
| Administrator / curator | Maintain corpus quality and user access. | Approve public documents, manage metadata, verify lawyer credentials, maintain the forum guide, and review system issues. |
| Project team / evaluator | Test and evaluate the prototype. | Use synthetic/demo data and authorized datasets; access only as required. |

Role definitions must be implemented in backend authorization checks; hiding a button in the frontend is not sufficient access control.

# **7\. Functional Requirements**

| ID | Requirement |
| :---- | :---- |
| FR-01 | The system shall store approved legal documents and their source metadata. |
| FR-02 | The system shall extract text from supported text-based PDFs and report failed or low-quality extraction. |
| FR-03 | The system shall split extracted text into retrievable chunks while preserving document/page references where possible. |
| FR-04 | The system shall return relevant document snippets for a search query. |
| FR-05 | The chatbot shall retrieve supporting passages before generating an answer. |
| FR-06 | The chatbot shall display citations that map to actual indexed source passages. |
| FR-07 | The chatbot shall state that evidence is insufficient when retrieval does not meet a chosen threshold. |
| FR-08 | The system shall provide basic summarization for supported document text. |
| FR-09 | The system shall enforce access rules for restricted documents on the backend. |
| FR-10 | The system shall distinguish public judgments/statutes from private or lawyer-provided case files. |
| FR-11 | The system shall expose lawyer verification status only according to a documented verification workflow. |
| FR-12 | The system shall support a basic evaluation process and record non-sensitive system metrics. |
| FR-13 | The system shall provide clear errors for unsupported files, empty extraction, and service failures. |
| FR-14 | The chatbot shall accept questions typed as text or spoken by voice. |
| FR-15 | The system shall convert Urdu and English speech to text and show the transcript for the user to edit before sending. |
| FR-16 | The system shall show clear messages when speech is unclear, microphone permission is denied, or the browser does not support voice; text input shall always remain available. |
| FR-17 | The system shall not store voice recordings after transcription unless the user gives consent. |
| FR-18 | Lawyer registration shall require only basic information; all other profile sections shall be optional and completable step by step. |
| FR-19 | The system shall display a profile completion percentage and suggest next steps. |
| FR-20 | Lawyers shall be able to set sensitive profile fields as public or private. |
| FR-21 | Credential fields shall display Verified only after an administrator check that records what was checked, by whom, and when; otherwise they shall display Self-reported. |
| FR-22 | Verification documents shall be visible to administrators only and never shown publicly. |
| FR-23 | The system shall classify a described problem into one or more categories from the fixed legal-category list, including an Other / unclear option. |
| FR-24 | The system shall show the court or department that usually handles the identified category and suggested first steps, sourced from the reviewed forum guide with citations. |
| FR-25 | The system shall indicate its confidence and use tentative wording when confidence is low. |
| FR-26 | The system shall ask clarifying questions (for example province, urban or agricultural land, government or private employment, amount involved) when the forum depends on missing facts. |
| FR-27 | The system shall display emergency help information first when a user describes immediate danger or violence. |
| FR-28 | The system shall offer a link to the lawyer directory filtered by the identified practice area. |
| FR-29 | Each chatbot answer shall have a speaker button that reads the answer aloud on request. |
| FR-30 | Users shall be able to play, pause, stop, and change the speed of read-aloud playback. |
| FR-31 | Read-aloud output shall use a voice matching the answer language (Urdu or English). |
| FR-32 | If read-aloud output is unavailable, the system shall show a clear message and the text answer shall remain fully usable. |

# **8\. Non-Functional Requirements**

| Area | Requirement / design target |
| :---- | :---- |
| Security | Authentication where needed, backend authorization, secure secrets, input validation, upload limits, and least-privilege access. |
| Privacy | Collect the minimum necessary data; redact personal identifiers in test data; document permissions and retention; discard voice recordings after transcription unless the user consents. |
| Reliability | Handle malformed PDFs and unavailable model/API services gracefully; return actionable error messages. |
| Traceability | Citations must resolve to real indexed source material; show source date and metadata where available. |
| Usability | Responsive interface, readable citations, clear Urdu/English language handling, text or voice input with read-aloud output, and simple navigation. |
| Maintainability | Separate frontend, API, ingestion, retrieval, and model components; use environment variables and migrations. |
| Performance | Measure retrieval and response latency on the project dataset; do not promise a target until measured. |
| Accessibility | Use legible typography, keyboard-friendly controls, and appropriate contrast. Voice input and read-aloud output support users who find typing or reading difficult. |
| Portability | Provide setup instructions, dependency versions, environment template, and database migration instructions. |

# **9\. Use Cases and User Journeys**

## **UC-01: Search a Legal Document**

Actor: Visitor or registered user. Preconditions: Public corpus is indexed. Main flow: enter query → API validates query → retrieval searches the corpus → UI displays ranked results with metadata/snippets → user opens the source. Alternate flow: no relevant results → display a clear no-results message and suggest query refinement.

## **UC-02: Ask a Question in the Legal Chatbot**

Actor: Visitor or registered user. Main flow: enter question → backend checks access scope → retrieve relevant passages → assess whether evidence is sufficient → generate answer from retrieved context → validate and return citations → UI displays answer and sources. Alternate flow: weak evidence or retrieval failure → abstain or explain that the available corpus does not answer the question.

## **UC-03: Summarize a Document**

Actor: Authorized user. Main flow: select an approved document → system extracts or loads text → summarizer produces a structured short summary → UI shows summary plus original-source link and limitations. Alternate flow: extraction is empty/poor → report failure rather than generating a summary from corrupted text.

## **UC-04: Add an Approved Document**

Actor: Authorized administrator/curator. Main flow: confirm source and permission → upload/ingest → validate file → extract text → attach metadata and access classification → index → run spot checks. Restricted case files must not enter the public index.

## **UC-05: View a Lawyer Profile**

Actor: Visitor. Main flow: browse/search directory → view practice areas and contact details → see a truthful verification label. The portal must not imply endorsement or guaranteed competence.

## **UC-06: Ask by Voice and Listen to the Answer**

Actor: Visitor or registered user. Main flow: tap the microphone → speak in Urdu or English → speech is converted to text → user reviews or edits the transcript → send → chatbot answers with citations (UC-02) → user taps the speaker button to hear the answer, with pause, stop, and speed controls. Alternate flow: speech is unclear, microphone permission is denied, or the browser lacks support → show a clear message and continue with typing; if no voice is available for output, show the text answer only.

## **UC-07: Identify the Legal Category and Forum**

Actor: Visitor or registered user. Main flow: describe a problem → system identifies the likely category and sub-issue → retrieves the matching forum-guide entry → shows category, usual court or department, first steps, confidence, and citations → offers Find a lawyer filtered by practice area. Alternate flows: the forum depends on missing facts → ask clarifying questions; low confidence → tentative wording or Other / unclear; the user describes immediate danger → show emergency help first.

## **UC-08: Build a Lawyer Profile**

Actor: Lawyer. Main flow: register with basic information → profile wizard shows completion percentage and the suggested next step → lawyer completes optional steps in any order, choosing public or private visibility → submits credentials for verification → administrator reviews and records the outcome → public profile shows completed sections and truthful verification labels. Alternate flows: lawyer skips steps → profile remains valid with empty sections hidden; verification rejected → label stays Self-reported and the reason is recorded for the lawyer.

# **10\. Proposed System Architecture**

High-level flow: Next.js UI → FastAPI endpoints → authentication/authorization → PostgreSQL metadata and application records → document ingestion and chunking → retrieval/index layer → RAG orchestration → language model → answer plus source references → UI.

| Component | Responsibility |
| :---- | :---- |
| Next.js frontend | Search, chat, document upload, citations, summary view, lawyer directory, responsive screens. |
| FastAPI backend | REST endpoints, validation, authentication integration, business logic, access control, orchestration. |
| PostgreSQL | Users, document metadata, permissions, lawyer profiles, query/evaluation records, and optionally vector data through an extension if available. |
| Ingestion pipeline | PDF validation, text extraction, page tracking, normalization, chunking, metadata preservation. |
| Retrieval layer | Keyword search and/or embeddings/vector similarity; optional reranking if feasible. |
| RAG layer | Select context, prompt the model, generate a grounded answer, return source identifiers, abstain when evidence is insufficient. |
| LLM / embedding provider | Selected model service or locally hosted model; assess privacy, cost, latency, language quality, and licensing. |
| Speech services | Speech-to-text for voice questions and text-to-speech for read-aloud answers; browser speech APIs first, optional backend service. |
| Category and forum guidance | Classify described problems into the fixed legal-category list and retrieve the matching forum-guide entry with citations. |
| Lawyer profile and verification module | Step-by-step profile data, completion calculation, field visibility, and administrator verification records. |

## **10.1 Architecture Principles**

* Do not treat model-generated citations as valid unless they map to retrieved source records.  
* Keep public corpus and restricted case-file indexes logically separated, with access checks before retrieval.  
* Preserve original documents and source metadata; never silently overwrite a source version.  
* Make it possible to replace the embedding model or LLM without rewriting the whole application.  
* Store secrets in environment variables; never commit API keys to Git.

## **10.2 Mermaid Diagram Source**

flowchart TD  
    U\[User / Admin\] \--\> FE\[Next.js Frontend\]  
    FE \--\> API\[FastAPI Backend\]  
    API \--\> AUTH\[Authentication and Authorization\]  
    API \--\> DB\[(PostgreSQL Metadata)\]  
    API \--\> ING\[PDF Ingestion and Chunking\]  
    ING \--\> IDX\[Search / Vector Index\]  
    API \--\> RET\[Retriever\]  
    IDX \--\> RET  
    RET \--\> RAG\[RAG Orchestrator\]  
    RAG \--\> LLM\[Language Model\]  
    RAG \--\> SRC\[Source and Citation Validator\]  
    SRC \--\> API  
    API \--\> FE  
    FE \--\> SPEECH\[Speech-to-Text / Text-to-Speech\]  
    API \--\> CLS\[Category and Forum Classifier\]  
    CLS \--\> RET  
    API \--\> LP\[Lawyer Profiles and Verification\]  
    LP \--\> DB

This is source text for a Mermaid diagram. It can be rendered in a Mermaid-compatible editor and refined after the final architecture is agreed.

# **11\. Technology Stack**

| Layer | Proposed technology | Reason / notes |
| :---- | :---- | :---- |
| Frontend | Next.js \+ TypeScript | Responsive web UI and reusable components. |
| Backend API | Python \+ FastAPI | Typed request/response schemas and convenient ML integration. |
| Database | PostgreSQL | Relational data, metadata, user records, and access rules. |
| ORM / migrations | SQLAlchemy \+ Alembic | Schema management and controlled database changes. |
| PDF extraction | A suitable PDF text extraction library | Choose after testing the actual documents; OCR is a separate stretch item. |
| Retrieval baseline | PostgreSQL full-text search and/or vector search | Start simple; compare keyword and semantic retrieval on test queries. |
| Embeddings | A multilingual embedding model compatible with Urdu/English | Benchmark on a small representative query set before committing. |
| LLM | A cost-appropriate hosted or local model | Select after checking current pricing, language support, privacy terms, and latency. |
| Baseline ML | scikit-learn | TF-IDF \+ Logistic Regression for a category-classification experiment if labeled data exists. |
| Transformer experiments | PyTorch \+ Hugging Face Transformers | Use only where compute/data/time justify the complexity. |
| Version control | Git \+ hosted repository | Feature branches, pull requests, issue tracking, and reproducible milestones. |
| Testing | pytest and frontend testing tools | Automated API, retrieval, permission, and UI smoke tests. |
| Speech-to-text | Browser Web Speech API (MVP); optional self-hosted Whisper | Free to start; move to Whisper through the backend if Urdu accuracy is poor or browser support is limited. |
| Text-to-speech | Browser SpeechSynthesis API (MVP); optional Azure Neural TTS or Meta MMS-TTS | Urdu voice availability varies by browser and operating system; pretrained voices, no training needed. |
| Category detection | Existing LLM constrained to the fixed category list, plus keyword fallback; optional TF-IDF \+ Logistic Regression | No training needed for the MVP; a trained classifier only as an academic comparison if labeled data exists. |

Exact model/provider choices are intentionally left open until a small pilot verifies Urdu/English quality, data handling terms, and cost. Avoid selecting a model solely because it is popular.

# **12\. Data Strategy and Governance**

## **12.1 Data Categories**

* Public legal material: statutes, rules, judgments, or other sources whose use and redistribution are permitted.  
* Lawyer-provided case files: potentially confidential material requiring written permission, strict access controls, and an explicit purpose.  
* Synthetic/demo documents: invented examples used for UI testing and presentations; label them as synthetic.

## **12.2 Ingestion Workflow**

12. Record the document source, title, type, jurisdiction, date, language, and permission/access category.  
13. Validate file type and size; reject unsupported or malformed files.  
14. Extract text and page references; record extraction status and any quality warnings.  
15. Manually spot-check representative pages, especially Urdu text and unusual layouts.  
16. Normalize whitespace without altering legal meaning; retain original source and extracted-text version.  
17. Chunk text with overlap appropriate to the document structure and preserve chunk-to-page mapping.  
18. Index only after permission and access classification have been confirmed.  
19. Test retrieval with known questions and record failures for correction.

## **12.3 Confidential Files**

Do not place real lawyer/client files in a public demo, shared index, or third-party model service unless the relevant permission and data-processing conditions have been reviewed. Minimize personal data, redact identifiers when possible, restrict access, define retention/deletion rules, and use synthetic data for presentations. The team should obtain supervisor approval for the data-governance plan.

## **12.4 Data Limitations**

The dataset may be small, imbalanced, incomplete, outdated, or dominated by a subset of courts and legal topics. The portal must show the corpus coverage and should not imply that a search result represents all Pakistani law.

## **12.5 Forum Guide Knowledge Base**

F-12 depends on a curated forum guide: a short reference document that maps each legal category to the courts, tribunals, or departments that usually handle it, typical first steps, and the governing law. It is stored and indexed like other approved documents so that category guidance can be cited.

* Draft entries from official sources (statutes, court and department websites) and record the source and access date.  
* Have each entry reviewed by the supervisor or a legal-domain reviewer before indexing, and note province-specific differences.  
* Review the guide when laws or institutions change (for example the transfer of cybercrime investigation functions) and show the review date.  
* Keep a labeled set of synthetic problem descriptions for evaluating category detection; never use real client facts.

## **12.6 Voice Data**

Voice recordings are processed only to produce a transcript and are discarded afterwards unless the user consents. If a browser speech API is used, audio may be processed by the browser vendor's service; disclose this to users and prefer a self-hosted option for sensitive use. Avoid logging transcripts of confidential questions.

# **13\. AI/ML Methodology**

## **13.1 RAG Pipeline**

20. Query input: accept a user question in supported language(s).  
21. Scope check: determine which document collection the user is permitted to query.  
22. Retrieval: find candidate passages using keyword search, embeddings, or a small hybrid approach.  
23. Context selection: remove duplicates and retain the most relevant passages with source metadata.  
24. Evidence check: if no passage meets a pilot-defined relevance threshold, abstain or ask for clarification.  
25. Answer generation: instruct the LLM to use only the supplied evidence and identify uncertainty.  
26. Citation validation: map each citation to an actual document and page/chunk reference.  
27. Response: return answer, citations, source dates, and a reminder to verify important decisions with a qualified lawyer.

## **13.2 Retrieval Baseline and Improvement**

Start with a simple keyword baseline. If time permits, compare it with multilingual embeddings and/or a hybrid search. Create a small query set and have a reviewer mark relevant passages. Compare methods using Precision@k, Recall@k, or nDCG@k where appropriate. Keep the comparison fair and document the query set.

## **13.3 Summarization**

Summarization should preserve important dates, parties/roles, issues, legal provisions, decision/reasoning, and outcome where present. It must not invent missing facts. Display a notice that summaries may omit details and are not a substitute for reading the original.

## **13.4 Classification Experiment**

Only attempt legal-category classification if the team obtains enough labeled examples with permission. Establish a majority-class baseline and a TF-IDF \+ Logistic Regression baseline before considering a transformer. Split by case/document rather than by overlapping chunks to avoid leakage. Report class distribution, macro-F1, precision/recall, confusion matrix, and limitations. If attempted, this experiment serves as the trained-model comparison for the LLM-based category detection described in 13.7.

## **13.5 Case Outcome Prediction**

Case-outcome prediction is not part of the committed MVP. It requires reliable labels, outcome definitions, temporal and jurisdictional coverage, leakage checks, and expert review. Without these, omit the feature rather than present speculative predictions as meaningful.

## **13.6 Human Review and Hallucination Controls**

* Require citations for material legal claims and test whether cited passages actually support the claims.  
* Use an abstention path when retrieval evidence is weak.  
* Show source date/version where available and flag possible outdated material.  
* Avoid instructions that encourage the model to fill gaps with guesses.  
* Keep a set of adversarial and unanswerable test questions.

## **13.7 Legal Issue Category and Forum Detection**

The MVP uses the existing LLM rather than a newly trained model:

* Category selection: the LLM chooses one or more categories only from the fixed list in Section 5.5 (including Other / unclear), with a sub-issue and a confidence level.  
* Keyword fallback: a simple keyword list (for example Khula, FIR, eviction) acts as a backup when the LLM service is unavailable and as a sanity check on its output.  
* Forum retrieval: the matching forum-guide entry is retrieved through the RAG pipeline, so the court or department and first steps are cited rather than generated from model memory.  
* Clarification: if the forum depends on missing facts (province, land type, employer type, amount), the system asks before answering.  
* Safety: a description of immediate danger triggers emergency help information first, and all guidance carries an informational-only notice.  
* Optional trained baseline: TF-IDF \+ Logistic Regression trained on roughly 300–500 synthetic, reviewed problem descriptions (Urdu, Roman Urdu, English), compared with the LLM using macro-F1, per-class precision/recall, and a confusion matrix. Fine-tuned transformers are future work.  
* Decision pending: the team will confirm with the supervisor whether to include the trained baseline (see Appendix A).

## **13.8 Speech Input and Output**

* Speech-to-text: start with the browser Web Speech API (Urdu and English); show the transcript for editing before sending, because misheard section numbers or law names could retrieve the wrong sources.  
* Upgrade path: a backend /api/transcribe endpoint using a self-hosted Whisper model if Urdu accuracy or browser support is insufficient.  
* Text-to-speech: start with the browser SpeechSynthesis API, choosing a voice by answer language; read the answer, short source names, and the disclaimer, and skip URLs and formatting.  
* Upgrade path: a backend /api/tts endpoint using Azure Neural TTS (Urdu voices, free tier) or self-hosted Meta MMS-TTS.  
* All speech models are pretrained; no model training is required.

# **14\. Database Design (Initial ERD Description)**

This is an initial logical design. Confirm exact fields and relationships during implementation.

| Entity | Example fields | Purpose |
| :---- | :---- | :---- |
| users | id, name, email, password\_hash/auth\_provider\_id, role, created\_at | Account identity and role. |
| documents | id, title, source\_url/source\_name, document\_type, jurisdiction, language, date, access\_level, checksum, status | Document metadata and provenance. |
| document\_versions | id, document\_id, version\_label, uploaded\_at, extracted\_text\_status | Track source/version changes. |
| chunks | id, document\_version\_id, page\_start, page\_end, chunk\_text, chunk\_hash | Retrievable text with source mapping. |
| lawyer\_profiles | id, user\_id, headline, bio, city, practice\_areas, court\_level, enrollment\_no, enrollment\_date, languages, consultation\_modes, fee\_range, pro\_bono, public\_contact, field\_visibility, completion\_percent, verification\_status, verified\_at | Lawyer directory data. |
| permissions | id, document\_id, user\_id/role, permission\_type, granted\_by, expires\_at | Restricted document access. |
| query\_logs | id, user\_id nullable, timestamp, query\_category, latency\_ms, feedback | Minimal, non-sensitive service evaluation. |
| evaluation\_cases | id, question, expected\_source\_ids, expected\_behavior, split | Controlled offline evaluation set. |
| lawyer\_specializations | id, lawyer\_id, category\_id, sub\_area | Specializations linked to legal categories. |
| lawyer\_education | id, lawyer\_id, degree, institution, year | Optional education step. |
| lawyer\_experience | id, lawyer\_id, role, organization, start\_date, end\_date, description | Optional experience step. |
| lawyer\_verifications | id, lawyer\_id, field\_checked, result, checked\_by, checked\_at, notes | Record of what was verified, by whom, and when. |
| lawyer\_documents | id, lawyer\_id, document\_type, storage\_ref, uploaded\_at | Private verification uploads (administrator-only). |
| legal\_categories | id, name, parent\_id, keywords | Fixed category list for F-12. |
| forums | id, name, forum\_type, jurisdiction, province, source\_document\_id | Courts and departments from the forum guide. |
| category\_forum\_map | id, category\_id, forum\_id, conditions, first\_steps | Which forum applies under which conditions. |

## **14.1 Relationship Notes**

* One document may have multiple versions; each version may contain multiple chunks.  
* A chunk must reference a specific document version and source location.  
* Access permissions must be checked before chunks are retrieved, not only after an answer is generated.  
* Avoid storing full confidential questions or answers in logs unless a documented, approved need exists.  
* One lawyer profile may have many specializations, education entries, experience entries, verification records, and private documents.  
* Each legal category may map to several forums depending on conditions such as province or claim amount; forum entries must reference a reviewed source document.

ERD to draw before implementation: users 1—many permissions; documents 1—many document\_versions; document\_versions 1—many chunks; users 1—0/1 lawyer\_profiles. The permission model may need a join table for users/roles and documents. Additional relationships: lawyer\_profiles 1—many lawyer\_education, lawyer\_experience, lawyer\_specializations, lawyer\_verifications, and lawyer\_documents; legal\_categories many—many forums through category\_forum\_map.

# **15\. API Design (Initial Specification)**

| Method \+ path | Purpose | Access |
| :---- | :---- | :---- |
| GET /health | Health/status check. | Public or internal |
| GET /api/documents | List/search permitted document metadata. | Public corpus; apply access filters |
| GET /api/documents/{id} | Fetch document metadata and permitted details. | Access checked |
| POST /api/documents/upload | Upload and ingest an approved PDF. | Admin/authorized contributor |
| POST /api/search | Search documents and return snippets/citations. | Scope checked |
| POST /api/chat | Retrieve evidence and return grounded answer with citations. | Scope checked |
| POST /api/summarize | Summarize permitted document/text. | Scope checked |
| GET /api/lawyers | List public lawyer profiles with filters (city, practice area, court level, language, consultation mode, pro bono, verified only). | Public |
| GET /api/lawyers/{id} | View lawyer profile and verification state. | Public |
| POST /api/feedback | Submit answer usefulness or citation feedback. | Rate-limited |
| POST /api/transcribe | Convert recorded audio to text (only if backend speech-to-text is used). | Rate-limited |
| POST /api/tts | Convert answer text to speech audio (only if backend text-to-speech is used). | Rate-limited |
| POST /api/classify | Return legal category, sub-issue, usual forum, first steps, and citations for a described problem (may also run inside /api/chat). | Scope checked; rate-limited |
| GET /api/categories | List legal categories for filters and the directory. | Public |
| PUT /api/lawyers/me/profile/{step} | Save one profile step. | Lawyer (own profile) |
| GET /api/lawyers/me/completion | Return completion percentage and suggested next steps. | Lawyer (own profile) |
| POST /api/lawyers/me/verification | Submit credentials and documents for verification. | Lawyer (own profile) |
| PATCH /api/admin/lawyers/{id}/verify | Approve or reject verification and record the check. | Administrator |

These are proposed endpoint names, not an assertion that endpoints already exist. Define Pydantic request/response schemas, error codes, upload size limits, rate limits, and authentication requirements before coding.

# **16\. UI/UX Screen Plan**

| Screen | Key elements |
| :---- | :---- |
| Landing page | Project explanation, search entry, links to legal documents and chatbot, limitations notice. |
| Legal document search | Search box, filters, result snippets, source title/date/type, pagination. |
| Chat interface | Question field with microphone button and editable transcript, answer, citations/source cards, speaker button with play/pause/stop and speed, category and forum card with a Find a lawyer link, insufficient-evidence message, feedback control. |
| Document detail | Metadata, original link/file if allowed, extracted text, page references, summary action. |
| Upload/curation dashboard | Permission/access classification, metadata form, extraction status, preview, indexing status. |
| Lawyer directory | Search and filters (city, practice area, court level, language, consultation mode, pro bono, verified only), profile cards, contact options, honest verification labels. |
| Admin/evaluation view | Document status, ingestion failures, aggregate metrics, and safe test feedback. |
| Lawyer profile wizard | Step-by-step sections, progress bar with completion percentage, next-step suggestions, skip and save-later, public/private toggles, verification submission. |
| Public lawyer profile | Completed sections only, Verified or Self-reported labels with verification date, contact options chosen by the lawyer, no endorsement claims. |
| Administrator verification queue | Pending lawyer submissions, private documents (administrator-only), approve or reject with notes, verification history. |
| Voice settings | Auto-read answers toggle (off by default), preferred voice language, and playback speed. |

Use clear Urdu/English labels as appropriate. The UI should distinguish 'source found' from 'legal conclusion' and should never imply that a profile is verified unless the verification process has actually been completed.

# **17\. Security, Privacy, and Legal Safety**

* Enforce authorization in FastAPI for every restricted document and endpoint.  
* Keep secrets and API keys outside source control; use a local environment file excluded from Git.  
* Validate file extension, MIME type, size, and parsing outcome; do not trust client-supplied metadata.  
* Restrict uploaded-file access and avoid exposing raw storage paths.  
* Use synthetic or redacted examples in demos and screenshots.  
* Record written permission and intended use for lawyer-provided documents.  
* Separate public legal material from confidential case-file storage and retrieval.  
* Add rate limiting and basic abuse/error handling for public-facing endpoints.  
* Display a clear disclaimer: informational support only, not a lawyer-client relationship or substitute for professional advice.  
* Show that legal documents may be incomplete or outdated and advise checking authoritative current sources.  
* Do not label lawyers as verified without recording what was checked, by whom, and when.  
* Provide a process to correct/remove wrongly indexed material and to handle permission withdrawal.  
* Process voice recordings only for transcription and discard them unless the user consents; disclose when a browser or third-party speech service processes audio.  
* Keep lawyer verification documents in restricted storage visible only to administrators; never expose them on public profiles.  
* Do not allow win rates, client names, or guaranteed-result claims on lawyer profiles; present category and forum guidance as general information, not legal advice.  
* Show emergency help information first when a user describes immediate danger.

# **18\. Testing and Evaluation Plan**

## **18.1 Test Types**

| Test type | Examples / evidence |
| :---- | :---- |
| Unit tests | PDF parser errors, chunking, metadata validation, citation mapping, permission helpers. |
| API integration | Upload-to-index flow, search response schema, chatbot source references, authentication failures. |
| Access-control tests | User without permission cannot retrieve restricted document text or receive it through RAG. |
| Retrieval evaluation | Queries with expected relevant source passages; measure Precision@k/Recall@k or graded relevance. |
| Citation evaluation | Manually check whether each sampled citation exists and supports the associated claim. |
| Groundedness / abstention | Unanswerable queries should not produce fabricated statute names or unsupported legal claims. |
| Extraction tests | Representative PDFs; count usable extraction, page mapping failures, and OCR limitations. |
| Usability test | A small number of test users complete search, ask a question, open citations, and report confusion. |
| Performance | Measure latency and failure rate on the test machine/service; report environment and sample size. |
| Speech tests | Word error rate on a small set of Urdu and English recordings, including different accents and background noise; transcript-editing flow; read-aloud controls and language matching across common browsers. |
| Category detection evaluation | Synthetic labeled problem descriptions; accuracy, macro-F1, per-class precision/recall, and confusion matrix; LLM versus trained baseline if built; forum correctness checked against the reviewed forum guide. |
| Lawyer profile tests | Optional steps can be skipped; completion percentage is correct; private fields and verification documents are never returned by public endpoints; Verified appears only after administrator approval. |

## **18.2 Suggested Evaluation Dataset**

Create a small, manually reviewed set of representative questions across the documents actually included. For each question, record the expected relevant document/passage, whether the corpus can answer it, and any expected abstention. Include Urdu and English queries only where the corpus/model can support them. Do not use confidential client facts in the evaluation set.

## **18.3 Reporting**

* Report the number and type of documents, language mix, and known corpus coverage gaps.  
* Report the number of queries, reviewer process, and scoring rules.  
* Show representative successes and failures, including incorrect/missing citations.  
* Do not claim broad legal accuracy based on a tiny prototype test.  
* For classifiers, report macro-F1 and per-class precision/recall, not accuracy alone when classes are imbalanced.  
* Report speech-recognition results separately for Urdu and English, with recording conditions and the browser or service used.

# **19\. Eight-Week Implementation Roadmap**

| Week | Main tasks | Expected deliverable / exit criterion |
| :---- | :---- | :---- |
| 1 | Confirm requirements; agree scope; select initial corpus; define roles; create repository, issues, architecture sketch, and data policy. | Approved scope, backlog, repo, architecture draft, initial data inventory. |
| 2 | Create FastAPI project, database schema/migrations, frontend skeleton, health endpoint, environment setup, basic auth/access design. | Frontend reaches API; database migrations run; setup instructions work. |
| 3 | Implement PDF upload/validation, text extraction, metadata storage, chunking, source/page mapping, basic search. | A small approved corpus is searchable and extraction failures are visible. |
| 4 | Implement retrieval and RAG answer flow, citation mapping, insufficient-evidence fallback, chat UI. Draft the forum guide and send it for legal review. | End-to-end chatbot demo with citations traceable to source passages. |
| 5 | Add basic summarization, document detail page, lawyer directory with profile wizard (Steps 1, 2, 3, 7), administrator verification queue, curation workflow, and category and forum guidance. | Core user journeys work with approved/demo data; lawyers can build and submit profiles; described problems receive category and forum guidance. |
| 6 | Add voice input (speech-to-text) and read-aloud output (text-to-speech), access-control tests, feedback/evaluation logging, and optional baseline classifier if labels exist; review privacy. | Users can ask by voice and listen to answers; safety checks pass; classifier is included only if data is adequate. |
| 7 | Run retrieval, citation, category, speech, and usability tests, fix high-priority bugs, evaluate latency, improve prompts and retrieval. | Evaluation report with actual results and known limitations. |
| 8 | Freeze MVP scope, deploy/demo, prepare final report, slides, architecture diagrams, test evidence, and future-work plan. | Reproducible demo and submission package ready for supervisor review. |

## **19.1 Weekly Working Routine**

* Monday: agree tasks, owners, and acceptance criteria.  
* Midweek: integrate one working vertical slice; raise blockers early.  
* Friday: demo the current build, run tests, update issues and documentation.  
* Merge small pull requests; avoid leaving integration until the final week.

## **19.2 Scope Control**

If the team falls behind, protect the core path: approved documents → extraction → retrieval → grounded answer → valid citations → evaluation. Defer advanced OCR, transformer fine-tuning, outcome prediction, payments, booking, backend speech upgrades, the trained category classifier, and lawyer profile Steps 4, 5, 6, and 8\. Browser-based voice input/output and LLM-based category guidance stay in scope because they reuse existing components.

# **20\. Team Responsibilities**

| Member | Primary ownership | Shared responsibilities |
| :---- | :---- | :---- |
| Member A — AI/RAG | Ingestion/chunking design, retrieval experiments, RAG prompts, citation validation, category detection and forum guide, optional trained classifier, evaluation. | Data review, API integration, tests, report. |
| Member B — Backend/Data | FastAPI, PostgreSQL, SQLAlchemy/Alembic, auth/permissions, upload endpoints, lawyer profile and verification APIs, optional speech endpoints, logging. | Architecture, data governance, integration, report. |
| Member C — Frontend | Next.js UI, search/chat/citation screens, voice input and read-aloud controls, upload/admin UI, lawyer directory and profile wizard, usability. | API contracts, testing, demo, report. |

All members should understand the complete user journey and be able to explain the architecture at the viva. Avoid a design where only one member can run or demonstrate the system.

# **21\. Budget and Resource Plan**

Planning cap: PKR 10,000 total. Actual prices, taxes, exchange rates, model usage charges, and hosting terms can change; verify before purchase. Keep a written record of any expense.

| Item | Plan | Budget handling |
| :---- | :---- | :---- |
| Cursor AI | Begin with free plans for all three members; use one paid seat only if a real bottleneck appears. | Do not buy three subscriptions upfront. |
| Hosting/deployment | Use free/student-tier services if suitable and permitted. | Keep a fallback local demo. |
| LLM/embedding usage | Prefer free allowances, small test sets, or a low-cost/local option after privacy review. | Set usage limits; never upload confidential case files without approval. |
| Database/vector search | Use local PostgreSQL or an eligible free tier for the prototype. | Avoid paid managed services unless necessary. |
| Contingency | Reserve funds for essential unexpected costs. | Approve spending as a team before purchase. |
| Speech services | Use free browser speech APIs first; consider a free tier or self-hosted model only if quality requires it. | No paid speech subscription without team approval. |

Budget principle: spend on the single bottleneck that most affects delivery, not on subscriptions by default. A paid tool does not replace scope control, testing, and integration.

# **22\. Risks and Mitigations**

| Risk | Impact | Mitigation |
| :---- | :---- | :---- |
| Hallucinated legal answer | User may rely on unsupported claims. | Ground answers in retrieved passages, validate citations, abstain on weak evidence, include disclaimer and human-review path. |
| Outdated/conflicting source | Answer may be incomplete or obsolete. | Track source date/version, prioritize authoritative sources, flag coverage limits, review updates. |
| Urdu OCR/extraction errors | Important legal text may be corrupted. | Start with text PDFs, inspect samples, report failures, manually verify high-risk passages; defer advanced OCR. |
| Confidentiality breach | Exposure of lawyer/client information. | Written permission, redaction, access control, separate indexes, safe demo data, documented retention. |
| Insufficient/biased ML labels | Misleading model evaluation or predictions. | Use baseline, inspect class distribution, avoid outcome prediction, report limitations. |
| Citation mismatch | Citation may not support the claim. | Map citations to actual chunks and manually audit a sample. |
| Cost or rate limits | Demo may fail or exceed budget. | Set usage limits, cache where safe, use small evaluation sets, prepare local/mock fallback. |
| Scope creep | Core system unfinished by deadline. | Lock MVP, defer stretch features, weekly integration and demo. |
| False lawyer verification | Users may be misled about credentials. | Define verification evidence, show status/date, avoid unsupported badges. |
| Team integration problems | Components fail to work together. | Agree API schemas early, use shared repo, integrate continuously, test each week. |
| Wrong category or forum | User may approach the wrong court or department. | Fixed category list, cited forum guide reviewed by a legal expert, clarifying questions, confidence wording, informational-only notice. |
| Speech misrecognition | Misheard law names or section numbers retrieve the wrong sources. | Editable transcript before sending, Urdu/English testing, typing fallback, Whisper upgrade path. |
| Limited Urdu voice support | Read-aloud may be unavailable or poor in some browsers. | Detect available voices, show a clear message, test common browsers, optional backend text-to-speech. |
| Misleading lawyer profile content | Users may rely on unverified or promotional claims. | Self-reported and Verified labels, administrator verification records, excluded fields, correction process. |
| Scope growth from new features | Core path may be delayed. | Browser-based speech first, LLM-based categories without training, profile Steps 4–6 and 8 deferrable. |

# **23\. Deliverables and Definition of Done**

## **23.1 Expected Deliverables**

* Project proposal and approved requirements document.  
* Source-code repository with setup instructions and environment template.  
* Database schema and migrations.  
* Document ingestion/search pipeline for the approved MVP corpus.  
* RAG chatbot with source citations and abstention behavior.  
* Basic summarization, lawyer directory with profile wizard and verification queue, and category and forum guidance if time permits.  
* Voice input and read-aloud output for the chatbot, with typing as a fallback.  
* Test suite and evaluation report with actual measurements.  
* Architecture/ERD/API documentation and risk/data-governance notes.  
* Deployment or reproducible local demo instructions.  
* Final FYP report, slides, and demonstration script.

## **23.2 Definition of Done for Core MVP**

* A new team member can follow the README and run the project using documented steps.  
* An approved PDF can be ingested or a clear failure is shown.  
* Search results show source metadata and relevant snippets.  
* Chat answers include citations that resolve to actual source passages.  
* Unanswerable questions trigger a safe fallback instead of fabricated certainty.  
* Restricted documents cannot be accessed by unauthorized users through UI or API.  
* A user can ask a question by voice, edit the transcript, and listen to the answer, or fall back to text.  
* A described problem returns a category, usual forum, and first steps with citations, or a clarifying question.  
* A lawyer can register with basic information only, complete optional steps later, and sees Verified only after administrator approval.  
* The team can present evaluation methods and actual results, including failures.  
* Known limitations and future features are clearly documented.

# **24\. Future Enhancements**

* Robust scanned-PDF and Urdu OCR pipeline with human verification.  
* Multilingual semantic reranking and legal-domain embedding evaluation.  
* Legal entity extraction and automated citation-reference checking.  
* Similar-case search with explainable similarity signals.  
* Automated checks against official Bar Council records for lawyer credential verification, if such a source becomes available.  
* Fine-tuned multilingual transformer (for example XLM-RoBERTa) for legal category classification with an expert-labeled dataset.  
* Higher-quality Urdu speech recognition and natural neural voices, including regional languages.  
* Moderated client reviews on lawyer profiles.  
* Consultation scheduling, reminders, and secure client portal.  
* Case complexity/urgency triage after expert validation.  
* Outcome prediction only as a carefully governed research experiment with adequate data.  
* Production-grade audit trails, monitoring, backups, and incident response.

# **25\. Conclusion**

The project is feasible as an eight-week academic MVP if the team protects the core scope and works with a small, authorized corpus. The strongest demonstration is a complete, testable path from legal document to retrieved evidence to a cited answer, with appropriate fallback behavior and clear limitations. Voice access, category and forum guidance, and detailed lawyer profiles extend this path for users without changing its evidence-grounded design. Advanced prediction and production features should remain future work unless data, validation, security, and time requirements are demonstrably satisfied.

# **Appendix A: Supervisor Decisions**

* Which university template, proposal headings, and citation style are mandatory?  
* Which Pakistani jurisdiction(s), courts, legal areas, and languages are in scope?  
* Which public legal sources may be used, and what are the copyright/reuse conditions?  
* Will any real lawyer/client case files be used? If yes, what written permission and privacy review are required?  
* Is a hosted LLM permitted, or must all data processing be local?  
* What minimum evaluation sample size and research methodology does the supervisor expect?  
* Which features are mandatory for grading, and which can be listed as future work?  
* What deployment/demo environment is acceptable?  
* Is moving voice input and read-aloud output into the MVP approved?  
* Should category detection use the LLM only, or also include a trained TF-IDF \+ Logistic Regression baseline for comparison?  
* Who will review the forum guide (courts and departments per legal category) for legal accuracy?  
* What evidence is acceptable for verifying lawyer credentials (for example Bar Council enrollment)?

# **Appendix B: Starter Reference List and Research Tasks**

The following are source categories to investigate, not a claim that a completed literature review has already been conducted. Verify each selected source, version, access date, and citation style before submitting.

* Official Pakistani legal and court websites for statutes, rules, judgments, and current source metadata.  
* Original RAG research paper(s) and authoritative documentation for the selected retrieval/LLM approach.  
* Peer-reviewed work on legal information retrieval, legal NLP, and evaluation of grounded question-answering.  
* Official documentation for FastAPI, Next.js, PostgreSQL, SQLAlchemy/Alembic, scikit-learn, PyTorch, and the selected embedding/LLM provider.  
* Relevant university research-ethics, data-protection, and FYP submission guidelines.

Literature-review task: identify at least 5–10 relevant, verifiable sources; summarize each source's method, dataset, findings, limitations, and relevance to this project. Do not add unverified or fabricated references.

# **Appendix C: Suggested Final Report Structure**

28. Chapter 1: Introduction, problem statement, objectives, scope, significance.  
29. Chapter 2: Literature review and related systems.  
30. Chapter 3: Requirements, methodology, data governance, and research design.  
31. Chapter 4: System analysis and design (architecture, ERD, use cases, APIs, UI).  
32. Chapter 5: Implementation details.  
33. Chapter 6: Testing, evaluation results, and discussion.  
34. Chapter 7: Conclusion, limitations, and future work.  
35. References and appendices.