---
date_created: "{{date}}"
tags: [ type/index ]
---


---
### Not indexed:
```dataviewjs 
var outlinks = dv.current().file.outlinks;
var idx_tags = dv.current().file.etags.filter(tag => tag.contains('idx/'));
var idx_files = dv.pages().file.filter(f => idx_tags.some(tag => f.etags.includes(tag)));
// exclude index notes and people notes
var excl_tags = ['#type/index', '#type/keyword', '#type/citation', '#cat/people'];
var idx_files = idx_files.filter(f => !excl_tags.some(tag => f.etags.includes(tag)));
var missing_links = idx_files.link.filter(l1 => !outlinks.some(l2 => l2.path === l1.path))
if (missing_links.length === 0) {
	dv.paragraph('- `All good!`')
} else {
	dv.list(missing_links)
}
```

### Sources
```dataviewjs
var idx_tags = dv.current().file.etags.filter(tag => tag.contains('idx/'));
var sources = dv.pages('#type/citation').filter(o => idx_tags.some(tag => o.file.etags.contains(tag)));
for (let i=0; i< sources.length; i++) {
	dv.paragraph("- " + sources.file.link[i] + " : *" + sources.title[i] + "*");
}
```

### People
```dataviewjs
var idx_tags = dv.current().file.etags.filter(tag => tag.contains('idx/'));
var people_links = dv.pages('#cat/people').file.filter(f => idx_tags.some(tag => f.etags.contains(tag))).link
dv.list(people_links);
```