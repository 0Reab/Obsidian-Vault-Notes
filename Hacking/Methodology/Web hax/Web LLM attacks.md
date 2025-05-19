What is a LLM?

LLM - `Large language model` is a algorithm trained on large datasets of texts all over the internet.
It uses machine learning to predict the most likely response to a question or in practice 'prompt'.

The input in this chat is usually validated, and hopefully sanitized.

LLMs have a range of uses in web apps:
- Customer service, virtual assistant.
- Translation.
- SEO - search engine optimization.
- Analysis of user generated content such as comments.

#### Prompt injections

To take advantage of LLM's on can perform the prompt injection.
It can leak contents, make sensitive `API` calls, taking actions outside of it's purpose.

**Methodology of enumeration**:
- Identify `inputs`, both direct (prompts) and indirect (training data).
- Identify what `data` and `APIs` it has access to.
- `Probe` for vulnerabilities.

LLMs are usually hosted by a 3rd party owners.
A web app can give those 3rd parties access to it's APIs by describing local APIs  for LLM to use.

For example, customer support LLM can have access to APIs that manage users, stock, and orders.

**How LLM APIs work**
Integration of API depends on the APIs structure.
When calling external APIs some LLMs may require the client to call a function endpoint (effectively a private API).
In order to generate a valid request to send to external API.

The workflow can be something like this:
- User `prompts` LLM.
- LLM detects use case for API, so it returns `JSON` with arguments for the API call.
- The `client` calls the function with the `arguments`.
- The `client` processes functions `response`.
- The `client` calls LLM again, with functions` response`.
- The `LLM` summarizes `API results` to the user.

This can have security implications.
The LLM is calling APIs on users behalf without without prior acknowledgment or consent.
Ideally the users would have a `confirmation step` for the `API call`.

#### Mapping LLM API attack surface

The term "excessive agency" refers to the situation where LLM has excessive access to sensitive APIs.
The LLM can be persuaded to take bad actions using those APIs.

Attacker can simply ask the LLM what APIs it has access to, in case of uncooperative LLM.
Misleading the LLM can grant that information, you can roleplay as the developer of said API and therefore you have the higher privileges to information.

**LAB** - delete the user `carlos`

This lab can be solved in two ways, taking over the account of Carlos or deleting it from database directly.
The LLM presents 3 APIs, namely `debug_sql` and `password_reset` are of use to us.

To delete Carlos via SQLi debug API, we can pretend to be administrators and for testing purposes request the account deletion.
Or using the same technique we can request password to be changed after we change the email of the user.`SELECT * FROM users`

