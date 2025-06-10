https://kontent.ai/
scope - `*.kontent.ai`
X-BugBounty: reab

---

#### API Scope

Following systems and services are in the scope:  
- Kontent.ai client → https://app.kontent.ai  
- Management API v2 → https://manage.kontent.ai/v2  
- Deliver API → https://deliver.kontent.ai  
- Delivery GraphQL API → https://graphql.kontent.ai  
- Presentation website → https://kontent.ai  
  
Including public SDKs on GH:  
- https://github.com/kontent-ai/data-ops  
- https://github.com/kontent-ai/rich-text-resolver-js  
- https://github.com/kontent-ai/delivery-sdk-js  
- https://github.com/kontent-ai/management-sdk-js  
- https://github.com/kontent-ai/management-sdk-net  
- https://github.com/kontent-ai/migration-toolkit

#### Domains

app.kontent.ai
brand.kontent.ai
karma.demo.kontent.ai
engage.kontent.ai
feedback.kontent.ai
gpt-chat.kontent.ai
login.kontent.ai
ficto-healthtech.sample.kontent.ai
ficto-imaging.sample.kontent.ai
ficto-surgical.sample.kontent.ai
getting-started.sample.kontent.ai
kickstart.sample.kontent.ai
status.kontent.ai
tracker.kontent.ai
trustcenter.kontent.ai

#### Google dorking

Useful resources:
https://github.com/kontent-ai


#### feedback forms

feedback.kontent.ai

Sensitive data?
```json
"users":[
{"id":997,"v":1748260292766223,"name":"Martin Michalik","gravatarHash":"b772955a7de516c84d3da7abea646765"}, 
{"id":3217,"v":1727794592538621,"name":"Tomas Hruby","gravatarHash":"dc2e4da272d300eed1056700409a3d21"},
```

Feedback data.
```json
"id":"121b1447-0e2a-41ca-a64e-d5b7ebe76fec",
"v":1749082351301973,
"ownerId":3217,
"featureId":798995,
"name":"Checking for element uniqueness",
"description":"yappington",
"imageUrl":null,
"humanId":174,
"slug":"174-checking-for-element-uniqueness",
"updatedAt":"2025-06-05T00:12:31.301973+00:00","createdAt":"2022-02-25T09:07:56.766382+00:00",
"portalVotesCount":13,"imageVerticalOffset":0,"contentMigrated":true
```

```http
POST /api/portal_votes HTTP/1.1
Host: feedback.kontent.ai
```
```json
// request

{"portal_vote":
	{"importance":1,
	"text":"test",
	"email":"atkeratker905@gmail.com",
	"portal_card_id":null
	}
}
```
```json
// response

{"portal_vote":
	{"id":"ae4d619e-fbb4-445d-842a-5a0a8b5d4889",
	"v":1749574992651811,
	"portal_id":"8708642c-e3dc-45f7-b837-a16aaeb6ba32",
	"importance":1,
	"text":"test",
	"portal_user_id":null,
	"portal_card_id":null,
	"created_at":"2025-06-10T17:03:12.651811+00:00",
	"status":0}}
```

Test this XSS in body of post `"</span>`.

#### APIS

```json
"sign_in_pardot_tracker_url_dev":{"type":"text","name":"Sign in Pardot tracker URL (Development)","value":"https://tracker.devkontentmasters.com/dcjs/849473/354/dc.js"},"join_pardot_tracker_url_dev":{"type":"text","name":"Join Pardot tracker URL (Development)","value":"https://tracker.devkontentmasters.com/dcjs/849473/352/dc.js"},"sign_in_pardot_tracker_url_prod":{"type":"text","name":"Sign in Pardot tracker URL (Production)","value":"https://tracker.kontent.ai/dcjs/849473/354/dc.js"},"join_pardot_tracker_url_prod":{"type":"text","name":"Join Pardot tracker URL (Production)","value":"https://tracker.kontent.ai/dcjs/849473/352/dc.js"
```

GET /dcjs/849473/354/dc.js?v=1749578402668 HTTP/1.1
Host: tracker.kontent.ai

GET /111e467b-6008-006d-7da0-df9b9e015029/items?system.codename=_in__the_ultimate_guide_to_modernizing_content_ope HTTP/1.1
Host: deliver.kontent.ai

https://login.kontent.ai/authorize?client_id=kk5POR5ME2uYwFQWovQ32n61Z5MwDWAH&scope=openid&audience=https%3A%2F%2Fapp.kenticocloud.com%2F&redirect_uri=https%3A%2F%2Fkickstart.sample.kontent.ai%2Fcallback&responseType=token+id_token&prompt=true&response_type=code&response_mode=web_message&state=T1BHVXV3UlFMSmhFQlVubmVySGFWLnRRR05wbWlwODhkYzM5TDZrbC1qQw%3D%3D&nonce=VTNOa0luc1BNSDRsYkpzN2poU25ZWEh0S1Z0eFhwOVJxY3Vhd1p4eVl0dA%3D%3D&code_challenge=C999gSzMvSi8micCCxYj06S25hHW-tdXZXR2DnXk4-4&code_challenge_method=S256&auth0Client=eyJuYW1lIjoiYXV0aDAtcmVhY3QiLCJ2ZXJzaW9uIjoiMi4zLjAifQ%3D%3D

https://Fkickstart.sample.kontent.ai/callback

Host: kickstart.sample.kontent.ai
GET /callback?error=unsupported_response_type&error_description=Unsupported%20response%20type%3A%200%22%3Ealert(1)&state=T1BHVXV3UlFMSmhFQlVubmVySGFWLnRRR05wbWlwODhkYzM5TDZrbC1qQw%3D%3D