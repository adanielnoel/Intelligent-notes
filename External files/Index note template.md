---
date_created: "{{date}}"
tags: [ type/index ]
---

```dataviewjs
const tag_to_header = (t) => {
	var str = t.replace('#idx/', '').replaceAll('_', ' ');
	return str.charAt(0).toUpperCase() + str.slice(1);
}

const tag_level = (tag) => {
	return (tag.match(/\//g) || []).length;
}

const augment_tag_tree = (tags, base) => {
    var new_tags = new Set()
	for(let tag of tags) {
	    var path = base;
	    for (let level of tag.replace(base, '').split('/')) {
	        path += level;
	        new_tags.add(path);
	        path += '/';
	    }
	}
	// sorting guarantees they are in increasing level of depth and alphabetical
	return [... new_tags].sort() 
}


///////////////////////////////////////////////////////
// Make an index of all notes in each base cathegory //
///////////////////////////////////////////////////////
let excl_tags = ['#type/index', '#type/keyword', '#type/citation', '#cat/people'];
var base_idx_tags = dv.current().file.etags.where(tag => tag.contains('idx/'));
for (let i=0; i<base_idx_tags.length; i++) {
	if (i > 0) {
		dv.paragraph("---");
	}
	dv.header(1, tag_to_header(base_idx_tags[i]));
	var subtags = dv.pages(base_idx_tags[i]).file.etags.filter(tag => tag.contains(base_idx_tags[i]));
	subtags = augment_tag_tree(subtags, base_idx_tags[i]); // add intermediate tags if missing
	var result = "";
	for (let subtag of subtags) {
		let components = subtag.split('/');
		var level = components.length - 2;
		if (level == 1) {
			dv.paragraph(result); // this trick adds a space between sections
			result = "";
		}
		
		if (level >= 1) {
			result += '\t'.repeat(level - 1) + "- **" + tag_to_header(components.pop()) + "**" + '\n';
		}
		var filtered_files = dv.pages(subtag).file.filter(f => f.etags.includes(subtag));
		filtered_files = filtered_files.filter(f => !excl_tags.some(tag => f.etags.includes(tag)));
		for (let file of filtered_files) {
			result += '\t'.repeat(level) + "- " + file.link + '\n';
		}
	}
	dv.paragraph(result);
	
	///////////////////////////////////
	// List sources if there are any //
	///////////////////////////////////
	var sources = dv.pages("#type/citation" + " and " + base_idx_tags[i]);
	if (sources.length > 0) {
		dv.header(2, "Sources")
		for (let source of sources) {
			dv.paragraph("- " + source.file.link + " : *" + source.title + "*");
		}
	}

	//////////////////////////////////
	// List people if there are any //
	//////////////////////////////////
	var people_links = dv.pages("#cat/people" + " and " + base_idx_tags[i]).file.link
	if (people_links.length > 0) {
		dv.header(2, "People")
		dv.list(people_links);
	}
}
```