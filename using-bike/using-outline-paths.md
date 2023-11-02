# Using Outline Paths

{% hint style="warning" %}
Outline paths are a work in progress and only available in the current preview release of Bike. They aren't very useful yet, but they are looking for your testing and feedback.
{% endhint %}

## What is an outline path?

On your computer you use file paths to specify files.

Outline paths are similar, but used to specify rows in your outline. Simple outline paths look like file paths. More powerful and complex outline paths are also possible.

Outline paths don't do much on their own, but they are an important building block for later features such as stylesheets and filtering. Right now my focus is on outline paths and on tools to make outline paths easier to understand.

<details>

<summary>Outline paths are related to XPath</summary>

Do you know what XPath is?

If you do then you have a leg up in understanding Bike's outline paths.

Generally outline paths can be thought of as XPath, but with a different syntax and specialized behavior to search Bike outlines. The query model and capabilities of outline paths are very similar to what's possible with XPath. Learn more about [XPath](https://developer.mozilla.org/en-US/docs/Web/XPath).

</details>

### Outline Path Explorer

<figure><img src="../.gitbook/assets/Outline Path Explorer.png" alt=""><figcaption><p>Outline Path Explorer</p></figcaption></figure>

The main tool that I'm working on is Outline Path Explorer.&#x20;

In the Outline Path Explorer you type an outline path in the text field at the top. The results are displayed in a label to the trailing side of the search field. Next you see a diagnostics text area that shows how your outline path was parsed. Last you see your outline with rows matching the path in green.

#### To open the Outline Path Explorer:

1. Open an outline where you would like to evaluate outline paths&#x20;
2. Open menu Window > Outline Path Explorer

### Basic paths

Unlike file paths the default test is "contains text" instead of "equals file name". Outline paths often have multiple matching rows.

*   `/a`

    Select top level rows containing "a"
*   `/a/b`

    Select "b" children of top level "a" rows
*   `.a`

    A relative path that selects the current row if it contains "a". Generally you won't need to use relative paths. But it's good to know that they exist, and good to know that paths need to start with `/` or `.` to match rows. Otherwise see "Value Expressions".

### Path Expressions

Use `union`, `except`, and `intersect` to combine the results of multiple outline paths.

*   `/a union /b`

    Top level rows that contain "a" or "b"
*   `/a except /b`

    Top level "a" rows removing "b" rows
*   `/a intersect /b`

    Top level rows that contain "a" and "b"
*   `(/a union /b) except /c`

    Top level rows that contain "a" or "b", but not "c"

### Path Steps

Paths are divided into steps. For example the path `/a/b` has two steps. Each step contains filtering logic. You don’t have to include all filtering options in each step. For example the following steps have the same  behavior.

*   `/a`

    Simple top level contains "a"
*   `/* a`

    Same behavior as above, but makes the row type test explicit. `*` means match "any" row type.
*   `/* @text contains "a"`

    Same behavior as above, but makes the predicate test explicit. See "Step Predicate" below to learn how predicates work.

#### Step Axes

By default each step passes the children of the matched rows to the next step. This is because "child" is the default axis. Other axes are also possible.

For example, say you want to search your entire outline for the text "pizza". That would be difficult if each step could only process the children of the previous step. To search the entire outline we would need to keep creating longer paths to seach each level of the outline:

* \`/pizza\`
* \`/\*/pizza\`
* \`/\*/\*/pizza\`

To solve this we can use the "descendant" axis. It selects all descendants of the rows passed into the step. You can search for pizza anywhere in your outline using the `//` descendant axis like this:

*   `//pizza`

    Descendant axis, selects all descendants of the outline root. They are then filtered to only the ones that contain pizza.

<details>

<summary>Advanced step axes </summary>

Another useful axis is "parent". This uses the same `..` sytax that file paths use to go to the parent directory.

*   `//pizza/..box`

    First use the descendant access to find "pizza". Then use the `..` parent axis select the parents of those "pizza" rows, and then filter those parents to those that contain "box. You've found the pizza boxes!

The above examples use the shortcut form of the descendant and parent axes. There is also a more general form where you enter the axis name followed by `::`. This is needed because there are more axes and some don't have shortcut forms:

*   `ancestor::`

    All ancestors of the rows passed into the step
*   `ancestor-or-self::`

    All rows passed into the step and their ancestors
*   `parent::` or shortcut `..`

    All parents the rows passed into the step
*   `self::` or shortcut `.`

    All rows passed into the step
*   `child::`

    All children of the rows passed into the step
*   `run::`

    All text runs of the rows passed into the step. This step is unique because you are filtering on text runs and their attributes, not rows. This query uses the text run axis to find all bold text in your outline `//*/run::@strong`.
