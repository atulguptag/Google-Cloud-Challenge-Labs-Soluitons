## `Lab Name` - *Cloud Speech API 3 Ways: Challenge Lab [ARC132]*

## `Lab Link` - [Click Here](https://www.cloudskillsboost.google/focuses/67215?parent=catalog)

## [YouTube Solution Link](https://youtu.be/ELtMXEyZq5g)

Run the following commands in the Cloud Shell Terminal.

## Task 1. Create an API key

In this task you have to create an API KEY.

* Go to *Navigation Menu* > *APIs and Services* > `Credentials` > Click `Create` then select `API KEY`.

* Save the `GENRATED API KEY`.

Now, run the below commands in the Cloud Shell Terminal.

```
gcloud compute ssh lab-vm --zone=<INSTANCE_ZONE>
```
 
```
export API_KEY=

TASK_2_FILE_NAME=""

TASK_3_REQUEST_FILE=""

TASK_3_RESPONSE_FILE=""

TASK_4_SENTENCE=""

TASK_4_FILE_NAME=""

TASK_5_SENTENCE=""

TASK_5_FILE_NAME=""
```

```
export PROJECT_ID=$(gcloud config get-value project)

source venv/bin/activate
```

* Creating synthesize-text.json file.

```
cat > synthesize-text.json <<EOF
{
    'input':{
        'text':'Cloud Text-to-Speech API allows developers to include
           natural-sounding, synthetic human speech as playable audio in
           their applications. The Text-to-Speech API converts text or
           Speech Synthesis Markup Language (SSML) input into audio data
           like MP3 or LINEAR16 (the encoding used in WAV files).'
    },
    'voice':{
        'languageCode':'en-gb',
        'name':'en-GB-Standard-A',
        'ssmlGender':'FEMALE'
    },
    'audioConfig':{
        'audioEncoding':'MP3'
    }
}

EOF
```

```
curl -H "Authorization: Bearer "$(gcloud auth application-default print-access-token) \
  -H "Content-Type: application/json; charset=utf-8" \
  -d @synthesize-text.json "https://texttospeech.googleapis.com/v1/text:synthesize" \
  > $TASK_2_FILE_NAME
```

* Creating tts_decode.py file.

```
cat > tts_decode.py <<EOF
import argparse
from base64 import decodebytes
import json
"""
Usage:
        python tts_decode.py --input "synthesize-text.txt" \
        --output "synthesize-text-audio.mp3"
"""
def decode_tts_output(input_file, output_file):
    """ Decode output from Cloud Text-to-Speech.
    input_file: the response from Cloud Text-to-Speech
    output_file: the name of the audio file to create
    """
    with open(input_file) as input:
        response = json.load(input)
        audio_data = response['audioContent']
        with open(output_file, "wb") as new_file:
            new_file.write(decodebytes(audio_data.encode('utf-8')))
if __name__ == '__main__':
    parser = argparse.ArgumentParser(
        description="Decode output from Cloud Text-to-Speech",
        formatter_class=argparse.RawDescriptionHelpFormatter)
    parser.add_argument('--input',
                       help='The response from the Text-to-Speech API.',
                       required=True)
    parser.add_argument('--output',
                       help='The name of the audio file to create',
                       required=True)
    args = parser.parse_args()
    decode_tts_output(args.input, args.output)
EOF
```

```
python tts_decode.py --input "$TASK_2_FILE_NAME" --output "synthesize-text-audio.mp3"
```

* Define variables

```
audio_uri="gs://cloud-samples-data/speech/corbeau_renard.flac"

## Generate speech_request.json file
cat > "$TASK_3_REQUEST_FILE" <<EOF
{
  "config": {
    "encoding": "FLAC",
    "sampleRateHertz": 44100,
    "languageCode": "fr-FR"
  },
  "audio": {
    "uri": "$audio_uri"
  }
}
EOF
```
* Make API call for French transcription

```
curl -s -X POST -H "Content-Type: application/json" \
    --data-binary @"$TASK_3_REQUEST_FILE" \
    "https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}" \
    -o "$TASK_3_RESPONSE_FILE"
```

* Updating the files.

```
sudo apt-get update
sudo apt-get install -y jq
```

* Set the variables for the Translation API request

```
source_lang="ja"
target_lang="en"

```

* Make the Translation API request using curl

```
response=$(curl -s -X POST \
  -H "Authorization: Bearer $(gcloud auth application-default print-access-token)" \
  -H "Content-Type: application/json; charset=utf-8" \
  -d "{\"q\": \"$TASK_4_SENTENCE\"}" \
  "https://translation.googleapis.com/language/translate/v2?key=${API_KEY}&source=${source_lang}&target=${target_lang}")

```

* Check if the response contains an error.

```
if [[ $response == *"error"* ]]; then
  echo "Translation API returned an error:"
  echo "$response"
else
  translation=$(jq -r '.data.translations[].translatedText' <<< "$response")
  if [[ -z "$translation" ]]; then
    echo "Translation is empty or null."
  else
    echo "$translation" > "$TASK_4_FILE_NAME"
    echo "Translation saved to $TASK_4_FILE_NAME:"
    cat "$TASK_4_FILE_NAME"
  fi
fi
```

* URL - decode the sentence.

```
decoded_sentence=$(python -c "import urllib.parse; print(urllib.parse.unquote('$TASK_5_SENTENCE'))")

# Make the Language Detection API request using curl
curl -s -X POST \
  -H "Authorization: Bearer $(gcloud auth application-default print-access-token)" \
  -H "Content-Type: application/json; charset=utf-8" \
  -d "{\"q\": [\"$decoded_sentence\"]}" \
  "https://translation.googleapis.com/language/translate/v2/detect?key=${API_KEY}" \
  -o "$TASK_5_FILE_NAME"
```

# Congratulations🎉! You're done with this Challenge Lab.