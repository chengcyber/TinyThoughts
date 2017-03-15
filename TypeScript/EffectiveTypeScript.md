# Intro to Static Typing

- number
- boolean
- string
- any
- (id: number, name: string) => void

```
let someString: string = "cool";
```

# Using Type Inference

```
let target: HTMLElement = document.getElementById("target");
target.onclick = (event: MouseEvent) => event.button;
```

```
let target: HTMLElement = document.getElementById("target");
target.onclick = (event: HTMLButtonElement) => event.button;  
// error TS2322: Type '(event: HTMLButtonElement) => any' is not assignable to type
// '(ev: MouseEvent) => any'.
// error TS2339: Property 'button' does not exist on type 'HTMLButtonElement'.
```

# Union Type Inference

```
let thing = string | number | string[] | boolean;
let returnSomething = (someThing: thing) => {
  return someThing;
};
console.log(returnSomeThing("coolGuy"));  // coolGuy
```

```
let thing = string | number | string[] | boolean;
let returnSomething = (someThing: thing) => {
  if (typeof someThing === "string" ||
      typeof someThing === "number" ||
      typeof someThing === "boolean") {
        console.log("somthing = ", someThing);
      }
  if (someThing instanceof Array) {
  let joinedThings = "";
  someThing.forEach((thing) =>{
    joinedThings += ` ${thing}`;
  });
  console.log("joinedThings", joinedThings);
};
}
```

If you Union type Objects with Not Objects, the compiler gets mad.

```
type stuff = string | {name:string};
```

If you Union type Objects without a common parameter, the compiler gets mad.

```
type coolThings = {name: string;} | {id: number;};
```
If you Union type Objects with a common parameter, you can access that common parameter.

```
type stuffAndThings = {cool: string; meh: string;} | {cool: string; lame: string; }
```

# Distinguishing between types of Strings

```
let unit: string = "arbitraryString"
let miles: "MILES" = "MILES" // pass
let miles: "MILES" = "MItES" // error on typo
```

```
type distanceMetric = "MILES" | "KILOMETERS" | "METERS" | "YARDS" | "FEET" | "INCHES";
function moveCharacter(distance: number, value: distanceMetric) {
  console.log(`You moved ${distance} ${value}`);  // error TS2345: Argument of type '"dragon"' is not 
                                                  // assignable to parameter of type '"MILES" | "KILOMETERS" | ... '
};
```

# Interfaces

```
let superHero: { secretIdentity: string; superHeroName: string; health: number };
let superVillain: { secretIdentity: string; superHeroName: string; health: number };
```

Two object with same type, this is where interface comes to rescue

```
interface ComicBookCharacter {
  secretIdentity?: string;
  alias: string;
  health: number;
}

let superHero: ComicBookCharacter;
let superVillain: ComicBookCharacter;
```

`?` means optional property

## Function interface

```
interface AttackFunction {
  (opponent: { alias: string; health: number; }, attackWith: number): number;
}

interface KrustyTheClown {
  alias: string;
  health: number;
  inebriationLevel: number;
  attack: AttackFunction;
}
```

## Optional attributes

```
interface OptionalAttributes {
  strength?: number;
  insanity?: number;
  dexterity?: number;
  healingFactor?: number;
}

interface ComicBookCharacter extends OptionalAttributes {
  secretIdentity?: string;
  alias: string;
  health: number;
  attack: AttackFunction;
}
```

## Class in TypeScript


```
// classes.ts
interface Opponent {
  alias: string;
  health: number;
}

class ComicBookCharacter {
  alias: string;
  health: number;
  strength: number;
  secretIdentity: string;

  attackFunc(opponent: Opponent, attackWith: number) {
    opponent.health -= attackWith;
    console.log(`${this.alias} attacked ${opponent.alias} who's health = ${opponent.health}`);
  }
}
```

All of the class properties are `public` by default

## Private
use `private` access modifier to make it private

```
  private secretIdentity: string;
```

## Constructor

demo for whole class

```
interface Opponent {
  alias: string;
  health: number;
}

class ComicBookCharacter {
  alias: string;
  health: number;
  strength: number;
  private secretIdentity: string;

  attackFunc(opponent: Opponent, attackWith: number) {
    opponent.health -= attackWith;
    console.log(`${this.alias} attacked ${opponent.alias} who's health = ${opponent.health}`);
  }
  
  constructor(alias: string, health: number, strength: number, secretIdentity: string) {
      this.alias = alias;
      this.health = health;
      this.strength = strength;
      this.secretIdentity = secretIdentity;
  }
}
```

Usage

```
let storm = new ComicBookCharacter("Storm", 100, 100, "Ororo Munroe");
let theBlob = new ComicBookCharacter("The Blob", 1000, 5000, "Fred J. Dukes");

