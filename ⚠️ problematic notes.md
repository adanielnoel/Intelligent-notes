---
date_created: "20 Aug 2021"
---
This note automatically tracks other notes which may contain errors or are inconsistent with the structure imposed by the tag system (see [[💡 README]]).
Ideally, the lists under the following headers should be empty.

## All notes
### Notes marked with `#flags/incomplete`
```dataviewjs
var notes = dv.pages('#flags/incomplete').file;
if (notes.length === 0) {
dv.paragraph('`All good!`')
} else {
dv.list(notes.link)
}
```

### Notes without type
```dataviewjs
var notes = dv.pages().file;
notes = notes.filter(f => f.folder != 'External files');
notes = notes.filter(f => f.name != '💡 README' && f.name != '⚠️ problematic notes');
notes = notes.filter(f => !f.tags.some(t => t.contains('#type/')));
if (notes.length === 0) {
dv.paragraph('- `All good!`')
} else {
dv.list(notes.link)
}
```

### Notes with proxy links
Notes that contain links to notes that are not created yet
==TO DO: as of yet, dataview does not index broken links==

---
## Literature and permanent notes
### Notes without an index
```dataviewjs
var notes = dv.pages("#type/literature or #type/permanent").file;
notes = notes.filter((f) => f.folder != 'External files');
notes = notes.filter((f) => !f.tags.some((t) => t.contains('#idx/')));
if (notes.length === 0) {
dv.paragraph('- `All good!`')
} else {
dv.list(notes.link)
}
```

### Literature notes without citations
(Notes with the tag `#cat/people` or `#flags/no_source` are excluded)
```dataviewjs
var notes = dv.pages("#type/literature").file;
notes = notes.filter((f) => f.folder != 'External files');
notes = notes.filter((f) => !f.outlinks.some((p) => p.path.contains('Citation notes/@')));
notes = notes.filter((f) => !f.tags.some((t) => t.includes('#cat/people') || t.includes('#flags/no_source')));
if (notes.length === 0) {
dv.paragraph('- `All good!`')
} else {
dv.list(notes.link)
}
```

### Unused notes
Literature and permanent notes which only appear on index and keyword notes (excl. notes with the tag `#cat/people`).
```dataviewjs
var notes = dv.pages("#type/literature or #type/permanent").file;
notes = notes.filter((f) => f.folder != 'External files');
var notes = notes.filter(f => !f.inlinks.some(l => !l.path.contains("4 Index notes/") && !l.path.contains("5 Keyword notes/")));
notes = notes.filter((f) => !f.tags.some((t) => t.includes('#cat/people')));
dv.list(notes.link);
//dv.paragraph('`' + notes[0].inlinks[0] + '`')
```