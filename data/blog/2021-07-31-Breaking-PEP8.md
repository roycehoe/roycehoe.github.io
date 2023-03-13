---
title: 'Breaking conventions: PEP8'
tags: [Python]
date: '2021-02-01'
draft: false
summary: 'Why I disagree with the PEP8 way of checking empty lists'
---

# Breaking conventions: PEP8

So there is a PEP8 convention that I unapologetically break. That would be PEP8’s recommended way on checking if a list is empty or not.

In this code snippet in the PEP8 documentation, here are the recommendations:

```python
# Correct:
if not seq:
if seq:

# Wrong:
if len(seq):
if not len(seq):
```

But here is how I write it instead:

```python
if seq == []:
```

---

Firstly, he reasons given for the `# Wrong` way of writing this conditional statement is perfectly valid. Lists are sequences, and empty sequences are false. So, the examples illustrated by `# Wrong` would take more time to compute than the examples in `# Correct`.

Perfectly valid reasoning that I disagree with because, `Explicit is better than implicit`, says The Zen of Python. Now back to the code.

---

```python
if seq == []:
```

I like explicit checks. Here, it is explicit that `seq` is of type list, and not of any other type. Armed with that information, it would be easier for any code maintainer to easily find bugs in my code, should I, say, absent-mindedly try to concatenate `seq` with a number.

Now let me criticize the example given by PEP8 instead:

```python
# Correct:
if not seq:
if seq:
```

At a glance, it is not clear what data type is `seq`. It could be an empty string, the number zero, the float zero, an empty list, an empty dictionary, an empty tuple, an empty set, an empty bytes etc. The type of `seq` cannot be inferred and must be inferred by looking at the rest of the code.

As a general rule of thumb, the more you need to scroll through a code base, the less explicit the code base is about its intentions.

---

There are also slightly more personal reasons why I would prefer explicitly checking.

As a full-stack developer, I have to juggle between two languages: Python and Javascript, each with it’s own unique quirks: empty lists in Python is `Falsey`, but empty lists in Javascript is `Truthy`.

Explicit checking helps me to switch between these two languages easily.

# Conclusion

It is my personal philosophy that a well designed system is a system that takes into account the most incompetent operator. And it does so by making the ambiguous clear. Because different languages handle `falsey` and `truthy` values differently, there should be a convention to explicitly check `truthy` or `falsey` values.
