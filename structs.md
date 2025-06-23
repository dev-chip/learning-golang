# Structs

## Define
Structs are defined just like they are in the C-family of languages:
```go
type cafe struct {
    name string
    address string
    rating int
}
```

## Assign
Once declared, you can assign variables of that type in different ways:
```go
var bobs cafe

ninetyTwo := cafe{}

beans := cafe{
    "Beans",
    "123 Hello World Drive",
    5,
}

henrys := cafe{
    "Henry's",
    "123 Foo Bar Avenue",  // with this method, not all fields need to be listed
}
```

## Annonymous Structs
You can declare a variable that uses a struct type without giving the struct type a name. This is called an anonymous struct:
```go
var person struct {
    name string
    age int
    pet string
}
person.name = "bob"
person.age = 50
person.pet = "dog"
pet := struct {
    name string
    kind string
}{
    name: "Fido",
    kind: "dog",
}
```

