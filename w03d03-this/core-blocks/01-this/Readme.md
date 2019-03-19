## Core Blocks

In this challenge,You will be learning about `this` keyword

## Release 0:

Try to understand what the following program does and identify the binding 

```js
var friend = {
    firstName: "Ash",
    sayHi: function(){
        return this.firstName + " says hello!";
    }
};

friend.sayHi(); 

var dog = {
    name: "Whiskey",
    sleep: function() {
        return this.name + " is sleeping: zzzzz...."
    }
}

dog.sleep(); 

var nested = {
    number: 1,
    anotherObject: {
        anotherNumber: 2,
        whatIsThis: function() {
            return this;
        }
    }
}

nested.anotherObject.whatIsThis();
```

## Release 1:

* What does this function output? Why?
```js
function logThis(){
    return this;
}

logThis(); // ?
```

## Release 2:

* What does this function output? Why?
```js
var instructor = {
    firstName: 'Tim',
    info: {
        catOwner: true,
        boatOwner: true,
        displayLocation: function(){
            return this.data.location;
        },
        data: {
            location: "Oakland"
        }
    },
}

instructor.info.displayLocation() // ?
```

## Release 3:

* What does this function output? Why?
```js
var instructor = {
    firstName: 'Tim',
    info: {
        catOwner: true,
        boatOwner: true
    },
    displayInfo: function(){
        console.log("Cat owner? " + this.catOwner);
    }
}

instructor.displayInfo() // ?
```


