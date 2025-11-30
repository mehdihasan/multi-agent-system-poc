# Intelligent Real Estate Recommender Pipeline using AI Agent

## Project Overview

This project implements an **Intelligent Property Evaluation Pipeline** that transforms raw property data into high-value, actionable insights using a **Multi-Agent System (MAS)** built on the **Google Agent Development Kit (ADK)**.

Moving beyond simple database filtering, the system automates **real-world feature enrichment** via external APIs and performs **dynamic, weighted evaluation** to automatically notify users when a property matches $\ge 90\%$ of their criteria.

## Key Architectural Concepts Demonstrated

The solution is specifically designed to showcase advanced multi-agent concepts, with a strong focus on distributed services and delegation:

| Concept | Demonstration | ADK Components Used |
| :--- | :--- | :--- |
| **Multi-Agent System** | Two primary agents working together: an **LlmAgent** Orchestrator and a **RemoteA2aAgent** service. | `LlmAgent`, `RemoteA2aAgent` |
| **A2A Protocol** | The **Enrichment Agent** is deployed as an A2A-compatible microservice (`uvicorn` server), and the Orchestrator accesses its functionality via the **`RemoteA2aAgent`** client wrapper. | `to_a2a`, `RemoteA2aAgent` |
| **Sequential Agents** | The **Orchestrator Agent** enforces the sequential flow: *Property Data* $\to$ **Enrichment (A2A Call)** $\to$ **Evaluation (Internal Logic)** $\to$ **Notification (Final Output)**. | `LlmAgent` instruction set |
| **Sessions & Memory** | The **`Runner`** and the primary agents utilize **`InMemorySessionService`** to maintain context and history during the evaluation session. | `InMemorySessionService` |
| **Tools** | The **Remote Enrichment Agent** bundles complex functionalities (simulated **Google Maps/OpenAPI**, **Custom Geospatial**, and **Code Execution**) into a single callable service for the orchestrator. | Custom Python Functions, `LlmAgent` |

## Pipeline Architecture

The system is defined by a powerful delegation pattern:

1. **Input:** Property data (Address, Lat/Lon, Areas) is fed into the Orchestrator.
2. **Delegation (A2A):** The **Orchestrator Agent** delegates the data collection task to the **Remote Enrichment Agent** via the A2A protocol.
3. **Enrichment:** The remote agent executes its internal tools (simulated API calls) to fetch transit times, water distances, and calculated garden area.
4. **Evaluation:** The Orchestrator receives the enriched JSON data back, applies the user's weighted requirements, and calculates the **Prop-Score**.
5. **Output:** If the score is $\ge 90\%$, the Orchestrator completes the process by triggering a simulated user notification.

## Running the Proof of Concept (PoC)

The provided Jupyter Notebook (`prop_score_90_poc.ipynb`) allows for a complete, self-contained demonstration of the architecture.

### Prerequisites

* Python 3.9+
* The Google Agent Development Kit (ADK)
* `uvicorn`, `requests`, and other standard dependencies.

### Execution Steps

1. **Start Remote Service:** The notebook first writes and executes a Python file (`enrichment_server.py`) in the background using `subprocess` to launch the **Enrichment Agent** via `uvicorn` on `localhost:8002`.
2. **Initialize Orchestrator:** The **Evaluation Orchestrator Agent** is initialized, and its tool (`property_data_enricher`) is bound to the remote service using **`RemoteA2aAgent`**.
3. **Run Delegation:** The main execution triggers the Orchestrator with mock property data, forcing the agent to use its A2A tool.
4. **Verification:** The final output demonstrates a successful evaluation (100% match) and the triggered notification, confirming the end-to-end A2A pipeline functionality.
5. **Cleanup:** The notebook automatically terminates the background `uvicorn` server process.

## Future Work

* Integrate actual **Google Maps** and **Overpass API** calls in the Enrichment Agent's tool functions.
* Implement a **vector memory bank** for the Orchestrator to store long-term user preferences (e.g., historical 'liked' properties).
* Develop a **Golden Dataset** for **Agent Evaluation** to rigorously test the scoring and notification correctness across various edge cases.
