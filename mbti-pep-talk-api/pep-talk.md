---
icon: code
description: >-
  성격 유형과 감정 상태에 따른 조언을 해주는 MBTI 조언(Pep Talk) API입니다. 조언을 불러오거나 추가, 삭제, 수정할 수
  있습니다.
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

# 레퍼런스

## 조언 가져오기

<mark style="color:green;">`GET`</mark> `/api/peptalk/`

다양한 필터 조건을 적용하여 조언을 가져옵니다.

### **헤더**

| 이름            | 값                  |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

### **쿼리 파라미터**

<table>
	<thead>
		<tr>
			<th width="128">이름</th>
			<th>타입</th>
			<th width="102">필수 여부</th>
			<th width="363">설명</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>mbti_type</td>
			<td>
				<code>string</code>
			</td>
			<td>
				<em>optional</em>
			</td>
			<td>16가지 MBTI 유형입니다.</td>
		</tr>
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
							<strong>지원하는 값</strong>
						</p>
						<p>
							<code>sad, angry, annoyed, anxious, frustrated, lonely, depressed, overwhelmed</code>
						</p>
					</td>
				</tr>
				<tr>
					<td>username</td>
					<td>
						<code>string</code>
					</td>
					<td>
						<em>optional</em>
					</td>
					<td>조언을 생성한 유저 이름</td>
				</tr>
				<tr>
					<td>ordering</td>
					<td>
						<code>string</code>
					</td>
					<td>
						<em>optional</em>
					</td>
					<td>
						<p>결과 반환 순서</p>
							<strong>지원 하는 값</strong>
						<br><br>
              <p>
							<code>'created_at', 'updated_at'</code>
              </p>
					</td>
				</tr>
				<tr>
					<td>page</td>
					<td>
						<code>integer</code>
					</td>
					<td>
						<em>optional</em>
					</td>
					<td>반환되는 페이지 수 (Default: 1)</td>
				</tr>
				<tr>
					<td>page_size</td>
					<td>
						<code>integer</code>
					</td>
					<td>
						<em>optional</em>
					</td>
					<td>반환되는 페이지 별 항목 개수 (Default: 10)</td>
				</tr>
			</tbody>
		</table>

### **요청 예시**

```url
http://api.peptalk.jiwonkwak.co/api/peptalk/?mbti_type=ENTJ&mood=angry&page=2&page_size=3
```

### 요청

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

### **응답**

{% tabs %}
{% tab title="200" %}
```json
{
  "mbti": "INTJ",
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

## MBTI와 기분에 따라 무작위 조언 가져오기

<mark style="color:green;">`GET`</mark> `/api/peptalk/random`

전달된 MBTI와 기분에 따른 무작위 조언 하나를 반환합니다.

### **헤더**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

### **요청**

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

### **응답**

{% tabs %}
{% tab title="200" %}
```json
{
    "id": 32,
    "mbti_type": "INTJ",
    "mood": "anxious",
    "pep_eng": "Breathe. Break it into steps. Anxiety hates clarity. 💨",
    "pep_kor": "숨 깊게 쉬고, 단계를 나눠봐. 불안은 명확함을 싫어하거든. 💨",
    "owner": "jiwonkwak",
    "created_at": "2025-01-26T02:40:45.495599Z",
    "updated_at": "2025-01-26T02:40:45.495618Z"
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

## ID로 조언 가져오기

<mark style="color:green;">`GET`</mark> `/api/peptalk/:id/`

특정 ID의 조언을 반환합니다.

### **헤더**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

### **요청**

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

### **응답**

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

## 조언 추가하기

<mark style="color:purple;">`POST`</mark>  `/api/peptalk/`

본문으로 파라미터를 전달하여 새로운 조언을 추가할 수 있습니다.

### **헤더**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

### 본문

<table>
	<thead>
		<tr>
			<th width="169">이름</th>
			<th>Type</th>
			<th width="363">설명</th>
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
			<td></td>
		</tr>
		<tr>
			<td>pep_kor</td>
			<td>
				<code>string</code>
			</td>
			<td></td>
		</tr>
		<tr>
			<td>user_id</td>
			<td>
				<code>integer</code>
			</td>
			<td></td>
		</tr>
	</tbody>
</table>

### 요청

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

## 조언 수정하기

<mark style="color:purple;">`PUT`</mark>  `/api/peptalk/:id/`

이미 추가된 조언을 일부 또는 전부 수정합니다.

### Example Request

```
PUT /api/peptalk/482/
```

### Request Body

```json
{
  "pep_kor": "너는 절대 혼자가 아니야—너의 머릿속은 이미 하나의 모험이야! 🚀✨ 누군가와 대화 나누고, 새로운 아이디어를 내봐! 세상은     너의 열기    를 기다리고 있어! 💡🔥"
}
```

### Response

{% tabs %}
{% tab title="Success" %}
```json
{
    "id": 479,
    "mbti_type": "INTJ",
    "mood": "sad",
    "pep_eng": "You're a mastermind, a strategist, a force to be reckoned with. Sadness is just a temporary glitch in your grand design. You've conquered bigger challenges, and this? This is just another data point in your journey to greatness. Feel it, process it, and then recalibrate. The world doesn't move without minds like yours. Keep going. REMOVE ME LATER",
    "pep_kor": "너는 절대 혼자가 아니야—너의 머릿속은 이미 하나의 모험이야! 🚀✨ 누군가와 대화 나누고, 새로운 아이디어를 내봐! 세상은 너의 열기를 기다리고 있어! 💡🔥",
    "owner": "ronald",
    "created_at": "2025-02-15T09:49:28.449252Z",
    "updated_at": "2025-02-25T12:08:29.515073Z"
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

## 조언 삭제하기

<mark style="color:purple;">`DELETE`</mark>  `/api/peptalk/:id/`

등록된 조언을 삭제합니다.

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
    "detail": "Not found."
}
```
{% endtab %}
{% endtabs %}

***
