# Tutorial 1.2: Go Function w/ Input And Vendor Folder. (3 minutes)

This example will show you how to test and deploy Go (Golang) code with vendored Dependencies to Oracle Functions. It will also demonstrate passing data in through stdin.

### First, run the following commands:

```sh
#Vendor Dependencies
glide install -v #Or what ever vendor tool you want. We have a glide.yaml for you here already.

# Initialize your function creating a func.yaml file
fn init <DOCKERHUB_USERNAME>/hello-go

# Test your function. This will run inside a container exactly how it will on the server
fn run

# Now try with an input
cat sample.payload.json | fn run

# Deploy your functions to the Oracle Functions server (default localhost:8080)
# This will create a route to your function as well
fn deploy myapp
```

### Now call your function:

```sh
curl http://localhost:8080/r/myapp/hello-go
```

Or call from a browser: [http://localhost:8080/r/myapp/go](http://localhost:8080/r/myapp/hello-go)

And now with the JSON input:

```sh
curl -H "Content-Type: application/json" -X POST -d @sample.payload.json http://localhost:8080/r/myapp/hello-go
```

That's it!

### Note on Dependencies

In Go, simply put them all in the `vendor/` directory.
This example uses logrus. Put logrus in the vendor folder you can just call:
`glide install -v`

# In Review

1. We piped JSON data into the function at the command line
    ```sh
    cat sample.payload.json | fn run
    ```

2. We received our function input through **stdin**
    ```go
    json.NewDecoder(os.Stdin).Decode(p)
    ```

3. We wrote our output to **stdout**
    ```go
    fmt.Printf("Hello")
    ```

4. We sent **stderr** to the server logs
    ```go
    log.Println("here")
    ```