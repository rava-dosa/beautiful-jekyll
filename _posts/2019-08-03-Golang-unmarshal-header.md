---
layout: post
title: Using golang reflection to write an unmarshal api for http headers
subtitle: 
tags: [golang]
---

### Content
1. Benefit of this pattern
2. How it works ?
3. Story behind it.
4. Reflection in other languages

### Benefit of this pattern.
You have to just define struct and tags, and pass the pointer of struct to getheader function. then getheader function will automatically, fill the struct. So the benefit of this pattern is that, for different api's you might be sending different headers, and you might want to acess different headers, so instead of writing code in the router for each of these, by doing header.get, what you can do is call this function instead. This makes the code cleaner, fewer lines of code in each router, and gives more structure to the code as you can know which headers you are accessing in a particuar file or router.

In golang you can add tags for vaidating struct further reducing line of codes you have to add. So basically unmarshalling of http headers follows golang pattern of writing code as well.
### How it works ?
This is how our struct looks like.

```go

type Register_Header struct{
	Method string `header:"x-api-key" validate:"len=10"`
	Agent string `header:"User-Agent"`
}
```

```go
func GetHeader(r *http.Request,data interface{}){
	//ValueOf returns a new Value initialized to the concrete value stored in the interface i.
	//.elem dereference
	val:=reflect.ValueOf(data).Elem()
	//TypeOf returns the reflection Type that represents the dynamic type of interface i.
	//basically it is used to access the metadata of variables inside struct.
	data_type:=reflect.TypeOf(data).Elem()
	header:=r.Header
	//now I am iterating over all the fields in the passed struct
	for i:=0 ;i<val.NumField();i++{
		fld:=val.Field(i)
		tag:=data_type.Field(i).Tag.Get("header")
		//for example for the first field the above line will return x-api-key and in second iteration it wil return User-agent.
		header_data,ok:= header[tag]
		if ok{
			fld.SetString(header_data[0])
		}
	}
}
```

### Story Behind it.
Story is quite simple. I have seen multiple orm's use reflection. In golang there is an unmarshal api, which maps json data to struct, so yeah I thought why not write unmarshal for http headers as well. I knew that this can be done. All credits to Gaurav(one of my dep and batchmate), who spent spent some time to read reflection and implement this.

### Other languages which has reflection
1. Java
2. Python
3. A lot of others as well.
C(we can do reflection pattern in c as well, it will be tough though)

### Basic idea behind reflection
Basic idea behind reflection is to look inside object. and be able to manipulate things accordingly. 