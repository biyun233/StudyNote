## Go

### Hello World

```
mkdir go-test
cd go-test
```

新建文件 main.go

```sh
go mod init go-test
```

main.go

```go
package main

import "fmt"

func main() {
	fmt.Print("Hello World")
}
```

```sh
go run main.go
```



### Variables & Constants

- `Variables` are used to *store values*, like **containers* for values**
  - **Variable names must be used**

- `Constants` are like variables, except that their values **cannot be changed**

### Formatting Output - printf

```go
fmt.Printf("Some text with a variable %s", myVariable)
```

- It takes a **template string** that contains the text that needs to be formatted

- Plus some **annotation verbs (or placeholder)** that tells the fmt functions how to format the variable passed in

```go
const conferenceTickets = 50
var remainingTickets = 50

fmt.Printf("Welcome to %v booking application\n", conferenceName)
fmt.Printf("We have total of %v tickets and %v are still available.\n", conferenceTickets, remainingTickets)
```

### Data Types

- You need to tell Go Compiler, the data type when declaring the variable
- Type Inference: BUT, Go can infer the type when you assign a value

```go
var conferenceName = "Go Conference"
// equals to 
// conferenceName := "Go Conference"
// but cannot be used with const or type 
var remainingTickets uint = 50

// remainingTickets = -1 will get error
```

#### Arrays

- Fixed size

- Only the same data type can be stored 

  ```go
  var bookings [50]string
  var bookings = [50]string{"Nina", "biyun"}
  bookings[2] = "test"
  ```

#### Slices

- Slice is an **abstraction of an Array**
- Slices are more flexible and powerful: **variable-length** or get an sub-array of its own
- Slices are also **index-based** and have a size, but is resized when needed

##### append

  - Adds the element(s) at the end of the slice

  - Grows the slice if a greater capacity is needed and returns the updated slice value

    ```go
    var bookings []string
    // bookings := []string{}
    bookings = append(bookings, userName)
    ```

##### Creating a slice with "make"

- Alternative way to create a slice

- We need to define the initial size of the slice

  ```go
  var bookings = make([]map[string]string, 0)
  ```

  

#### Maps

- Maps **unique keys to values**

- You can retrieve the value by using its key

- All keys have the same data type

- All values have the same data type

  ```go
  var userData = make(map[string]string)
  userData["userName"] = userName
  userData["numOfTickets"] = strconv.FormatUint(uint64(userTickets), 10)
  ```

#### Structs

- hold mixed data types

  ```go
  // The type keyword creates a new type, with the name you specify
  var bookings = make([]UserData, 0)
  
  type UserData struct {
  	userName     string
  	numOfTickets uint
  }
  
  var userData = UserData{
      userName:     userName,
      numOfTickets: userTickets,
  }
  
  bookings = append(bookings, userData)
  fmt.Println(bookings[0].userName)
  ```

  

### Getting User Input

```go
var userName string
// ask user for their name
fmt.Scan(userName)
// = fmt.Scan("") -> passing the value
// Go compiler will not wait for user's input

fmt.Scan(&userName)
// = fmt.Scan(0xc00002a080) -> passing the reference
// Scan function can now assign the user's value to the userName variable
// Because it has a pointer to its memory address
```

### Loops

Only `for` loops, not `while`, `for each`

```go
for remainingTickets > 0 {

}
```



```go
// booking = firstName + lastName
firstNames := []string{}
for index, booking := range bookings {
    var names = strings.Fields(booking)
    var firstName = names[0]
    firstNames = append(firstNames, firstName)
}
// index not used
```

#### Blank Identifier "_"

- To ignore a variable you don't want to use

  ```go
  for _, booking := range bookings {
      var names = strings.Fields(booking)
      var firstName = names[0]
      firstNames = append(firstNames, firstName)
  }
  ```

### If ... Else

```go
if remainingTickets == 0 {
    // end program
    fmt.Println("Our conference is booked out. Come back next year.")
    break
} else if blabla {
    
} else {
    
}
```

### User Input Validation

```go
func validateUserInput(userName string, userTickets uint, remainingTickets uint) (bool, bool) {
	isValidName := len(userName) > 2
	isValidTickerNumber := userTickets > 0 && userTickets <= remainingTickets
	return isValidName, isValidTickerNumber
}

isValidName, isValidTicketNumber := validateUserInput(userName, userTickets, remainingTickets)

```

### Package Level Variables

- Defined at the top **outside all functions**
- They can be **accessed inside any of the functions**
- And in all files, which are in the **same package**
- They cannot be used with `:=`

### Local Variables

- Defined **inside a function or a block**
- They can be **accessed only inside that function or block of code**

### Packages in Go

- Go programs are organized into packages

