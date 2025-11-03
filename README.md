# Assignment 2: Multi-Agent Travel Planner

## Overview

This project implements a multi-agent travel planning application using Streamlit. The application transforms a vague user prompt (e.g., "a week-long trip to Europe for a student") into a detailed, validated, day-by-day itinerary.

This system is built using the `assign_2.py` template and relies on a collaborative workflow between two distinct AI agents: a Planner and a Reviewer.

## Objectives

* Implement a multi-agent application that converts a vague user request into a concrete, actionable travel plan.
* Define two specialized agent roles:
    1.  **Planner Agent**: An "offline" agent that generates a creative, detailed itinerary based only on its internal knowledge.
    2.  **Reviewer Agent**: An "online" agent that validates the plan, fact-checks details (prices, hours), and identifies logical issues.
* Correctly integrate and use the `internet_search` tool (Tavily) within the Reviewer Agent for real-time validation.
* Demonstrate the complete Planner $\rightarrow$ Reviewer workflow within the Streamlit interface.

## Key Features

* **Multi-Agent Workflow**: A two-step pipeline where the Planner drafts the plan and the Reviewer refines it.
* **Dynamic Fact-Checking**: The Reviewer Agent uses the `tavily-python` library to perform live internet searches, verifying prices, opening hours, and travel times.
* **Structured Output (Delta List)**: The Reviewer provides feedback as a "Delta List," which details specific issues, suggested fixes, and the reasons for each change.
* **Interactive UI**: Built with Streamlit, the app features a main chat window and a sidebar logger that shows live tool-call activity.
* **Core Framework**: Powered by the `openai-agents` library, which provides the `Agent`, `Runner`, and `function_tool` components.

## Usage (How to Run)

Follow these steps to run the application locally.

### 1. Install Dependencies

Required Python libraries must be installed, included in `function_tool.txt`.

### 2. Set Environment Variables
This application requires API keys for both OpenAI and Tavily (for search). Here is an example.

Run the following commands in the terminal:
```bash
export API_KEY="/*YOUR API KEY HERE*/"
export TAVILY_API_KEY="/*YOUR API KEY HERE*/"
```

### 3. Run the Application
Once dependencies are installed and keys are set, run the following command in the terminal:
```bash
streamlit run assign_2.py
```