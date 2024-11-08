```python
from urllib import request

url = 'http://xd.txt.com'

def download_file(url):
	response = request.urlopen(url)
	csv = response.read()
	csv_str = str(csv)
	lines = csv_str.split("\\n")
	dest_url = r'googl.csv'
	fx = open(dest_url, "w")

	for line in lines:
		fx.write(line + "\n")
	fx.close()

download_file(url)
```