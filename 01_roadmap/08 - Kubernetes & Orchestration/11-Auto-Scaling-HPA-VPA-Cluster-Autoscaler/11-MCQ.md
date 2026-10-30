# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
What is the mathematical formula used by HPA to calculate desired replica count?
- [ ] A) $	ext{CurrentReplicas} 	imes (	ext{Target} / 	ext{Current})$
- [x] B) $\lceil 	ext{CurrentReplicas} 	imes (	ext{CurrentMetric} / 	ext{TargetMetric}) ceil$
- [ ] C) $	ext{CurrentReplicas} + 	ext{TargetMetric}$
- [ ] D) $(	ext{MaxReplicas} - 	ext{MinReplicas}) / 2$

<details>
<summary>Explanation</summary>
The HPA calculates desired replicas by multiplying current replicas by the ratio of current metric value over target metric value, rounding up to the nearest integer.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
