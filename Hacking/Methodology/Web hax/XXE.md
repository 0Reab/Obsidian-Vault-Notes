Is a vulnerability that can occur within the web apps that use XML data.

Here is an example of xml data:
(You can think of it as folders)

![[Pasted image 20241205174350.png]]

It has some similarities to html tags.
And It utilizes DTD (line 8) to determine what data is expected.

Now the important bit.

There are external xml entities (XXE).
*Which are placeholders that are used for repeating data or for external references.*

And we are interested in that external part.
Possibly trying to reference something that we shouldn't have access to.
A good candidate payload for PoC is good old /etc/passwd file.

![[Pasted image 20241205175136.png]]

In this example the entity thmFile is an entity reference to passwd file.
Which is referenced via &ext (that's a mistake it should be &thmFile).

