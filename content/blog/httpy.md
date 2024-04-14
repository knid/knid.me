+++
title = "Interact with APIs in a better way - HTTPy"
date = "2023-06-23"
description = "HTTPy is modern, user-friendly, programmable command-line HTTP client for the API."
tags = [
    "python",
    "api",
    "cli"
]
+++

[HTTPy](https://github.com/knid/httpy) is a command line HTTP client.
Its purpose is to make duplicate web requests on a single line.
httpy is designed for testing, debugging, and generally interacting with APIs and HTTP servers.
The `httpy` command allows creating and sending arbitrary HTTP requests.
They use simple and natural syntax and provide formatted and colored output.
Under favour of its programmable structure, it can perform many tasks at the same time.
For example, you can pull data for user IDs 0, 1, and 2 at the same time

## Features

* Expressive and intuitive syntax

* Formatted and colorized terminal output

* Programmable requests
    - Multiple requests one line

    - Value increment each time

    - Random number per request

    - Read each value from the lines in the file

    - Value per each request as a list of multiple values

* Built-in JSON support

* Arbitrary request data

* Custom headers

## Installation

```bash
$ pip3 install httpy-cli
```

## Usage

Simple get request:

```bash
httpy httpbin.org/get
```

Synopsis:

```bash
$ httpy <URL> <METHOD> <HEADERS,QUERIES,DATA> --exec <COMMAND> <ARGS>
```

We will use `api.service.com` as a API server for simulating requests.

Let's start with a simple request:

```bash
$ httpy api.service.com/users
```

This command will return all user objects. But if we want get only users with id 0, 1, 2, 3. Normally we have to do like this:

```bash
$ httpy api.service.com/users/0
  ...
$ httpy api.service.com/users/1
  ...
$ httpy api.service.com/users/2
  ...
$ httpy api.service.com/users/3
  ...
```

But we can do this in one line with `httpy`:

```bash
$ httpy 'api.service.com/users/{i}' -X i:0,1,2
```

This will be return all response for these ids.

`i` is arg name and it can be everything. And `-X` argument execute commands.

Variable name must be same with inside of `{}`.
We can use `{i}` in everything. Headers, query values etc.

We can simply more command:

```bash
$ httpy 'api.service.com/users/{i}' -X i:++:4
```

This command increment the variable each time.

The command syntax must be like this:

```bash
<VALUE>:<OPERATION>:<MAX_RUN>
```

We can use different operation:

|Operation            |Description
|---------------------|-------------------------------
| `++` | Increment
| `--` | Decrement
| `rand(0,10)` | Random number from 1 to 10
| `read(path/to/file)` | Read lines from file
| `item1,item2` | List
| `item` | Text

For exaple we can get random number and use in request:

```bash
$ httpy 'api.service.com/users/{i}' -X 'i:rand(1,10)'
```

or we can use file as a token list for deleting users:

```bash
$ httpy 'api.service.com/users/me' DELETE 'Authorization:{i}' -X 'i:read(tokens.txt)'
```

We can get only body with `-B` argument.


```bash
$ httpy 'api.service.com/users/{i}' -X i:3,4,5 -B
```

We can use other arguments for choosing what will see:

|Argument             |Description
|---------------------|-------------------------------
| `-B`, `--body`      | Only show body
| `-H`, `--header`    | Only show headers
| `-S`, `--status`    | Only show status

We can combine args. For example we can print only body and status with `-B` and `-S`:

```bash
$ httpy 'api.service.com/users/{i}' -X i:3,4,5 -B -S
```

That's it. You can check [project page](https://github.com/knid/httpy) for all "operations" and all usages.
