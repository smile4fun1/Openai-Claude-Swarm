# Multi-Agent Swarm System with Claude 3.5 and ChatGPT 4.0

This project is a multi-agent system built using **Claude 3.5** for high-level reasoning and task finalisation, **ChatGPT 4.0** agents for intermediate data aggregation, and **GPT-Swarm** for orchestrating agent interactions. The entire system operates within a **React + Vite** framework, and all data flows are optimised using vectorisation for efficient storage and retrieval. The system is designed to be fast, cost-effective, and scalable, with real-time dashboards and control panel interfaces.

## Project Structure

```bash
root/
├── backend/
│   ├── app/
│   │   └── main.py # FastAPI server handling API requests
│   ├── agents/
│   │   ├── claude_intake.py # Claude 3.5 intake agent
│   │   ├── chatgpt_aggregator.py # ChatGPT 4.0 aggregation agents
│   │   └── claude_output.py # Claude 3.5 final output agent
│   └── vector_storage.py # Vector storage and retrieval functions using FAISS or ScaNN
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── Dashboard.js # React component for task monitoring
│   │   │   └── ControlPanel.js # React component for task submission
│   │   ├── api.js # API handlers for interaction with FastAPI
│   │   └── App.js # Main React app with routes
├── GPT-Swarm/ # Cloned repository for GPT-Swarm
├── .env # Environment variables for API keys (Claude, GPT)
└── README.md # This file
```

## How It Works

### Task Flow:

1.  **Task Submission**:
    *   User submits a task via the frontend.
    *   The task is sent to the **Claude Intake Agent** through the FastAPI backend.

2.  **Claude 3.5 Intake**:
    *   **Claude 3.5** performs initial reasoning and splits the task into subtasks.
    *   The task and subtasks are vectorised for efficient retrieval.

3.  **ChatGPT Aggregation**:
    *   **ChatGPT 4.0** agents handle intermediate processing, aggregation, and summarisation.
    *   Each agent is aware of the full task context via vectorised data storage, allowing them to work in sync.
    *   Each agent knows how to interact with the data and with other agents driven by logic and complex operations that work seamlessly.
    *   Each agent has specific instructions that are repeatead to it in each prompt to make sure that they adhere to it as master prompt.
    *   More features coming.
   
4.  **Claude 3.5 Output**:
    *   After intermediate processing, **Claude 3.5** retrieves the vectorised data, performs final reasoning, and generates the final response.
    *   The result is delivered back to the user via the **Dashboard** interface.

## Technology Stack

*   **Frontend**:
    *   **React**: UI components for task submission and monitoring.
    *   **Vite**: Build tool for optimised React development.
*   **Backend**:
    *   **FastAPI**: REST API to manage agent communication, task orchestration, and task retrieval.
*   **Agents**:
    *   **Claude 3.5 API**: Handles complex reasoning, task splitting, and final result generation.
    *   **ChatGPT 4.0 API**: Manages intermediate data aggregation and processing.
*   **Data Handling**:
    *   **FAISS/ScaNN**: Vectorised data storage for efficient task retrieval across agents.
    *   **Pinecone/Weaviate**: Production-ready vector databases for scalable data management.
    *   **Redis**: Caching layer for frequently accessed data.
    *   **Cloud-managed databases (e.g., AWS RDS, Google Cloud SQL)**: Persistent storage for task data and logs.
*   **Orchestration**:
    *   **GPT-Swarm**: Manages the interaction between Claude 3.5, ChatGPT agents, and task synchronisation.
    *   **Redis/Celery**: Task queue for asynchronous task distribution and management.
*   **Monitoring and Logging**:
    *   **ELK/Graylog**: Centralized logging system for comprehensive monitoring.
    *   **Prometheus/Grafana**: Metrics monitoring and visualisation.
*   **Other Components**:
    *   **Redis** (optional): For caching frequently accessed vectorised data.
    *   **Prometheus**: For system performance and health monitoring.

## Code Logic and Agent Roles

### Claude Intake Agent (`claude_intake.py`)

*   **Task**: Receives user-submitted tasks, uses **Claude 3.5** for initial reasoning, and vectorises the task for further processing.
*   **Logic**:
    *   Breaks down complex tasks into subtasks.
    *   Saves task context in vectorised form.

### ChatGPT Aggregator Agents (`chatgpt_aggregator.py`)