*   `descendant::` or shortcut `//`

    All descendants of the rows passed into the step
*   `descendant-or-self::` or shortcut `///`

    All rows passed into the step and their descendants
*   `following-sibling::`

    All siblings after the rows passed into the step
*   `following::`

    All rows (in outline) following the rows passed into the step
*   `preceding-sibling::`

    All siblings before the rows passed into the step
*   `preceding::`

    All rows (in outline) before the rows passed into the step

</details>

#### Step Type

Each step can include a row type test at the start.

*   `//task`

    Match all rows of type task
*   `/heading//task`

    Match all tasks that are contained by a top level heading.
*   `//"task"`

    Match all rows that contain the text "task". When you want to search for text that in some way conflicts with outline path syntax put that text in quotes to make it a value.

<details>

<summary>List of row types</summary>

* `body`
* `heading`
* `quote`
* `code`
* `note`
* `unordered`
* `ordered`
* `task`
* `hr`
* &#x20;`*` Matches any type

</details>

#### Step Predicate

Each step can include a predicate test. You can then combine predicates with `and`, `or`, and `not`. Use `@` to name the row attribute to testing against.

*   `//@done`

    Matches rows that have a @done attribute.
*   `//not @done`

    Matches rows that do not have a @done attribute.
*   `//@text contains "get rich"`

    Match rows that contain the text "get rich". This example uses the `contains` relation.
*   `//@text contains "get rich" and not @done`

    Combine predicates. Use it to find all rows that will make you rich and are unfinished!

<details>

<summary>More on row attributes</summary>

Each row in your outline has associated attributes that you can use in outlihne path predicate tests.

Some attributes are built in to all rows, other attributes are optional and maybe be set by scripts or other features within Bike. For example when you click the checkmark of a task row it adds the @done attribute.

Open Window > Outline Path Explorer and notice that the outline view showns each row's attributes. The built in attributes include:

* `@id`
* `@type`
* `@level`
* `@text`

</details>

<details>

<summary>More on comparison relations</summary>

Use the following relations in your comparision predicates:

* `beginswith`
* `contains`
* `endswith`
* `matches`
* `=`
* `!=`
* `<`
* `<=`
* `>`
* `>=`

Use relation modifiers in brackets after the relation to change how it is evaluated. For example `beginswith[s]` will perform a case sensitive test instead of the default case insensitive test. The available modifiers are:

* `i` Case insensitive compare (default)
* `s` Case sensitive compare
*   `n` Numeric compare

    Values are converted to numbers before comparing. This means `"01"` will equal `"1.0"`, which is not true when doing the default string compare.

</details>

#### Step Slice

Each step produces a list of ordered matches. Use position based slicing if you want to limit the step results by position.

*   `//a[0]`

    Match the first row that contains "a"
*   `//a[-1]`

    Match the last row that contains "a"
*   `//a[1:]`

    Match all rows that contain "a", but skip the first.
*   `//a[:-1]`

    Match all rows that contain "a", but skip the last.
*   `//a[1:4]`

    Match second, third, and fourth rows that contain "a".

### Value Expressions

You have already seen lots of simple value expression such as `a` or `"a"`. That's maybe all you will ever need, but for some outline paths you might want more...

Normally you put values expressions on the right side of comparison predicates, but an entire outline path can also be a values expression. If you don't start your outline path with a `/` (global path) or `.` (relative path) then it is processed as a value expression.

You can see this in the Outline Path Explorer window, type "hello world". Notice that the message on the trailing side of the search field says "hello world" instead of telling you how many rows you matched. This is because "hello world" is a value expression that evaluates to "hello world".

*   `hello world`

    Unquoted text value expression that evaluates to `hello world`.
*   `"hello world"`

    Quoted text value expression that evaluates to `hello world`. Quoting is need when your text value conflicts with other outline path syntax.
*   `@attribute`

    Attribute value expression that returns the value of the attribute named "attribute" for the current row. This value expression will always return `nil` if it's not used within a path step.
*   `$variable`

    Variable value expression that returns the value of the variable named "variable". Currently no variables are set, but in the future I think they will be important for some advanced features. For example `$now` will be current time. `$focused` will be id of focused row. Those will be useful for outline paths in stylesheets.
*   `1` or `(1 + 1) / 2`

    Math value expression that evaluates to `1`. Math operators (`+`, `-`, `*`, `/`) require single whitespace on either side. This is so `/` doesn't conflict with path step separator. It doesn't make sense to use Math operators with text. `1 + "1"` is an invalid path. `1 + @attribute` is ok, but will return `nan` if the attribute can't be converted to a number.
*   `count(//a)`

    Function value expression that returns the count of rows containing "a".
