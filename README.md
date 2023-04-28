# fixer

[![Build Status](https://travis-ci.org/peterhellberg/fixer.svg?branch=master)](https://travis-ci.org/peterhellberg/fixer)
[![Go Report Card](https://goreportcard.com/badge/github.com/peterhellberg/fixer)](https://goreportcard.com/report/github.com/peterhellberg/fixer)
[![GoDoc](https://img.shields.io/badge/godoc-reference-blue.svg?style=flat)](https://godoc.org/github.com/peterhellberg/fixer)
[![License MIT](https://img.shields.io/badge/license-MIT-lightgrey.svg?style=flat)](https://github.com/peterhellberg/fixer#license-mit)

Go client for [Fixer.io](https://fixer.io/) (Foreign exchange rates and currency conversion API)

> You need to [register](https://fixer.io/product) for a free access key if using the default Fixer API.

The default client loads its access key from the environment variable `FIXER_ACCESS_KEY`

## Installation

    go get -u github.com/hnord-vdx/fixer

## Usage examples

**SEK quoted against USD and EUR**

```go
package main

import (
	"context"
	"encoding/json"
	"os"

	"github.com/hnord-vdx/fixer"
)

func main() {
	resp, err := fixer.Latest(context.Background(),
		fixer.Base(fixer.SEK),
		fixer.Symbols(
			fixer.USD,
			fixer.EUR,
		),
	)
	if err != nil {
		return
	}

	encode(resp)
}

func encode(v interface{}) {
	enc := json.NewEncoder(os.Stdout)
	enc.SetEscapeHTML(false)
	enc.SetIndent("", " ")
	enc.Encode(v)
}
```

```json
{
 "base": "SEK",
 "date": "2017-05-24T00:00:00Z",
 "rates": {
  "EUR": 0.10265,
  "USD": 0.1149
 },
 "links": {
  "base": "https://api.fixer.io",
  "self": "https://api.fixer.io/latest?base=SEK&symbols=EUR%2CUSD"
 }
}
```

**Using the [Foreign exchange rates API](https://exchangeratesapi.io/) instead**

```go
package main

import (
	"context"
	"encoding/json"
	"os"
	"time"

	"github.com/hnord-vdx/fixer"
)

func main() {
	f := fixer.ExratesClient

	resp, err := f.At(context.Background(), time.Now(),
		fixer.Base(fixer.GBP),
		fixer.Symbols(
			fixer.SEK,
			fixer.NOK,
		),
	)
	if err != nil {
		return
	}

	encode(resp)
}

func encode(v interface{}) {
	enc := json.NewEncoder(os.Stdout)
	enc.SetEscapeHTML(false)
	enc.SetIndent("", " ")
	enc.Encode(v)
}
```

```json
{
 "base": "GBP",
 "date": "2020-04-17T00:00:00Z",
 "rates": {
  "NOK": 12.9739704293,
  "SEK": 12.4799374554
 },
 "links": {
  "base": "https://api.exchangeratesapi.io",
  "self": "https://api.exchangeratesapi.io/2020-04-18?base=GBP&symbols=NOK%2CSEK"
 }
}
```

## API documentation

<https://fixer.io/documentation>

## License (MIT)

Copyright (c) 2017-2020 [Peter Hellberg](https://c7.se/)

> Permission is hereby granted, free of charge, to any person obtaining
> a copy of this software and associated documentation files (the "Software"),
> to deal in the Software without restriction, including without limitation
> the rights to use, copy, modify, merge, publish, distribute, sublicense,
> and/or sell copies of the Software, and to permit persons to whom the
> Software is furnished to do so, subject to the following conditions:
>
> The above copyright notice and this permission notice shall be included
> in all copies or substantial portions of the Software.
>
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
> EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES
> OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
> IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM,
> DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
> TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE
> OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
