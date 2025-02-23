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

#### Common Error  Codes

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

Retrieves a random pep talk with advanced filtering.

### **Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

### **Query Parameters**

<table><thead><tr><th width="128">Name</th><th>Type</th><th>Required</th><th width="363">Description</th></tr></thead><tbody><tr><td>mbti_type</td><td><code>string</code></td><td><em>optional</em></td><td></td></tr><tr><td>mood</td><td><code>string</code></td><td><em>optional</em></td><td><p>Specifies the emotional state for the pep talk. Randomly chosen if omitted. <br><strong>Supported values</strong></p><pre class="language-python" data-overflow="wrap"><code class="lang-python">sad, angry, annoyed, anxious, frustrated, lonely, depressed, overwhelmed
</code></pre></td></tr><tr><td>username</td><td><code>string</code></td><td><em>optional</em></td><td>Username of the person that created the pep talk.</td></tr><tr><td>search</td><td><code>string</code></td><td><em>optional</em></td><td>A search keyword.</td></tr><tr><td>ordering</td><td><code>string</code></td><td><em>optional</em></td><td><p>The order of returned results.</p><p></p><p><strong>Supported values</strong></p><pre class="language-python"><code class="lang-python"><strong>'created_at', 'updated_at'
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
  'http://api.peptalk.jiwonkwak.co/api/peptalk/?mbti_type=ENTJ&mood=angry&page=2&page_size=3' \
  -H 'accept: application/json' \
  -H 'authorization: Basic aml3b25rd2FrOmF0bGFudGljQ2hhcnRlcjA4MTQ=' \
  -H 'X-CSRFTOKEN: Bp0ZFxJAa77cdttXw8VqodapUt3n9KailNTs5JuaCIq4cTcowpq5o6BZz3I5NY5x'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const message = "hello world";
console.log(message);
```
{% endtab %}

{% tab title="Python" %}
```python
message = "hello world"
print(message)
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

## Get Random Pep Talk by Mood and MBTI

<mark style="color:green;">`GET`</mark> `/api/peptalk/`

Retrieves a random pep talk for a specified MBTI and mood.

### **Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

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
const message = "hello world";
console.log(message);
```
{% endtab %}

{% tab title="Python" %}
```python
message = "hello world"
print(message)
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
  "error": "Invalid request"
}
```
{% endtab %}
{% endtabs %}

***

## Get MBTI-Specific Pep Talk for a Mood

<mark style="color:green;">`GET`</mark> `/api/peptalk/:mbti?mood={mood}`

Retrieves a random pep talk for a specific MBTI and a chosen mood.

### **Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

### **Query Parameters**

<table>
	<thead>
		<tr>
			<th width="99">Name</th>
			<th>Type</th>
			<th>Required</th>
			<th width="363">Description</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>mood</td>
			<td>
				<code>string</code>
			</td>
			<td>
				<em>optional</em>
			</td>
			<td>
				<p>Specifies the emotional state for the pep talk. If omitted, the API returns a random pep talk for the MBTI’s default mood. 
					<br>
						<br>
							<strong>Supported values</strong>
						</p>
						<p>
							<code>sad</code>, 
							<code>angry</code>, 
							<code>annoyed</code>, 
							<code>anxious</code>, 
							<code>frustrated</code>, 
							<code>lonely</code>, 
							<code>depressed</code>, 
							<code>overwhelmed</code>
						</p>
					</td>
				</tr>
			</tbody>
		</table> 

### **Example Request**

```url
GET /api/peptalk/INTJ?mood=sad
```

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

{% tab title="400" %}

{% endtab %}
{% endtabs %}

***

## Retrieve All Pep Talks

<mark style="color:green;">`GET`</mark> `/api/peptalk/all?page={page}&limit={limit}`

Returns all available pep talks.

### **Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

### **Query Parameters**

<table><thead><tr><th>Name</th><th>Type</th><th>Required</th><th width="363">Description</th></tr></thead><tbody><tr><td>page</td><td><code>number</code></td><td><em>optional</em></td><td>Page number (default: 1)</td></tr><tr><td>limit</td><td><code>number</code></td><td><em>optional</em></td><td>Number of results per page (default: 20)</td></tr></tbody></table>

### Example Request

```
GET /api/peptalk/all?page=1&limit=10
```

### Response

{% tabs %}
{% tab title="Success" %}
```
{
  "page": 1,
  "total_pages": 5,
  "data": [
    {
      "id": 12,
      "mbti_type": "ENFP",
      "mood": "excited",
      "pep_eng": "Your enthusiasm is contagious. Spread it wisely.",
      "pep_kor": "네 열정은 전염성이 있어. 현명하게 퍼뜨려."
    }
  ]
}
```
{% endtab %}

{% tab title="Error" %}

{% endtab %}
{% endtabs %}

***

## Add a New Pep Talk

<mark style="color:purple;">`POST`</mark>  `/api/peptalk/add`

Allows users/admins to add a new pep talk entry.

### **Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

### Request Body&#x20;

<table>
	<thead>
		<tr>
			<th width="169">Name</th>
			<th>Type</th>
			<th width="363">Description</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>mbti</td>
			<td>
				<code>string</code>
			</td>
			<td>Page number (default: 1)</td>
		</tr>
		<tr>
			<td>mood</td>
			<td>
				<code>string</code>
			</td>
			<td>Number of results per page (default: 20)</td>
		</tr>
		<tr>
			<td>pep_eng</td>
			<td>
				<code>string</code>
			</td>
			<td>Pep talk in English</td>
		</tr>
		<tr>
			<td>pep_kor</td>
			<td>
				<code>string</code>
			</td>
			<td>Pep talk in Korean</td>
		</tr>
	</tbody>
</table>  

### Example Request

```
{
  "mbti_type": "ISTJ",
  "mood": "overwhelmed",
  "pep_eng": "You don’t have to do everything at once. Prioritize.",
  "pep_kor": "모든 걸 한 번에 할 필요 없어. 우선순위를 정해."
}
```

### Response

{% tabs %}
{% tab title="Success" %}
```json
{
  "message": "Pep talk added successfully!",
  "id": 57
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

## Update a Pep Talk

<mark style="color:purple;">`PUT`</mark>  `/api/peptalk/:mbti/update/:id`

Updates an existing pep talk.

### Example Request

```
PUT /api/peptalk/ENFP/update/45
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

<mark style="color:purple;">`DELETE`</mark>  `/api/peptalk/:mbti/delete/:id`

Deletes a pep talk.

### Example Request

```
DELETE /api/peptalk/ISFJ/delete/20
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
    "error": {
        "code": 404,
        "message": "Page not found."
    }
}
```
{% endtab %}
{% endtabs %}

***

## Search Pep Talks by Keyword

<mark style="color:purple;">`GET`</mark>  `/api/peptalk/search?query={text}`

Searches for pep talks containing a specific keyword in the text.

### Example Request

```
GET /api/peptalk/search?query=confidence
```

### Response

{% tabs %}
{% tab title="Success" %}
```json
{
  "results": [
    {
      "id": 22,
      "mbti_type": "ENTP",
      "mood": "confident",
      "pep_eng": "Confidence is built, not given. Keep moving.",
      "pep_kor": "자신감은 주어지는 게 아니라 만들어지는 거야. 계속 나아가."
    }
  ]
}
```
{% endtab %}

{% tab title="404" %}

{% endtab %}
{% endtabs %}
