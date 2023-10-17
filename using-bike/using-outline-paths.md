# Using Outline Paths

{% hint style="warning" %}
Outline paths are a work in progress and only available in the current preview release of Bike. They aren't very useful yet, but they are looking for your testing and feedback.
{% endhint %}

## What is an outline path?

On your computer you use file paths to specify files.

Outline paths serve a similar purpose, but are uses to specify rows in your outline. Simple outline paths look like file paths. More powerful and complex outline paths are also possible.

Outline paths don't do much on their own, but they are an important building block for stylesheets and filtering. For now my focus is on their design and on tools to make them easier to understand. In future releases I'll be using them to implement those other features.

* Bike > Window > Outline Path Explorer

### Basic paths

Unlike file paths the default test is "contains text" instead of "equals file name". Outline paths often have multiple matching rows.

*   `/a`

    Select top level rows containing "a"
*   `/a/b`

    Select "b" children of top level "a" rows

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

Paths are divided into steps. For example the path `/a/b` has two steps. Each step contains filtering logic.

You don’t have to include all filtering options in each step. For example the following steps have the same  behavior.

*   `/a`

    Simple top level contains "a"
*   `/* a`

    Same behavior as above, but makes the row type test explicit. `*` means match "any" row type.
*   `/* @text contains "a"`

    Same behavior as above, but makes the predicate test explicit. See "Step Predicate" below to learn how predicates work.

#### Step Axis

By default each step passes the children of matched rows into the next step. This is because "child" is the default axis, but other axis's are also possible.

*   `/a`

    Default child axis (what we've been using). Children of the outline root are passed to the step.
*   `//a`

    Descendant axis, all descendants of the outline root are passed to the step. This searches entire outline for "a".
*   `//a/..b`

    This has two non-default axis's. First use the descendant access to search outline for "a". Then use the ".." parent axis to pass the parents of the "a" rows and match the parents that contain "b".
* `/a` can also be written as `/child::a` to be more explicit. Some common steps have shortcut syntax, others need to be written out followed by `::`.

<details>

<summary>List of step axis</summary>

* `parent::` or shortcut `..`
* `self::` or shortcut `.`
* `child::`
* `descendant::` or shortcut `//`
* `descendant-or-self::` or shortcut `///`
* `following-sibling::`
* `following::`
* `preceding-sibling::`
* `preceding::`

</details>

#### Step Type

Each step can include a row type test. Only rows matching that type are eligible to match the step.

*   `//task`

    Match all rows of type task
*   `/heading//task`

    Match all tasks that are contained by a top level heading.
*   `//"task"`

    Match all rows that contain the text "task". When you want to search for text that in some way conflicts with outline path syntax you need to put that text in quotes.

<details>

<summary>Row Types</summary>

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

    Match rows that contain the text "get rich". See "Comparison relations" for a full list of comparisons you can make.
*   `//not @done and @text contains "get rich"`

    Combine predicates. This is a valuable outline path! Use it to find all rows that are unfinished and will make you rich.

<details>

<summary>Row attributes</summary>

Each row in your outline has associated attributes that you can use in predicate tests.

Some attributes are built in to all rows, other attributes are optional and maybe be set by scripts or other features within Bike. For example when you click the checkmark of a task row it toggles the row's @done attribute.

Use Window > Outline Path Explorer and notice that the outline shown in that window shows each row's attributes.

Built in attributes include:

* `@id`
* `@type`
* `@level`
* `@text`

</details>

<details>

<summary>Comparison relations</summary>

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

You can also include relation modifiers in brackets after the relation. For example `beginswith[s]` will perform a case sensitive test.&#x20;

The available modifiers are:

* `i` Case insensitive compare (default)
* `s` Case sensitive compare
* `n` Numeric compare. Values are converted to numbers before comparing. This means `"01"` will equal `"1.0"`, which is not true when doing the default string compare.

</details>

#### Step Slice

Each step produces an outline ordered list of matches that are passed on to the next step. Use slicing if you want to limit what is passed based on ordered position.

### Value Expressions
