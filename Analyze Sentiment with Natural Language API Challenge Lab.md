## `Lab Name` - *Analyze Sentiment with Natural Language API: Challenge Lab [ARC130]*

## `Lab Link` - [*Click Here*](https://www.cloudskillsboost.google/focuses/66586?parent=catalog)

<!-- ## [YouTube Solution Link]() -->

Run the below commands in the SSH of your VM-Instance.

## Task 1. Create an API key

* *Navigation Menu* > *APIs and Services* > *Credentials* > Click `+Create Credentials` > Choose `API KEY`.

```
gcloud compute ssh lab-vm --zone=<INSTANCE_ZONE>
```

```
export API_KEY=
```


## Task 2. Set up Google Docs and call the Natural Language API

* Open this link in Incoginato Mode [Click Here](https://docs.google.com/document/create)
* Login using your Lab Credentials.
* Click `Externsion` > then `Apps Script`.

* Paste the below code in the `code.js file`.

```
  /**
  * @OnlyCurrentDoc
  *
  * The above comment directs Apps Script to limit the scope of file
  * access for this add-on. It specifies that this add-on will only
  * attempt to read or modify the files in which the add-on is used,
  * and not all of the user's files. The authorization request message
  * presented to users will reflect this limited scope.
  */
  /**
  * Creates a menu entry in the Google Docs UI when the document is
  * opened.
  *
  */
  function onOpen() {
    var ui = DocumentApp.getUi();
    ui.createMenu('Natural Language Tools')
      .addItem('Mark Sentiment', 'markSentiment')
      .addToUi();
  }
  /**
  * Gets the user-selected text and highlights it based on sentiment
  * with green for positive sentiment, red for negative, and yellow
  * for neutral.
  *
  */
  function markSentiment() {
    var POSITIVE_COLOR = '#00ff00';  //  Colors for sentiments
    var NEGATIVE_COLOR = '#ff0000';
    var NEUTRAL_COLOR = '#ffff00';
    var NEGATIVE_CUTOFF = -0.2;   //  Thresholds for sentiments
    var POSITIVE_CUTOFF = 0.2;
    var selection = DocumentApp.getActiveDocument().getSelection();
    if (selection) {
      var string = getSelectedText();
      var sentiment = retrieveSentiment(string);
      //  Select the appropriate color
      var color = NEUTRAL_COLOR;
      if (sentiment <= NEGATIVE_CUTOFF) {
        color = NEGATIVE_COLOR;
      }
      if (sentiment >= POSITIVE_CUTOFF) {
        color = POSITIVE_COLOR;
      }
      //  Highlight the text
      var elements = selection.getSelectedElements();
      for (var i = 0; i < elements.length; i++) {
        if (elements[i].isPartial()) {
          var element = elements[i].getElement().editAsText();
          var startIndex = elements[i].getStartOffset();
          var endIndex = elements[i].getEndOffsetInclusive();
          element.setBackgroundColor(startIndex, endIndex, color);
        } else {
          var element = elements[i].getElement().editAsText();
          foundText = elements[i].getElement().editAsText();
          foundText.setBackgroundColor(color);
        }
      }
    }
  }
  /**
  * Returns a string with the contents of the selected text.
  * If no text is selected, returns an empty string.
  */
  function getSelectedText() {
    var selection = DocumentApp.getActiveDocument().getSelection();
    var string = "";
    if (selection) {
      var elements = selection.getSelectedElements();
      for (var i = 0; i < elements.length; i++) {
        if (elements[i].isPartial()) {
          var element = elements[i].getElement().asText();
          var startIndex = elements[i].getStartOffset();
          var endIndex = elements[i].getEndOffsetInclusive() + 1;
          var text = element.getText().substring(startIndex, endIndex);
          string = string + text;
        } else {
          var element = elements[i].getElement();
          // Only translate elements that can be edited as text; skip
          // images and other non-text elements.
          if (element.editAsText) {
            string = string + element.asText().getText();
          }
        }
      }
    }
    return string;
  }
  /** Given a string, will call the Natural Language API and retrieve
    * the sentiment of the string.  The sentiment will be a real
    * number in the range -1 to 1, where -1 is highly negative
    * sentiment and 1 is highly positive.
  */
  function retrieveSentiment (line) {
  var apiKey = "YOUR_API_KEY_HERE";
  var apiEndpoint =
'https://language.googleapis.com/v1/documents:analyzeSentiment?key='
+ apiKey;
  //  Create a structure with the text, its language, its type,
  //  and its encoding
  var docDetails = {
    language: 'en-us',
    type: 'PLAIN_TEXT',
    content: line
  };
  var nlData = {
    document: docDetails,
    encodingType: 'UTF8'
  };
  //  Package all of the options and the data together for the call
  var nlOptions = {
    method : 'post',
    contentType: 'application/json',
    payload : JSON.stringify(nlData)
  };
  //  And make the call
  var response = UrlFetchApp.fetch(apiEndpoint, nlOptions);
  var data = JSON.parse(response);
  var sentiment = 0.0;
  //  Ensure all pieces were in the returned value
  if (data && data.documentSentiment
          && data.documentSentiment.score){
     sentiment = data.documentSentiment.score;
  }
  return sentiment;
}
```
* In place of "*YOUR_API_KEY_HERE*", replace it with your created `API_KEY` which you make it in task 1.

## Task 3. Analyze syntax and parts of speech with the Natural Language API

```
cat > analyze-request.json <<EOF
{
  "document":{
    "type":"PLAIN_TEXT",
    "content": "Google, headquartered in Mountain View, unveiled the new Android phone at the Consumer Electronic Show.  Sundar Pichai said in his keynote that users love their new Android phones."
  },
  "encodingType": "UTF8"
}
EOF

curl -s -H "Content-Type: application/json" \
-H "Authorization: Bearer $(gcloud auth print-access-token)" \
"https://language.googleapis.com/v1/documents:analyzeSyntax" \
-d @analyze-request.json > analyze-response.txt

cat analyze-response.txt
```

## Task 4. Perform multilingual natural language processing

```
cat > multi-nl-request.json <<EOF
{
  "document": {
    "type": "PLAIN_TEXT",
    "content": "Le bureau japonais de Google est situé à Roppongi Hills, Tokyo.",
    "language": "fr"
  }
}
EOF

curl -s -H "Content-Type: application/json" \
-H "Authorization: Bearer $(gcloud auth print-access-token)" \
"https://language.googleapis.com/v1/documents:analyzeEntities" \
-d @multi-nl-request.json > multi-response.txt

cat multi-response.txt
```

# Congratulations🎉!, You're all done with this Challenge lab.