# Codeforces Contest Solver - AI-Powered Solution Generator

## 🎯 Project Overview

An intelligent system that **automatically solves Codeforces competitive programming problems** using a multi-agent AI architecture. The system analyzes problem statements, retrieves similar solved problems, generates solution plans, creates code, and iteratively improves it based on test execution feedback.

### Key Features
- 🤖 **Multi-Agent Architecture** - 5 specialized agents working together (Retriever, Planner, Verifier, Generator, Improver)
- 🧪 **Sandboxed Code Execution** - Safe, isolated environment to test generated code with resource limits
- 🔄 **Iterative Refinement** - Feedback loop automatically improves code until tests pass
- 📊 **Multiple LLM Support** - OpenAI, Anthropic, Groq, Mistral, Ollama, GitHub Models
- 📝 **Comprehensive Evaluation** - Batch processing of problems with detailed metrics

---

## 🏗️ Architecture

```
Problem Input
    ↓
[Retriever Agent] → Finds similar solved problems & algorithm patterns
    ↓
[Plan Generator Agent] → Creates solution strategy
    ↓
[Plan Verifier Agent] → Validates plan with confidence score
    ↓
[Code Generator Agent] → Writes Python code
    ↓
[Sandbox Executor] → Tests code against unit tests
    ↓
    Failed? → [Code Improver Agent] → Modify & retry
    Passed? → Return Solution
```

---

## 🚀 Quick Start

### Prerequisites
- Python 3.11+
- API Keys for LLM providers (Anthropic, OpenAI, etc.)

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/Ritvvvik/GENAI-projects.git
cd GENAI-projects
```

2. **Create virtual environment**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

4. **Set up environment variables**
```bash
cp .env.example .env
# Edit .env and add your API keys
nano .env
```

5. **Configure settings** (optional)
Edit `config/config.toml`:
```toml
max_iter = 5  # Max improvement iterations
[llm_generator]
name = "anthropic:claude-3-5-sonnet-20241022"
api_key = "sk-ant-..."
temperature = 0.7
max_tokens = 4096
```

### Run Web UI
```bash
python gradio_ui.py
```
Then open `http://localhost:7860` in your browser

### Run Batch Evaluation
```bash
python evaluator.py
```

---

## 💡 How It Works

### Example: Solving a Problem

**Input:**
```json
{
  "description": "Given array of n integers, find two numbers that sum to target",
  "unit_tests": [
    {"input": "1 2 3 4\n6", "output": "1 3"},
    {"input": "1 2 3\n5", "output": "1 4"}
  ]
}
```

**Process:**
1. **Retriever** finds similar "Two Sum" problems and suggests "Hash Map" algorithm
2. **Planner** creates step-by-step strategy
3. **Verifier** scores confidence (e.g., 0.92)
4. **Generator** writes Python code
5. **Executor** tests against unit tests
6. If tests fail, **Improver** refines code + tries again
7. Returns final working solution

---

## 📊 Metrics & Evaluation

The system tracks:

| Metric | Description |
|--------|-------------|
| **PASSED** | Code correct & all tests pass |
| **WRONG_ANSWER** | Code runs but output incorrect |
| **TIME_LIMIT_EXCEEDED** | Takes >2 seconds |
| **RUNTIME_ERROR** | Code crashes |
| **COMPILATION_ERROR** | Syntax/import error |
| **MEMORY_LIMIT_EXCEEDED** | Exceeds memory |

**Success Rate** = (Problems Solved / Total Problems) × 100%

---

## 🔧 Configuration

### Supported LLM Models

```python
# Anthropic Claude
"anthropic:claude-3-5-sonnet-20241022"

# OpenAI GPT
"openai:gpt-4"
"openai:gpt-4-turbo"

# Groq (Fast inference)
"groq:mixtral-8x7b-32768"

# Local Ollama
"ollama:mistral"

# Mistral
"mistral:mistral-large"

# Google Gemini
"google-gla:gemini-1.5-pro"
```

### Configuration File (config/config.toml)

```toml
max_iter = 5  # Maximum improvement iterations

[llm_generator]
name = "anthropic:claude-3-5-sonnet-20241022"
api_key = "your-api-key"
base_url = ""  # Optional, for custom endpoints
temperature = 0.7
max_tokens = 4096

[logfire]
token = "your-logfire-token"

[file_paths]
src_dir = "./data"
eval_file = "problems.jsonl"
results_file = "results/output.jsonl"
```

---

## 📁 Project Structure

