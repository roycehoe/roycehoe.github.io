---
title: 'Classes: A wrapper for Structs'
tags: [Python, Pydantic, OOP]
date: '2021-02-01'
draft: false
summary: 'Strong typing with Python'
---

# Introduction

One of the best and worst things about Python is, it is not a strongly typed language.

At first this might seem like a feature. You can get thing up and running really quickly. Without the need to think about typing, it means that you need less code to get the same amount of work done.

However, prototypes do not stay prototypes forever. Soon, prototypes grow to into large complex systems. If you were working with 1 schema from 1 database, try 5. If you only had 1 API that returned 1 object, try having 10 APIs. Soon enough, you would start to lose track of the schema of the data you are sending and receiving.

---

Now, in other languages, you have structs. Structs define the shape of data. And now this is where things start to get cool.

The majority of functions we write is to manipulate data. The most rudimentary function would take in some data, process it in some way, and return some data. To break it down, functions typically do this:

Receive input → Manipulate input → Return output

And if we can define the input and output type, and somehow tell our computer that for this function, this MUST be the input/output type, our computer would be able to tell us if our function is indeed manipulating the input in the right way. How, you might ask?

My answer is, static type checking.

---

# Static type checking

```python
user_data = {"id": 123, "name": "Mr Incognito"}

def get_id(user_data: dict) -> int:
	return user_data["id"]
```

Let’s say your function retrieves user data. You manipulate the input to get user id. And you return `user_id`. Simple enough. But what happens if your function receives a dictionary that does not contain `id`?

```python
bad_data = {"foo": 123, "bar": "Mr Incognito"}

def get_id(user_data: dict) -> int:
	return user_data["id"]
```

Now your function would throw a key error. But what if we were to guarantee the shape of the object `get_id` receives? That is to say, we annotate our function to say that the object that it receives will always contain `id`? By doing so, so long as the function receives said object, it would always be able to retrieve `id` and return it. Let us do this:

```python
user_data = {"id": 123, "name": "Mr Incognito"}

class UserData:
	def __init__(self, id: int, name: str):
			self.name = name
			self.id = id

def get_id(user_data: UserData) -> int:
	return user_data.id
```

Great! Now we can guarantee that, so long as this function receives a UserData object, it would be able to return an `id` value.

But wait? This function is still flawed. It is unable to guarantee that the return object is an integer. What if, some user decides to funk around with my UserData class and do something like this:

```python
user_data = {"id": "I am a bad user", "name": "Mr Incognito"}

class UserData:
	def __init__(self, id: int, name: str):
			self.name = name
			self.id = id

def get_id(user_data: UserData) -> int:
	return user_data.id

data = UserData(**user_data)
id = get_id(data)

print(id) # output: "I am a bad user"
```

`id` is now a string. So we would need to guarantee its type somehow. You can do so by using the inbuilt TypedDict or dataclass module, or downloading external libraries like Pydantic to help you. Either way, with the type guaranteed, and with the object shape guaranteed, you are guaranteed that the `get_id` function will always return the correct data, with the correct data type.

---

# Closing thoughts

Python does not have inbuilt structs. However, there are ways to implement structs through the use of classes, and extending classes with certain modules.

Ultimately, typing the shape of your object makes it more difficult for bugs to hide, since your static type checker would identify type-related bugs in your code.
