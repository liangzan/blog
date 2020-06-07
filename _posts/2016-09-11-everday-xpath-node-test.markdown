---
layout: post
title: "Everday XPath - Node Test"
date: 2016-09-11 06:11
comments: true
categories:
---

This blog post is part of a series on XPath. The content comes from my ebook [EverydayXPath](http://www.everydayxpath.com). Part of the content from the book will be released to the public as blog posts. In this post, we learn about the only mandatory part of the XPath expression: Node Test.

<!-- more -->

## Node Test

The node test is used as a filter to select the nodes we want. It is the only mandatory part in a XPath expression. We break a node test into 3 types.

 * __Name test__: It makes use of the inherent properties of a node to select it. Properties such as the type of the node.

 * __Node type test__: Nodes can be generalized as a comment node, a text node, a processing instruction node or a catchall node type. The node type tests helps to filter along these lines.

 * __Targeted processing instruction test__: This test selects processing instruction nodes with a defined type.

For the following examples, we would use the below as the reference document.

``` html
<table>
  <tr class="odd">
    <td>Alpha</td>
  </tr>
  <tr class="even" style="margin: 12px">
    <td>Bravo</td>
  </tr>
  <tr class="odd" style="margin: 12px">
    <td>Charlie</td>
  </tr>
  <tr class="even">
    <td>Delta</td>
  </tr>
  <tr class="odd">
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

## Name Test

The name test filter makes use of properties of the nodes.

### Asterisk `*`

The asterisk or the glob, selects everything.

```
/table/tr/td/*
```

This expression selects all the nodes that come after the `td` nodes of the root level `table` node. It then selects the inner `table` as it came after the `td` nodes.

``` html
<table style="margin: 12px">
  <tr>
    <td>Echo</td>
  </tr>
  <tr>
    <td>Foxtrot</td>
  </tr>
</table>
```

### QName

When we want to select something more specific, we use the *QName*. QName stands for qualified name and can be either a prefixed name or an unprefixed name.

```
QName = PrefixedName | UnprefixedName
```

A prefixed name is made up of 2 parts: a prefix and a localpart separated by a colon. The prefix is a name space prefix and is optional.

```
PrefixedName = Prefix ':' LocalPart
```

The unprefixed name and local part resolves to become a *NCName* which stands for non-colonized name: name without colons.

```
UnprefixedName = LocalPart
Prefix = NCName
LocalPart = NCName
```

`td` in the following expression below is a *NCName*. It means to select elements of the `td` type.

```
//td
```

The results returns a list of 7 `td` nodes.

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
<td>Echo</td> <6>
<td>Foxtrot</td> <7>
```

## Node Type Test

There are times when you need a broader selection than a *NCName* and narrower than a *glob*. In such cases, there are 4 kinds of node types test which you can make use of.

 * __comment()__: Selects comment nodes.
 * __node()__: Selects all nodes irregardless of type.
 * __text()__: Selects the text from the current context.
 * __processing-instruction()__: Selects the processing instruction nodes.

For the following examples, we would use the below as the reference document.

``` html
<table>
  <tr class="odd">
    <td>Alpha<img src="foo.jpg"></td>
  </tr>
  <tr class="even" style="margin: 12px">
    <td>Bravo</td>
  </tr>
  <tr class="odd" style="margin: 12px">
    <td>Charlie</td>
  </tr>
  <tr class="even">
    <!-- <td>Delta</td> -->
  </tr>
  <tr class="odd">
    <td>
      <table style="margin: 12px">
        <tr>
          <!-- <td>Echo</td> -->
        </tr>
        <tr>
          <td>Foxtrot</td>
        </tr>
      </table>
    </td>
  </tr>
</table>
```

### comment()

Selects comment nodes from the current context.

```
//tr[@class="even"]/comment()
```

This expression selects the comment nodes from the context of the `tr` node with a class with value *even*.

``` html
<!-- <td>Delta</td> -->
```

### node()

Selects all nodes irregardless of type, It even includes comment and processing instruction nodes.

```
//tr[@class="even"]/node()
```

This expression selects all the nodes from the context of the `tr` node with a class with value *even*.

``` html
<td>Bravo</td> <1>
<!-- <td>Delta</td> --> <2>
```

Compare this to the *glob*.

```
//tr[@class="even"]/*
```

*node()* captures everything, even more than the *glob*.

``` html
<td>Bravo</td>
```

### text()

Selects the text from the current context.

```
//td/text()
```

This expression selects the inner text from the nodes selected. Notice that it returns an empty string if the context does not contain any text.

``` html
'Alpha' <1>
'Bravo' <2>
'Charlie' <3>
'' <4>
'Foxtrot' <5>
'' <7>
```

### processing-instruction()

Selects the processing instruction nodes. Processing instruction nodes are usually found in XML documents. It usually carries meta information for applications. An example of a processing instruction node is below:

``` html
<?xml-stylesheet type="text/xsl" href="style.xsl"?>
```

It can be targeted. The example below selects only processing instructions with the target "xml-stylesheet"

```
processing-instruction("xml-stylesheet")
```
