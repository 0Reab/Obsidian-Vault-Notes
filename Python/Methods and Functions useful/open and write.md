```python
fw = open('sample.txt', 'w')
fw.write('Writing some stuff in my file\n')
fw.write('I like bacon\n')
fw.close()
```

```python
fr = open('sample.txt', 'r')
text = fr.read()
print(text)
fw.close()
```