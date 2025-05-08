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

