---
title: "Hello Web Go"
date: 2019-03-14T14:38:04-07:00
draft: false
---

This is the most basic web server written in Go.

{{< highlight go >}}
package main

import (
	"fmt"
	"net/http"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintf(w, "Hello World!")
	})
	http.ListenAndServe(":80", nil)
}
{{< /highlight >}}