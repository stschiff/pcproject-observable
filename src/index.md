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
        <div>${basePositionsPlot}</div>
    </div>
    <div class="card">
        <h1>Base Population Selection</h1>
        <p>based on modern data. Select groups from the table, sort by clicking on the headers.</p>
        <div>${baseGroupTable}</div>
        <p>Selected ${d3.sum(baseGroupsSelected.map(d => d['nr of Samples']))} modern samples in ${baseGroupsSelected.length} groups</p>
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
let filter_cond = '';
if(baseGroupsSelected.length == 1)
    filter_cond = `WHERE P.Group = '${baseGroupsSelected[0].Group}'`;
if(baseGroupsSelected.length > 1)
    filter_cond = `WHERE P.GROUP IN (${baseGroupsSelected.map(d => `'${d.Group}'`)})`;
const q = `SELECT P.* FROM pcPositions AS P JOIN baseGroups BG ON P.Group = BG.Group ${filter_cond};`;
const basePositionsSelected = sql([q]);
```

```js
const basePositionsPlot = Plot.plot({
    marks: [
        Plot.dot(basePositionsAll, {
            x: "PC1",
            y: "PC2",
            fill: "gray",
            r: 2,
            fillOpacity: 0.2
        }),
        Plot.dot(basePositionsSelected, {
            x: "PC1",
            y: "PC2",
            fill: "Group",
            r: 3,
            fillOpacity: 0.7}),
    ]
})
```