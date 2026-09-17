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

---
Part of [QAcart](https://qacart.com) course material.
