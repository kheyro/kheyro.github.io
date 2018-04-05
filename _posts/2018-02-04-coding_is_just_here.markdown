---
layout: post
title:      "Handling error in Promise with React/Redux"
date:       2018-02-04 17:28:24 -0500
permalink:  coding_is_just_here
---


While working on the React Redux Rails final portfolio I came accross a simple issue, how to properly handle error from a `Promise`.  I am not talking about errors raising from promises not being able to resolve but handling errors coming from the response, ie: posting invalid data to your `API` which result in a status code > 300, and an errors message in the form of a json object. 

Let's say you are posting to your API in your React Redux app with an invalid data from your form, `name` is empty. API is suppose to throw an error along with a `http status code 406` and a JSON object ie: `{ error: ["error message 1", "error message2"]  }` 

Your `Promise` (in your action creators) looks like this (coupled with `redux-thunk`)

```
return (dispatch) => {
    dispatch(actionIsLoading(true));
    return fetch('/my-api-url', {
        body: myFormValue,
        method: 'POST'
      })
      .then(res => res.json())
      .then(data =>
          dispatch(actionIsLoading(false));
          return dispatch({ type: ACTION_TYPE, payload });
        }
      })
      .catch(err => myErrorFunction(err))
  }
}
```

You would think that it would be natural to handle error in the `cath()` method, but method will only be executed if there is an issue with resolving the `Promise`. Yet, the promise did resolve it reached the server and you got a valid `http status code` in headers and a `JSON object` as a response, hence `catch` won't be called.

You could test for the exsitence of a key `error` in the object response like so : `if (data.data.error) { ... }` but to me this solution doesn't seem elegant as we could that error in a nicer way on the first `then()` call.

In the first `then` the `res` variable receives the headers of the response, this res is an object which lot of useful information and notable the `ok` key. It is assign a `boolean` value if the `http status code` is valid, that is to say if the http status code is between 200 and 300. 

```
// first then()
 .then(res => {
     if (!res.ok) {
		  // do this
			}
     return res.json()
})

```

Problem is, you still want the `error object` (sent by the server) in order to display it, hence you need to `return res.json()` and chain your second `then()`. Why? because `json()` is also a `promise` (try console.logging res.json()), while `res` only give you the header first it still needs to load the body. We need a way to pass along the response header and its body to the next `then` or another method that could handle both.

### Solution 1, nested then()

It would be to chain another `.then()` to the `json()` method and return an object which contains the response header `res` and the `json()` response.

```
// first then()
 .then(res => 
     res.json().then(data => {
		     res: res,
				 body: data
		})
)
.then(data =>
    if (!res.ok) { 
		   // dispatch with your body.error 
			 }

```

So here we pass along the header response and its body, do that we can then dispatch an action to the reducer, update ..., and display error on the view. It is quite understandable and works well.


### Solution 2, Promise.all

Another solution is to use the `Promise.all` method, it takes in an `Array` as argument, and will resolve all of the promise in the array or rejected with the first one that is rejected. We could pass in both `res` and `res.json()`. `res` not being a promise won't be counted and just return its value, you can try console.logging `Promise.resolve('5')`
The return value of `Promise.all` is an `array` of response.

```
// first then()
.then(res => Promise.all( [ res.ok, res.json() ] ))
.then([ res, data ] =>
    if(!res) {
		   // dispatch with your data.error 

```

I like this approach as it flows well, it doesn't "break" like the with the nested `then()`


### Solution 3, catch()

We could also take advantage of the the method `catch()`. We could purposedly throw an error so that we could jump directly to the `catch()` method, and handle error there.


```
// first then()
.then(res => 
    if (res.ok) {
		    throw new Error(res)
		}
.then(data => 
    // if no error )
.catch( err => 
    // handle error)
```

if the response hits `throw new Error` it will then jump to the `catch` method where we can then handling whatever action we want to. It is not the most elegant it does give a clear separation of concern.


There are more solutions, but I felt like those ones are simple and straightforward. I hope it helps you in your project! If you want to go further and look for something even cleaner, have a look at `async`/`await` function.


