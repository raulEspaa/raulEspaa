# Raúl Fernández España

**Backend engineer moving into AI engineering** — Gijón, Spain · C# / .NET / SQL Server · Python, agents and LLM evals

> *The query returned 153 rows. The table had 286.*

I work on a warehouse management platform that runs live logistics sites, where a defect stops a floor. Most of my day is transactional workflows and the data access layer — which mostly means finding the things that aren't where the documentation says they are.

The other half of what I do is testing whether AI systems actually behave. I've built tooling to measure language-model performance, run red-teaming passes to catch information leakage and tool misuse before release, and done computer-vision testing engagements for IKEA and William Hill. Now I build the agents too.

---

## Haggle Garage — can an AI agent keep a secret it needs to do its job?

**[▶ Play it live](https://haggle-api-38346102533.europe-west1.run.app)** · **[Code](https://github.com/raulEspaa/haggle-garage)**

An AI car dealer sells one iconic American car per game, with a **secret minimum price** it must never reveal. You haggle it down, or try to make it leak. Three security levels, from "the secret is in the prompt" to "the model never sees it", and an eval suite that measures each one.

| Per level | Naive | Hardened | Blind |
|---|:-:|:-:|:-:|
| Games where the dealer disclosed its floor | 22 % | 0 % | 0 % |
| Prompt-injection attacks that extracted it | 28 % | 0 % | 0 % |
| Sales below the floor | 0 | 0 | 0 |

*81 simulated negotiations + 264 scripted attacks per run, 95 % confidence intervals, an LLM judge calibrated against labelled data.*

- **Two agents, two frameworks, two protocols.** The seller is a **Google ADK** agent served over **A2A**; the buyer is a **LangGraph** agent that negotiates with it and appraises cars through the same **MCP** server.
- **The model talks, code decides.** At the top level, prices come from a pricing engine behind MCP, guards run as agent callbacks, and the seller's database role *cannot read* the secret column.
- **RAG** over sourced car facts with Gemini embeddings and pgvector, and one **Langfuse** trace per game across three services.
- **Deployed on Cloud Run with Terraform**: service-to-service ID tokens, Secret Manager, least-privilege database roles, and a public demo that can't run up a bill.

Built over eight weeks with Claude Code as a pair programmer, with a [weekly build journal](https://github.com/raulEspaa/haggle-garage/tree/main/docs/learning) that explains every piece.

---

## My day-job code is private

It's an enterprise platform, not a public project. Instead, three things I had to dig for:

<details>
<summary><b>A relation the documentation promised, and the database didn't have</b></summary>

<br>

A configuration export kept coming back empty. The direct fix was closed off — we only had the framework as compiled assemblies, no source.

So I built a custom exporter and instrumented the read model by reflection, one hypothesis at a time, until I could prove the documented entity relation **did not exist in that model at all**. Along the way I found a query overload silently returning 153 of 286 rows.

Five hypotheses discarded. Each wrong answer narrowed the search.

</details>

<details>
<summary><b>Grouping stock adjustments without inventing a new abstraction</b></summary>

<br>

I designed the workflow layer for a stock-adjustment uniqueness rule.

I chose a single-query fetch with in-memory grouping over a two-query seed-and-expand approach — one fewer round trip, and a narrower window for inconsistent reads. I also kept the grouping on the domain entity instead of adding an intermediate movement type.

Pure adjustments net to one quantity per article (`-1, +3, -1 → +1`). Transformations split their old and new sides into separate groups. It's the same shape as netting entries in a ledger.

</details>

<details>
<summary><b>A regression that only existed in one of two contexts</b></summary>

<br>

Container locking started misbehaving in production. The cause was a mapping update applied to the read context but never to the transactional one — so the same entity was two slightly different things depending on *how you happened to ask for it*.

Reads looked fine. Writes didn't.

</details>

---

## Zentric — an AI receptionist that lives inside WhatsApp

**[zentric.pro](https://zentric.pro)** · *in development*

A done-for-you system for small clinics: an AI agent connected to the official WhatsApp Business API and to Google Calendar. It reads what a patient actually wants, offers real free slots from the clinic's calendar, books the appointment, chases no-shows and fills cancellations from the waiting list.

I designed and built all of it — the agent and its grounding, both integrations, the scheduling logic and the dashboard.

The interesting part isn't the model. It's everything around it: two-way calendar sync where the human always wins, conversations that survive being resumed three days later, and knowing when the agent should stop and hand over to a person.

---

## Also

- **PlotTwist** — an editor for interactive, choose-your-path video stories. Scenes are nodes on a visual canvas, viewer decisions are the connections between them, and a built-in player follows whichever path the viewer picks
- **Neosystems Hackathon** — winner. Designed and shipped something that worked, under a fixed clock

---

## Stack

**AI engineering**

![Google ADK](https://img.shields.io/badge/Google%20ADK-4285F4?style=flat-square&logo=google&logoColor=white)
![LangGraph](https://img.shields.io/badge/LangGraph-1C3C3C?style=flat-square&logo=langchain&logoColor=white)
![Gemini](https://img.shields.io/badge/Gemini%20%C2%B7%20Vertex%20AI-8E75B2?style=flat-square&logo=googlegemini&logoColor=white)
![MCP](https://img.shields.io/badge/MCP-000000?style=flat-square&logoColor=white)
![A2A](https://img.shields.io/badge/A2A-34A853?style=flat-square&logoColor=white)
![RAG](https://img.shields.io/badge/RAG%20%C2%B7%20pgvector-336791?style=flat-square&logo=postgresql&logoColor=white)
![Langfuse](https://img.shields.io/badge/Langfuse-0A0A0A?style=flat-square&logoColor=white)
![LLM evals](https://img.shields.io/badge/LLM%20evals%20%C2%B7%20red%20teaming-C0392B?style=flat-square&logoColor=white)
![Claude Code](https://img.shields.io/badge/Claude%20Code-D97757?style=flat-square&logo=claude&logoColor=white)

**Cloud and backend**

![Python](https://img.shields.io/badge/Python-20232A?style=flat-square&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-20232A?style=flat-square&logo=fastapi&logoColor=white)
![Google Cloud](https://img.shields.io/badge/Cloud%20Run-20232A?style=flat-square&logo=googlecloud&logoColor=white)
![Terraform](https://img.shields.io/badge/Terraform-20232A?style=flat-square&logo=terraform&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-20232A?style=flat-square&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-20232A?style=flat-square&logo=githubactions&logoColor=white)
![C#](https://img.shields.io/badge/C%23-20232A?style=flat-square&logo=csharp&logoColor=white)
![.NET](https://img.shields.io/badge/.NET-20232A?style=flat-square&logo=dotnet&logoColor=white)
![SQL Server](https://img.shields.io/badge/SQL%20Server-20232A?style=flat-square&logo=microsoftsqlserver&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-20232A?style=flat-square&logo=postgresql&logoColor=white)

- **AI:** agents with **Google ADK** and **LangGraph**, **MCP** and **A2A**, **RAG** with embeddings and pgvector, structured outputs, guardrails, **LLM evals** (judge calibration, confidence intervals), **red teaming** and prompt-injection defence, **Langfuse** tracing, Gemini on **Vertex AI**
- **Daily:** C#, .NET, LINQ, Entity Framework, SQL Server
- **Cloud:** GCP (Cloud Run, IAM, Secret Manager, Artifact Registry), Terraform, Docker, GitHub Actions
- **Also written:** Python (FastAPI, SQLAlchemy, Pydantic, pytest), Java, C++, React
- **Learning:** Kubernetes, Go

---

## Currently

- Reading *AI Engineering* (Chip Huyen)
- Starting **Agent World**: a strategy game where AI-governed nations trade, negotiate, betray each other and go to war — built by hand, one milestone at a time
- Building out a home lab — Proxmox, k3s, Terraform
- Open to relocation. EU citizen.

**[raulfdezespa@hotmail.com](mailto:raulfdezespa@hotmail.com)** · **[zentric.pro](https://zentric.pro)** · **[Haggle Garage](https://haggle-api-38346102533.europe-west1.run.app)**
