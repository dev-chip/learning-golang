# Maps

## Create
To create a map with the `string` type for the key and the `int` type for the value:
```go
var myMap map[string]int
```

You can use a := declaration to create a map variable by assigning it a map literal:
```go
myMap := map[string]int{}
```

You can create a populated map like this:
```go
places := map[string][]string {
    "Manchester": []string{"North", "Bees", "Cotton"},
    "Stoke": []string{"North", "Pottery", "Bet365"},
    "London": []string{"South", "Palace", "Big Ben"},
}
```

## Read and Write
Values in a map are read and written the same as in Python:
```go
totalWins["Lions"] = 2
fmt.Println(totalWins["Orcas"])
```

Maps return a zero value (for that type e.g. `0`, `""`, etc.) if you attempt to index on a key that doesn't exist.

Go provides the `comma ok idiom` to tell the difference between a key that’s associated with a zero value and a key that’s not in the map:
```go
m := map[string]int{
 "hello": 5,
 "world": 0,
}
v, ok := m["hello"]
fmt.Println(v, ok)  // 5 true

v, ok = m["world"]
fmt.Println(v, ok)  // 0 true

v, ok = m["cya"]
fmt.Println(v, ok)  // 0 false
```
If `ok` is `true`, the key is present in the map. If `ok` is `false`, the key is not present.

## Delete
You can delete an item in a map using the built-in `delete` function: 
```go
delete(myMap, "hello")
```

## Clear
You can clear a map using the built-in `clear` function: 
```go
clear(myMap)
```

## Comparison
To compare two maps:
```go
maps.Equal(m, n)
```