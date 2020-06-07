---
layout: post
title: "Everday XPath - Operators"
date: 2016-09-11 06:28
comments: true
categories:
---

This blog post is part of a series on XPath. The content comes from my ebook [EverydayXPath](http://www.everydayxpath.com). Part of the content from the book will be released to the public as blog posts. In this post, we explain what XPath does. We disect the components of an XPath expression. And why the context is the key to forming the expression.

<!-- more -->

## Operators

Operators are special characters that has special meaning in a XPath expression.

 * __Child__ `/`: Selects the children of the current context. When used at the beginning of the expression, it starts from the root node.
 * __Recursive descent__ `//`: Searches the document, descending from the current context. When used at the beginning of the expression, it searches the entire document from the root node.
 * __Dot__ `.`: Selects the current context. By default it takes the root node as the current context.
 * __Double dot__ `..`: Selects the parent of the current context.
 * __Wildcard__ `*`: Selecta all nodes from the current context.
 * __Attribute__ `@`: Selects the node with the attribute and returns the attribute.
 * __Attribute wildcard__ `@*`: Selects nodes with any attributes and returns the attributes.
 * __Round brackets__ `()`: Expressions within round brackets takes precedence in the evaluation order.
 * __Square brackets__ `[]`: Used both as a subscript operator and to encapsulate a filter expression.
 * __Addition__ `+`: Performs addition.
 * __Subtraction__ `-`: Performs subtraction.
 * __Division__ `div`: Performs division.
 * __Multiplication__ `*`: Performs multiplication.
 * __Modulo__ `mod`: Performs modulo

For the following examples, we would use the below as the reference document.

``` html
<table class="active">
  <tr>
    <td>Alpha</td>
  </tr>
  <tr>
    <td>Bravo</td>
  </tr>
  <tr>
    <td>Charlie</td>
  </tr>
  <tr>
    <td>Delta</td>
  </tr>
  <tr>
    <td>
      <table style="margin: 12px">
        <tr>
          <td>Echo</td>
        </tr>
        <tr>
          <td>Foxtrot</td>
        </tr>
      </table>
    </td>
  </tr>
</table>
```

### Child operator `/`

The child operator selects the children of the current context. When used at the beginning of the expression, it starts from the root node.

```
/table
```

This expression selects the `table` node starting from the root.

``` html
<table class="active">
  <tr>
    <td>Alpha</td>
  </tr>
  <tr>
    <td>Bravo</td>
  </tr>
  <tr>
    <td>Charlie</td>
  </tr>
  <tr>
    <td>Delta</td>
  </tr>
  <tr>
    <td>
      <table style="margin: 12px">
        <tr>
          <td>Echo</td>
        </tr>
        <tr>
          <td>Foxtrot</td>
        </tr>
      </table>
    </td>
  </tr>
</table>
```

```
/table/tr/td
```

This expression selects all the `td` nodes who are children of `tr` nodes who are children of the `table` node. Notice that it does not select the `td` nodes in the inner `table`.

``` html
<td>Alpha</td> <1>
<td>Bravo</td> <2>
<td>Charlie</td> <3>
<td>Delta</td> <4>
<td> <5>
  <table style="margin: 12px">
    <tr>
      <td>Echo</td>
    </tr>
    <tr>
      <td>Foxtrot</td>
    </tr>
  </table>
</td>
```

### Recursive descent `//`

The recursive descent operator searches the document, descending from the current context. When used at the beginning of the expression, it searches the entire document from the root node.

```
//td
```

This expression selects all the `td` nodes. Notice that it selects all the `td` nodes regardless of their parents.

``` html
<td>Alpha</td> <1>
<td>Bravo</td> <2>
<td>Charlie</td> <3>
<td>Delta</td> <4>
<td> <5>
  <table style="margin: 12px">
    <tr>
      <td>Echo</td>
    </tr>
    <tr>
      <td>Foxtrot</td>
    </tr>
  </table>
</td>'
<td>Echo</td> <6>
<td>Foxtrot</td> <7>
```

```
/table/tr/td//td
```

This expression selects all `td` nodes descending from the context of the `td` node of the root `table`. Notice that the first level `td` nodes are not selected.

``` html
<td>Echo</td> <1>
<td>Foxtrot</td> <2>
```

### Dot operator `.`

The dot operator selects the current context. By default it takes the root node as the current context.

```
//td[.="Bravo"]
```

This expression selects the `td` node whose value is `Bravo`.

``` html
<td>Bravo</td>
```

### Double dot operator `..`

The double dot operator selects the parent of the current context.

```
/table/tr/td/..
```

This expression selects the parent node of the `td` nodes.

``` html
<tr> <1>
  <td>Alpha</td>
</tr>
<tr> <2>
  <td>Bravo</td>
</tr>
<tr> <3>
  <td>Charlie</td>
</tr>
<tr> <4>
  <td>Delta</td>
</tr>
<tr> <5>
  <td>
    <table style="margin: 12px">
      <tr>
        <td>Echo</td>
      </tr>
      <tr>
        <td>Foxtrot</td>
      </tr>
    </table>
  </td>
</tr>
```

### Wildcard operator `*`

The wildcard operator selects all nodes from the current context.

```
/table/tr/td/table/*/td
```

This expression selects all `td` nodes from the inner `table`.

``` html
<td>Echo</td> <1>
<td>Foxtrot</td> <2>
```

### Attribute operator `@`

The attribute operator selects the node with the attribute and returns the attribute.

```
//table/@style
```

This expression selects the `table` with the `style` attribute and returns the `style` attribute.

``` html
style="margin: 12px"
```

### Attribute wildcard operator `@*`

The attribute wildcard operator selects nodes with any attributes and returns the attributes.

```
//@*
```

This expression selects all the attributes found in any of the nodes.

``` html
class="active" <1>
style="margin: 12px" <2>
```

### Round brackets operator `()`

Expressions within round brackets takes precedence in the evaluation order.

```
(//@*)[1]
```

The expression within the round brackets is evaluated first, and the first result is returned by the subscript operator.

``` html
class="active"
```

### Square brackets operator `[]`

Square brackets are either used as a subscript operator or to encapsulate a filter expression.

```
(//td)[2]
```

This expression selects the second `td` node from the results of the expression encapsulated in the round brackets operator.

``` html
<td>Bravo</td>
```

Here is another example as a subscript operator

```
/table/tr[2]
```

This expression selects the second `tr` node which is a child of the `table` node.

``` html
<tr>
  <td>Bravo</td>
</tr>
```

The following example shows the square brackets operator encapsulating a filter expression.

```
(//td)[last()]
```

This expression selects the last node from the results of the expression in the round brackets operator.

``` html
<td>Foxtrot</td>
```

Here is another example of encapsulating a filter expression.

```
//td[table]
```

This expression selects the `td` node which contains a `table` node.

``` html
<td>
  <table style="margin: 12px">
    <tr>
      <td>Echo</td>
    </tr>
    <tr>
      <td>Foxtrot</td>
    </tr>
  </table>
</td>
```

### Arithmetic operators `+ - * div mod`

The arithmetic operators evaluates expressions to return numerical results.

```
count(//td) + count(//table)
```

The `count` function returns the number of nodes returned from evaluating the expression. There are 7 `td` nodes and 2 `table` nodes which when added, returns 9.

```
9.0
```
