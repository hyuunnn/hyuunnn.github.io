---
layout: post
title: "Top Down Operator Precedence - Pratt Parsing"
description: ""
date: 2024-01-07
tags: []
---

<a href="https://www.yes24.com/Product/Goods/103157156">밑바닥부터 만드는 인터프리터 in Go</a>

```
우리의 목표는 더 높은 우선순위를 가진 연산자를 포함하는 표현식이, 트리상에서 더 깊게 위치하도록 만드는 데 있다.
우리는 parseExpression 메서드에서 우선순위(precedence)를 인수로 넘겨서 이 목표를 달성했다. - p109
```

<a href="https://eli.thegreenplace.net/2010/01/02/top-down-operator-precedence-parsing">Top-Down operator precedence (Pratt) parsing - Eli Bendersky</a>

<a href="https://crockford.com/javascript/tdop/tdop.html">Top Down Operator Precedence - Douglas Crockford</a> (<a href="https://github.com/douglascrockford/TDOP">Github</a>)

<a href="https://journal.stuffwithstuff.com/2011/03/19/pratt-parsers-expression-parsing-made-easy/">Pratt Parsers: Expression Parsing Made Easy - Bob Nystrom</a>

<a href="https://11l-lang.org/archive/simple-top-down-parsing/">Simple Top-Down Parsing in Python - Fredrik Lundh</a>

<a href="https://tdop.github.io/">Top Down Operator Precedence - Vaughan R. Pratt</a>

<a href="https://en.wikipedia.org/wiki/Operator-precedence_parser">Operator-precedence parser - wikipedia</a>

<a href="https://www.oilshell.org/blog/2016/11/01.html">Pratt Parsing and Precedence Climbing Are the Same Algorithm</a>

<a href="https://www.engr.mun.ca/~theo/Misc/pratt_parsing.htm">From Precedence Climbing to Pratt Parsing</a>

<a href="https://www.oilshell.org/blog/2016/11/03.html">Pratt Parsing Without Prototypal Inheritance, Global Variables, Virtual Dispatch, or Java</a>
