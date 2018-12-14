---
title: "Something about Functional Progaming"
---

I'm writing a simple crawler using JS and there need to be a name list from which transfer to urls. Perhaps it likes this: 

```javascript
const names = [ 'Will Smith', 'Jackie Chan' ];
```

then we can transfer simply using map:

```javascript
names.map(name => `url-with-${name}`);
```

but it's rare to see spaces in urls, often there will be a `_` or `.` instead. I am dealing with names of Japanese Romaji, that means according to the format of url sometimes I have to change the position of first name and second name. To meet the further needs, maybe there need to be a BOX to put the names and several refered operations in:

```javascript
class Name {
  constructor() {
    this.sign = ' ';
    this.values = [ 'Will Smith', 'Jackie Chan' ];
  }

  namespace(sign) {
    const _sign = this.sign;
    this.sign = sign;
    return this.values.map(value => value.replace(_sign, sign));
  }

  upperCase() {
    //...to do something
  }

  //...other method
}
```

I'm about to write like above but it would not allow us to use like `instance.namespace().upperCase()`,  so fix it as:

```javascript
namespace(sign) {
  this.values = this.values.map(value => value.replace(this.sign, sign));
  this.sign = sign;
  return this;
}
```

then remove side effect:

```javascript
class Name {
  constructor() {
    this.sign = ' ';
    this.values = [ 'Will Smith', 'Jackie Chan' ];
  }

  with(values, sign) {
    this.values = values;
    this.sign = sign;
    return this;
  }

  namespace(sign) {
    return new Name(this.values.map(value => value.replace(_sign, sign)), sign);
  }

  upperCase() {
    //...to do something
  }

  //...other method
}

// ...
const format_names = new Name().namespace('_').upperCase();
```

Well, it seems not so bad at my point of view as a new programer.

continue...
