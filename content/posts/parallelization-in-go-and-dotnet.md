---
title: "Parallelization in Go and .NET"
date: 2019-03-21T16:33:02-07:00
draft: false
---

This is a comparison of bounded parallel processing with error handling and cancellation in Go and .NET.

The sample code will take an array of strings and print the ASCII code representation of each string in parallel. For example "Lorem" would evaluate to `Lorem/76/111/114/101/109/`. If a string is empty, an error occurs and processing is canceled.

First, the Go code.

{{< highlight go >}}
package main

import (
	"context"
	"fmt"
	"strings"

	"golang.org/x/sync/errgroup"
)

func main() {
	parallelLimit := 4
	testData := strings.Split("Lorem ipsum dolor sit amet consectetur adipiscing elit", " ")

	g, _ := errgroup.WithContext(context.Background())
	dataCh := make(chan string)
	go func() {
		defer close(dataCh)
		for _, s := range testData {
			dataCh <- s
		}
	}()
	for i := 0; i < parallelLimit; i++ {
		g.Go(func() error {
			for s := range dataCh {
				ascii, err := toASCII(s)
				if err != nil {
					println(err.Error())
					return err
				}
				println(ascii)
			}
			return nil
		})
	}
	g.Wait()
}

func toASCII(data string) (string, error) {
	if data == "" {
		return "", fmt.Errorf("empty data not allowed")
	}
	s := data + "/"
	for i := 0; i < len(data); i++ {
		s += fmt.Sprintf("%d/", data[i])
	}
	return s, nil
}
{{< /highlight >}}

And now the .NET code.

{{< highlight csharp >}}
using System;
using System.Linq;
using System.Threading;

namespace dotnetP
{
    class Program
    {
        static void Main(string[] args)
        {
            var cts = new CancellationTokenSource();
            var parallelLimit = 4;
            var testData = "Lorem ipsum dolor sit amet consectetur adipiscing elit".Split(" ");

            testData.AsParallel()
                .WithDegreeOfParallelism(parallelLimit)
                .WithCancellation(cts.Token)
                .ForAll(s =>
                {
                    try
                    {
                        Console.WriteLine(ToAscii(s));
                    }
                    catch (Exception ex)
                    {
                        Console.WriteLine(ex.Message);
                        cts.Cancel();
                    }
                });

            Console.ReadLine();
        }

        private static string ToAscii(string s)
        {
            if (string.IsNullOrWhiteSpace(s)) throw new ArgumentException("empty data not allowed");
            var result = s + "/";
            foreach (char a in s) result += $"{(int)a}/";
            return result;
        }
    }
}
{{< /highlight >}}

It is debatable which is clearer, but I do like the elegance of Linq in C#.