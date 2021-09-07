# Builder pattern.

Hi everyone, today I gonna talk about the Builder pattern. The Builder pattern is a design pattern that allows us to create complex objects with a simple interface. Maybe you don't know what a builder pattern is, but I guarantee that this pattern is extensively used in many libraries and frameworks, and we will understand and implement it in this article, building a validation class.

## How Builder pattern works?

Builder pattern is a creation pattern that allow us to create complex objects step by step. The idea is to have some methods to create parts of that object.

Imagine that we want to create a house, we will need to create methods to build walls, floors, doors, windows, etc, something like that.

```typescript
const house = new HpHouseBuilder();

house
  .buildWalls()
  .buildFloors()
  .buildDoors()
  .buildWindows()
  .buildRoof()
  .buildGarage()
  .buildPool();
```

## Builder Implementation

In this example we will create a validation class to validate strings using typescript, fell free to use your favorite programming language. We will make simple validations, like checking if the string is empty, max and min length, and regex.

```typescript
export class StringValidator {
  constructor(private value: string, private messages: Array<string>) {}

  isEmpty(message: string): boolean {
    if (this.value.trim().length === 0) {
      this.messages.push(message);
    }
    return this;
  }

  maxLength(max: number, message: string): boolean {
    if (this.value.length > max) {
      this.messages.push(message);
    }
    return this;
  }

  minLength(min: number, message: string): boolean {
    if (this.value.length < min) {
      this.messages.push(message);
    }
    return this;
  }

  is(pattern: RegExp, message: string): boolean {
    if (!pattern.test(this.value)) {
      this.messages.push(message);
    }
    this;
  }

  build(): Array<string> {
    return this.messages;
  }
}
```

This is a very simple example of a validation class, but I believe that it is a good example to show how the builder pattern works. Let's a brief explanation about our implementation.

1.  Create a class called StringValidator.
2.  Create a constructor that receives a string as a value and an array of strings as a message.
3.  Create a method called isEmpty that receives a string, if the value is empty, it will add the message to the messages array, and return the instance.
4.  Create a method called maxLength that receives a number and a string, if the value is greater than the number, it will add the message to the messages array, and return the instance.
5.  Create a method called minLength that receives a number and a string, if the value is less than the number, it will add the message to the messages array, and return the instance.
6.  Create a method called is that receives a regex and a string, if the value doesn't match the regex, it will add the message to the messages array, and return the instance.
7.  Create a method called build that returns the messages array.

In order to build the messages array, we need to return the instance, so we can chain the others methods.
