#AJAX get request
``` javascript
$.get(' some url ', function(data){


  });
  ```

  the function part is callback - won't do something until the first part is finished.  Waits until data returned before it does something with the data

  anonymous function

  ``` javascript

  console.log("hi");
  $.get(' some url ', function(data){
  console.log("there")
  console.log(data)

    });
    console.log("here");

    ```

results will be

```
"hi"
"here"
"there"
```

it attaches the function in the get request to an event - request coming back

page loads

when gets an event watcher, it puts to the side in a watch list and saves the response in a messages Q.  But it won't print this Q until the rest of the page has loaded

1. execute code
2. look at Q
3. execute code
4. look at Q

is the event loop
