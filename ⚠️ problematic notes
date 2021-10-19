---
date_created: "20 Aug 2021"
---
This note automatically tracks other notes which may contain errors or are inconsistent with the structure imposed by the tag system (see [[💡 README]]).
Ideally, the lists under the following headers should be empty.

## Notes marked with `#flags/incomplete`
```dataviewjs
var notes = dv.pages().file.filter((f) => f.folder != 'External files').filter((f) => f.name != 'README').filter((f) => f.name != dv.current().file.name);
var todo_notes = notes.filter((f) => f.tags.some((t) => t.contains('#flags/incomplete')));
if (todo_notes.length === 0) {
dv.paragraph('`All good!`')
} else {
dv.list(todo_notes.link)
}
```

## Notes without type
```dataviewjs
var notes = dv.pages().file.filter((f) => !['External files', 'Day Planners'].includes(f.folder)).filter((f) => f.name != 'README').filter((f) => f.name != dv.current().file.name);
var typeless_notes = notes.filter((f) => !f.tags.some((t) => t.contains('#type')));
typeless_notes = typeless_notes.filter((f) => f.folder != 'External files');
if (typeless_notes.length === 0) {
dv.paragraph('- `All good!`')
} else {
dv.list(typeless_notes.link)
}
```


## Notes without category (excl.fleeting)
```dataviewjs
var notes = dv.pages().file.filter((f) => !['External files', 'Day Planners'].includes(f.folder)).filter((f) => f.name != 'README').filter((f) => f.name != dv.current().file.name);
var catless_notes = notes.filter((f) => !f.tags.some((t) => !t.contains('#type')));
var catless_notes_nofleeting = catless_notes.filter((f) => !f.tags.some((t) => t.contains('#type/fleeting')))
if (catless_notes_nofleeting.length === 0) {
dv.paragraph('- `All good!`')
} else {
dv.list(catless_notes_nofleeting.link)
}
```

### Fleeting notes without cateory
For this notes, it may be allowable to leave them as-is due to their temporary nature.
```dataviewjs
var notes = dv.pages().file.filter((f) => f.folder != 'External files').filter((f) => f.name != 'README');
var catless_notes = notes.filter((f) => !f.tags.some((t) => !t.contains('#type')));
var catless_notes_fleeting = catless_notes.filter((f) => f.tags.some((t) => t.contains('#type/fleeting')))
if (catless_notes_fleeting.length === 0) {
dv.paragraph('- `All good!`')
} else {
dv.list(catless_notes_fleeting.link)
}
```

## Literature notes without citations
(Notes with the tag `#cat/people` or `#flags/no_source` are excluded)
```dataviewjs
var notes = dv.pages().file.filter((f) => f.folder != 'External files').filter((f) => f.name != 'README');
var lit_notes = notes.filter((f) => f.tags.some((t) => t.contains('#type/literature')));
var without_citations = lit_notes.filter((f) => !f.outlinks.some((p) => p.path.contains('Citation notes/@')));
var without_citations_problematic = without_citations.filter((f) => !f.tags.some((t) => t.includes('#cat/people') || t.includes('#flags/no_source')));
if (without_citations_problematic.length === 0) {
dv.paragraph('- `All good!`')
} else {
dv.list(without_citations_problematic.link)
}
```

