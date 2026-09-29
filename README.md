# One-Tool Calculator Agent

A beginner-friendly Python CLI demonstrating the agent loop:

**User -> LLM decides -> calculate tool -> observation -> LLM reply**

Exactly one function (`calculate`) is exposed using Ollama's native function calling.
The model chooses whether to call it. Greetings need no tool; arithmetic uses it.
Raw tool calls print before execution. Conversation history supports follow-up questions.

## Run

Requires Python 3.10+ and [Ollama](https://ollama.com/download).
No pip dependencies or API key. Downloading Ollama and the model needs internet;
afterward inference runs locally. Speed and memory needs depend on your computer.

1. Install and open Ollama, then open a new terminal.
2. Download the tool-capable model:

   ```powershell
   ollama pull llama3.2
   ```

3. In this project folder:

   ```powershell
   python agent.py
   ```

4. Try:

   ```text
   Hi!
   What is 18 percent of 1250?
   Add 100 to that result.
   Oru item 250 rupees. 3 items-ku 10 percent discount na total evlo?
   What is 10 divided by zero?
   /reset
   /exit
   ```

Optional: `python agent.py --model YOUR_TOOLS_CAPABLE_MODEL`.
If Ollama is unreachable, open its app or run `ollama serve` in another terminal.
If already running, do not start a second server. If a model is missing, run its pull command.
Small models can misinterpret requests or skip tools; inspect the printed call and retry
with a clearer question. Tool calling is a model decision, not a keyword rule.

## Verification

```powershell
python -m unittest discover -s tests -v
```

Tests cover arithmetic, unsafe inputs, tool-free responses, result feedback,
invalid tool requests, raw-call logging order, and the iteration limit.
They use mocked model replies and do not prove live model performance.
For live verification, run the prompts above with Ollama and inspect tool logs.

## Files

- `agent.py`: safe calculator, one tool schema, Ollama HTTP client, agent loop, CLI.
- `tests/test_agent.py`: deterministic tests without model downloads.
- `LEARN_TANGLISH.md`: step-by-step explanation and practice.
- `SUBMISSION.md`: GitHub steps, screen-recording script, LinkedIn draft.

## Design notes

Arithmetic uses an AST allowlist, never Python `eval`. Expression length,
tree size, powers, and result magnitude are bounded. Normal Python floating-point
rounding applies; this is an educational calculator, not an exact decimal ledger.
Malformed tool arguments become observations the model can explain or correct.
Five model rounds per turn prevent endless tool loops. `/reset` clears chat history.
The app prints execution events, not the model's private chain of thought.

API reference: [Ollama tool calling](https://docs.ollama.com/capabilities/tool-calling).
Model: [Llama 3.2](https://ollama.com/library/llama3.2).
