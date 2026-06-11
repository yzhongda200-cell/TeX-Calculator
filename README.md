# TeX-Calculator

A collection of arbitrary-precision decimal arithmetic macros for plain TeX and LaTeX.

The project currently provides macros for addition/subtraction, multiplication, nim-sum, and comparison. All macros are fully expandable from the first token, require no registers, and can be used directly in expansion-only contexts such as `\edef` and `\romannumeral`.

## Usage

First, download the files and place them in your working directory. Then load the files you want to use:

```tex
\input{numadd.tex}
\input{nummult.tex}
\input{nimsum.tex}
\input{ifcompare.tex}
```

The files are completely independent, so you only need to load the ones you actually use.

For complete examples, see `sample_for_LaTeX.tex` and `sample_for_pdfTeX.tex`.

The following sections describe the specifications and capabilities of these macros.

## Features

These macros have the following properties:

* Compatible with plain TeX
* Fully expandable from the first token
* Arbitrary-precision decimal arithmetic
* No registers used

This may sound impossible, but it is unfortunately true.

Because the macros are fully expandable from the first token, you can use them directly in expansion-only contexts:

```tex
\edef\result{\nummult{1.5}{3.141592}} % \result is defined as 4.7123880
```

You can even use them directly after primitives such as `\romannumeral`:

```tex
\romannumeral\numadd{20}{-16} % expands to iv
```

## Comparison

The file `ifcompare.tex` provides the expandable conditional macro `\ifcompare`.

Syntax:

```tex
\ifcompare[<number1>]<operator>[<number2>]{<true>}{<false>}
```

Supported comparison operators are:

* `<`
* `>`
* `=`
* `<=`
* `>=`
* `<>`

Examples:

```tex
\ifcompare[123.45]>[100]{true}{false}
% expands to true

\ifcompare[-1.5]<>[-1.5]
  {true}
  {false}
% expands to false
```

Like the arithmetic macros, `\ifcompare` is fully expandable from the first token and requires no registers.

## Performance and Limitations

Although the macros are advertised as arbitrary precision, practical limits are imposed by available memory.

With LaTeX engines, you can perform calculations involving tens of thousands or even hundreds of thousands of digits. On my machine, arithmetic on two 100-digit numbers takes approximately:

* Addition/Subtraction: about 1 second
* Multiplication: about 5 seconds
* Nim-sum: about 2 seconds

When using plain TeX engines such as pdfTeX, memory limits (particularly on argument size and expansion depth) significantly reduce the maximum supported precision. Nevertheless, you can still perform exact calculations involving several dozen digits.

## Limitations

The arguments of these macros are treated as numeric literals and are not expanded before processing.

Therefore, nested calls such as

```tex
\numadd{\numadd{1}{2}}{3}

\ifcompare[\nummult{2}{3}]=[6]{yes}{no}
```

are not supported.

Supporting such constructions would significantly reduce the maximum practical precision due to TeX's memory limitations.

## Future Plans

I plan to continue adding new functionality.

If you have any suggestions, feature requests, or bug reports, feel free to open an issue.
