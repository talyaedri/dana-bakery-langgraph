# Dana’s Bakery — LangGraph Homework

Deterministic bakery order workflow implemented as a Google Colab notebook using LangGraph. No LLM calls or API keys.

## Files
- `prompt.md`: implementation specification and revision notes.
- `dana_bakery.ipynb`: graph implementation, embedded diagram, and 18 test orders with assertions.

## Colab
https://colab.research.google.com/drive/1c9bhlTSFr4hS1w8gt6TaUiW8AVdjt-Rw

Open the notebook in Colab and select Runtime → Run all. The last test cell should report: `All 18 graph cases and pricing-boundary checks passed.`

Each invocation independently uses the initial inventory. The notebook documents validation and rejected-output conventions. The exactly-150 discount boundary is checked directly at the pricing node because it cannot be reached using the initial stock.
