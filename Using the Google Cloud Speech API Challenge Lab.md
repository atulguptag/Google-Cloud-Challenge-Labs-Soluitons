## `Lab Name` - *Using the Google Cloud Speech API: Challenge Lab [ARC131]*

## `Lab Link` - [*Click Here*](https://www.cloudskillsboost.google/focuses/65993?parent=catalog)

<!-- ## [YouTube Solution Link]() -->

Run the below commands in the SSH of your VM-Instance.

## Task 1. Create an API key

* *Navigation Menu* > *APIs and Services* > *Credentials* > Click `+Create Credentials` > Choose `API`.

```
export API_KEY=
```

### Define variables for file names.

```
TASK_2_REQUEST_FILE=

TASK_2_RESPONSE_FILE=

TASK_3_REQUEST_FILE=

TASK_3_RESPONSE_FILE=
```

## Task 2. Transcribe speech to English text

```
cat > "$TASK_2_REQUEST_FILE" <<EOF
{
  "config": {
    "encoding": "LINEAR16",
    "languageCode": "en-US",
    "audioChannelCount": 2
  },
  "audio": {
    "uri": "gs://spls/arc131/question_en.wav"
  }
}
EOF
```
* Make API call for English transcription

```
curl -s -X POST -H "Content-Type: application/json" --data-binary @"$TASK_2_REQUEST_FILE" \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}" > "$TASK_2_RESPONSE_FILE"
```

## Task 3. Transcribe speech to Spanish text

```
cat > "$TASK_3_REQUEST_FILE" <<EOF
{
  "config": {
    "encoding": "FLAC",
    "languageCode": "es-ES"
  },
  "audio": {
    "uri": "gs://spls/arc131/multi_es.flac"
  }
}
EOF
```

* Make API call for Spanish transcription

```
curl -s -X POST -H "Content-Type: application/json" --data-binary @"$TASK_3_REQUEST_FILE" \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}" > "$TASK_3_RESPONSE_FILE"
```

# Congratulations 🎉! You're all done with this Challenge Lab.