*   **Task**: Intermediate processing of subtasks, aggregating information while being aware of the entire task context.
*   **Logic**:
    *   Retrieves subtasks from vectorised storage.
    *   Works asynchronously to process and aggregate data efficiently.
    *   Vectorises the processed results for Claude to retrieve.

### Claude Output Agent (`claude_output.py`)

*   **Task**: Final task execution and reasoning, consolidating the results from ChatGPT agents and delivering the final output.
*   **Logic**:
    *   Retrieves intermediate results from the vectorised data.
    *   Applies high-level reasoning and delivers the final output.

## Use Cases

1.  **Complex Task Management**:
    *   Users can submit complex tasks that require multiple steps of processing. Claude 3.5 splits the task, ChatGPT agents handle intermediate steps, and Claude finalises the result.
2.  **Data Aggregation and Summarisation**:
    *   Tasks requiring aggregation from multiple sources (e.g., research summaries) can be processed by ChatGPT agents, with Claude providing the final structured output.
3.  **Real-time Task Monitoring**:
    *   Users can monitor task progress in real-time via the dashboard, seeing how each agent interacts with the task and how close it is to completion.
4.  **Cost-effective Data Handling**:
    *   Vectorised storage ensures that similar tasks are handled more efficiently, reducing the need for repetitive API calls and lowering costs.

## Production Deployment Considerations

*   **Infrastructure:** Deploy on a scalable cloud platform (e.g., AWS, Google Cloud, Azure) using containerisation (Docker) and orchestration (Kubernetes) for resilience and efficient resource utilisation.
*   **Data Management:** Use a production-ready vector database (e.g., Pinecone, Weaviate), implement caching (Redis), and ensure persistent storage for data and logs.
*   **Agent Orchestration:** Containerise agents, integrate GPT-Swarm for communication and synchronisation, and utilise a task queue (Redis/Celery) for asynchronous processing.
*   **Monitoring and Error Handling:** Implement centralized logging (ELK/Graylog), metrics monitoring (Prometheus/Grafana), alerting systems, and robust error handling within agents.
*   **Testing:** Conduct thorough unit, integration, and load testing before deployment.
*   **Deployment:** Employ a staged rollout strategy to minimize risk and allow for iterative improvements.
*   **Continuous Improvement:** Continuously monitor performance, analyse logs, and implement updates and enhancements to maintain system efficiency and adaptability.

## Best Practices

### 1. Async Task Processing**:

*   Use asynchronous processing to ensure that agents can handle tasks concurrently, improving overall speed.
*   Task queues (e.g., Redis, Celery) help synchronise the asynchronous processing when needed.

### 2. Vectorised Data Handling**:

*   **Claude** and **ChatGPT** agents work with vectorised data to reduce storage and retrieval times.
*   Vectorisation ensures that even large prompts or tasks can be stored efficiently, reducing costs and latency.

### 3. Modular and Scalable Architecture**:

*   The architecture allows easy addition of new agents or tasks.
*   Agents are modular, meaning they can be extended with new capabilities without impacting the entire system.

### 4. Error Handling and Logging**:

*   Implement robust error handling for API interactions.
*   Use logging to monitor agent activity, task progress, and system health.

### 5. Cache Frequently Used Data**:

*   Use Redis or another caching system to store frequently queried vectorised data, reducing API calls to **Claude** and **ChatGPT**.

## Next Steps and Future Improvements

*   **Improved Monitoring**: Add more detailed task monitoring to the dashboard, including per-agent performance and task durations.
*   **Agent Feedback Loop**: Implement a feedback loop where agents can refine their responses based on user feedback.
*   **Dynamic Scaling**: Use Kubernetes or Docker to dynamically scale the system depending on task load.

## Installation and Setup

1.  **Clone the repository**:

```bash
git clone https://github.com/smile4fun1/openai-claude-swarm.git
cd openai-claude-swarm
```

2.  **Install dependencies**:

```bash
npm install # For React frontend
pip install -r requirements.txt # For FastAPI and agents
```

3.  **Set up environment variables**:
    *   Create a `.env` file with the following:

```
CLAUDE_API_KEY=
GPT_API_KEY=
```

4.  **Start the backend**:

```bash
uvicorn backend.app.main:app --reload
```

5.  **Run the frontend**:

```bash
npm run dev
```

## Contributing

If you'd like to contribute to this project, please fork the repository and create a pull request. We welcome bug reports, suggestions, and new features!

## License

This project is licensed under the MIT License.
```
