---
icon: code
description: This document provides the specifications of the MBTI Pep Talk API.
cover: ../.gitbook/assets/banner.png
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Reference Docs

## :computer: Authentication

This API uses Bearer Token Authentication. Clients must include the API key in the Authorization header of each request.

#### Getting an API Key

1\. Sign up at \[MBTI Pep TAlk API]\([https://api.peptalk.jiwonkwak.co/](https://api.peptalk.jiwonkwak.co/)).

2\. Retrieve your API key after signing up.

3\. Use this key in the Authorization header for every request.

{% tabs %}
{% tab title="Curl" %}
```
curl -X GET "https://api.example.com/resource" \
     -H "Authorization: Bearer YOUR_API_KEY"
```
{% endtab %}

{% tab title="JavaScript" %}

{% endtab %}

{% tab title="Python" %}
```python
import requests

headers = {
    "Authorization": "Bearer YOUR_API_KEY"
}

response = requests.get("https://api.example.com/resource", headers=headers)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Authorization Header Format

```json
Authorization: Bearer YOUR_API_KEY
```

Replace YOUR\_API\_KEY with your actual API key.

#### Handling Invalid API Keys

• If you provide an invalid or missing API key, the API will return an 401 Unauthorized error.

• Expired or revoked keys will also trigger authentication failures.

## :warning: Error Codes

When the API encounters an error, it returns an appropriate HTTP status code along with a JSON response containing an error message.

#### Error Response Format

```json
{
    "error": {
        "code": 401,
        "message": "Invalid API Key"
    }
}
```

#### Common Error Codes

| Status Code | Error Message         | Description                                          |
| ----------- | --------------------- | ---------------------------------------------------- |
| 400         | Bad Request           | The request is malformed or missing parameters.      |
| 401         | Unauthorized          | The API key is invalid, expired, or missing.         |
| 403         | Forbidden             | The API key does not have the necessary permissions. |
| 404         | Not Found             | The requested resource does not exist.               |
| 429         | Too Many Requests     | The request exceeded the API rate limit.             |
| 500         | Internal Server Error | An unexpected server error occurred.                 |

## Get Pep Talk

<mark style="color:green;">`GET`</mark> `/api/peptalk/`

Retrieves pep talks with advanced filtering.

### **Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

### **Query Parameters**

<table><thead><tr><th width="128">Name</th><th>Type</th><th>Required</th><th width="363">Description</th></tr></thead><tbody><tr><td>mbti_type</td><td><code>string</code></td><td><em>optional</em></td><td><p>Personality type. Randomly chosen if omitted.<br><strong>Supported values</strong></p><pre class="language-python" data-overflow="wrap"><code class="lang-python">ISTJ, ISFJ, INFJ, INTJ, ISTP, ISFP, INFP, INTP, ESTP, ESFP, ENFP, ENTP, ESTJ, ESFJ, ENFJ, ENTJ
</code></pre></td></tr><tr><td>mood</td><td><code>string</code></td><td><em>optional</em></td><td><p>Specifies the emotional state for the pep talk. Randomly chosen if omitted.<br><strong>Supported values</strong></p><pre class="language-python" data-overflow="wrap"><code class="lang-python">sad, angry, annoyed, anxious, frustrated, lonely, depressed, overwhelmed
</code></pre></td></tr><tr><td>username</td><td><code>string</code></td><td><em>optional</em></td><td>Username of the person that created the pep talk.</td></tr><tr><td>search</td><td><code>string</code></td><td><em>optional</em></td><td>A search keyword.</td></tr><tr><td>ordering</td><td><code>string</code></td><td><em>optional</em></td><td><p>The order of returned results.</p><p><strong>Supported values</strong></p><pre class="language-python"><code class="lang-python"><strong>'created_at', 'updated_at'
</strong></code></pre></td></tr><tr><td>page</td><td><code>integer</code></td><td><em>optional</em></td><td>Number of pages returned. (Default: 1)</td></tr><tr><td>page_size</td><td><code>integer</code></td><td><em>optional</em></td><td>Number of entries per page returned. (Default: 10)</td></tr></tbody></table>

### **Example Request**

```url
http://api.peptalk.jiwonkwak.co/api/peptalk/?mbti_type=ENTJ&mood=angry&page=2&page_size=3
```

### **Request**

{% tabs %}
{% tab title="Curl" %}
{% code overflow="wrap" %}
```url
curl -X 'GET' \
  'http://api.peptalk.jiwonkwak.co/api/peptalk/?mbti_type=ENTJ&mood=angry&page=1&page_size=3' \
  -H 'accept: application/json' \
  -H 'authorization: Basic aml3b25rd2FrOmF0bGFudGljQ2hhcnRlcjA4MTQ=' \
  -H 'X-CSRFTOKEN: Bp0ZFxJAa77cdttXw8VqodapUt3n9KailNTs5JuaCIq4cTcowpq5o6BZz3I5NY5x'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer 2acc6dc3ee05dd9c12e341bf98ea54bbe6ca6662");

const requestOptions = {
  method: "GET",
  headers: myHeaders,
  redirect: "follow"
};

fetch("api.peptalk.jiwonkwak.co/api/peptalk/?mbti_type=ENTJ&mood=angry&page=1&page_size=3", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "api.peptalk.jiwonkwak.co/api/peptalk/?mbti_type=ENTJ&mood=angry&page=1&page_size=3"

payload = {}
headers = {
  'Authorization': 'Bearer 2acc6dc3ee05dd9c12e341bf98ea54bbe6ca6662'
}

response = requests.request("GET", url, headers=headers, data=payload)

print(response.text)

```
{% endtab %}
{% endtabs %}

{% code title="Request URL" fullWidth="true" %}
```http
http://api.peptalk.jiwonkwak.co/api/peptalk/?mbti_type=ENTJ&mood=angry&page=2&page_size=3
```
{% endcode %}

### **Response**

{% tabs %}
{% tab title="200" %}
```json
{
  "mbti_type": "INTJ",
  "mood": "default",
  "pep_eng": "Trust your strategy. Overthinking won’t make it perfect.",
  "pep_kor": "네 전략을 믿어. 과한 생각이 완벽함을 만들진 않아."
}
```
{% endtab %}

{% tab title="404" %}
```json
{
    "detail": "No PepTalk matches the given query."
}
```
{% endtab %}

{% tab title="400" %}
```json
{
    "mood": [
        "Select a valid choice. hangry is not one of the available choices."
    ]
}
```
{% endtab %}
{% endtabs %}

***

## Get a Random Pep Talk by Mood and MBTI

<mark style="color:green;">`GET`</mark> `/api/peptalk/random`

Retrieves a random pep talk for a specified MBTI and mood.

### **Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

### **Query Parameters**

<table><thead><tr><th width="128">Name</th><th>Type</th><th>Required</th><th width="363">Description</th></tr></thead><tbody><tr><td>mbti_type</td><td><code>string</code></td><td><em>required</em></td><td><p></p><p>Personality type. Randomly chosen if omitted.<br><strong>Supported values</strong></p><pre class="language-python" data-overflow="wrap"><code class="lang-python">ISTJ, ISFJ, INFJ, INTJ, ISTP, ISFP, INFP, INTP, ESTP, ESFP, ENFP, ENTP, ESTJ, ESFJ, ENFJ, ENTJ
</code></pre></td></tr><tr><td>mood</td><td><code>string</code></td><td><em>required</em></td><td><p>Specifies the emotional state for the pep talk. Randomly chosen if omitted.<br><strong>Supported values</strong></p><pre class="language-python" data-overflow="wrap"><code class="lang-python">sad, angry, annoyed, anxious, frustrated, lonely, depressed, overwhelmed
</code></pre></td></tr><tr><td>page</td><td><code>integer</code></td><td><em>optional</em></td><td>Number of pages returned. (Default: 1)</td></tr><tr><td>page_size</td><td><code>integer</code></td><td><em>optional</em></td><td>Number of entries per page returned. (Default: 10)</td></tr></tbody></table>

### **Request**

{% tabs %}
{% tab title="Curl" %}
```url
curl -X 'GET' \
  'http://api.peptalk.jiwonkwak.co/api/peptalk/random/?mbti_type=INTJ&mood=anxious' \
  -H 'accept: application/json' \
  -H 'authorization: Basic aml3b25rd2FrOmF0bGFudGljQ2hhcnRlcjA4MTQ=' \
  -H 'X-CSRFTOKEN: Bp0ZFxJAa77cdttXw8VqodapUt3n9KailNTs5JuaCIq4cTcowpq5o6BZz3I5NY5x'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer 2acc6dc3ee05dd9c12e341bf98ea54bbe6ca6662");

const requestOptions = {
  method: "GET",
  headers: myHeaders,
  redirect: "follow"
};

fetch("api.peptalk.jiwonkwak.co/api/peptalk/random/?mbti_type=INTJ&mood=anxious", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "api.peptalk.jiwonkwak.co/api/peptalk/random/?mbti_type=INTJ&mood=anxious"

payload = {}
headers = {
  'Authorization': 'Bearer 2acc6dc3ee05dd9c12e341bf98ea54bbe6ca6662'
}

response = requests.request("GET", url, headers=headers, data=payload)

print(response.text)

```
{% endtab %}
{% endtabs %}

{% code title="Request URL" fullWidth="true" %}
```http
http://api.peptalk.jiwonkwak.co/api/peptalk/random/?mbti_type=INTJ&mood=anxious
```
{% endcode %}

### **Response**

{% tabs %}
{% tab title="200" %}
```json
{
  "mbti_type": "INTJ",
  "mood": "default",
  "pep_eng": "Trust your strategy. Overthinking won’t make it perfect.",
  "pep_kor": "네 전략을 믿어. 과한 생각이 완벽함을 만들진 않아."
}
```
{% endtab %}

{% tab title="404" %}
```json
{
    "detail": "Not found."
}
```
{% endtab %}
{% endtabs %}

***

## Get Pep Talk by ID

<mark style="color:green;">`GET`</mark> `/api/peptalk/:id/`

Retrieves a specific pep talk by ID.

### **헤더**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

### **Request**

{% tabs %}
{% tab title="Curl" %}
```url
curl -X 'GET' \
  'https://api.peptalk.jiwonkwak.co/api/peptalk/65/' \
  -H 'Authorization: ••••••'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer 2acc6dc3ee05dd9c12e341bf98ea54bbe6ca6662");

const requestOptions = {
  method: "GET",
  headers: myHeaders,
  redirect: "follow"
};

fetch("https://api.peptalk.jiwonkwak.co/api/peptalk/65/", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://api.peptalk.jiwonkwak.co/api/peptalk/65/"

payload = {}
headers = {
  'Authorization': 'Bearer 2acc6dc3ee05dd9c12e341bf98ea54bbe6ca6662'
}

response = requests.request("GET", url, headers=headers, data=payload)

print(response.text)


```
{% endtab %}
{% endtabs %}

{% code title="Request URL" fullWidth="true" %}
```http
http://api.peptalk.jiwonkwak.co/api/peptalk/random/?mbti_type=INTJ&mood=anxious
```
{% endcode %}

### **Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "id": 65,
    "mbti_type": "ISTJ",
    "mood": "sad",
    "pep_eng": "Even the strongest foundations shake sometimes. It’s okay to take a moment to reset. 🏗️",
    "pep_kor": "가장 강한 기초도 가끔 흔들려. 다시 시작할 시간을 가져도 괜찮아. 🏗️",
    "owner": "jiwonkwak",
    "created_at": "2025-01-27T05:47:55.148094Z",
    "updated_at": "2025-01-27T05:47:55.148100Z"
}
```
{% endtab %}

{% tab title="404" %}
```json
{
    "detail": "Not found."
}
```
{% endtab %}
{% endtabs %}


***

## Add a New Pep Talk

<mark style="color:purple;">`POST`</mark> `/api/peptalk/add`

Add a new pep talk.

### **Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

### Request Body

<table><thead><tr><th width="169">Name</th><th>Type</th><th width="363">Description</th></tr></thead><tbody><tr><td>mbti</td><td><code>string</code></td><td>Page number (default: 1)</td></tr><tr><td>mood</td><td><code>string</code></td><td>Number of results per page (default: 20)</td></tr><tr><td>pep_eng</td><td><code>string</code></td><td>Pep talk in English</td></tr><tr><td>pep_kor</td><td><code>string</code></td><td>Pep talk in Korean</td></tr></tbody></table>

### Example Request

{% tabs %}
{% tab title="Curl" %}
```url
curl -X 'GET' \
  'https://api.peptalk.jiwonkwak.co/api/peptalk/' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: ••••••' \
  -D '{
    "mbti_type": "ESTP",
    "mood": "frustrated",
    "pep_eng": "You'\''re an ESTP—bold, sharp, and built for action. Frustration? That'\''s just your energy being       temporarily bottled up. You thrive on momentum, and setbacks are just launchpads for your next big move.            Channel that fire, adapt, and push forward. The game isn’t over—it’s just getting interesting.",
    "pep_kor": "당신은 ESTP—대담하고 예리하며 행동을 위해 태어난 사람입니다. 좌절감? 그건 단지 당신의 에너지가 잠시 갇혀 있는 상태일 뿐이에요. 당       신은 속도를 즐기는 사람이고, 장애물은 그저 다음 도약을 위한 발판일 뿐입니다. 이 불꽃을 활용하고, 적응하며 앞으로 나아가세요. 게임은 끝난 게 아        니라, 이제부터 더 흥미로워질 뿐입니다."
    }'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const myHeaders = new Headers();
myHeaders.append("Content-Type", "application/json");
myHeaders.append("Authorization", "Bearer 2acc6dc3ee05dd9c12e341bf98ea54bbe6ca6662");

const raw = JSON.stringify({
  "mbti_type": "ESTP",
  "mood": "frustrated",
  "pep_eng": "You're an ESTP—bold, sharp, and built for action. Frustration? That's just your energy being temporarily bottled up. You thrive on momentum, and setbacks are just launchpads for your next big move. Channel that fire, adapt, and push forward. The game isn’t over—it’s just getting interesting.",
  "pep_kor": "당신은 ESTP—대담하고 예리하며 행동을 위해 태어난 사람입니다. 좌절감? 그건 단지 당신의 에너지가 잠시 갇혀 있는 상태일 뿐이에요. 당신은 속도를 즐기는 사람이고, 장애물은 그저 다음 도약을 위한 발판일 뿐입니다. 이 불꽃을 활용하고, 적응하며 앞으로 나아가세요. 게임은 끝난 게 아니라, 이제부터 더 흥미로워질 뿐입니다."
});

const requestOptions = {
  method: "POST",
  headers: myHeaders,
  body: raw,
  redirect: "follow"
};

fetch("https://api.peptalk.jiwonkwak.co/api/peptalk/", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json

url = "https://api.peptalk.jiwonkwak.co/api/peptalk/"

payload = json.dumps({
  "mbti_type": "ESTP",
  "mood": "frustrated",
  "pep_eng": "You're an ESTP—bold, sharp, and built for action. Frustration? That's just your energy being temporarily bottled up. You thrive on momentum, and setbacks are just launchpads for your next big move. Channel that fire, adapt, and push forward. The game isn’t over—it’s just getting interesting.",
  "pep_kor": "당신은 ESTP—대담하고 예리하며 행동을 위해 태어난 사람입니다. 좌절감? 그건 단지 당신의 에너지가 잠시 갇혀 있는 상태일 뿐이에요. 당신은 속도를 즐기는 사람이고, 장애물은 그저 다음 도약을 위한 발판일 뿐입니다. 이 불꽃을 활용하고, 적응하며 앞으로 나아가세요. 게임은 끝난 게 아니라, 이제부터 더 흥미로워질 뿐입니다."
})
headers = {
  'Content-Type': 'application/json',
  'Authorization': 'Bearer 2acc6dc3ee05dd9c12e341bf98ea54bbe6ca6662'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

```
{% endtab %}
{% endtabs %}

{% code title="Request URL" fullWidth="true" %}
```http
http://api.peptalk.jiwonkwak.co/api/peptalk/random/?mbti_type=INTJ&mood=anxious
```
{% endcode %}

### Response

{% tabs %}
{% tab title="Success" %}
```json
{
    "id": 482,
    "mbti_type": "ENTP",
    "mood": "lonely",
    "pep_eng": "You’re never truly alone—your mind is an adventure all on its own! 🚀✨ Go spark a conversation, challenge a thought, or create something wild. The world needs your chaos! 💡🔥",
    "pep_kor": "너는 절대 혼자가 아니야—너의 머릿속은 이미 하나의 모험이야! 🚀✨ 누군가와 대화 나누고, 새로운 아이디어를 던져봐! 세상은 너의 혼돈을 기다리고 있어! 💡🔥",
    "owner": "jiwonkwak",
    "created_at": "2025-02-25T12:04:34.866778Z",
    "updated_at": "2025-02-25T12:04:34.866818Z"
}
```
{% endtab %}

{% tab title="404" %}
```
{
    "detail": "Not found."
}
```
{% endtab %}
{% endtabs %}

***

## Update a Pep Talk

<mark style="color:purple;">`PUT`</mark> `/api/peptalk/:id/`

Updates an existing pep talk.

### **Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

### Path Parameter

| Name | Type      | Description                            |
| ---- | --------- | -------------------------------------- |
| id   | _integer_ | ID of the pep talk you want to update. |

### Example Request

```
PUT /api/peptalk/45/
```

### Request Body

```json
{
  "pep_eng": "Your ideas are brilliant. Don’t be afraid to execute them."
}
```

### Response

{% tabs %}
{% tab title="Success" %}
```json
{
  "message": "Pep talk updated."
}
```
{% endtab %}

{% tab title="404" %}
```
{
    "error": {
        "code": 404,
        "message": "Page not found."
    }
}
```
{% endtab %}
{% endtabs %}

***

## Delete a Pep Talk

<mark style="color:purple;">`DELETE`</mark> `/api/peptalk/:id/`

Deletes a pep talk.

### **Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

### Path Parameter

| Name | Type      | Description                            |
| ---- | --------- | -------------------------------------- |
| id   | _integer_ | ID of the pep talk you want to delete. |

### Example Request

```
DELETE /api/peptalk/40/
```

### Response

{% tabs %}
{% tab title="Success" %}
```json
{
  "message": "Pep talk removed."
}
```
{% endtab %}

{% tab title="404" %}
```
{
    "detail": "Not found."
}
```
{% endtab %}
{% endtabs %}

***

