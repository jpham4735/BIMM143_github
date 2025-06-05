# Class 6: R functions
Jeremy Pham (A16830268)

- [1. Function Basics](#1-function-basics)
- [2. Generate DNA sequence](#2-generate-dna-sequence)
- [3. Generate Protein Function](#3-generate-protein-function)

## 1. Function Basics

Let’s start writting our first silly function to add some numbers:

Every R function has 3 things:

- Name (we get to pick this)
- Input arguments (there can loads of these separated by a comma)
- The body (the R code that does the work)

``` r
add <- function(x, y=10, z=0){
  x + y + z
}
```

I can just use this function like any other function as long as R knows
about it (i.e. run the code chunk)

``` r
add(1,100)
```

    [1] 101

``` r
add(x=c(1,2,3,4), y=100)
```

    [1] 101 102 103 104

``` r
add(1)
```

    [1] 11

Functions can have “required” input arguments and “optional” input
arguments. THe optional arguments are defined with an equals default
value (`y=0`) in the function definition.

``` r
add(x=1, y=100, z=10)
```

    [1] 111

> Question: Write a function to return a DNA sequence of a user
> specified length. Call it `generate_dna()`

``` r
#generate_dna <- function(size=5){}

students <- c("jeff", "jeremy", "peter")

sample(students, size = 5, replace= TRUE)
```

    [1] "jeff"   "jeremy" "jeremy" "jeremy" "peter" 

## 2. Generate DNA sequence

Now work with `bases` rather than `students`

``` r
bases <- c("A", "C", "G", "T")

sample(bases, size = 10, replace = TRUE)
```

     [1] "G" "G" "T" "A" "A" "A" "T" "C" "T" "C"

Now I have a working ‘snippet’ of code, I can use this as the body of my
first function version here:

``` r
generate_dna <- function(size=5){
  bases <- c("A", "C", "G", "T")
  sample(bases, size = size, replace = TRUE)
}
```

``` r
generate_dna(100)
```

      [1] "A" "A" "G" "G" "G" "C" "C" "T" "A" "G" "A" "T" "A" "T" "G" "T" "T" "T"
     [19] "G" "C" "G" "A" "G" "G" "T" "A" "C" "C" "A" "T" "A" "G" "G" "T" "G" "G"
     [37] "A" "A" "C" "G" "A" "A" "C" "T" "C" "C" "T" "C" "G" "C" "A" "C" "G" "C"
     [55] "T" "A" "C" "A" "G" "A" "C" "T" "G" "G" "A" "A" "G" "A" "A" "T" "T" "A"
     [73] "A" "T" "A" "G" "C" "A" "C" "G" "G" "G" "C" "G" "G" "A" "C" "T" "G" "A"
     [91] "C" "A" "A" "G" "T" "G" "T" "A" "C" "C"

``` r
generate_dna()
```

    [1] "C" "A" "A" "C" "A"

I want the ability to return a sequence like “AGTACCTG” i.e. a one
element vector where the bases are all together.

``` r
generate_dna <- function(size=5, together = TRUE){
  bases <- c("A", "C", "G", "T")
  sequence <- sample(bases, size = size, replace = TRUE)
  if(together){
  sequence <- paste(sequence, collapse ="")
  }
  return(sequence)
}
```

``` r
generate_dna()
```

    [1] "GAACA"

``` r
generate_dna(together = FALSE)
```

    [1] "T" "C" "G" "C" "G"

## 3. Generate Protein Function

We can get the set of 20 natural amino-acids from the **bio3d** package.

``` r
aa <- bio3d::aa.table$aa1[1:20]
```

> Question: Write a protein sequence generating function that will
> return sequences of a user specified length?

``` r
generate_protein <- function(size=3, together=TRUE){
  ## Get the 20 Amino-acids as a vector
  aa <- bio3d::aa.table$aa1[1:20]
  sequence <- sample(aa, size=size, replace=TRUE)
  
  ## Optionally return a single element string
  if(together){
    sequence <-paste(sequence, collapse="")
  }
  return(sequence)
}
```

``` r
generate_protein()
```

    [1] "SYD"

> Question: Generate random protein sequences of length 6 to 12 amino
> acids

``` r
generate_protein(7)
```

    [1] "VAVWYMD"

``` r
generate_protein(8)
```

    [1] "FKMCNCSA"

``` r
generate_protein(9)
```

    [1] "RKWIWVFSL"

``` r
#generate_protein(size=6:12)
```

We can fix this inability to generate multiple sequences by either
editing and adding to the function body code (e.g. a for loop) or by
using the R **apply** family of utility functions

``` r
sapply(6:12, generate_protein)
```

    [1] "FQYEVD"       "PKCWVRK"      "SRAKYGIR"     "CFRHDWHYM"    "SGHVVRKIAF"  
    [6] "WVHGKVRDCGA"  "DNMKSTEHLWYH"

It would be cool and useful if I could get FASTA format output

``` r
ans <- sapply(6:12, generate_protein)
ans
```

    [1] "PCIPNC"       "FPQRIDV"      "RMTTVRTW"     "TNVRCEPTG"    "TAEFNSCLQM"  
    [6] "FRGHSVGFVKS"  "AIWKIPQSRAMF"

``` r
cat(ans, sep="\n")
```

    PCIPNC
    FPQRIDV
    RMTTVRTW
    TNVRCEPTG
    TAEFNSCLQM
    FRGHSVGFVKS
    AIWKIPQSRAMF

I want this to look like FASTA format with an ID line, e.g.

    >ID.6
    KTCWRD
    >ID.7
    MPGNFIF
    >ID.8
    QLNPDRKF

The functions `paste()` and `cat()` can help us here…

``` r
cat(paste(">ID.", 6:12, "\n", ans, sep=""), sep="\n")
```

    >ID.6
    PCIPNC
    >ID.7
    FPQRIDV
    >ID.8
    RMTTVRTW
    >ID.9
    TNVRCEPTG
    >ID.10
    TAEFNSCLQM
    >ID.11
    FRGHSVGFVKS
    >ID.12
    AIWKIPQSRAMF

``` r
id.line <- paste(">ID.", 6:12, sep="")
id.line
```

    [1] ">ID.6"  ">ID.7"  ">ID.8"  ">ID.9"  ">ID.10" ">ID.11" ">ID.12"

``` r
id.line <- paste(">ID.", 6:12, sep="")
seq.line <- paste(id.line, ans, sep="\n")
cat(seq.line, sep="\n")
```

    >ID.6
    PCIPNC
    >ID.7
    FPQRIDV
    >ID.8
    RMTTVRTW
    >ID.9
    TNVRCEPTG
    >ID.10
    TAEFNSCLQM
    >ID.11
    FRGHSVGFVKS
    >ID.12
    AIWKIPQSRAMF

> Question: Determine if these sequences can be found in nature or are
> they unique? Why or why not?

I BLASTp searched my FASTA format sequences against NR and found that
length 6, 7, and 8 are not unique and can be found in the databases with
100% coverage and 100% identity.

Random sequences of length 9 and above are unique and can’t be found in
the databases.