storm.getSecreIdentity(); // Storm's secret identity is Ororo Munroe
```

## Constructor with access modifier args

Adding access modifiers to the constructor arguments lets the class know that they're properties of a class. If the arguments don't have access modifiers, they'll be treated as an argument for the constructor function and not properties of the class.

```
class ...

  constructor(public alias: string, public health: number, public strength: number, private secretIdentity: string) {}
  
```

with access modifier on constructor args, they will be treat as properties of the class.

## Static method

```
class ...

  static createTeam(teamName: string, members: ComicBookCharacter[]) {
    name: teamName,
    members: members
  }

```

static method can only be used in Class, not instance

```
let team = ComicBookCharacter.createTeam("oddCouple, [storm, theBlob]);
console.log(team) 
```

## Protected

`protected` access modifier share class behavior with inheritance

```
class ComicBookCharacter (
  constructor{
    public alias: string, public health: number , public strength: number,
    protected secretIdentity: string
  ) {}
}

class SuperHero extends ComicBookCharacter {
  traits = ["empathy", "strong moral code"];
  getSecretId() { console.log(this.secretIdentity); }
}

let jubilee = new SuperHero("Jubilee", 23, 233, "Jubilation Lee");

console.log(jubilee.getSecretId());

// CONSOLE OUTPUT
// Jubilation Lee
```

# Type Assertion

tell the compiler that we know more about something's type

```
interface SuperHero {
  powers: string[];
  savesTheDay: () => void;
}

interface BadGuy {
  badDeeds: string[];
  getRandomBadDeed: () => string;
  commitBadDeed: () => void;
}

function saveDayOrBadDeed(something: SuperHero | BadGuy) {
  if (something.powers) {}
}
```

compiler error, since BadGuy doesn't have powers property

here is `Assertion` comes to rescue

property `as` type

```
function saveDayOrBadDeed(something: SuperHero | BadGuy) {
  if ((something as SuperHero).powers) {}
}
```

`angle bracket` type property

```
if (<SuperHero>something.powers) {} // angle bracket syntax
```

# Generic in TypeScript

It can be painful to write the same function repeatedly with different types. Typescript generics allow us to write 1 function and maintain whatever type(s) our function is given.

```
function pushSomethingIntoCollection<T>(something: T, collection: T[]) {
    collection.push(something);
    console.log(collection);
}

let jeanGrey = { name: "Jean Grey" };
let wolverine = { name: "Wolverine" };

let superHeroes = [jeanGrey];
let powers = ["telekinesis", "esp"];

interface SuperHero { name: string; }

pushSomethingIntoCollection<SuperHero>(wolverine, superHeroes);
pushSomethingIntoCollection<string>("adamantium claws", powers);

```

`T` which is short for type, is just the convention. Now that the generic has been declared, we can use it to type our arguments.

first arg type should be `T`,
second arg type should be array of `T`.

# Practical Generics in TypeScript

## Basic generic interface

```
interface Crocodile { personality: string; }
interface Taxes { year: number; }
interface Container<T> { unit: T; }

let crocContainer: Container<Crocodile> = {unit: { personality: "mean"}};
let taxContainer: Container<Taxes> = {unit: {year: 2011}};
```

## Generic Constraints

You have to pass the type argument with an interface constraint

```
interface CrocContainer<T extends Crocodile> { crocUnit: T; }

let blueCrocContainer: CrocContainer<BlueCroc> = {crocUnit: {personality: "cool", color: "blue"}};

```

## Class generic type inheritance

1. Class without constructor

```
class ClassyContainer<T extends Crocodile> {
  classyCrocUnit: T;
}
```

1.1 not pass generic interface

class type as Crocodile

```
let classyCrocContainer = new ClassyContainer();
classyCrocContainer.classyCrocUnit = {personality: "classy"};
```

2.1 pass generic interface inheritance

```
let classyCrocContainer = new ClassyContainer<RedCroc>();
classyCrocContainer.classyCrocUnit = {personality: "classy", color: "red"};
```

Review:

if we don't use the constructor shorthand for setting properties on instantiation, we can get away without passing the type argument but the property can **only** contain the constraint type.

2. Class with contructor

```
class CCC<T extends Crocodile> {
  constructor(public cccUnit: T) {}
}
```

2.1 not pass generic interface

```
let ccc = new CCC({personality: "ultra classy", likesCheetos: true});
```

2.2 pass generic interface inheritance

```
let ccc = new CCC<BlueCroc>({personality: "ultra classy", likesCheetos: true, color: "blue"});
```

error, `likeCheetos` doesn't exist on type constraints

```
let ccc = new CCC<BlueCroc>({personality: "ultra classy", color: "blue"});
```

works fine

Review:

If we do use the constructor shorthand for setting properties on instantiation and we want more specificity, we can pass the type argument. If we want less specificity, we don't have to. As long as the instance has the constraint, we can add extra properties.

