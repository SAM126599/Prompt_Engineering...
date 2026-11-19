# AI-2: Prompt Engineering Report

**Name:** YOUR NAME
**Model used:** e.g. Gemini 2.0 Flash / GPT-4o-mini
**Tool:** Google AI Studio / OpenAI Playground
**Settings:** temperature = 0.2, reasoning/thinking mode off

---

## 1. Zero-shot vs. Few-shot (Sentiment Classification)

### Zero-shot prompt
```
Classify the sentiment of this review as Positive, Negative, or Mixed. Reply with one word.
Review: "{review}"
```

### Few-shot prompt
```
Classify the sentiment of this review as Positive, Negative, or Mixed. Reply with one word.

Review: "Fantastic quality, would buy again."
Sentiment: Positive

Review: "Oh great, it broke on day one. Just what I wanted."
Sentiment: Negative

Review: "Camera is superb but the battery barely lasts a day."
Sentiment: Mixed

Review: "Terrible customer service and the product is flimsy."
Sentiment: Negative

Review: "{review}"
Sentiment:
```

### Raw outputs

| # | Input | Zero-shot output (raw) | Few-shot output (raw) |
|---|-------|------------------------|-----------------------|
| 1 | "Absolutely love it, works perfectly." | PASTE | PASTE |
| 2 | "Great, another charger that died in a week." | PASTE | PASTE |
| 3 | "Battery is amazing but the screen scratches way too easily." | PASTE | PASTE |
| 4 | "It's fine, I guess." | PASTE | PASTE |
| 5 | "Not bad at all!" | PASTE | PASTE |
| 6 | "Arrived late, but the quality made up for it." | PASTE | PASTE |
| 7 | "Wow, so helpful. I only waited 3 hours for support." | PASTE | PASTE |
| 8 | "Does what it says." | PASTE | PASTE |

### Analysis
WRITE 2-3 SENTENCES: Which inputs changed between zero-shot and few-shot? Did formatting or consistency improve? Where did few-shot NOT help?

---

## 2. Chain-of-Thought

### Problem A (math)
> A shop sells notebooks at $4 each, with a 25% discount if you buy 5 or more. Maria buys 7 notebooks and pays with a $50 bill. Tax is 10% on the discounted total. How much change does she get?

**Direct prompt:** `{problem} Answer with just the final number.`

**Raw output:**
```
PASTE
```

**CoT prompt:** `{problem} Think step by step, then give the final answer.`

**Raw output:**
```
PASTE
```

### Problem B (logic)
> Anna is taller than Ben. Carl is shorter than Ben. Dev is taller than Anna but shorter than Eva. Who is the third tallest?

**Direct prompt:** `{problem} Answer with just the name.`

**Raw output:**
```
PASTE
```

**CoT prompt:** `{problem} Think step by step, then give the final answer.`

**Raw output:**
```
PASTE
```

### Results

| Problem | Direct correct? | CoT correct? |
|---------|-----------------|--------------|
| A (math) | Yes / No | Yes / No |
| B (logic) | Yes / No | Yes / No |

### Analysis
WRITE 2-3 SENTENCES: Did "think step by step" improve accuracy? If both were correct, say so honestly and note whether the reasoning was at least more transparent.

---

## 3. Structured Output (JSON extraction)

### Baseline prompt (no schema)
```
Extract the name, date, and amount from this text: "{paragraph}"
```

### Schema prompt
```
Extract the details from the text below. Return ONLY valid JSON matching this schema, no extra text:
{"name": string, "date": "YYYY-MM-DD", "amount": number, "currency": string}
If a field is missing, use null.

Text: "{paragraph}"
```

### Schema + worked example (and/or JSON mode on)
```
Extract the details from the text below. Return ONLY valid JSON matching this schema, no extra text:
{"name": string, "date": "YYYY-MM-DD", "amount": number, "currency": string}
If a field is missing, use null.

Example:
Text: "Anil paid 500 rupees on 2 Feb 2026."
Output: {"name": "Anil", "date": "2026-02-02", "amount": 500, "currency": "INR"}

Text: "{paragraph}"
Output:
```

### Test inputs and raw outputs

| # | Input | Baseline (raw) | Schema (raw) | Schema + example / JSON mode (raw) |
|---|-------|----------------|--------------|-------------------------------------|
| 1 | "Hey, it's Priya. Just paid you the rent, 12,500 rupees, on the 3rd of March 2026." | PASTE | PASTE | PASTE |
| 2 | "Invoice from Mr. John D. Smith dated 11/04/26 totaling $1,299.50 (USD)." | PASTE | PASTE | PASTE |
| 3 | "Ravi sent ₹800 yesterday." | PASTE | PASTE | PASTE |
| 4 | "Payment of 45 euros from Chloé Martin, 2026-01-15, ref #4432." | PASTE | PASTE | PASTE |
| 5 | "Two payments: Sam paid $20 on May 1 and Lee paid $35 on May 2." | PASTE | PASTE | PASTE |

### Success rate

| Version | Valid JSON | Correct fields |
|---------|-----------|----------------|
| Baseline | _/5 | _/5 |
| Schema | _/5 | _/5 |
| Schema + example / JSON mode | _/5 | _/5 |

### Analysis
WRITE 2-3 SENTENCES: Which technique improved reliability most? Which inputs failed (e.g. the ambiguous date in #2, missing date in #3, two payments in #5) and why?

---

## Conclusion
WRITE 2-3 SENTENCES: Which technique helped most, and where was it unnecessary or ineffective?
