# ref-log.md

##  Reference Sources

* `assign_2.py`: The multi-agent Streamlit application template provided by the course.
* `openai-agents` (Python Library): The external library used to provide the core agent framework, including `Agent`, `Runner`, and `function_tool`.
* `tavily-python` (Python Library): The external library used to provide the `internet_search` tool.
* Course Assignment 2 Brief: Used as the primary source for defining the required agent roles, constraints, and output formats.

## Reflection

### Learnings

I learned that the core value of a multi-agent workflow lies in **separation of concerns** and **information asymmetry**.

* **Separation of Concerns**: The Planner Agent was designed as a "creative but isolated" specialist. By having no internet access, it could quickly (and cheaply) generate a complete draft. The Reviewer Agent was the "rigorous but expensive" specialist, whose only job was to validate and correct. This split is much more efficient than having one giant "mono-agent" try to do both.
* **Information Asymmetry**: This workflow, where one agent acts "blind" (Planner) and the other acts "sighted" (Reviewer), is a powerful way to model real-world collaboration. The Planner provides the *structure*, while the Reviewer provides the *accuracy*.
* **Prompting for Collaboration**: The "Delta List" requirement was critical. Without this structured output, the Reviewer might have returned a vague paragraph. By forcing it into a "list of concrete changes with reasons," the prompt ensures its output is actionable and easy to read.

### Challenges
My primary challenges were not in the agent logic itself, but in the **environment configuration and dependency management**.

* **Challenge 1: `ModuleNotFoundError: No module named 'tavily'`**
    * **Problem**: The application failed on startup because it couldn't find the search tool library.
    * **Solution**: I identified this as a missing third-party dependency and resolved it by running `pip install tavily-python`.

* **Challenge 2: `ModuleNotFoundError: No module named 'agents'`**
    * **Problem**: The application failed on startup because it couldn't find the search tool library.
    * **Solution**: I identified that `assign_2.py` was built for the `openai-agents` Python library and solved the error with `pip install openai-agents`.

* **Challenge 3: `bash: export: =: not a valid identifier`**
    * **Problem**: My terminal failed to set the API keys using the `export` command.
    * **Solution**: I realized this was a simple (but common) bash syntax error. I had used spaces around the `=` sign. Removing the spaces (e.g., `export TAVILY_API_KEY="..."`) fixed the issue and allowed the script to authenticate.

### Ideas
My main design choices were focused on the **prompt engineering** of the `INSTRUCTIONS` to make the agent roles as robust as possible.

* **Planner Prompt Design**: I deliberately added the `**CRITICAL LIMITATION:** You have **NO internet access**` instruction. This was vital to *force* the Planner to rely on its internal (and potentially outdated) knowledge. This "intentional weakness" is what makes the Reviewer's job necessary and valuable.
* **Reviewer Prompt Design**: I used strong, explicit commands like `You **MUST** use the provided internet_search tool` and `Your final output **MUST** be a "Delta List"`. This prevents the agent from "getting lazy" and trying to validate from its own knowledge (which would defeat the purpose) and ensures its output is always structured as required by the assignment.

## Testing

* **Test Environment**:
    * Ran the application locally using `streamlit run assign_2.py`.
    * API keys (`OPENAI_API_KEY`, `TAVILY_API_KEY`) were set as environment variables using `export` in the terminal session.

* **Test Prompt 1: "Plan a week-long Europe trip for a student on a $1,500 budget who loves history and food"**
    * **Planner Agent Output**: Successfully generated a detailed 7-day itinerary for Rome, Florence, Venice, and Prague. The plan included many estimated prices (e.g., $18 Colosseum, $12 Uffizi Gallery, $15 Doge's Palace).
    * **Reviewer Agent Output**: **Success**. The Reviewer produced a correct "Delta List" that "Verified official ticket prices" and corrected the Uffizi to €25, the Doge's Palace to €30, etc. This confirms the fact-checking workflow.

* **Test Prompt 2: "3-day Paris trip for art lovers with $800 budget"**
    * **Planner Agent Output**: Successfully generated a 3-day Paris itinerary. It included a critical *feasibility error*: it scheduled a visit to the Centre Pompidou at 9:30 AM.
    * **Reviewer Agent Output**: **Success**. Its "Delta List" correctly identified the issue: `Issue: Visit timing conflicts with opening hours. Found Hours: Opens 11:00 AM`. This perfectly demonstrated the validation requirement of the assignment.

* **Conclusion**: Both tests confirm that the Planner agent (offline) and Reviewer agent (online, with tools) are interacting correctly. The Reviewer successfully uses the `internet_search` tool to create an actionable "Delta List" that corrects the Planner's drafts.