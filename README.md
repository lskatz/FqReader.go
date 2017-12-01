# FqReader

An implementation of the fastq reader found on Heng Li's site at https://github.com/lh3/readfq

## Installation

    go get github.com/lskatz/FqReader.go

## Example

The following code reads a fastq file via `stdin` and then prints it out again.
Essentially, this script transforms any fastq text into a four-line-per-entry
output.

    package main

    import (
      "bufio"
      "fmt"
      "os"
      "flag"
      "path"
      "github.com/lskatz/FqReader.go"
    )

    func main() {
      flag.Usage = func() {
        fmt.Fprintf(os.Stderr, "Usage: zcat file.fastq | %s > fourLines.fastq\n", path.Base(os.Args[0]))
        flag.PrintDefaults()
      }
      flag.Bool("Help", false, "Help menu")
      flag.Parse()

      var fqr FqReader.FqReader
      fqr.Reader = bufio.NewReader(os.Stdin)
      for r, done := fqr.Iter(); !done; r, done = fqr.Iter() {
        fmt.Println("@"+r.Name+"\n"+r.Seq+"\n+"+r.Qual)
      }

      return
    }

