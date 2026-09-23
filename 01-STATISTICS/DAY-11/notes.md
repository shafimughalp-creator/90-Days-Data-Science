# Day 11 — Chi-Square Test (Categorical Variables)

## What is Chi-Square?
- Tests whether two **categorical** variables are **independent or associated**
- Used when BOTH columns are categories (e.g. Position, Injured — not numbers)
- Cannot use t-test here because there is no mean of "GK" or "Yes"

---

## Core Idea
> If two variables had NOTHING to do with each other — what would the counts look like?
> Chi-square measures how far our actual (observed) counts are from that expectation.

- Big gap between Observed and Expected → big chi2 stat → small p-value → association likely
- Small gap → small chi2 stat → large p-value → probably just random noise

## Key Formula

```
χ² = Σ [ (Observed - Expected)² / Expected ]
```

- Square the difference so negatives don't cancel out
- Divide by Expected so small-count gaps are weighted properly
- Sum across every cell in the table

## Expected Count Formula

```
Expected = (Row Total × Column Total) / Grand Total
```

## Degrees of Freedom

```
dof = (rows - 1) × (columns - 1)
```

## chi2_contingency() — Always Returns 4 Things

```python
chi2, p, dof, expected = stats.chi2_contingency(ct)
```

## Reading the Result

```
p < 0.05  →  Reject H0  →  Strong evidence of association
p > 0.05  →  Fail to reject H0  →  No evidence of association
```

### Wording Rules
- "strong evidence of association" — not "proves they are related"
- "associated with injury rate" — not "causes injuries"
- A p-value is not the probability that the relationship exists

## What Chi-Square Does NOT Tell You

| Question | Tool |
|---|---|
| Are they associated? | Chi-Square |
| How STRONGLY associated? | Cramér's V |
| WHY are they associated? | Experiments / domain knowledge |

## Common Mistakes

| Mistake | Fix |
|---|---|
| `chi2, p = stats.chi2_contingency(ct)` | Always unpack all 4 values |
| Passing raw columns instead of crosstab | Always do `pd.crosstab()` first |
| Using chi-square on continuous data | Bin continuous data into categories first |
| Expected count < 5 in any cell | Use Fisher's Exact Test instead |

## Interview Questions

**Q: When do you use chi-square vs t-test?**  
t-test = continuous outcome compared across groups. Chi-square = both variables are categorical.

**Q: p = 0.03. What do you conclude?**  
At α = 0.05, p = 0.03 means we reject H0. There is strong evidence the two variables are associated.
