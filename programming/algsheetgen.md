---
title: AlgSheetGen
---
A small JS program for generating alg sheets in Markdown.

<script src="/assets/js/jsoneditor.js"></script>
<script src="/assets/js/liquid.browser.min.js"></script>

<div id="editor">
</div>

<br>

<button onclick="document.getElementById('outputJSON').value = JSON.stringify(getJSON());">Generate</button>
<input type="text" id="outputJSON" placeholder="JSON" onchange="editor.setValue(JSON.parse(value));">
<button onclick="copyToClipboard('outputJSON');">Copy to clipboard</button>

<br><br>

<select id="converter" onchange="convert(value);">
  <option value="md.liquid">Markdown</option>
  <option value="md_br.liquid">Markdown (&lt;br&gt; support)</option>
</select>
<button onclick="copyToClipboard('outputMarkup');">Copy to clipboard</button>
<br>
<textarea id="outputMarkup" placeholder="Markup Output" cols=50 rows=25 readonly></textarea>

<script>
const editor = new JSONEditor(document.getElementById("editor"), {
schema: {title:"Algorithm Sheet",type:"object",required:["title"],definitions:{algset:{type:"object",title:"Subset",properties:{title:{type:"string",title:"Name", default:"Name"},description:{type:"string",title:"Description"},imageLink:{type:"string",title:"Image link"},algs:{type:"array",format:"tabs-top",title:"Algs",items:{anyOf:[{type:"string",title:"Single algorithm"},{type:"array",title:"Multiple algorithms",items:{type:"string",title:"Single algorithm"}},{"$ref":"#/definitions/algset"}]}}},required:["title","algs"]}},properties:{title:{type:"string",title:"Title",default:"Title"},description:{type:"string",title:"Description"},imageLink:{type:"string",title:"Image link",default:"http://www.cubing.net/api/visualcube/?view=plan&fmt=svg&case=$ALG"},algs:{type:"array",format:"tabs-top",title:"Algorithms",items:{"$ref":"#/definitions/algset"}},end:{type:"string",title:"End"},footer:{type:"string",title:"Footer"},author:{type:"string",title:"Author"},bold:{type:"boolean",title:"Bolden the first algorithm"}}},
disable_edit_json: true,
disable_properties: true,
array_controls_top: true
});

function getJSON() {
  return editor.getValue();
}

function copyToClipboard(value) {
  var copyText = document.getElementById(value);
  copyText.select();
  copyText.focus();
  document.execCommand("copy");
}

function convert(file) {
  (new liquidjs.Liquid({
    cache: true,
	root: ["templates/"]
  })).renderFile(file, getJSON()).then(output => document.getElementById('outputMarkup').innerHTML = output);
}
</script>

---

Made using [JSON Editor](https://github.com/json-editor/json-editor){:target="_blank"} and [liquidjs](https://liquidjs.com/){:target="_blank"}.