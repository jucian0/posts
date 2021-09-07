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