```
GENAI-projects/
├── agents/                    # AI agents
│   ├── retriever_agent.py     # Finds similar problems
│   ├── plan_generator_agent.py # Creates solution plans
│   ├── plan_verifier_agent.py  # Validates plans
│   ├── code_generator_agent.py # Generates code
│   └── code_improver_agent.py  # Improves failed code
├── code_engine/
│   └── code_executor.py       # Sandboxed code execution
├── llm/
│   ├── llm.py                 # LLM provider abstraction
│   └── prompt_engine.py       # Prompt templates
├── config/
│   ├── config.toml            # Configuration file
│   └── config_reader.py       # Config parser
├── utils/
│   ├── problem.py             # Problem data structure
│   ├── parser.py              # Response parsing
│   └── jsonl.py               # JSONL utilities
├── code_contest_solver.py     # Main orchestrator
├── evaluator.py               # Batch evaluation
├── gradio_ui.py               # Web UI
├── Dockerfile                 # Container setup
├── requirements.txt           # Python dependencies
└── README.md                  # This file
```

---

## 🔌 Key Components

### 1. Code Executor (Sandbox)
```python
executor = CodeExecutor()
result = executor.evaluateWithFeedback(
    src_uid="problem_123",
    unittests=[{"input": "...", "output": "..."}],
    code="print('hello')"
)
```

**Features:**
- Isolated process execution using multiprocessing
- 2-second timeout protection
- Stderr/stdout capture
- Return code validation

### 2. Agent Orchestration
```python
solver = CodeContestSolverMapCoder()
is_solved, plan, code = await solver.generate(problem)
```

**Flow:**
1. Retrieve similar problems
2. Generate & verify plans (sorted by confidence)
3. Generate code from best plan
4. Execute & collect feedback
5. Improve iteratively or return solution

### 3. LLM Integration
```python
from llm.llm import build_model

model = build_model(
    model_name="anthropic:claude-3-5-sonnet-20241022",
    api_key="sk-ant-...",
    temperature=0.7,
    max_tokens=4096
)
```

---

## 🚀 Deployment

### Option 1: Hugging Face Spaces (Recommended)
1. Go to [huggingface.co/spaces](https://huggingface.co/spaces)
2. Create new Space with Docker SDK
3. Connect this GitHub repo
4. Add secrets: `ANTHROPIC_API_KEY`, `OPENAI_API_KEY`, `LOGFIRE_TOKEN`
5. Deploy!

### Option 2: Docker Locally
```bash
docker build -t codeforces-solver .
docker run -p 7860:7860 \
  -e ANTHROPIC_API_KEY="sk-ant-..." \
  -e OPENAI_API_KEY="sk-..." \
  codeforces-solver
```

### Option 3: Local Development
```bash
python gradio_ui.py
```

---

## 📈 Performance & Results

**Metrics tracked:**
- ✅ Success rate (% of problems solved)
- ⏱️ Average execution time
- 🔄 Average iterations to solve
- 📊 Error distribution by type

**Example Results (from evaluation):**
```
Total Problems: 100
Solved: 85 (85%)
Failed:
  - Wrong Answer: 10
  - Runtime Error: 3
  - Timeout: 2
Average Iterations: 2.3
```

---

## 🔍 Observability

The system uses **Logfire** for detailed execution tracking:

```python
logfire.instrument_pydantic_ai()  # AI agent tracing
logfire.instrument_openai()        # LLM call tracing
```

View logs at [logfire.io](https://logfire.io)

---

## 🛠️ Development

### Adding a New Agent

1. Create `agents/new_agent.py`:
```python
from pydantic_ai import Agent

new_agent = Agent(
    "anthropic",
    name="NewAgent",
    system_prompt="You are a specialized agent..."
)
```

2. Add to `code_contest_solver.py`
3. Integrate into the orchestration flow

### Running Tests
```bash
# Add your test suite
pytest tests/
```

---

## 📝 Environment Variables

```bash
# Required
ANTHROPIC_API_KEY=sk-ant-...
OPENAI_API_KEY=sk-...

# Optional
LOGFIRE_TOKEN=...
GITHUB_TOKEN=...
GROQ_API_KEY=...
MISTRAL_API_KEY=...
```

---

## 🤝 Contributing

Feel free to:
- Report issues
- Suggest improvements
- Submit pull requests

---

## 📚 References

- [Pydantic AI Documentation](https://docs.pydantic.dev/latest/concepts/ai_models/)
- [Anthropic Claude API](https://docs.anthropic.com)
- [Gradio Documentation](https://www.gradio.app)
- [Codeforces API](https://codeforces.com/apiHelp)

---

## 📄 License

MIT License - See LICENSE file for details

---

## 👤 Author

**Ritvik K** (Ritvvvik)
- GitHub: [@Ritvvvik](https://github.com/Ritvvvik)
- Project: [GENAI-projects](https://github.com/Ritvvvik/GENAI-projects)

---

## 🎯 Future Roadmap

- [ ] MCP Server integration for tool standardization
- [ ] Support for multiple programming languages (C++, Java, Go)
- [ ] Problem difficulty classification
- [ ] Solution explanation generation
- [ ] Performance benchmarking dashboard
- [ ] Integration with real Codeforces API
- [ ] Distributed evaluation system

---

**Made with ❤️ for competitive programming enthusiasts**
