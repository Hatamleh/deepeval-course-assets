# DeepEval course assets

Sample data files for the QAcart course **DeepEval — the complete LLM testing framework**.

## questions.csv

45 support questions for the imaginary **ShopBot** store, with the ideal answer
for each one. Used in the course to build an `EvaluationDataset` of goldens
without writing test cases by hand.

| column | what it is |
|---|---|
| `input` | the question a shopper asks |
| `expected_output` | the ideal answer |
| `category` | returns, refunds, shipping, orders, products, payment, account — ignored by DeepEval, handy for slicing results |

### Using it

```python
from deepeval.dataset import EvaluationDataset

dataset = EvaluationDataset()
dataset.add_goldens_from_csv_file(
    file_path="questions.csv",
    input_col_name="input",
    expected_output_col_name="expected_output",
)

print(len(dataset.goldens))   # 45
```

`add_goldens_from_csv_file` reads through pandas, so install it too:

```bash
uv add deepeval pandas
```

## questions.json

The same 45 questions, plus a `context` list per question — two policy chunks
that a complete answer should be grounded in. JSON because `context` is a
**list**, and a CSV cell can only hold text.

```python
dataset.add_goldens_from_json_file(
    file_path="questions.json",
    input_key_name="input",
    expected_output_key_name="expected_output",
    context_key_name="context",
)

print(len(dataset.goldens[0].context))   # 2
```

Use the CSV for plain question-and-answer goldens; use the JSON as soon as a
golden needs `context`, `retrieval_context` or tool calls.

## explainer-data.json

13 goldens for **المُفسِّر** (*the Explainer*) — a real Arabic AI tutor that
explains any topic for beginners in Jordanian dialect. This is the dataset the
course uses once it stops testing an imaginary app and starts testing a running
one.

| key | what it is |
|---|---|
| `name` | short id — what a failure is called in the report |
| `input` | the question, in Jordanian Arabic |
| `expected_output` | the facts a correct answer must contain — **present on only 7 of the 13** |
| `additional_metadata.category` | technical, non_technical, ambiguous, adversarial, english_input, harmful |

The six ambiguous, adversarial, English-input and harmful goldens deliberately
carry **no** `expected_output`. There is no single right text for *"بايثون"* —
the only correct behaviour is to ask which one is meant — so those are judged on
behaviour, not on matching an answer.

```python
from deepeval.dataset import EvaluationDataset

dataset = EvaluationDataset()
dataset.add_goldens_from_json_file(
    file_path="explainer-data.json",
    input_key_name="input",
    expected_output_key_name="expected_output",
)

print(len(dataset.goldens))   # 13
```

---
Part of [QAcart](https://qacart.com) course material.
