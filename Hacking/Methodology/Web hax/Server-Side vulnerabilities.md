#### Path traversal

Covered in detail in `Path traversal file and labs`

# Access control

Determines who or what is authorized to preform actions or access files/data.
It is dependent on authentication and session management.

- Authentication confirms that the user is who they say they are.
- Session management assures requests sent are made by the same user.
- Authorization ensures the user can carry out the action they are attempting.

Broken access controls are common critical security vulnerability.
These systems are intricate and designed by humans which makes them prone to errors.
It is an complex and dynamic process.


#### Vertical privilege escalation

If an user can gain access to functionality  they were not mean to have this is called vertical privilege escalation.
For example if user gains access to an admin panel and thus the privilege to delete other users.

#### Unprotected functionality

For an basic example, unprotected admin panel which can be accessed by a normal user.
Even if that URL is not disclosed anywhere, brute force attacks might reveal it or a robots.txt file might disclose it.

In that case the admin panel does not have any protection for sensitive functionality thus a regular user can escalate privileges vertically.

**LAB** - Delete user `carlos` - robots.txt discloses `/administrator-panel` and delete user.

**Security thru obscurity**
In this example...
We have an admin panel that is obscured and not secured by having an URL to the panel not predictable.
`administrator-panel-yb556`

This is just hidden, but not secured so any user can access it via that URL path.
However an app might still leak the URL in some way.
For example in HTML we have javascript.

```javascript
<script> 
var isAdmin = false;
if (isAdmin) { ... var adminPanelTag = document.createElement('a'); 
adminPanelTag.setAttribute('href', 'https://insecure-website.com/administrator-panel-yb556');
adminPanelTag.innerText = 'Admin panel'; ... }
</script>
```

This script checks if the user is admin, if so the page will have an element (link tag) that will lead to the admin panel URL.
But if we just simply take the URL from the javascript we will end up on that URL regardless.

**LAB** - find admin panel - `/admin-bckcw2` it was client side JS - keep in mind it will be plain white text, no HTML syntax highlighting or whatevs.

#### Parameter based access control

Some applications utilize parameters for access control.
These parameters can be in form of:
- hidden field.
- cookie.
- preset query string parameter.

example:
`https://website.com/login/home.jsp?admin=true`
`https://website.com/login/home.jsp?role=1`

This is insecure because an user can modify the value.

**LAB** - delete user by accessing admin panel, modifying the cookie key - admin, value - true.
- note to self, editing cookies can be done in `storage` section in dev tools, but not in network or others.

#### Horizontal privilege escalation

If an user has the ability to access information or functionality of another user, that is called horizontal privilege escalation.
For example if an employee can access records of other employees.

This type of privilege escalation uses similar exploits to it's counter part, vertical priv esc.
For example utilizing IDOR vulnerability (insecure object reference) to modify an url parameter and access data in this case `id` parameter.
`https://website.com/myaccount?id=123`

In some cases, the application will have exploitable parameter such as `id` but it will not have a predictable value.
That value could be a `GUIDs` (globally unique identifiers).
These identifiers are not guessable most of the time, but they could be disclosed somewhere else, for example messages or reviews.


**LAB** - find the API key of user carlos, API key is accessible for your account on your profile page, found post made by carlos, found his GUID in source of the page and sent request on profile page with newly obtained GUID instead of mine.

More often than not, horizontal privilege escalation can turn into vertical the only difference being the attack target is an user with administrative rights rather than regular user.

**LAB** - vulnerable ID parameter with password disclosure.
Profile page parameter is just plain username, thus administrator profile is named as such.
On each profile page password field is populated with masked password, it can be obtained via viewing source.

#### Authentication vulnerabilities

Auth vulns are usually easy to understand but they are critical as well.
So, in this section the following will be covered:

- Common auth methods.
- Potential vulns in those methods.
- Inherent vulns in different auth methods.
- Typical vulns that are introduced by improper implementation.
- Hardening auth methods.

Difference between authentication and authorization.
Authentication verifies that the user is who they claim to be.
Authorization allows user to have access to functionality or information.

#### Brute force attacks

is when an attacker utilizes a method of attempting logins with credentials originating from either wordlists or raw bruteforcing of character increments.
This attack attempts logins in quick succession, and if the application does not have a rate limiting against this attack attacker might gain access to users/admin accounts.

Brute forcing usernames is easier part, usually they conform to a standard such as `firstname.lastname@somecompany.com`.
Even if most of the users do not comfort the standard more often than not important accounts such as administrator and IT will do.
During auditing, check if the website discloses usernames/emails by checking their profile.
Even if actual contents of the profile is hidden, name used in profile is often the same as username, also check http responses for email disclosure.

