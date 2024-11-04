# Integration of LLM and Google Search API using LangChain Agents

## Project Overview

This project demonstrates the integration of a Large Language Model (LLM) with the Google Search API via LangChain agents to automate data retrieval and provide summarized, real-time insights. By combining the historical data capabilities of the LLM with the live information from Google Search, we can enhance productivity by streamlining the process of retrieving and summarizing information.

## Motivation

Manually searching and summarizing data is time-consuming. This project leverages the power of LLM for its comprehensive, static knowledge and combines it with SerpAPI for up-to-date insights. This hybrid approach saves time and ensures relevant, real-time information.

## Key Components

- **LLM Model**: Gemini 1.5 Pro (accessible via Google AI Studio)
- **LangChain Agents and Tools**: Manage and automate interactions between LLM and Google Search
- **Google Search API**: SerpAPIWrapper for real-time search data

## Requirements

To run this project, you need to set up API keys from both:
- **Google AI Studio** for access to the Gemini model (free tier available with limitations)
- **SerpAPI** for Google Search functionality (free plan available with request limits)

### Dependencies

- `langchain`
- `dotenv`
- `langchain_community`
- `serpapi`
- `os`

## Setup Instructions

### Step 1: Clone the Repository

```bash
git clone https://github.com/yourusername/LLM-Google-Search-Integration.git
cd LLM-Google-Search-Integration
```

### Step 2: Install Required Libraries

Use the following command to install the necessary dependencies:

```bash
pip install -r requirements.txt
```

### Step 3: Configure Environment Variables

1. Get your API keys:
   - **Gemini API Key**: Obtain this from your Google AI Studio account.
   - **SerpAPI Key**: Register at [SerpAPI](https://serpapi.com/) and obtain your API key.

2. Create a `.env` file in the project directory and add your API keys:

```env
gemini_api_key="YOUR_GEMINI_API_KEY"
serpapi_api_key="YOUR_SERPAPI_KEY"
```

### Step 4: Initialize the LLM and Google Search API

```python
# Import necessary libraries
import os
from dotenv import load_dotenv
from langchain_google_genai import ChatGoogleGenerativeAI
from langchain.agents import AgentType
from langchain_community.utilities import SerpAPIWrapper
from langchain.agents import load_tools, initialize_agent

# Load environment variables
load_dotenv()

# Set up API key for Gemini
genapi_key = os.getenv("gemini_api_key")

# Initialize LLM
model = ChatGoogleGenerativeAI(api_key=genapi_key, model="gemini-1.5-pro")

# Initialize SerpApi
search = SerpAPIWrapper()

# Initialize LangChain agent
myagent = AgentType.CHAT_ZERO_SHOT_REACT_DESCRIPTION
mytools = load_tools(['serpapi'])

# Combine LLM and search tool in agent
google_chain = initialize_agent(agent=myagent, tools=mytools, llm=model, verbose=True)
```

### Step 5: Run an Example Query

To see the integration in action, you can use a sample query to retrieve the current date in India.

```python
# Example query to check LLM's data cut-off date
message = "Today date in India in one line"
ai_msg = model.invoke(message)
print("Response from LLM (historic data):", ai_msg.content)

# Example query to get current date using real-time search
live_date = google_chain.run("Today date in India in one line")
print("Response from Google Search API:", live_date)
```

## Results and Insights

### Objective Comparison
- **LLM Response**: Provides answers based on historical data up to the model's cut-off date.
- **Google Search API Response**: Retrieves live data, filling the gap left by LLM's static knowledge.

### Example Insight
Using the LLM alone might yield outdated information; however, integrating with SerpAPI allows for updated insights without manual search efforts.

## Conclusion

This project demonstrates how integrating LLM with real-time search APIs can automate data retrieval and summary generation, saving time and ensuring up-to-date information. This hybrid approach combines the strengths of both LLM for knowledge and SerpAPI for live data.

## Future Work

Consider expanding the project to handle different types of information queries, such as news, trends, or other sources, to broaden the scope of insights provided.

---

## Note on Free API Usage

Both **Gemini (Google AI Studio)** and **SerpAPI** offer free tiers with limited usage. Please review the respective documentation for information on request limits and potential charges on exceeding those limits.

