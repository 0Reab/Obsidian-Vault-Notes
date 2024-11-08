If the user has access to a path containing python interpreter.
It can be utilized for escalating privileges by spawning a shell using python code and running it thru.

Example path: /usr/bin/python3.7

```shell
pythonX -c 'import sys; print("\n".join(sys.path))'
```

https://medium.com/analytics-vidhya/python-library-hijacking-on-linux-with-examples-a31e6a9860c8
