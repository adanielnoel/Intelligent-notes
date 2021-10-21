---
date_created: "{{date}}"
tags: [ type/keyword ]
---


---
### Not indexed:
```dataviewjs 
var outlinks = dv.current().file.outlinks;
var cat_tags = dv.current().file.etags.filter(tag => tag.contains('cat/'));
var cat_files = dv.pages().file.filter(f => cat_tags.some(tag => f.etags.includes(tag)));
// exclude index notes and people notes
var excl_tags = ['#type/index', '#type/keyword'];
var cat_files = cat_files.filter(f => !excl_tags.some(tag => f.etags.includes(tag)));
var missing_links = cat_files.link.filter(l1 => !outlinks.some(l2 => l2.path === l1.path))
if (missing_links.length === 0) {
	dv.paragraph('- `All good!`')
} else {
	dv.list(missing_links)
}
```