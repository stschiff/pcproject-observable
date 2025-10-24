---
title: "PCproject"
toc: false
sql:
    pcPositions: data/Cambridge1_clean.tsv
    baseGroups: data/Europe1_clean.tsv
---

# PCproject

<div class="grid grid-cols-2">
    <div class="card">
        <h1>Projection Basis</h1>
        <p>based on modern data</p>
    </div>
    <div class="card">
        <h1>Base Population Selection</h1>
        <p>based on modern data</p>
    </div>
</div>
<div class="grid grid-cols-2">
    <div class="card">
        <h1>Projected individuals</h1>
    </div>
    <div class="card">
        <h1>Projected Population Selection</h1>
    </div>
</div>

```sql id=[{totalCount}]
SELECT COUNT(*) AS totalCount FROM pcPositions;
```
In total, have ${totalCount} individuals in the dataset.

```sql id="basePositions"
SELECT P.* FROM pcPositions AS P JOIN baseGroups BG ON P.Group = BG.Group;
```

In total, have ${[...basePositions].length} individuals in the base positions dataset.

```js
Inputs.table(basePositions, {height: 300, width: 400})
```