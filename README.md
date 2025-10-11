<img alt="TRAJECT-Bench — Trajectory-Aware Tool-Use Evaluation for Agents" src="assets/benchmark_logo9.png" width="700">

[![Name](https://img.shields.io/badge/Benchmark-TRAJECT--Bench-7c3aed)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
![Python](https://img.shields.io/badge/python-3.12%2B-blue)
<!-- [![HF Dataset](https://img.shields.io/badge/HuggingFace-dataset-blue)](https://huggingface.co/datasets/bigboss24/TRAJECT-Bench) -->

✨ A comprehensive benchmark for evaluating tool-using language models across multiple practical domains, designed to test models' ability to effectively utilize external tools for real-world tasks. Bring new insights to LLM-based agentic tool usage!

## 🆕 News
📢 [**2025/9/24**] We release all parallel and sequential data. We will release paper and results soon.

📢 [**2025/8/24**] We release the tool evaluation script for ReAct (agentic method), supporting both static and dynamic tool retrieval.

📢 [**2025/8/23**] We release the tool evaluation script for LLMs, supporting multiple query methods and tool selection modes.

📢 [**2025/8/21**] We release the first version of TRAJECT-Bench, including a high-quality executable production-style tool set and a novel tool-usage query dataset. The tool-calling trajectory is driven by real-world task types and invlove multiple tools from 3 to 10, enabling a scalable evaluation of tool-using capabilities. Queries consist of simple and hard versions, enabling deeper investigation on complexity.

---

## 🚀 Quickstart (TL;DR)

```bash
git clone <repository-url>
cd ToolData-public
pip install -r requirements.txt

# Tool evaluation on LLMs (not agentic evaluation)
python evaluation/tool_evaluation_model.py -model [model name] -tool_select [tool selection mode] -method [problem solving method] -k [tool pool size] -emb_model [embedding model] -emb_model_dir [embedding model directory] -traj_type [trajectory type] -traj_file [trajectory file name] -log_dir [log directory] -chk_dir [checkpoint directory] -base_data_dir [base data directory]

## default settings (direct prompting, domain mode, parallel trajectory, simple version, default model claude_v37)
python evaluation/tool_evaluation_model.py -model claude_v37 -tool_select domain -method direct -traj_type parallel -traj_file simple_ver -log_dir ./log/model -chk_dir ./chk/model -base_data_dir ./

## CoT setting (default model claude_v37, prompts in evaluation/evaluation_prompt.json)
python evaluation/tool_evaluation_model.py -model claude_v37 -tool_select domain -method cot -traj_type parallel -traj_file simple_ver -log_dir ./log/model -chk_dir ./chk/model -base_data_dir ./

## Retrieval tool pool setting (default model claude_v37, direct prompting, embedding model all-MiniLM, embedding model directory ./retriever, tool pool size 20)
python evaluation/tool_evaluation_model.py -model claude_v37 -tool_select retrieval -method direct -k 20 -emb_model all-MiniLM -emb_model_dir ./retriever -traj_type parallel -traj_file simple_ver -log_dir ./log/model -chk_dir ./chk/model -base_data_dir ./

# Agentic evaluation (ReAct)
python evaluation/tool_evaluation_agent.py -model [model name] -tool_select [tool selection mode] -method [problem solving method] -k [tool pool size] -emb_model [embedding model] -emb_model_dir [embedding model directory] -retrieve_mode [retrieve mode] -retrieve_pool [retrieve pool] -traj_type [trajectory type] -traj_file [trajectory file name] -log_dir [log directory] -chk_dir [checkpoint directory] -base_data_dir [base data directory]

## default settings (ReAct, domain tool pool, no retrieval, parallel trajectory, simple version, default model claude_v37)
python evaluation/tool_evaluation_agent.py -model claude_v37 -tool_select domain -method react -traj_type parallel -traj_file simple_ver -log_dir ./log/react -chk_dir ./chk/react -base_data_dir ./

## Static retrieval (default model claude_v37, static retrieval from domain tool pool)
python evaluation/tool_evaluation_agent.py -model claude_v37 -tool_select retrieval -method react -retrieve_mode static -retrieve_pool domain -traj_type parallel -traj_file simple_ver -log_dir ./log/react -chk_dir ./chk/react -base_data_dir ./

## Dynamic retrieval (default model claude_v37, dynamic retrieval from domain tool pool)
python evaluation/tool_evaluation_agent.py -model claude_v37 -tool_select retrieval -method react -retrieve_mode dynamic -retrieve_pool domain -traj_type parallel -traj_file simple_ver -log_dir ./log/react -chk_dir ./chk/react -base_data_dir ./

# Claude agentic tool-use
python evaluation/claude_tool_evaluation.py -model claude_v37 -tool_select domain -traj_type parallel -traj_file simple_ver -log_dir ./log/claude -chk_dir ./chk/claude -base_data_dir ./

# Other model's agentic evaulation is the same.
```

## 🎯 Overview

**🌟 Hightlights:**
- 🔧 **Multi-Tool Selection**: Combining multiple executable, production-style tools to solve complex queries  
- 🎯 **Practical Task Solving**: Incorporating practical tasks across diverse domains
- 🛤️ **Trajectory Structure Support**: First to evaluate both parallel and sequential tool-calling trajectories
- 📊 **Advanced Metrics**: Trajectory-aware evaluation metrics for comprehensive assessment
- 📈 **Query Difficulty Control**: Structured difficulty levels for progressive evaluation
- 🎲 **Multiple Tool-Pool Regimes**: Support for various tool pool setups, including whole toolset (mixture of tools), domain-specific tools, tool retrieval, small-scale fixed tool pool
- 🤖 **Agentic Method Support**: Evaluation frameworks for ReAct and other agentic approaches

<!-- Comparison with other tool benchmarks: -->

| Benchmark                         | Practical tools | Large&diverse tool| Trajectory structure<sup>1</sup> | Trajectory scaling<sup>2</sup> | Trajectory-aware metrics<sup>3</sup> | Query difficulty control | Tool-pool regimes<sup>4</sup> | Agentic methods |
|-----------------------------------|-----------------|-----------------|----------------------|-----------------------------|--------------------------|---------------------------|----------------------------|----------------|
| MetaTool                          | ✅              | ❌              | ❌                   | ❌                          | ❌                       | ❌                        | ❌                         | ❌             |
| API-Bank                          | ✅              | ❌              | ❌                   | ❌                          | ❌                       | ❌                        | ❌                         | ❌             |
| ToolBench                         | ✅              | ✅              | ❌                   | ❌                          | ❌                       | ❌                        | ❌                         | ✅             |
| Gorilla                           | ✅              | ✅              | ❌                   | ❌                          | ❌                       | ❌                        | ✅                         | ❌             |
| Berkeley Function-Calling (BFCL)  | ✅              | ✅              | ❌                   | ❌                          | ❌                       | ❌                        | ❌                         | ❌             |
| ToolQA                            | ❌              | ❌              | ❌                   | ❌                          | ❌                       | ✅                        | ❌                         | ✅             |
| **TRAJECT-Bench (ours)**          | **✅**          | **✅**          | **✅**               | **✅**                       | **✅**                   | **✅**                    | **✅**                      | **✅**          |

<sup>1</sup> **Trajectory structure**: Evaluates support for different tool-calling patterns, including parallel (independent tools) and sequential (dependent tool chains) execution strategies  
<sup>2</sup> **Trajectory scaling**: Tests model performance across varying task complexity levels, from simple 3-tool scenarios to complex 10+ tool orchestration  
<sup>3</sup> **Trajectory-aware metrics**: Provides comprehensive evaluation beyond final results, measuring the quality of the entire tool-calling trajectory  
<sup>4</sup> **Tool-pool regimes**: Supports diverse evaluation strategies including whole toolset, domain-specific pools, retrieval-based selection, and fixed small-scale tool pools


**Data synthesis overview**

The following figure shows the data synthesis of TRAJECT-Bench, including *parallel* and *sequential*.

<figure>
  <img src="assets/illustration.png" alt="TRAJECT-Bench Illustration" width="100%">
</figure>




### 🔗 Quick Links

<!-- - 📊 Dataset on Hugging Face: [`bigboss24/TRAJECT-Bench`](https://huggingface.co/datasets/bigboss24/TRAJECT-Bench) -->
- 📁 Public data folder: `public_data/`
- 🧪 Evaluation scripts: `evaluation/tool_evaluation_model.py`, `evaluation/tool_evaluation_agent.py`
## 🏗️ Benchmark Structure

### 🌍 Domains Covered

The benchmark covers 10 carefully selected domains that require external tools rather than internal model capabilities:

- ✈️ **Travel**: Hotel booking, flight information, trip planning, transportation
- 🌤️ **Weather**: Forecasts, meteorological data, climate information
- 💰 **Finance**: Market data, trading platforms, cryptocurrency, banking
- 🗺️ **Mapping**: Location services, routing, geographic data
- 🎵 **Music**: Streaming, lyrics, artist information, music metadata
- 📰 **News & Media**: News articles, multimedia content, current events
- 📚 **Education**: Learning resources, academic data, research tools
- 📧 **Email**: Communication, automation, folder management
- 🎮 **Gaming**: Game data, statistics, account management
- 🛒 **eCommerce**: Online shopping, inventory, product information

### 📁 Data Organization

```
public_data/
├── tools/           # Tool definitions and APIs for each domain
└── parallel/        # Parallel trajectory sets
│   ├── Education/   # Education domain
│       ├──hard_ver.json   # Hard queries
│       └──simple_ver.json # Simple queries
│   ├── Finance/     # Domain-specific test cases
│   ├── Travel/      # Multi-tool scenarios
│   └── ...          # Other domains
└── sequential/        # Sequential trajectory sets
│   ├── Education/   # Education domain
│       └──traj_query.json # Trajectory and query data
│   ├── Finance/     # Domain-specific test cases
│   ├── Travel/      # Multi-tool scenarios
│   └── ...          # Other domains
```

### 📊 Dataset at a Glance (for parallel, sequential on the way)

- 🌍 **10 domains**: Education, Email, Finance, Gaming, Mapping, Music, News_Media, Travel, Weather, eCommerce
- 🎯 **50 practical task types**: 5 representative user task types for each domain
- 🛤️ **2,000 total parallel trajectories** with different depth: 5 trajectory per number of tools (from 3-10) for each task
- 📝 **2,000 queries per difficulty**: one simple and one hard for each trajectory
- 🔢 **4,000 total parallel trajectories**: simple (2,000) + hard (2,000)
- 🛠️ Tool metadata: `public_data/tools/*.json`

### 🔄 Query/Trajectories Types

We take a trajectory->query strategy

1. 🔗 **Parallel tool-calling trajectory**: Independent tools collaborate for one task. Test sub-task planning and tool-usage capability.
2. ⏭️ **Sequential tool-calling trajectory**: Trajectories with strong dependency among tools, i.e. latter tools require former tools' outputs.
3. 📝 **Simple Queries**: Straightforward and clear instructions requiring multiple tools
4. 🧠 **Hard Queries**: Indirect and indicating queries that are challenging, but avoid vagueness and much openness.

### 📋 Query JSON Structure

Each entry in `public_data/parallel/<Domain>/*.json` follows this structure:

- **query**: Natural language instruction
- **tool_list**: Tool-calling trajectory for solving the query
  - **tool name**: `<parent tool name>: <API name>`
  - **tool description**: Brief description of the API endpoint
  - **required parameters**: List of objects `{ "name": string, "value": string }`
  - **optional parameters**: List of objects `{ "name": string, "value": string }` (can be empty)
  - **parent tool name**: Name of the tool/provider
  - **API name**: Endpoint name
  - **domain name**: Domain category
- **trajectory_type**: `parallel` or `sequential`
- **task_name**: Short title of the general task type
- **task_description**: Extended description of the task type

Example:

```json
{
  "query": "First, retrieve the post with ID P36288 from FluentMe. Then, get information about the university with ID 394596. Finally, show me a list of video game adaptations from the Transmedia Storytelling database.",
  "tool_list": [
    {
      "tool name": "thefluentme: Get post by id",
      "tool description": "Returns a specific post using its post_id identifier...",
      "required parameters": [{ "name": "post_id", "value": "P36288" }],
      "optional parameters": [],
      "parent tool name": "thefluentme",
      "API name": "Get post by id",
      "domain name": "Education"
    },
    ...
  ],
  "trajectory_type": "parallel",
  "task_name": "Language and Culture Homework Helper",
  "task_description": "This task assists students with language and culture assignments. It involves finding definitions, translations, character information, and contextual examples for various languages."
}
```

### 📝 Minimal sequential example (illustrative)

```json
{
  "query": "Find a flight from NYC to SFO next Friday and then get the 3-day weather forecast for the arrival dates in SFO.",
  "tool_list": [
    {
      "tool name": "skyscanner_flights: search_flights",
      "required parameters": [{ "name": "origin", "value": "NYC" }, { "name": "destination", "value": "SFO" }, { "name": "date", "value": "<next_friday>" }],
      "optional parameters": [],
      "parent tool name": "skyscanner_flights",
      "API name": "search_flights",
      "domain name": "Travel"
    },
    {
      "tool name": "forecast_lookup: 3day",
      "required parameters": [{ "name": "location", "value": "SFO" }, { "name": "date", "value": "<arrival_date>" }],
      "optional parameters": [],
      "parent tool name": "forecast_lookup",
      "API name": "3day",
      "domain name": "Weather"
    }
  ],
  "trajectory_type": "sequential",
  "task_name": "Travel planning with weather guardrails",
  "task_description": "Plan travel and check weather constraints based on retrieved itinerary."
}
```

## 📊 Evaluation Metrics

The benchmark evaluates models on:

- 🎯 **Exact Match**: Exact same selected tools with/without order consideration
- 🔍 **Tool includion**: Fraction of ground-truth tools that appear in the predicted trajectory
- ⚙️ **Tool parameterization**: Proper usage of tools, including input values and formats
- 🏆 **Trajectory win rate**: Given the predicted vs. reference trajectories, an LLM judge picks which better satisfies the task
- ✅ **Trajectory satisfication**: Judge rates whether the predicted trajectory sufficiently satisfies the query (LLM judge)
- 🎯 **Solution Accuracy**: Compare predicted solution with the ground truth solution (executation involved)

📋 Detailed metrics in `utils/metrics.py`

## 📊 Evaluation Results

Results of individual model’s tool-use capability with domain-specific tools on parallel queries (direct query).

| **Model**            | **Simple** |               |           |                  |         | **Hard** |               |           |                  |         |
|----------------------|------------|---------------|-----------|------------------|---------|----------|---------------|-----------|------------------|---------|
|                      | **EM**     | **Inclusion** | **Usage** | **Traj-Satisfy** | **Acc** | **EM**   | **Inclusion** | **Usage** | **Traj-Satisfy** | **Acc** |
| **Claude-3.7**       | 0.676      | 0.746         | 0.706     | 6.831            | 0.714   | 0.135    | 0.554         | 0.603     | 1.582            | 0.246   |
| **Claude-4**         | 0.846      | 0.855         | 0.839     | 8.549            | 0.905   | 0.445    | 0.668         | 0.794     | 4.882            | 0.517   |
| **Gemini-2.5-flash** | 0.714      | 0.759         | 0.784     | 7.215            | 0.782   | 0.216    | 0.538         | 0.666     | 2.340            | 0.263   |
| **Gemini-2.5-pro**   | 0.851      | 0.854         | 0.835     | 8.599            | 0.911   | 0.442    | 0.652         | 0.785     | 4.849            | 0.498   |
| **GPT5-mini**        | 0.817      | 0.825         | 0.819     | 8.236            | 0.834   | 0.155    | 0.589         | 0.626     | 1.701            | 0.380   |
| **o4-mini**          | 0.823      | 0.830         | 0.824     | 8.316            | 0.863   | 0.376    | 0.629         | 0.726     | 3.849            | 0.472   |
| **gpt-oss:120b**     | 0.652      | 0.667         | 0.715     | 6.588            | 0.726   | 0.174    | 0.559         | 0.671     | 1.909            | 0.387   |
| **DeepSeek**         | 0.833      | 0.836         | 0.829     | 8.417            | 0.889   | 0.439    | 0.669         | 0.757     | 4.817            | 0.458   |
| **qwen3-235b-A22B**  | 0.844      | 0.856         | 0.809     | 8.529            | 0.898   | 0.440    | 0.667         | 0.796     | 4.828            | 0.479   |
| **Kimi-k2**          | 0.815      | 0.876         | 0.872     | 8.236            | 0.902   | 0.321    | 0.666         | 0.772     | 3.522            | 0.448   |

Results of individual model’s tool-use capability with domain-specific tools on sequential queries.

| **Model**            | **EM** | **Inclusion** | **Usage** | **Traj-Satisfy** | **Acc** |
|----------------------|--------|---------------|-----------|------------------|---------|
| **Claude-3.7**       | 0.583  | 0.724         | 0.584     | 6.010            | 0.573   |
| **Claude-4**         | 0.819  | 0.832         | 0.775     | 8.243            | 0.813   |
| **Gemini-2.5-flash** | 0.613  | 0.695         | 0.714     | 6.219            | 0.652   |
| **Gemini-2.5-pro**   | 0.807  | 0.821         | 0.809     | 8.119            | 0.848   |
| **GPT5-mini**        | 0.693  | 0.715         | 0.692     | 7.042            | 0.677   |
| **o4-mini**          | 0.789  | 0.807         | 0.748     | 8.134            | 0.761   |
| **gpt-oss:120b**     | 0.538  | 0.619         | 0.694     | 5.546            | 0.653   |
| **DeepSeek**         | 0.825  | 0.849         | 0.811     | 8.305            | 0.823   |
| **qwen3-235b-A22B**  | 0.824  | 0.837         | 0.772     | 8.194            | 0.791   |
| **Kimi-k2**          | 0.821  | 0.827         | 0.793     | 8.363            | 0.833   |


Results of individual models combined with retrieval-based tool-selection strategy.

| **Claude_v37** |                    | **Simple**         |        |               |           |                  |         | **Hard**           |        |               |           |                  |         |
|----------------|--------------------|--------------------|--------|---------------|-----------|------------------|---------|--------------------|--------|---------------|-----------|------------------|---------|
| **Emb_mdel**   | **Retrieval pool** | **Retrieval rate** | **EM** | **Inclusion** | **Usage** | **Traj-Satisfy** | **Acc** | **Retrieval rate** | **EM** | **Inclusion** | **Usage** | **Traj-Satisfy** | **Acc** |
| **bge-large**  | domain             | 0.906              | 0.681  | 0.792         | 0.738     | 7.134            | 0.708   | 0.585              | 0.035  | 0.410         | 0.692     | 0.541            | 0.127   |
|                | all                | 0.842              | 0.639  | 0.762         | 0.728     | 6.592            | 0.665   | 0.482              | 0.020  | 0.341         | 0.657     | 0.334            | 0.098   |
| **all-miniLM** | domain             | 0.913              | 0.685  | 0.793         | 0.749     | 7.257            | 0.717   | 0.584              | 0.029  | 0.403         | 0.683     | 0.265            | 0.109   |
|                | all                | 0.868              | 0.645  | 0.751         | 0.745     | 6.613            | 0.680   | 0.460              | 0.012  | 0.403         | 0.641     | 0.140            | 0.082   |
| **ToolLM-IR**  | domain             | 0.945              | 0.703  | 0.814         | 0.778     | 7.142            | 0.715   | 0.578              | 0.030  | 0.419         | 0.698     | 0.294            | 0.139   |
|                | all                | 0.877              | 0.652  | 0.783         | 0.758     | 6.770            | 0.696   | 0.475              | 0.024  | 0.425         | 0.656     | 0.259            | 0.132   |
| **Claude_v4**  |                    |                    |        |               |           |                  |         |                    |        |               |           |                  |         |
| **bge-large**  | domain             | 0.906              | 0.852  | 0.867         | 0.835     | 8.631            | 0.902   | 0.585              | 0.031  | 0.397         | 0.672     | 0.328            | 0.264   |
|                | all                | 0.842              | 0.785  | 0.823         | 0.770     | 8.053            | 0.876   | 0.482              | 0.012  | 0.292         | 0.656     | 0.111            | 0.189   |
| **all-miniLM** | domain             | 0.913              | 0.833  | 0.859         | 0.786     | 8.764            | 0.870   | 0.584              | 0.033  | 0.410         | 0.663     | 0.355            | 0.277   |
|                | all                | 0.868              | 0.817  | 0.806         | 0.773     | 8.352            | 0.832   | 0.460              | 0.015  | 0.267         | 0.620     | 0.171            | 0.168   |
| **ToolLM-IR**  | domain             | 0.945              | 0.906  | 0.928         | 0.833     | 9.117            | 0.916   | 0.578              | 0.028  | 0.420         | 0.680     | 0.286            | 0.241   |
|                | all                | 0.877              | 0.852  | 0.861         | 0.764     | 8.613            | 0.879   | 0.475              | 0.014  | 0.298         | 0.653     | 0.190            | 0.164   |

Evaluation of model’s inherent agentic tool-use capability.

| **model**          |         | **Simple** |               |           |                  |         | **Hard** |               |           |                  |         |
|--------------------|---------|------------|---------------|-----------|------------------|---------|----------|---------------|-----------|------------------|---------|
|                    |         | **EM**     | **Inclusion** | **Usage** | **Traj-Satisfy** | **Acc** | **EM**   | **Inclusion** | **Usage** | **Traj-Satisfy** | **Acc** |
| **Claude-4**       | agentic | 0.832      | 0.868         | 0.816     | 8.407            | 0.893   | 0.440    | 0.637         | 0.751     | 4.828            | 0.486   |
|                    | context | 0.846      | 0.855         | 0.839     | 8.549            | 0.905   | 0.445    | 0.668         | 0.794     | 4.882            | 0.517   |
| **Gemini-2.5-pro** | agentic | 0.828      | 0.866         | 0.876     | 8.367            | 0.917   | 0.416    | 0.674         | 0.768     | 4.564            | 0.503   |
|                    | context | 0.851      | 0.847         | 0.835     | 8.599            | 0.911   | 0.442    | 0.652         | 0.785     | 4.849            | 0.498   |
| **Deepseek**       | agentic | 0.819      | 0.857         | 0.756     | 8.256            | 0.845   | 0.416    | 0.682         | 0.783     | 4.564            | 0.481   |
|                    | context | 0.835      | 0.833         | 0.729     | 8.417            | 0.889   | 0.439    | 0.669         | 0.757     | 4.817            | 0.458   |
| **Kimi-k2**        | agentic | 0.853      | 0.977         | 0.893     | 8.620            | 0.951   | 0.315    | 0.717         | 0.765     | 3.456            | 0.437   |
|                    | context | 0.815      | 0.876         | 0.872     | 8.236            | 0.902   | 0.321    | 0.666         | 0.772     | 3.522            | 0.448   |

## 🚀 Getting Started

### 📋 Prerequisites

- 🐍 Python 3.12+
- 📦 Required packages (see requirements.txt)

### ⚙️ Installation

```bash
git clone <repository-url>
cd ToolData-public
pip install -r requirements.txt
```
### ⚠️ Before running

1. Setup models in utils/model_providers.py
2. The execuation of tools relies on endpoints provided by [ToolBench](https://github.com/OpenBMB/ToolBench). Thanks to their contribution!


### 🧪 Running Evaluations (to finish soon)

```bash
# Simple query evaluation with direct prompting, CoT, etc. Support multiple tool selection modes and trajectory types.
python evaluation/tool_evaluation.py

# Agentic evaluation (ReAct) Support both static and dynamic tool retrieval.
python evaluation/tool_evaluation_react.py
```

### 📁 Data Access

The benchmark data is publicly available in the `public_data/` directory:

- 🛠️ **Tool Definitions**: JSON files containing API specifications and parameters
- 📝 **Test Queries**: Curated queries with ground truth tool execution paths


### 📊 Data Source

Primary dataset hosting: This repo

<!-- - 🤗 Hugging Face dataset: [`bigboss24/TRAJECT-Bench`](https://huggingface.co/datasets/bigboss24/TRAJECT-Bench) -->

<!-- ## Benchmark Design Principles

1. **Practical Focus**: Queries are designed around real-world use cases
2. **Tool Dependency**: All queries require external tools for complete solutions
3. **Multi-Tool Scenarios**: Complex problems requiring tool coordination
4. **Domain Diversity**: Coverage across various practical domains
5. **Scalable Evaluation**: Consistent metrics across different model types -->

<!-- ## Leaderboard (placeholder)

| Model | Tool Selection | Planning | Overall |
|------|-----------------|----------|---------|
| Your Model | — | — | — |

Submit results via PR or issue. We will maintain a simple leaderboard here. -->
## 🔮 Future update

- ⭐ (Highest priority) Complete evaluation pipeline, add detailed instructions
- ⭐ (Highest priority) Add sequential trajectory and query data
- 🌍 Add more domains and tasks


## 🤝 Contributing

We welcome contributions to improve the benchmark:

- 🌍 Additional domains and tools
- 📊 Enhanced evaluation metrics
- 🔄 New query types and scenarios
- ⚡ Performance optimizations

<!-- ## Citation

If you use this benchmark in your research, please cite:

```bibtex
@article{he2025traject,
  title={TRAJECT-Bench: A Trajectory-Aware Benchmark for Evaluating Agentic Tool Use},
  author={He, Pengfei and Dai, Zhenwei and He, Bing and Liu, Hui and Tang, Xianfeng and Lu, Hanqing and Li, Juanhui and Ding, Jiayuan and Mukherjee, Subhabrata and Wang, Suhang and others},
  journal={arXiv preprint arXiv:2510.04550},
  year={2025}
}
``` -->

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 📞 Contact

For questions or feedback about the benchmark, please open an issue on GitHub or contact the maintainers.

---

*This benchmark is designed to advance the field of tool-using language models by providing comprehensive evaluation scenarios that reflect real-world tool usage patterns.*