- A package is **a collection of Go files**

  ```
  - package main
  |-- main.go
  |-- helper.go
  ```

  ```go
  // main.go
  package main
  
  import (
  	"fmt"
  )
  
  const conferenceTickets = 50
  
  var conferenceName = "Go Conference"
  var remainingTickets uint = 50
  var bookings = []string{}
  
  func main() {
  	greetUsers()
  
  	// infinite For Loop never ends
  	for {
  		userName, userTickets := getUserInput()
  
  		isValidName, isValidTicketNumber := validateUserInput(userName, userTickets)
  
  		if isValidName && isValidTicketNumber {
  			remainingTickets = remainingTickets - userTickets
  			bookings = append(bookings, userName)
  
  			fmt.Printf("User %v booked %v tickets.\n", userName, userTickets)
  			fmt.Printf("%v tickets remains for %v.\n", remainingTickets, conferenceName)
  
  			if remainingTickets == 0 {
  				// end program
  				fmt.Println("Our conference is booked out. Come back next year.")
  				break
  			}
  		} else {
  			if !isValidName {
  				fmt.Println("user name entered is too short")
  			}
  			if !isValidTicketNumber {
  				fmt.Println("number of tickets is invalid")
  			}
  		}
  	}
  }
  
  func greetUsers() {
  	fmt.Printf("Welcome to %v booking application\n", conferenceName)
  	fmt.Printf("We have total of %v tickets and %v are still available.\n", conferenceTickets, remainingTickets)
  	fmt.Println("Get your tickets here to attend")
  }
  
  func getUserInput() (string, uint) {
  	var userName string
  	var userTickets uint
  	// ask user for their name
  	fmt.Println("Enter your first name:")
  	fmt.Scan(&userName)
  	fmt.Println("Enter number of tickets:")
  	fmt.Scan(&userTickets)
  	return userName, userTickets
  }
  ```

  ```go
  // helper.go
  package main
  
  func validateUserInput(userName string, userTickets uint) (bool, bool) {
  	isValidName := len(userName) > 2
  	isValidTickerNumber := userTickets > 0 && userTickets <= remainingTickets
  	return isValidName, isValidTickerNumber
  }
  ```

  

  ```sh
  go run main.go helper.go
  # or
  go run .
  ```

### Organize Code with Go Packages

#### Exporting a variable

- Make it available for all packages in the app

- Capitalize first letter -> `ValidateUserInput`

```
-- go-test
|-- helper
	|-- helper.go
|-- go.mod
|-- main.go
```

```go
// helper.go
package helper

func ValidateUserInput(userName string, userTickets uint, remainingTickets uint) (bool, bool) {
	isValidName := len(userName) > 2
	isValidTickerNumber := userTickets > 0 && userTickets <= remainingTickets
	return isValidName, isValidTickerNumber
}
```

```go
// go.mod
module go-test

go 1.21.0
```

```go
// main.go
package main

import (
	"fmt"
	"go-test/helper"
)

const conferenceTickets = 50

var conferenceName = "Go Conference"
var remainingTickets uint = 50
var bookings = []string{}

func main() {
	greetUsers()

	// infinite For Loop never ends
	for {
		userName, userTickets := getUserInput()

		isValidName, isValidTicketNumber := helper.ValidateUserInput(userName, userTickets, remainingTickets)

		if isValidName && isValidTicketNumber {
			remainingTickets = remainingTickets - userTickets
			bookings = append(bookings, userName)

			fmt.Printf("User %v booked %v tickets.\n", userName, userTickets)
			fmt.Printf("%v tickets remains for %v.\n", remainingTickets, conferenceName)

			if remainingTickets == 0 {
				// end program
				fmt.Println("Our conference is booked out. Come back next year.")
				break
			}
		} else {
			if !isValidName {
				fmt.Println("user name entered is too short")
			}
			if !isValidTicketNumber {
				fmt.Println("number of tickets is invalid")
			}
		}
	}
}

func greetUsers() {
	fmt.Printf("Welcome to %v booking application\n", conferenceName)
	fmt.Printf("We have total of %v tickets and %v are still available.\n", conferenceTickets, remainingTickets)
	fmt.Println("Get your tickets here to attend")
}

func getUserInput() (string, uint) {
	var userName string
	var userTickets uint
	// ask user for their name
	fmt.Println("Enter your first name:")
	fmt.Scan(&userName)
	fmt.Println("Enter number of tickets:")
	fmt.Scan(&userTickets)
	return userName, userTickets
}

```

### Scope Rules in Go

#### 3 Levels of Scope

- Local
  - Declaration **within function**, **Can be used** only within that function
  - Declaration **within block (e.g. for, if-else)**, **Can be used** only within that block
- Package
  - Declaration **outside all functions**, **Can be used** everywhere in the same package
- Global
  - Declaration **outside all functions & uppercase first letter**, **Can be used** everywhere across all packages

### Goroutines - Concurrency in Go

#### "go" keyword

- "go ..." - starts a new goroutine
- A goroutine is a **lightweight thread** managed by the Go runtime

#### Waitgroup

- Waits for the launched goroutine to finish

- Package "sync" provides basic synchronization functionality

- **Add**: Sets the number of goroutines to wait for (increase the counter by the provided number)

- **Wait**: Blocks until the WaitGroup counter is 0

- **Done**: Decrements the WaitGroup counter by 1, So this is called by the goroutine to indicate that it's finished

  ```go
  var wg = sync.WaitGroup{}
  func main() {
      bookTicket(userName, userTickets)
      wg.Add(1)
      go sendTicket()
      wg.Wait()
  }
  func sendTicket() {
  	time.Sleep(10 * time.Second)
  	fmt.Println("###########")
  	fmt.Println("test")
  	fmt.Println("###########")
  	wg.Done()
  }
  ```

  
