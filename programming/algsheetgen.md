---
title: AlgSheetGen
---
A small JS program for generating alg sheets in Markdown.

## Input
<textarea id="input" oninput="generate(value);" cols=80 rows=30></textarea>

## Output
<pre id="output"></pre>

<script>
function genAlgTable(caseLink, alg, name, depth, algSeparator, lines) {
  if(depth <= 6) {
    console.log(alg);
    if(name && ((typeof alg) == "object")) {
	  lines.push("");
	  lines.push("#".repeat(depth) + " " + name);
	  var i = 0;
	  if(Array.isArray(alg)) {
	    const len = alg.length;
	    if(len > 0) {
		  lines.push("|Case|Algorithm|");
	      lines.push("|---|---|")
	      for(; i < len; i++) {
		    genAlgTable(caseLink, alg[i], undefined, depth + 1, algSeparator, lines);
		  }
		}
	  } else {
	    const keys = Object.keys(alg);
	    const len = keys.length;
	    for(; i < len; i++){
	      const key = keys[i];
          genAlgTable(caseLink, alg[key], key, depth + 1, algSeparator, lines);
        }
	  }
	} else if (Array.isArray(alg)){
	  lines.push(["|![](", caseLink.replace("$ALG", encodeURIComponent(alg[0])), ")|", alg.join(algSeparator), "|"].join(""));
	} else {
	   lines.push(["|![](", caseLink.replace("$ALG", encodeURIComponent(alg)), ")|", alg, "|"].join(""));
	}
  }
}

function generate(value) {
  console.log("Input: " + value);
  const json = JSON.parse(value);
  var lines = [];
  
  if(json.title) {
    lines.push("# " + json.title);
  }
  
  if(json.description) {
    lines.push(json.description);
  }
  
  if(json.algs) {
    const caseLink = json.caseLink ? json.caseLink : "http://www.cubing.net/api/visualcube/?view=plan&fmt=svg&case=$ALG";
	const algSeparator = json.algSeparator ? json.algSeparator : "&#60;br&#62;";
	const keys = Object.keys(json.algs);
	var i = 0;
	const len = keys.length;
	for(; i < len; i++){
	  const key = keys[i];
      genAlgTable(caseLink, json.algs[key], key, 2, algSeparator, lines);
    }
  }
  
  if(json.end) {
    lines.push(json.end);
  }
  
  if(json.footer) {
    lines.push("---");
	if(json.author) {
	  lines.push("By " + json.author);
	}
    lines.push(json.footer);
  } else if(json.author) {
    lines.push("---<br>By " + json.author);
  }
  document.getElementById("output").innerHTML = lines.join("<br>");
}
</script>