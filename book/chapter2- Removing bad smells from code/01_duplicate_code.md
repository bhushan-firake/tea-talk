Patterns are not easy to understand, but they reward the effort of study. Over the last 3 years, I’ve been struck by one of the underlying principles that leads to
better designs: remove duplication. It’s also been highlighted by mantras in a couple of recent books: 

  1. the DRY (Don’t Repeat Yourself) principle in the Pragmatic Programmer (A. Hunt, and D. Thomas, Addison Wesley,1999) and 
  2. “Once and Only Once” from Extreme Programming Explained: Embrace Change (K. Beck, Addison Wesley, 1999).
  
The principle is simple: say anything in your program only once. Yet identifying and removing repetition from your code can lead to many interesting consequences. 
If you are determined to remove all repetition, it can lead you a long way toward a good design and can help you
apply and understand the patterns that are common in good designs.

Here is the discussion with key points:

### Name
Avoid Repetition.

### Summary
This improvement applies to the situation where you have spotted and identified duplicate/repeated code in your code base. This technique helps remove duplicate code using **The Rule of Three**


**The Rule of Three**

The first time you do something, you just do it. The second time you do something similar, you wince at the duplication, but you do the duplicate thing
anyway. The third time you do something similar, you refactor.

Tip: Three strikes and you refactor.

### Associated Pattern

[DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)

### Motivation

**Why**

We can apply this refactoring technique for following reasons:
 1. Remove multiple representations of information.
 2. Remove unintended duplication from our designs.
 3. Remove "copy and paste" code.
 4. Foster code reuse.

**Why NOT**

We should skip refactoring duplicate code in following circumstances:
 1. Removing duplicate code adds more complexity and reduces the readability of existing code.
 2. Removing duplicate code adds [ofuscation](https://en.wikipedia.org/wiki/Obfuscation).
 
### Mechanics

 1. Identify duplicate code blocks in your code base.
 2. Identify what is constant or common and what varies.
 3. Isolate common stuff from variations
 4. Remove the redundancy in the common stuff.
 
### Example
 
 **Scenario**

A client sells his products. For each product order, he generates invoice. Now, to access this invoice, he has different clients in his admin panel viz., A commandline client and a browser to see the invoice. So the developer codes for displaying his invoice for those two clients.

He writes following code for a commandline client:

```
//For commandline client

String cliStatement() {
  StringBuffer result = new StringBuffer();
  result.append("Bill for" + customer + "\n");
  Iterator it = items.iterator();
  while (it.hasNext()) {
   LineItem each = (LineItem) it.next();
   result.append("\t" + each.product() + "\t\ t" + each.amount() + "\n);
   }
   result.append("total owed: " + total + "\n");
   return result.toString();
  }

```

and for HTML client he writes:

```
//For HTML client
String htmlStatement() {
 StringBuffer result = new StringBuffer();
 result.append("<p>Bill for <i>" + customer + "</i></p>");
 result.append("<table>");
 Iterator it = items.iterator();
 while (it.hasNext()) {
  LineItem each = (LineItem) it.next();
  result.append("<tr><td>" + each.product() + "</td><td>" + each.amount() + "</td></tr>");
 }
 result.append("</table>");
 result.append("<p> total owed:<b>" + total + "</b></p>");
 return result.toString();
}
```

In both cases, the structure is
  1. Print some header for the invoice,
  2. Loop through each item printing a line, and
  3. Print a footer for the invoice.
  
Now, we will try to apply our **mechanics** on this example:

**1. Identify duplicate code blocks in the code**

- Declaring a StringBuffer
```
StringBuffer result = new StringBuffer();
```
- Iterating items
```
Iterator it = items.iterator();
  while (it.hasNext()) {
   LineItem each = (LineItem) it.next();
   
   }
```
- And last part to convert StringBuffer to a string
```
return result.toString();
```

**2. Identify what is constant or common and what varies**

From the code it is evident that the printing header of invoice, printing individual line of invoice and then printing total is different. Rest of the code in step-1 is constant.

**3. Isolate common stuff from variations**
To do this the developer can do something like below:

```
//For commandline client
String cliStatement() {
 StringBuffer result = new StringBuffer();
 result.append(cliHeader(this));
 Iterator it = items.iterator();
 while (it.hasNext()) {
  LineItem each = (LineItem) it.next();
  result.append(cliItem(each));
 }
 result.append(cliFooter(this));
 return result.toString();
}

public String cliHeader(Invoice invoice) {
 return "Bill for" + invoice.customer + "\n";
}
public String cliItem(LineItem line) {
 return "\t" + line.product() + "\t\t" + line.amount() + "\n";
}
public String cliFooter(Invoice invoice) {
 return "total owed:" + invoice.total + "\n";
}
```

and for HTML client as below:

```
String htmlStatement() {
 StringBuffer result = new StringBuffer();
 result.append(htmlHeader(this));
 Iterator it = items.iterator();
 while (it.hasNext()) {
  LineItem each = (LineItem) it.next();
  result.append(htmlItem(each));
 }
 result.append(footer(this));
 return result.toString();
}

public String htmlHeader(Invoice invoice) {
 return "<p>Bill for <i>" + invoice.customer + "</i></p><table>";
}
public String htmlItem(LineItem line) {
 return "<tr><td>" + line.product() + "</td><td>" + line.amount() + "</td></tr>";
}
public String htmlFooter(Invoice invoice) {
 return "</table><p> total owed:<b>" + invoice.total + "</b></p>";
}

```

**4. Remove redundancy from common stuff**

Now, looking at the code, we are immediately able to spot the common & redundant code. Obviously, both printing methods. We can still remove the redundancy and those separate bloating methods for each interface. 

OOP comes to rescue here. You can just define an interface (polymorphic interface to remove those bloating methods), and you are free to plugin different implementations of this interface to print your invoice in your chosen format.

```
//Interface for invoice printing
interface Printer {
 String header(Invoice iv);
 String item(LineItem line);
 String footer(Invoice iv);
}
```
The implementations can be like this:

```
//CLI Printer
class CliPrinter implements Printer {
 public String header(Invoice iv) {
  return "Bill for " + iv.customer + "\n";
 }
 public String item(LineItem line) {
  return "\t" + line.product() + "\t\t" + line.amount() + "\n";
 }
 public String footer(Invoice iv) {
  return "total owed:" + iv.total + "\n";
 }
}
```

```
//HTML Printer
class HTMLPrinter implements Printer {
 public String header(Invoice iv) {
  return "<p>Bill for <i>" + iv.customer + "</i></p><table>");
}
public String item(LineItem line) {
 return "<tr><td>" + line.product() + "</td><td>" + line.amount() + "</td></tr>");
}
public String footer(Invoice iv) {
 return "</table><p> total owed:<b>" + iv.total + "</b></p>");;
}
}
```

Refactored code is as below:

```
public String statement(Printer pr) {
 StringBuffer result = new StringBuffer();
 result.append(pr.header(this));
 Iterator it = items.iterator();
 while (it.hasNext()) {
  LineItem each = (LineItem) it.next();
  result.append(pr.item(each));
 }
 result.append(pr.footer(this));
 return result.toString();
}

public String cliStatement() {
 return statement(new CliPrinter());
}

public String htmlStatement() {
 return statement(new HTMLPrinter());
}
```

Looking at the code, we can notice that

1. There is no more duplication in the code.
2. It is very simple to understand
3. If you get any bugs, you can directly go to particular implementations of the interface and fix them
4. If you want to add another format to these two, you can easily add it by just implementation Printer interface.
