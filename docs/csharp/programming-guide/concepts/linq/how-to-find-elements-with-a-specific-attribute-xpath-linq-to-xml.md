---
title: "How to find elements with a specific attribute (XPath-LINQ to XML) (C#)"
description: This C# example compares XPath to LINQ to XML for how they find elements that have a specific attribute.
ms.date: 07/20/2015
ms.assetid: daed00dd-923a-43be-8a90-eee406f6f574
---
# How to find elements with a specific attribute (XPath-LINQ to XML) (C#)
Sometimes you want to find all elements that have a specific attribute. You are not concerned about the contents of the attribute. Instead, you want to select based on the existence of the attribute.  
  
 The XPath expression is:  
  
 `./*[@Select]`  
  
## Example  
 The following code selects just the elements that have the `Select` attribute.  
  
```csharp  
XElement doc = XElement.Parse(  
@"<Root>  
    <Child1>1</Child1>  
    <Child2 Select='true'>2</Child2>  
    <Child3>3</Child3>  
    <Child4 Select='true'>4</Child4>  
    <Child5>5</Child5>  
</Root>");  
  
// LINQ to XML query  
IEnumerable<XElement> list1 =  
    from el in doc.Elements()  
    where el.Attribute("Select") != null  
    select el;  
  
// XPath expression  
IEnumerable<XElement> list2 =  
    ((IEnumerable)doc.XPathEvaluate("./*[@Select]")).Cast<XElement>();  
  
if (list1.Count() == list2.Count() &&  
        list1.Intersect(list2).Count() == list1.Count())  
    Console.WriteLine("Results are identical");  
else  
    Console.WriteLine("Results differ");  
foreach (XElement el in list1)  
    Console.WriteLine(el);  
```  
  
 This example produces the following output:  
  
```output  
Results are identical  
<Child2 Select="true">2</Child2>  
<Child4 Select="true">4</Child4>  
```  
  