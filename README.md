# KeyMapStorage
Algorithm that performs a quick search through data stored in RAM based on primary keys

## Example

Initialization
```go
db := &Storage{}
db.Init()
```

Single write
```go
type Person struct {
	FirstName string
	LastName string
	Age int
	City string
}
...
x := Person{"Daniil", "Furmanov", 10, "Moscow"}
// write with primary keys "FirstName" and "LastName"
db.Write(GetIdentifier(x, []string{"FirstName", "LastName"}), x)
```

Search
```go
searchPattern := map[string]interface{} {
		"FirstName":  "Daniil",
		"LastName": "Furmanov",
}

segment := GetIdentifierFromMap(searchPattern, []string{"FirstName", "LastName"})

seg, err := db.FindSegment(segment)
if err != nil {
	fmt.Println("Segment not found")
	return
}

records := seg.FindRecords(searchPattern)

if len(records) > 0 {
	fmt.Println(records[0].Data)
} else {
	fmt.Println("Zero results found")
}
```

## Benchmarks
```
Tested on MacBook Pro 15` (2014). Intel Core i7

Single write: 14.449µs
Write 1,000,000 records: 2.122833s
Search through 1,000,000 records by two parameters: 378.061µs
```
