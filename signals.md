# Signals

Signal are the most basic reactive primitives. 

They store a single value.

```ts
// create signal
const [name, setName] = createSignal("John")

// get value
name()

// set value directly
setName("Alex") 

// set value using callback
setName(value => value + " Doe")
```

## Create signal

Use **createSignal** function to create a signal. 

```ts
const [score, setScore] = createSignal(0)
```

1. **score** - getter function, used to get signal's value
2. **setScore** - setter function, used to update signal's value

The value stored in signal can be just anything.

```ts
// string
const [name, setName] = createSignal("John")

// number
const [age, setAge] = createSignal(18)

// boolean
const [isReady, setReady] = createSignal(false)

// array 
const [items, setItems] = createSignal(["Milk", "Bread", "$2"])

// object
const [config, setConfig] = createSignal({ mode: 'dark', lang: 'eng' })

// function
const [adder, setAdder] = createSignal((a, b) => a + b)
```

## Update signal

You have two ways to update signal's value. 

### Direct update

```ts
const [age, setAge] = createSignal(18)

setAge(19)
```

### Update using callback

Use this variant if you need to do something with signal's current value.

Callback give you signal's current value.

```ts
const [age, setAge] = createSignal(18)

// simple as that
setAge(value => value + 1)
setAge(value => value + value + 1)

// ❌ never do this. if `age` changes it may have different values for every call
setAge(age() + 1) 
setAge(age() + age() + 1)
```

For updating referenced types like arrays or objects use spread operator.

Arrays
```ts
const [people, setPeople] = createSignal([
    { name: 'Ruslan', age: 19 },
    { name: 'Steve', age: 43 },
    { name: 'Elon', age: 51 }
])

// add new item using spread operator
setPeople(value => [...value, { name: 'Kamran', age: 30 }]) 

// ❌ don't do this. update will not trigger if reference kept
setPeople(value => {
    value.push({ name: 'Kamran', age: 30 });
    return value;
}) 
```
Objects
```ts
const [config, setConfig] = createSignal({
    mode: 'dark',
    lang: 'eng'
})

// update property using spread
setConfig(value => ({ ...value, lang: 'aus' }))

// ❌ don't do this. update will not trigger if reference kept
setPeople(value => {
    value.lang = 'aus'
    return value;
}) 
```
