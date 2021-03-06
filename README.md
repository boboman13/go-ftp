# Go-FTP

[![GoDoc](https://godoc.org/github.com/boboman13/go-ftp/ftp?status.png)](https://godoc.org/github.com/boboman13/go-ftp/ftp) ![Project status](http://img.shields.io/status/beta.png?color=blue)

> Go-FTP is a FTP library that is designed for use in Go projects. It does not come as a standalone, functional FTP server (just a simple standalone, anonymous example is provided), and is an easy to use library that comes with an authentication API.

Go-FTP **does not work yet**! Do not attempt using it.

## Example Usage
This can be viewed in file form @ [here](https://github.com/boboman13/go-ftp/blob/master/example/main.go).
```go
package main

import (
	"fmt"
	"github.com/boboman13/go-ftp/ftp"
)

// Starts server
func main() {
	server := ftp.CreateServer("0.0.0.0", 21)

	// Configures authentication; WARNING: do not use this code, it is insecure
	server.Config.ConfigAuthentication(func(user string, password string) (authenticated bool, dir string) {
		fmt.Println("Logged in " + user + " w/ pass " + password)
		return true, "/home/" + user + "/ftp"
		})

	// Starts server
	err := server.Start()
	if err != nil {
		fmt.Println("Error occurred starting FTP server: " + err.Error())
	}
}
```

## API Methods
### Create server object
This will return a pointer to an FTPServer, configured with the correct host and port. It takes a host as a string and a port as an int.
```go
server := ftp.CreateServer("0.0.0.0", 21)
```
### Start server
This will return an error if it did not succeed; if it did succeed, it will return nil. *This method is blocking*, and might want to be run in a `goroutine`.
```go
err := server.Start()
```
### Configure server authentication
Go-FTP does not contain any authentication methods to begin with, and if you do not configure this method, it will not run correctly.

`server.config.ConfigAuthentication` takes one paramater, a function passed as a paramater. This function takes two paramaters - the user, as a string, and the password, as a string. After checking whether the user is authenticated, it should return two values - a boolean, whether or not the user is authenticated, and a string, the directory that the user has access to.
```go
server.config.ConfigAuthentication(func(user string, password string) (authenticated bool, dir string) {
	fmt.Println("Logged in " + user + " w/ pass " + password)
	return true, "/root/go/ftp/test"
	})
```
