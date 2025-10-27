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
        <p>based on ${d3.sum(baseGroupsSelected.map(d => d['nr of Samples']))} modern samples in ${baseGroupsSelected.length} groups</p>
        ${basePositionsPlot}
    </div>
    <div class="card">
        <h1>Base Population Selection</h1>
        <p>based on modern data. Select groups from the table, sort by clicking on the headers.</p>${baseGroupTable}
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


```sql id="baseGroups"
SELECT P.Group, COUNT(*) AS 'nr of Samples' FROM pcPositions AS P JOIN baseGroups BG ON P.Group = BG.Group
  GROUP BY P.Group
  ORDER BY P.Group;
```

```sql id="basePositionsAll"
SELECT P.* FROM pcPositions AS P JOIN baseGroups BG ON P.Group = BG.Group;
```

```js
const baseGroupTable = Inputs.table(baseGroups);
const baseGroupsSelected = Generators.input(baseGroupTable);
```

```js
const q = `SELECT P.* FROM pcPositions AS P JOIN baseGroups BG ON P.Group = BG.Group WHERE P.Group IN (${baseGroupsSelected.map(d => d.Group)})`;
const basePositionsSelected = sql([q]);
```

```js
const basePositionsPlot = Plot.plot({
    marks: [
        Plot.dot(basePositionsSelected, {
            x: "PC1",
            y: "PC2",
            fill: "Group",
            r: 3}),
    ]
})
```