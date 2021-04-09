A small application for finding out how long your csTimer scrambles are.  
If you got a scramble with 13 moves or less, you're cool.

<textarea id="input" cols="50" rows="15" placeholder="csTimer CSV"></textarea>
<br>
<textarea id="output" cols="50" rows="15" placeholder="Output" readonly></textarea>
<br>
<button onclick="process();">Process</button>

<script>
const moveRemove = /['2 ]+/g;
function process() {
	const scrambles = document.getElementById("input").value
	.split("\n")
	.slice(1)
	.map(it => it.split(";")[3].replaceAll(moveRemove, "").trim().length)
	.sort((a, b) => a - b);
	console.log(scrambles);
	let moves = 0;
	const amounts = [];
	for(let scramble of scrambles) {
		moves += scramble;
		if(amounts[scramble]) {
			amounts[scramble]++;
		} else {
			amounts[scramble] = 1;
		}
	}
	const min = scrambles[0];
	const max = scrambles[scrambles.length - 1];
	
	const output = [
	"Solve amount: ", scrambles.length,
	"\nLowest movecount: ", min,
	"\nHighest movecount: ", max,
	"\nAverage movecount: ", (moves / scrambles.length).toFixed(2),
	"\nDistribution:"
	];
	let j = min;
	while(j <= max) {
		output.push("\n", j, " movers: ", amounts[j++]);
	}
	document.getElementById("output").value = output.join("");
}
</script>