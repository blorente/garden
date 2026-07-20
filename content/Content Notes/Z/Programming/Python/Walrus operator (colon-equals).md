---
publish: true
created: 2026-06-23
modified: 2026-06-23T12:44:42.114+01:00
published: 2026-06-23T12:44:42.114+01:00
links:
  - "[[python]]"
sources: https://docs.python.org/3/whatsnew/3.8.html#assignment-expressions
---

# Walrus operator (colon-equals)

The walrus operator (`:=`) means "assign to variable and return the value".

Useful to bind variables in loop conditions:

```
while chunk := fileobj.read(_CHUNK):
      digest.update(chunk)
      size += len(chunk)
      out.write(chunk)
```

Available from Python 3.8: https://docs.python.org/3/whatsnew/3.8.html#assignment-expressions
