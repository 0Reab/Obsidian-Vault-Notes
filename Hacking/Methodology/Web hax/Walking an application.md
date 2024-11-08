Using your browser to manually go thru an application.

Here is a list of tools that you can use in your browser.

- View source - display source html web page code
- Inspector - make changes to how content is displayed
- Debugger - Inspect and control how JS is being executed
- See all network request the page makes

As a pentester your job is to look for possible ways of how this particular application can be hacked.
So that means looking for ways you can interact with it, that includes input fields all the way looking at JS and everything in between.


Looking at the source code.
You can find, plugins/frameworks/CMS (content management system) versions.
HTML comments from the developers that contain sensitive data.
Links to other directories of the web app.

Inspecting the pages to remove something can be done via finding the which div element is the one we are looking to remove.
And in it's style replacing the display parameter from block; to none;

Debugger, as any other has breakpoints that can be set on a particular line of code of your choosing.
This way you can control the code flow and do your magic.

Network tools.
As mentioned, closely monitor what requests or actions a webpage is doing in regards to networking. (communicating)

