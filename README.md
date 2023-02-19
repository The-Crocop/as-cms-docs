Audio Description
---

# Introduction
The Audio Description api provides the users the ability to generate played audio in different voices for a multitude of languages.
Additionally it offers the users the ability to translate texts automatically.
Finally we offer also an embeddable player that can be added to any website.

# Authentication
In order to access the api you first will need an authentication token. 
This can be retrieved via Open ID Connect Client Credentials flow.
After you have received your client credentials you can use them like this:

`curl -u <client_id>:<client_secret> -d 'grant_type=client_credentials'  https://dev-64698101.okta.com/oauth2/default/v1/token`

The response looks like this:
`{"token_type":"Bearer","expires_in":3600,"access_token":"eyJ....","scope":"audio-descriptions"}`

For the following Requests the access_token field from the response can be used as Authorization Header.

# Audio Models

In order to use the api you have to choose an audio model appropriate for the text you want to synthesize. For most of the languages we provide a [wavenet](https://en.wikipedia.org/wiki/WaveNet) model. These are models that use a neural network and have an especially authentic voice.
For most of the languages there is female and male voices to choose from.
Usually you should pick a wavenet model when available.

To get a list of possible models you can call the following endpoint (Access token should be the token retrieved from Authorization chapter):
`curl -H "Authorization: Bearer $ACCESS_TOKEN" 'https://audiodescription.citizenjournalist.io/api/audio-models?page=0&size=10000'`

TODO Table of audio models

# Audio Description API
Now in order to synthesize a text following api can be used (Access token should be the token retrieved from Authorization chapter).

```
curl -H "Authorization: Bearer $ACCESS_TOKEN" \
      -H 'Accept: application/json' \
      -H 'Content-Type: application/json' \
      -d '{"targetLanguage": "GERMAN", "inputText": "Das ist ein Test.", "audioModel": { "id": 2, "language": "GERMAN", "gender": "FEMALE", "modelName": "de-DE-Wavenet-F" }}' -XPOST 'https://audiodescription.citizenjournalist.io/api/audio-descriptions'
```
Here the field `targetLanguage` is the selected Language of the text. Possible values are: `ENGLISH,GERMAN,FRENCH,ITALIAN,SPANISH,CATALAN,UKRAINIAN,PORTUGESE,JAPANESE`.
The field `inputText` denotes the actual text to be synthesized.
The field `audioModel` denotes one of the audio models from the Audio Model Chapter. For technical reasons (this will be simplified in the future) it is important to pass at least the fields `id,language ,modelName,gender` from the audio Model. Only passing the id is not enough.

!IMPORTANT a single paragraph must not be longer than 5000 characters.
You can automatically let the api create chapters by adding empty lines to the text. The player will then automatically have functionallity to jump between the paragraphs.

The result looks like this:
```
{
"id":1057,
"targetLanguage":"GERMAN",
"inputText":"Das ist ein Test.",
"audioFile":null,
"audioFileContentType":"audio/mpeg",
"url":"https://storage.googleapis.com/as-cms-audiodescriptions/f2d3c29b-5033-406a-b8de-9eadcdc2d6f4.mp3",
"audioModel":{"id":2,"name":null,"language":null,"gender":null,"modelName":null
  }
}
```
The id can be used to pass to the embeddable player.

# The Player
<Link to a player image>
 To make use of the automatic audio description you should use our embedded player. Just use the embed code below into your website and replace the id field with the one from the response.

```
<div style="height: 0; max-width: 410px; position: relative; padding-bottom: 62px;"><iframe src="https://as.citizenjournalist.io/audio-description/<ID-from-response>/player?embedded=true&theme=light" scrolling="no" style="border: none; overflow: hidden; position: absolute; top: 0; left: 0; width: 100%; height: 100%;" allowtransparency="true"></iframe></div>
```
You can adjust the theme by replacing the theme paramer with dark. At the moment only those 2 themes exist.

# UI
Beyond the API there is also the ui reachable at 
https://as.citizenjournalist.io 
There you can play around and create audio descriptions and translation with the web interface. After login just go to the menu at entities -> Audio Description.




