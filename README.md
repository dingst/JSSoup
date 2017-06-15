JSSoup
=============================
I'm a fan of Python library BeautifulSoup. It's feature-rich and very easy to use. But when I am working on a small react-native project, and I tried to find a HTML parser library 
like BeautifulSoup, I failed.  
So I want to write a HTML parser library which can be so easy to use just like BeautifulSoup in Javascript.  
**JSSoup** uses [tautologistics/node-htmlparser](https://github.com/tautologistics/node-htmlparser) as HTML dom parser, 
and creates a series of BeautifulSoup like API on top of it.  
JSSoup supports both **node** and **react-native**.  

[![Build Status](https://travis-ci.org/chishui/JSSoup.svg?branch=master)](https://travis-ci.org/chishui/JSSoup)

# Naming Style
JSSoup tries to use the same interfaces as BeautifulSoup so BeautifulSoup user can use JSSoup seamlessly. 
However, JSSoup uses Javascript's camelCase naming style instead of Python's underscore naming style.
Such as `find_all()` in BeautifulSoup is replaced as `findAll()`.

# Install
```
$ npm install jssoup 
```

# How to use JSSoup
### Make Soup
```javascript
// react-native
var soup = JSSoup('<html><head>hello</head></html>');
// node
var soup = JSSoup('<html><head>hello</head></html>').default;
```
### Navigation
#### .previousElement, .nextElement
```javascript
var data = `
<div>
  <a>1</a>
  <b>2</b>
  <c>3</c>
</div>
`
var soup = JSSoup(data);
var div = soup.nextElement;
var b = div.nextElement.nextElement;
// b.string: '2'
var a = b.previousElement;
// a.string: '1'
```
#### .previousSibling, .nextSibling
```javascript
var soup = JSSoup(data);
var div = soup.nextElement;
var a = div.nextElement;
var b = a.nextSibling;
var c = b.nextSibling;
c.nextSibling == undefined;
```
#### .contents
```javascript
div.contents
// [<a>1</a>, <b>2</b>, <c>3</c>]
```
#### .descendants
```javascript
div.descendants
// [<a>1</a>, 1, <b>2</b>, 2, <c>3</c>, 3]
```
#### .parent
```javascript
div.parent == soup
```
### Edit
#### .extract()
```javascript
b.extract();
div.contents
// [<a>1</a>, <c>3</c>]
```
#### .append()
```javascript
b.extract();
div.append(b)
div.contents
// [<a>1</a>, <c>3</c>, <b>2</b>]
```
### Search
#### .findAll()
```javascript
var data = `
<div>
  <div class="h1"></div>
  <a>hello</a>
</div>
`
var soup = JSSoup(data);
soup.findAll('a')
// [<a>hello</a>]
soup.findAll('div', 'h1')
// [<div class="h1"></div>]
```
#### .find()
```javascript
var data = `
<div>
  <p> hello </p>
  <p> world </p>
</div>
`
var soup = JSSoup(data);
soup.find('p')
// <p> hello </p>
```
### Output
#### .prettify()
```javascript
var soup = JSSoup('<html><head>hello</head></html>');
soup.nextElement.prettify()
// <html>
//  <head>
//   hello
//  </head>
// </html>
```
#### .getText(), .text
```javascript
div.text
// '123'
div.getText('|')
// '1|2|3'
```
#### .string
```javascript
b.string == '2';
var soup = JSSoup('<html><head>hello</head></html>');
soup.string == 'hello';
```

# Run Test
```
npm test
```
# Status
There's a lot of work need to be done.

