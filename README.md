   * [Introduction](#introduction)
   * [Building image](#building-image)
   * [Usage](#usage)

# Introduction

This project creates a [Docker](http://docker.com) image which contains the Go dependency management tool **dep**.

The **dep** binary is installed from [https://github.com/golang/dep](https://github.com/golang/dep).

# Building image
Clone this repository and then run:

```
> docker build -t alpine-dep .
```


# Usage

The container runs **dep** so you just pass in parameters as if calling bare-metal version of **dep**. It is necessary to:
* Volume map your directory
* Set _GOPATH_

For example, take the following simple layout:

```
$ tree 
.
└── src
    └── github.com
        └── yourname
            └── super
                └── main.go
```

The tool would be run as so:
```
$ docker run -it -e GOPATH=/go -v `pwd`:/go -w /go hipposareevil/alpine-dep init src/github.com/yourname
Using master as constraint for direct dep...
Locking in master....
$
```

Which results in a **vendor** directory in your source:
```
$ ls -la src/github.com/yourname/vendor
total 0
drwxr-xr-x  3 you group   102B Jan  9 11:29 github.com/
$
```

You can then build your project as normal
```
$ export GOPATH=`pwd`
$ go build github.com/yourname/super
$ ls
pkg src super
$ ./super
```
