---
author: Rajesh Venkatesan
comments: true
date: 2024-02-12 00:00:00+00:00
layout: post
link: http://flowphysics.com/numpy-arrays-in-pydantic/
slug: numpy-arrays-in-pydantic
title: Numpy arrays in Pydantic
tags: numpy arrays pydantic fastapi
---

Recently, I have been developing a [FastAPI](https://fastapi.tiangolo.com/){:target="_blank" rel="noopener"} application which relies on the excellent [Pydantic](https://docs.pydantic.dev/latest/){:target="_blank" rel="noopener"} package for data validation and serialization.  

I will give a very short intro, in case you are not familiar with Pydantic. If we develop our Python class objects as derived from Pydantic's BaseModel class, then we can have very helpful things like type validation, type hinting, JSON data serialization and [so on](https://docs.pydantic.dev/latest/#why-use-pydantic){:target="_blank" rel="noopener"} for our class.  e.g.

{% highlight python %}
from pydantic import BaseModel
from typing import List

class Book(BaseModel):
    id: int  
    name: str
    author: str
    subject: List[str]

book_data = {'id': 12,
             'name': "What is Life?",
             'author': "Schrodinger, Erwin",
             'subject': ["physics", "biology"]}
my_book = Book(**book_data)  

print(my_book.id)  
#> 12
print(my_book.model_dump())  
"""
{
    'id': 12,
    'name': "What is Life?",
    'author': "Schrodinger, Erwin",
    'subject': ["physics", "biology"]
}
"""
{% endhighlight %}

And this plays nicely with [ORM](https://en.wikipedia.org/wiki/Object%E2%80%93relational_mapping){:target="_blank" rel="noopener"} as well. 

If you are dealing with scientific or numerical data in Python, naturally you will use Numpy arrays. But how to handle Numpy arrays within Pydantic BaseModel?

### Numpy array as an 'Annotated' type

We can define a custom type for our Numpy arrays using the `Annotated` type. This will wrap around Numpy's original [`ndarray`](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.html){:target="_blank" rel="noopener"} class. But we need to provide two additional things:

1. A function to convert a provided string input to Numpy array - a before validation method
1. A function to serialize a provided Numpy array into List/string - a custom serialization method

Sample code for this custom datatype `MyNumPyArray` creation is given below:  

{% highlight python %}

import numpy as np
from pydantic import BaseModel, Field, BeforeValidator, PlainSerializer
from typing import Annotated
import ast

def nd_array_before_validator(x):
    # custom before validation logic
    if isinstance(x, str):
        x_list = ast.literal_eval(x)
        x = np.array(x_list)
    if isinstance(x, List):
        x = np.array(x)
    return x

def nd_array_serializer(x):
    # custom serialization logic
    return x.tolist()
    # return np.array2string(x,separator=',', threshold=sys.maxsize)

MyNumPyArray = Annotated[   np.ndarray,
                            BeforeValidator(nd_array_before_validator),
                            PlainSerializer(nd_array_serializer, return_type=List),
                        ]

# Remember to add 'model_config = ConfigDict(arbitrary_types_allowed=True)' to the model class when using MyNumPyArray
{% endhighlight %}  

Now, you can include `numpy` arrays in Pydantic classes as given below:

{% highlight python %}
from pydantic import BaseModel, ConfigDict

class SomeClass(BaseModel):
    """
        Sample class that has a Numpy array field
    """
    name: str
    data: MyNumPyArray

    model_config = ConfigDict(arbitrary_types_allowed=True)


# Testing
sample_data = np.array([[1,2],[3,4]])
test_instance = SomeClass(name="Test", data=sample_data)

print(test_instance.model_dump())
#> {'name': 'Test', 'data': [[1,2], [3,4]]}

{% endhighlight %}  

### References and notes:

1. [https://github.com/pydantic/pydantic/issues/7017#issuecomment-1670142686](https://github.com/pydantic/pydantic/issues/7017#issuecomment-1670142686){:target="_blank" rel="noopener"}
1. You can use [`pydantic.dataclasses`](https://docs.pydantic.dev/latest/concepts/dataclasses/){:target="_blank" rel="noopener"} if you only require data validation from Pydantic for Numpy arrays. See, for example, [https://stackoverflow.com/questions/70306311/pydantic-initialize-numpy-ndarray](https://stackoverflow.com/questions/70306311/pydantic-initialize-numpy-ndarray){:target="_blank" rel="noopener"}
