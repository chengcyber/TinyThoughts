# Safe Property Initialization

turn on the following compiler options

```json
{
  "compilerOptions": {
    "strictPropertyInitialization": true,
    "strictNullCheck": true
  }
}
```


```js
class Book {
  public title;
}

const book = new Book();

const shortTitle = book.title.substring(0, 5);
```

this is unsafe because title might not be initialized yet.

# Readable Number Separator

add underscore between number won't influence the output js.

```js
setTimeout(
  () => console.log('hello, world'),
  2_500
);
```

# Automatic Type Inference using "in"


```js
interface Admin {
  role: string;
}

interface User {
  homepage: string;
}

// if admin redirect to /admin, otherwise to user.homepage
function redirect(user: User | Admin): string {
  if ('role' in user) {
    return user.role;
  } else {
    return user.homepage;
  }
}
```

with `in`, TS can correctly infer the user is Admin type

# Automatic Type Inference in Switch Cases


```ts
interface Action {
  type: string;
}

class Add implements Action {
  // 1. be caution that type here is not set to string type
  // 2. readonly is essential to settle down type
  readonly type = 'Add';
  constructor(public payload: string) {
  }
}

class RemoveAll implements Action {
  readonly type = 'RemoveAll';
}

type TodoActions = Add | RemoveAll;
// | TodoActions can be inferred by action.type now

interface ITodoState {
  todos: string[];
}

function todoReducer(
  action: TodoActions,
  state: ITodoState = { todos: [] }
): ITodoState {
  switch(action.type) {
    case "Add":
      return {
        todos: [...state.todos, action.payload],
      }
    case "RemoveAll":
      return { todos: [] }
    default:
      const ret: never = action;
      // this is no meaning in run-time, it serves in compilation.
      return state
    }
}
```

# + and - controls mapped type modifier

```ts
interface IPet {
  number: string;
  age: number;
  favorite?: string;
}

// every property is readonly now +
type ReadonlyPet = {
  +readonly [K in keyof IPet]: IPet[K]
}

// every property is required now -?
type AllRequiredPet = {
  [K in keyof IPet]-?: IPet[K]
}

// every property is optional now ?
type AllOptionalPet = {
  [K in keyof IPet]?: IPet[K]
}
```

# Interface Merging

```ts
interface Foo {
  a: string
}

interface Foo {
  b: string
}

const foo: Foo = {
  a: 'a',
  b: 'b',
}
```

interfaces with same name will be merged into one. This is very useful to extend third party library interface.

```
interface Window {
  __Store__: any
}

window.__Store__ = {}; // no longer complains
```

# Self Reference Type

```ts
interface TreeNode<T> {
  value: T;
  left: TreeNode<T>;
  right: TreeNode<T>;
}

interface LinkedListNode<T> {
  value: T;
  next: LinkedListNode<T>;
}
```

# TS with iterators

imagining we are implementing backward time traveling through redux store

```ts
interface Action {
  type: string;
}

interface LinkNode<T> {
  value: T;
  prev: LinkNode<T>;
  next: LinkNode<T>;
}

class BackwardsActionIterator extends IterableIterator<Action> {

  constructor(private _currentActionNode: ListNode<Action>) {
  }

  [Symbol.Iterator](): IterableIterator<Action> {
    return this;
  }
  
  next(): IteratorResult<Action> {
    const current = this._currentActionNode;
    if (!current || !current.value) {
      return { value: null, done: true };
    }
    // 1. move through each item in the list
    this._currentActionNode = current.prev;
    // 2. return each item
    return { value: current.value, done: false };
  }
}
```


```ts
const action1: Action = { type: '1' };
const action2: Action = { type: '2' };
const action3: Action = { type: '3' };

const actionNode1: LinkNode<Action> = {
  value: action1,
  prev: null,
  next: null,
}

const actionNode2: LinkNode<Action> = {
  value: action2,
  prev: actionNode1,
  next: null,
}
actionNode1.next = actionNode2;

const actionNode3 = LinkNode<Action> = {
  value: action3,
  prev: actionNode2,
  next: null,
}
actionNode2.next = actionNode3;

const backwardsActions = new BackwardsActionIterator(actionNode3);

for (let action of backwardsActions) {
  console.log(action.type)
}
// print out:
// 3
// 2
// 1
```

# unknown type

be suitable for a better type security than `any`


```ts
interface Response {
  data: any;
}

let res: Response;

const data = res.data;

if (typeof data === 'string') {
   // do something
}

data.a.b.c.d.e // no complain here...because of any
```


