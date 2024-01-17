---
layout: default
title: Mutable in d3.js
parent: NIMS 👩‍💻 
# nav_order: 1
---

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

### mutable in d3.js
[Observable Introduction to mutable state]("https://observablehq.com/@observablehq/mutable")   
[Observable ani-patterns and code smells]("https://observablehq.com/@tmcw/observable-anti-patterns-and-code-smells#mutation")

In JavaScript, mutation is defined as "directly changing the value of the original variable," meaning that it alters the value of the original variable rather than creating a new, modified version of it.

```
// Run this cell
a = [1, 2, 3, 4]
// a = Array(3) [2, 3, 4]
// Then run this cell a few times
a.splice(0, 1)
// Array(1) [1]
// Then run this one
a.length
// 4 
```

For example, in the given notebook, there are cells for defining an array and directly modifying its elements using the splice() method. When the second cell is run, the array is directly changed, so the part displaying the array length in the third cell does not automatically update. This is because the splice() method directly changes the array without returning a new value. In such cases, the notebook cannot know when or if the array is changed.

To avoid this issue, it's recommended to use alternatives that don't change the data or to make and modify a copy of the data. For instance, using Array#concat, which returns a new array instead of Array#push, or creating and modifying a copy of the data using Array#slice().

Similarly, with objects, it's recommended to change data by creating a new object rather than directly modifying the properties of an object. The spread syntax ... can be used for this purpose. For example, instead of adding a new property to a given object, create a new object that includes both the existing and new properties.

Observable notebooks consist of cells. Each cell is a piece of code (click on the left margin!), and it reacts to updates in other cells it references.

To make the most of this reactivity, it's generally best to avoid mutation in Observable code. Instead of imperatively accessing and setting values of other cells, each cell should typically return new values when executed. All cells referencing that value will execute whenever the parent cell recalculates.

However, sometimes you might face situations where manipulating this reactivity can be a bit tricky. For example:   

**_What if you want to update a cell only at certain points when the value of another cell changes?_**   
What if you want to use the value of another cell in a long-running cell like an animation loop, and you don't want to reset the loop every time the value changes?   
What if you want to programmatically set the value of a cell?
In these situations, there are two main options:

Functional Style: Rewrite the code in a functional style and change it to avoid mutation (usually the best method).   
**_Mutable Keyword: Use Observable's mutable keyword to define a mutable value in other cells. This value can be changed in other cells._**   

Using mutable allows you to control the timing of when the dependent cells should be re-evaluated. This helps ensure that cells share values without creating circular dependencies and prevents them from updating too frequently. At the same time, it ensures that any cells depending on them continue to receive normal reactive updates.







