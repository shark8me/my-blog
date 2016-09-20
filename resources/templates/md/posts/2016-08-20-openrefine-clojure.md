{:title "Using Clojure Expressions in OpenRefine"
 :layout :post
 :page-index 0
 :draft? false
 :tags ["Clojure instead of GREL OpenRefine" "OpenRefine Clojure"]
 }

Given my programming background, I seldom had a need to use Microsoft Excel (or other spreadsheet equivalents). However, in the Data Science field, I find that Excel is a preferred weapon of choice to get a quick idea about the data and its characteristics, especially used by folks from a non-programming background. It has the ability to filter, splice and pull up graphs (and a whole host of features that I know nothing about). 

While I've seen that Excel is a great tool for this task, I feel slightly hamstrung by the lack of programming support . I know there are macros and other geegaws available, but for someone coming from the Java/Clojure side of the world, investing the time to learn a new set of tools seems like an awfully large side project.

Enter [OpenRefine](http://openrefine.org/). At first glance, it looks like a web based version of an Excel-like tool, but some of its features hit the sweet spot

* It can be run on a server and accessed remotely. If the size of the data is big, it can be run on a server machine and accessed from a lightweight laptop
* It keeps a record of all edits and maintains a comprehensive history, which makes it easy to roll back to a particular version and see the list of operations/transformations done at each step.
* It has elementary support for faceting and graphing.
* Did I mention it is open source and free. There are also commercial API connectors for things like reconciliation and geo-coding.
* What I found most interesting was that among the 3 mini-languages supported for expressions, Clojure is one of them.
* It has excellent documentation, including, but not limited to, several how-to guides, [screencasts](https://github.com/OpenRefine/OpenRefine/wiki/Screencasts), a [wiki](https://github.com/OpenRefine/OpenRefine/wiki) and [a book](https://www.packtpub.com/big-data-and-business-intelligence/using-openrefine) as well.


I didn't find any documentation on using Clojure for doing transformations, so the rest of this post is on that topic.

## Variables available in expression REPL

The [Variables](https://github.com/OpenRefine/OpenRefine/wiki/Variables) lists the set of variables that can be accessed in the expressions window. However, the list of Clojure-accessible variables are just these:

* _value_: the value of the cell in the base column of the current row. This is an instance of the String class
* _row_ : the current row, an instance of [WrappedRow](https://github.com/OpenRefine/OpenRefine/blob/master/main/src/com/google/refine/expr/WrappedRow.java)
* _cells_ : the cells of the current row, with fields that correspond to the column names, an instance of [CellTuple](https://github.com/OpenRefine/OpenRefine/blob/master/main/src/com/google/refine/expr/CellTuple.java)
* _cell_ : the cell in the base column of the current row, an instance of [WrappedCell](https://github.com/OpenRefine/OpenRefine/blob/master/main/src/com/google/refine/expr/WrappedCell.java)

These are Java instances,not pure Clojure, and the API backing these objects supports only a few methods. 

## Transformations

Here are a few basic transformations:

* Getting the value of a cell 

```
(.-value (.-cell cell))
```

the easier alternative is just
```
value
```

* Get a different cell in the row (indexed by number, which is _1_ in this case)

```
(.getCellValue (.-row row) 1)
```

* Get a different cell in the row by column name

```
(.-value (.-cell (.getField cells "ColumnName" (new java.util.Properties))))
```

The above API requires a _java.util.Properties_ argument, which seems to be unused in this class.

* Concatenate 2 column values separated by a colon character, the current cell along with another cell in the same row

```
(str value ":" (.-value (.-cell (.getField cells "Column2" (new java.util.Properties)))))

```

* a Test that returns true if current cell and Column2 cell have non-empty values or not.

```
(every? true? (map empty? [value (.-value (.-cell (.getField cells "Column2" (new java.util.Properties))))]))
```

---

As we can see, the API is not very user-friendly or Clojuresque in nature. However for quick transformations such as creating facets, it is quite capable.


