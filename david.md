---
layout: default
---

Text can be **bold**, _italic_, or ~~strikethrough~~.

## David Page

# Blogs of other devs

https://twitter.com/jkup/status/1105225154332786688

```
The final element.
```

Javascript stuff

```var x = (a, b=()=>a) => {
  var a = 2;
  return [a, b()];
}

console.log(x(5)); // [2,5]

// this is an example of 2 different a's because
// the a in the parameter has its own execution context
```

```

function Foo(name){
    this.name = name  // Cannot set property 'name' of undefined
}

Foo.prototype.myName = function(){
    return this.name
}

function Bar(name){
  console.log('zzz ', this);
    Foo.call(this, name)   // Cannot set property 'name' of undefined
}

Bar.prototype = Object.create(Foo.prototype)
const a = new Bar('tyler')
console.log(a.myName())  // tyler

Foo.prototype.hello = function() {
  return 'hello there';
}

console.log('hello is ',a.hello())  // 'hello there'

function test() {
  console.log(this);
}

test();

```
