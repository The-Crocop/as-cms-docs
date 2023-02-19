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

```
curl -u <client_id>:<client_secret> -d 'grant_type=client_credentials'  https://dev-64698101.okta.com/oauth2/default/v1/token
```

The response looks like this:
```
{"token_type":"Bearer","expires_in":3600,"access_token":"eyJ....","scope":"audio-descriptions"}
```

For the following Requests the access_token field from the response can be used as Authorization Header.

# Audio Models

In order to use the api you have to choose an audio model appropriate for the text you want to synthesize. For most of the languages we provide a [wavenet](https://en.wikipedia.org/wiki/WaveNet) model. These are models that use a neural network and have an especially authentic voice.
For most of the languages there is female and male voices to choose from.
Usually you should pick a wavenet model when available.

To get a list of possible models you can call the following endpoint (Access token should be the token retrieved from Authorization chapter):
```
curl -H "Authorization: Bearer $ACCESS_TOKEN" 'https://audiodescription.citizenjournalist.io/api/audio-models?page=0&size=10000'
```

For a list of available audio models check [end](#available-audio-models) of this article

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
![Player Image](player.png)

<div style="height: 0; max-width: 410px; position: relative; padding-bottom: 62px;"><iframe src="https://as.citizenjournalist.io/audio-description/1054/player?embedded=true&theme=light" scrolling="no" style="border: none; overflow: hidden; position: absolute; top: 0; left: 0; width: 100%; height: 100%;" allowtransparency="true"></iframe></div>

 To make use of the automatic audio description you should use our embedded player. Just use the embed code below into your website and replace the id field with the one from the response.

```
<div style="height: 0; max-width: 410px; position: relative; padding-bottom: 62px;"><iframe src="https://as.citizenjournalist.io/audio-description/<ID-from-response>/player?embedded=true&theme=light" scrolling="no" style="border: none; overflow: hidden; position: absolute; top: 0; left: 0; width: 100%; height: 100%;" allowtransparency="true"></iframe></div>
```
You can adjust the theme by replacing the theme paramer with dark. At the moment only those 2 themes exist.

# UI
Beyond the API there is also the ui reachable at 
<https://as.citizenjournalist.io>
There you can play around and create audio descriptions and translation with the web interface. After login just go to the menu at entities -> Audio Description.

# Available Audio Models

| id   | name             | language   | gender | modelName        |
| ---- | ---------------- | ---------- | ------ | ---------------- |
| 1    | English (F)      | ENGLISH    | FEMALE | en-US-Wavenet-F  |
| 2    | German (F)       | GERMAN     | FEMALE | de-DE-Wavenet-F  |
| 3    | French (F)       | FRENCH     | FEMALE | fr-FR-Wavenet-E  |
| 4    | Italian (F)      | ITALIAN    | FEMALE | it-IT-Wavenet-B  |
| 5    | Spanish (F)      | SPANISH    | FEMALE | es-ES-Wavenet-C  |
| 6    | Catalan (F)      | CATALAN    | FEMALE | ca-es-Standard-A |
| 7    | Ukrainian (F)    | UKRAINIAN  | FEMALE | uk-UA-Wavenet-A  |
| 1001 | English-H (F)    | ENGLISH    | FEMALE | en-US-Wavenet-H  |
| 1002 | English-G (F)    | ENGLISH    | FEMALE | en-US-Wavenet-G  |
| 1003 | English-B (M)    | ENGLISH    | MALE   | en-US-Wavenet-B  |
| 1004 | English-C (F)    | ENGLISH    | FEMALE | en-US-Wavenet-C  |
| 1005 | English-I (M)    | ENGLISH    | MALE   | en-US-Wavenet-I  |
| 1006 | English-A (M)    | ENGLISH    | MALE   | en-US-Neural2-A  |
| 1007 | English-E (F)    | ENGLISH    | FEMALE | en-US-Wavenet-E  |
| 1008 | English-A (M)    | ENGLISH    | MALE   | en-US-Wavenet-A  |
| 1009 | English-J (M)    | ENGLISH    | MALE   | en-US-Wavenet-J  |
| 1010 | German-A (F)     | GERMAN     | FEMALE | de-DE-Wavenet-A  |
| 1011 | English-D (M)    | ENGLISH    | MALE   | en-US-Wavenet-D  |
| 1012 | English-F (F)    | ENGLISH    | FEMALE | en-US-Neural2-F  |
| 1013 | German-E (M)     | GERMAN     | MALE   | de-DE-Wavenet-E  |
| 1014 | French-A (F)     | FRENCH     | FEMALE | fr-FR-Wavenet-A  |
| 1015 | Italian-C (M)    | ITALIAN    | MALE   | it-IT-Wavenet-C  |
| 1016 | German-C (F)     | GERMAN     | FEMALE | de-DE-Wavenet-C  |
| 1017 | Italian-D (M)    | ITALIAN    | MALE   | it-IT-Wavenet-D  |
| 1018 | German-B (M)     | GERMAN     | MALE   | de-DE-Wavenet-B  |
| 1019 | Spanish-D (F)    | SPANISH    | FEMALE | es-ES-Wavenet-D  |
| 1020 | Spanish-B (M)    | SPANISH    | MALE   | es-ES-Wavenet-B  |
| 1021 | German-D (M)     | GERMAN     | MALE   | de-DE-Wavenet-D  |
| 1022 | French-D (M)     | FRENCH     | MALE   | fr-FR-Wavenet-D  |
| 1023 | French-C (F)     | FRENCH     | FEMALE | fr-FR-Wavenet-C  |
| 1024 | French-B (M)     | FRENCH     | MALE   | fr-FR-Wavenet-B  |
| 1025 | Italian-A (F)    | ITALIAN    | FEMALE | it-IT-Wavenet-A  |
| 1026 | English-C (F)    | ENGLISH    | FEMALE | en-US-Neural2-C  |
| 1027 | English-D (M)    | ENGLISH    | MALE   | en-US-Neural2-D  |
| 1028 | English-E (F)    | ENGLISH    | FEMALE | en-US-Neural2-E  |
| 1029 | English-G (F)    | ENGLISH    | FEMALE | en-US-Neural2-G  |
| 1030 | English-I (M)    | ENGLISH    | MALE   | en-US-Neural2-I  |
| 1031 | English-H (F)    | ENGLISH    | FEMALE | en-US-Neural2-H  |
| 1032 | English-J (M)    | ENGLISH    | MALE   | en-US-Neural2-J  |
| 1033 | German-F (F)     | GERMAN     | FEMALE | de-DE-Neural2-F  |
| 1034 | German-D (M)     | GERMAN     | MALE   | de-DE-Neural2-D  |
| 1035 | French-A (F)     | FRENCH     | FEMALE | fr-FR-Neural2-A  |
| 1036 | French-B (M)     | FRENCH     | MALE   | fr-FR-Neural2-B  |
| 1037 | French-C (F)     | FRENCH     | FEMALE | fr-FR-Neural2-C  |
| 1038 | French-D (M)     | FRENCH     | MALE   | fr-FR-Neural2-D  |
| 1039 | French-E (F)     | FRENCH     | FEMALE | fr-FR-Neural2-E  |
| 1040 | Japanese-C (M)   | JAPANESE   | MALE   | ja-JP-Wavenet-C  |
| 1041 | Arabic-A (F)     | ARABIC     | FEMALE | ar-XA-Wavenet-A  |
| 1042 | Arabic-C (M)     | ARABIC     | MALE   | ar-XA-Wavenet-C  |
| 1043 | Portuguese-A (F) | PORTUGUESE | FEMALE | pt-PT-Wavenet-A  |
| 1044 | Portuguese-B (M) | PORTUGUESE | MALE   | pt-PT-Wavenet-B  |
| 1045 | Portuguese-D (F) | PORTUGUESE | FEMALE | pt-PT-Wavenet-D  |
| 1046 | Japanese-D (M)   | JAPANESE   | MALE   | ja-JP-Wavenet-D  |
| 1047 | Japanese-A (F)   | JAPANESE   | FEMALE | ja-JP-Wavenet-A  |
| 1048 | Arabic-D (F)     | ARABIC     | FEMALE | ar-XA-Wavenet-D  |
| 1049 | Portuguese-C (M) | PORTUGUESE | MALE   | pt-PT-Wavenet-C  |
| 1050 | Japanese-B (F)   | JAPANESE   | FEMALE | ja-JP-Wavenet-B  |
| 1051 | Chinese-C (M)    | CHINESE    | MALE   | cmn-CN-Wavenet-C |
| 1052 | Chinese-B (M)    | CHINESE    | MALE   | cmn-CN-Wavenet-B |
| 1053 | Russian-A (F)    | RUSSIAN    | FEMALE | ru-RU-Wavenet-A  |
| 1054 | Chinese-D (F)    | CHINESE    | FEMALE | cmn-CN-Wavenet-D |
| 1055 | Russian-B (M)    | RUSSIAN    | MALE   | ru-RU-Wavenet-B  |
| 1056 | Arabic-B (M)     | ARABIC     | MALE   | ar-XA-Wavenet-B  |
| 1057 | Russian-D (M)    | RUSSIAN    | MALE   | ru-RU-Wavenet-D  |
| 1058 | Chinese-A (F)    | CHINESE    | FEMALE | cmn-CN-Wavenet-A |
| 1059 | Russian-C (F)    | RUSSIAN    | FEMALE | ru-RU-Wavenet-C  |
| 1060 | Russian-E (F)    | RUSSIAN    | FEMALE | ru-RU-Wavenet-E  |
| 1061 | English-K (F)    | ENGLISH    | FEMALE | en-US-News-K     |
| 1062 | English-L (F)    | ENGLISH    | FEMALE | en-US-News-L     |
| 1063 | English-M (M)    | ENGLISH    | MALE   | en-US-News-M     |
| 1064 | English-N (M)    | ENGLISH    | MALE   | en-US-News-N     |
| 1065 | Italian-A (F)    | ITALIAN    | FEMALE | it-IT-Neural2-A  |
| 1066 | Spanish-B (M)    | SPANISH    | MALE   | es-ES-Neural2-B  |
| 1067 | Italian-C (M)    | ITALIAN    | MALE   | it-IT-Neural2-C  |
| 1068 | Spanish-F (M)    | SPANISH    | MALE   | es-ES-Neural2-F  |
| 1069 | Spanish-A (F)    | SPANISH    | FEMALE | es-ES-Neural2-A  |
| 1070 | Spanish-D (F)    | SPANISH    | FEMALE | es-ES-Neural2-D  |
| 1071 | Spanish-C (F)    | SPANISH    | FEMALE | es-ES-Neural2-C  |
| 1072 | Spanish-E (F)    | SPANISH    | FEMALE | es-ES-Neural2-E  |
| 1075 | Japanese-B (F)   | JAPANESE   | FEMALE | ja-JP-Neural2-B  |
| 1074 | Japanese-C (M)   | JAPANESE   | MALE   | ja-JP-Neural2-C  |
| 1073 | Japanese-D (M)   | JAPANESE   | MALE   | ja-JP-Neural2-D  |
| 1076 | English-O (F)    | ENGLISH    | FEMALE | en-US-Studio-O   |
| 1077 | English-M (M)    | ENGLISH    | MALE   | en-US-Studio-M   |
