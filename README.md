# Raúl Fernández España

**Backend engineer** — Gijón, Spain · C# / .NET / SQL Server

> *The query returned 153 rows. The table had 286.*

I work on a warehouse management platform that runs live logistics sites, where a defect stops a floor. Most of my day is transactional workflows and the data access layer — which mostly means finding the things that aren't where the documentation says they are.

The other half of what I do is testing whether AI systems actually behave. I've built tooling to measure language-model performance, run red-teaming passes to catch information leakage and tool misuse before release, and done computer-vision testing engagements for IKEA and William Hill.

---

## Most of my code is private

It's an enterprise platform, not a public project — so there isn't much to browse here. Instead, three things I had to dig for:

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

- **[OpenCode](https://github.com/sst/opencode)** — contributing to the open-source AI coding agent: issues opened, pull requests submitted
- **Neosystems Hackathon** — winner. Designed and shipped something that worked, under a fixed clock
- **Branching narrative editor** — final degree project: a desktop editor for interactive branching stories, with a visual graph of nodes and conditional paths

---

## Stack

![C#](https://img.shields.io/badge/C%23-20232A?style=flat-square&logo=csharp&logoColor=white)
![.NET](https://img.shields.io/badge/.NET-20232A?style=flat-square&logo=dotnet&logoColor=white)
![SQL Server](https://img.shields.io/badge/SQL%20Server-20232A?style=flat-square&logo=microsoftsqlserver&logoColor=white)
![Python](https://img.shields.io/badge/Python-20232A?style=flat-square&logo=python&logoColor=white)
![Java](https://img.shields.io/badge/Java-20232A?style=flat-square&logo=openjdk&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=flat-square&logo=react&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-20232A?style=flat-square&logo=docker&logoColor=white)
![Git](https://img.shields.io/badge/Git-20232A?style=flat-square&logo=git&logoColor=white)

Daily: **C#, .NET, LINQ, Entity Framework, SQL Server**
Also written: Java, Python, C++, React
Learning: Go, Kubernetes, Terraform

---

## Currently

- Reading *AI Engineering* (Chip Huyen)
- Building out a home lab — Proxmox, k3s, Terraform
- Open to relocation. EU citizen.

**[raulfdezespa@hotmail.com](mailto:raulfdezespa@hotmail.com)** · **[zentric.pro](https://zentric.pro)**
