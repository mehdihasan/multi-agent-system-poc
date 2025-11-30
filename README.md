# Intelligent Real Estate Recommender Pipeline using AI Agent

## Intelligent Property Evaluation Pipeline: Multi-Agent System (MAS) Integration

This project demonstrates the integration of an **Intelligent Multi-Agent System (MAS)** into a conventional data pipeline (conceptualized by Dagster) to automate and enhance the evaluation of residential properties. The core objective is to move beyond simple data filtering and implement **real-world feature enrichment** and **dynamic, weighted requirement matching** to notify users of high-potential matches ($\ge 90\%$) in near real-time.

## Key Features & Concepts Demonstrated

* **Multi-Agent Architecture:** A sequential workflow powered by four specialized agents: **Requirements** (LLM-powered), **Enrichment** (Tool-based), **Evaluation** (Scoring Logic), and **Notification**.
* **Tool Integration:** The system utilizes diverse tools to gather real-world data, including simulations of the **Google Maps API** (OpenAPI), **Custom Geospatial Tools** (Overpass/OSM), and **Built-in Code Execution** for complex calculations.
* **Sessions & State Management:** A unified **`PropertyContext`** object manages the session state, ensuring coherent data flow and traceability across all agent steps.
* **Pipeline Enhancement:** Transforms raw property data into deeply enriched, scored assets, demonstrating how intelligent systems can deliver high-value, actionable insights at the end of a standard ETL/ELT pipeline.
* **Observability:** Built-in logging provides **step-by-step execution traces** for high debuggability (NFR4).