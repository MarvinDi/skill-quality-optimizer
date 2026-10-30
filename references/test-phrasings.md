# Multi-Phrasing Test Case Template

This document provides 3 different phrasing test cases to verify skill trigger stability, corresponding to "Pitfall 5: published without testing".

## Table of Contents

- [Test Method](#test-method)
- [Test Case Template](#test-case-template)
- [Judgment Criteria](#judgment-criteria)
- [Example: Financial Report Extraction Skill Test Cases](#example-financial-report-extraction-skill-test-cases)

---

## Test Method

For the same core requirement, write 3 different user phrasings, input each to AI, and observe whether AI triggers the same skill and takes the same code path.

1. Phrasing A: most direct keyword expression
2. Phrasing B: colloquial/indirect expression
3. Phrasing C: scenario-based/context expression

If any phrasing fails to trigger the target skill, description trigger conditions are incomplete, or body rules have conflicts.

---

## Test Case Template

```
## Test Case: <skill-name>

### Core Requirement
<One sentence describing what the user wants>

### Phrasing A (direct keywords)
<User input example 1>

### Phrasing B (colloquial/indirect)
<User input example 2>

### Phrasing C (scenario-based/context)
<User input example 3>

### Expected Result
AI should all invoke <skill-name>, output consistent result format.

### Actual Result
- Phrasing A: <triggered/not triggered> <skill-name>
- Phrasing B: <triggered/not triggered> <skill-name>
- Phrasing C: <triggered/not triggered> <skill-name>

### Diagnosis
If any not triggered, check:
- Does description contain keywords from that phrasing?
- Do body rules conflict with that phrasing?
- Does description overlap with other skills?
```

---

## Judgment Criteria

| Result | Meaning | Action |
|--------|---------|--------|
| All 3 phrasings trigger | Trigger is stable | No change needed |
| 2 trigger, 1 doesn't | Trigger conditions incomplete | Add missing trigger keywords to description |
| 1 or 0 trigger | Description severely lacking | Rewrite description, add trigger scenarios |
| All 3 trigger but results differ | Body rules have conflicts | Check Pitfall 4 (duplicated rules) |

---

## Example: Financial Report Extraction Skill Test Cases

### Core Requirement
Extract revenue, net profit, and growth rate from PDF quarterly financial reports, output JSON.

### Phrasing A (direct keywords)
"Extract revenue, net profit, and growth rate from this PDF financial report, output JSON."

### Phrasing B (colloquial/indirect)
"Help me pull the financial data from the quarterly report — revenue and net profit, plus growth rate."

### Phrasing C (scenario-based/context)
"I have a last quarter financial report PDF, need to do quarterly financial analysis, please extract key metrics."

### Expected Result
AI invokes the financial report extraction skill for all three, outputs JSON with same field names.

### Actual Result
- Phrasing A: triggered "financial report extraction" skill ✅
- Phrasing B: triggered "financial report extraction" skill ✅
- Phrasing C: not triggered, mis-triggered "general document summary" skill ❌

### Diagnosis
Description missing scenario words like "quarterly", "quarterly report", "key metrics". Need to supplement:

```
description: "Extract revenue, net profit, and growth rate from PDF quarterly financial reports, output JSON. Use when user mentions 'financial report', 'quarterly report', 'quarterly', 'extract financial data', 'key metrics'."
```

---

## Quick Self-Check Checklist

1. [ ] Write 3 different phrasing test cases for each core capability.
2. [ ] Input each to AI separately, observe if triggered skill is consistent.
3. [ ] If inconsistent, check if description trigger conditions cover all phrasings.
4. [ ] If results differ, check if body rules have conflicts (Pitfall 4).