```ts
interface IComment {
  message: string;
  data: Date;
}

interface Response {
  data: unknown;
}

let res: Response;

const data = res.data;

data.a.b.c.d // ts complains !!!

if (isComment(data)) {
  data.message.toUpperCase();
} else {
  // if data type is already known
  const numbers = <number[]>data;
  numbers.indexOf(1);
}

function isComment(data: any): data is IComment {
  return (<IComment>data).message !== undefined;
}
```

# Conditional Types

## 1. used in generic type

```ts
interface StringContainer {
  value: string;
}

interface NumberContainer {
  amount: number;
}

type Item<T> {
  id: T;
  container: T extends string ? StringContainer : NumberContainer;
}
```

## 2. type filter

```ts
type ArrayFilter<T> = T extends any[] ? T : never;

type StringOrNumbers = ArrayFilter<string | number | string[] | number[]>;
```

1. distribute -> never | never | string[] | number[]
2. never is ignored -> string[] | number[]

So, the StringOrNumbers eventually guards string[] or number[] type.


## 3. function overload

```ts
// index by number
interface TV {
  id: number;
  diagonal: number;
}

// index by string
interface Book {
  id: string;
  toc: string[];
}

interface IItemService {
  getItem(id: number): TV
  getItem(id: string): Book
}

interface IItemService2 {
  getItem<T extends string | number>(id: T): T extends string ? Book : TV;
}
```

## 4. Flattern type


```ts
const numbers = [1, 2];

type FlatternArray<T extends any[]> = T[number];
type NumbersArrayFlatterned = FlatternArray<typeof numbers>; // --> number
```

```ts
const obj = {
  id: 1,
  name: 'hey',
};

type FlatternObject<T extends object> = T[keyof T];
type ObjectFlatterned = FlatternObject<typeof obj>; // --> number | string
// keyof obj --> "id" | "name"
// T["id" | "name"] --> T["id"] | T["name"] --> number | string
```

generic type and accepts any type to flattern

```ts
type Flattern<T> = T extends any[] ? T[number] :
  T extends object ? T[keyof T] :
  T;
  
const numbers = [1,2];
const obj = {
  id: 1,
  name: 'key',
};
const boolean = true;
  
type NumbersArrayFlatterned = Flattern<typeof numbers>;
type ObjectFlatterned = Flattern<typeof obj>;
type BooleanFlatterned = Flattern<typeof boolean>;
```

# Infer Return Type

```ts
function generateId(seed: number) {
  return seed + 5;
}

type ReturnType<T> = T extends (...args: any[]) => infer R ? R : any;
type Id = ReturnType<typeof generateId>;
// Id dynamically changes via generateId return
```

```ts
type UnpackPromise<T> = T extends Promise<infer K>[] ? K : any;
const arr = [Promise.resolve(true)];

type ExpectedBoolean = UnpackPromise<typeof arr>;
```

# Deeply Mark Readonly

```ts
interface Store {
  userId: number;
  todos: string[];
}

type DeepReadonlyObject<T> = {
  +readonly [K in keyof T]: DeepReadonly<T[K]>
};

// be careful, it becomes complicate with array of array
type DeepReadonly<T> = T extends (infer E)[][] ?
  ReadonlyArray<ReadonlyArray<DeepReadonlyObject<E>>> :
  T extends (infer E)[] ?
  ReadonlyArray<DeepReadonlyObject<E>> :
  T extends object ? DeepReadonlyObject<T> :
  T;


type ReadonlyStore = DeepReadonly<Store>;

const store: ReadonlyStore;

store.todos.map(todo => todo.toUpperCase());
```

# Decorators

```ts
interface ITodo {
  userId: number;
  id: number;
  title: string;
  completed: boolean;
}

function Get(url: string) {
  // target: constructor of the class
  // name: the name of the property decorating
  return function(target: any, name: string) {
    const hiddenInstanceKey = '_$$' + name + '$$_';
    const init = () => {
      return fetch(url)
        .then(response => response.json());
    };

    Object.defineProperty(target, name, {
      get: function() {
        return this[hiddenInstanceKey] || (this[hiddenInstanceKey] = init());
      },
      configurable: true,
    });
  }
}

function First() {
  return function(target: any, name: string) {
    const hiddenInstanceKey = '_$$' + name + '$$_';
    const prevInit = Object.getOwnPropertyDescriptor(target, name).get;
    const init = () => {
      return prevInit()
        .then(response => response[0]);
    };
    
    Object.defineProperty(target, name, {
      get: function() {
        return this[hiddenInstanceKey] || (this[hiddenInstanceKey] = init());
      },
      configurable: true,
    });
  }
}

class TodoService {
  @First()
  @Get('https://jsonplaceholder.typicode.com/todos')
  todos: Promise<ITodo[]>;
}

const todoService = new TodoService();
todoService.todos.then(
  (todos) =>
    console.log(todos)
);
```

