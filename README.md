JavaScript-StringMatcher
========================

Javascript typing-test style string matching tool

This script expects something to this effect will be run somewhere else:

```JavaScript
function initTest() {
	IsLog.timerOn();  //  This turns on the timer for the IsLog.
  //  This variable ( window["typingTest"] ) is loaded with the test the user is meant to be typing
  //  FYI - the text in there is the recent State of the Union address...
	window["typingText"] = "Mr. Speaker, Mr. Vice President, Members of Congress, fellow citizens: Fifty-one years ago, John F. Kennedy declared to this Chamber that “the Constitution makes us not rivals for power but partners for progress... It is my task,” he said, “to report the State of the Union – to improve it is the task of us all.” Tonight, thanks to the grit and determination of the American people, there is much progress to report. After a decade of grinding war, our brave men and women in uniform are coming home. After years of grueling recession, our businesses have created over six million new jobs. We buy more American cars than we have in five years, and less foreign oil than we have in twenty. Our housing market is healing, our stock market is rebounding, and consumers, patients, and homeowners enjoy stronger protections than ever before. Together, we have cleared away the rubble of crisis, and can say with renewed confidence that the state of our union is stronger. But we gather here knowing that there are millions of Americans whose hard work and dedication have not yet been rewarded. Our economy is adding jobs – but too many people still can’t find full-time employment. Corporate profits have rocketed to all-time highs – but for more than a decade, wages and incomes have barely budged.";
  //  This variable is the id assigned by you to the element the user will be typing in
	window["typingTextInputId"] = "input-element";
  //  This variable is the id assigned by you to the element where this script is meant to output the HTML result of the match
	window["typingTextOutputId"] = "checkThis";
  
  //  The following is error checking, it can be ommitted but if you are experiencing problems, this will help you ensure that the problems are not your fault.
	if(typeof window["typingText"] != "undefined")
		var primaryText = window["typingText"];
	else {
		IsLog.c("Error: can't match string, \"typingText\" not defined.");
		return -1;
	}
	if(typeof window["typingTextOutputId"] != "undefined") {
		if($("#"+window["typingTextOutputId"])) {
			var outputElement = $("#"+window["typingTextOutputId"]);
			outputElement.html(document.createTextNode(primaryText));
		} else {
			IsLog.c("Error: can't output results, \"#"+window["typingTextOutputId"]+"\" element not found.");
			return -1;
		}
	} else {
		IsLog.c("Error: can't output results, \"typingTextOutputId\" not defined.");
		return -1;
	}
  
  //  This is a sample event bind which will run the match.
  //  I suggest you bind it to mouse events (and touch) so that when the user clicks to correct mistakes those mistakes are reflected in the match output immediately.
	$("#againstThis").bind("keyup click touchstart touch", function(event) {
    //  KeyCode 32 is space " " and the others are the click events... if you run it on all keyup events JavaScript will run this function too often and severe process lag will occur. Please limit this with the "if"
		if(event.keyCode == 32 || this.originalEvent instanceof MouseEvent || this.originalEvent instanceof TouchEvent) {
      //  This is the connection point to the library. This is the match and the display rolled into one.    
			updateTestDisplay();
		} 
	});
}
```

The assumption we have made with this tool is that you will have static text against which user input text will be compared and the resulting HTML will be output on the document and styled with the following classes:

```CSS
//  This class is applied to all words with errors in them. (NOTE: this does not include words which are entirely in error)
.typing-error {
  color: red;
}
//  This class is applied to all "missing" errors
.missing-letter, .missing-word {
  color: blue;
}
//  This class is applied to all "extra" errors
.extra-letter, .extra-word {
	color: green;
}
//  This class is applied to all mistyped individual letters
.mistyped-letter {
	color: red;
}
//  This class is applied to all whitespace between words that is determined to be missing
.missing-space {
	background-color: red;
	text-decoration: underline;
	border-bottom: 1px pink;
}
//  This class is applied to all words that are without errors
.correct {
	color: black;
}
//  This class is applied to the remaining text that wasn't found in the user input text.
.untyped {
	color: grey;
}
```
