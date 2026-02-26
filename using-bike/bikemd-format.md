# Bike Markdown Format

Bike Markdown (`.bikemd`) is Bike's text-based file format. It's a subset of standard markdown — open a `.bikemd` file in any markdown viewer and it will render reasonably well.

Because it's plain text, `.bikemd` works well with version control systems like Git and other text-based tools, while still supporting all Bike features.

### Markdown Subset

Bike Markdown uses standard markdown unordered lists to encode outline hierarchy. Each row is a list item (`- `), and tab indentation creates nesting:

```
- Parent
	- Child
		- Grandchild
	- Another child
```

This is valid markdown — any markdown renderer will display it as a nested list.

### Row Types

Within that list structure, Bike uses standard markdown syntax to represent different row types:

| Type            | Syntax        | Example                 |
| --------------- | ------------- | ----------------------- |
| Body            | `- `          | `- Regular text`        |
| Heading         | `- # `        | `- # Section title`     |
| Blockquote      | `- > `        | `- > Quoted text`       |
| Task            | `- [ ] `      | `- [ ] Incomplete task` |
| Task (done)     | `- [x] `      | `- [x] Completed task`  |
| Ordered list    | `1. `         | `1. First item`         |
| Unordered list  | `+ `          | `+ Bullet item`         |
| Code block      | `` - `...` `` | `` - `code content` ``  |
| Horizontal rule | `- ---`       | `- ---`                 |

Most of these — headings, blockquotes, task checkboxes, ordered lists — are standard markdown or widely supported extensions (like GFM task lists).

### Text Formatting

Inline formatting uses standard markdown syntax:

| Format            | Syntax        |
| ----------------- | ------------- |
| **Bold**          | `**text**`    |
| _Italic_          | `*text*`      |
| ~~Strikethrough~~ | `~~text~~`    |
| `Code`            | `` `text` ``  |
| Link              | `[text](url)` |

### Pandoc Attributes

Standard markdown has no way to attach metadata to rows or text spans. Bike rows and text can carry attributes — persistent IDs, classes, timestamps, custom data — that need to be preserved in the file.

Bike Markdown uses [Pandoc's attribute syntax](https://pandoc.org/MANUAL.html#heading-identifiers) to fill this gap. Attributes are written in curly braces and support IDs, classes, and key-value pairs:

```
{#identifier .class key="value"}
```

**Row attributes** appear at the end of a row:

```
- Important item {id="abc123" created="2024-01-15"}
- A note {type="note"}
- Styled row {.highlight}
```

**Inline attributes** use Pandoc's bracketed span syntax `[text]{attrs}` for formatting that has no standard markdown equivalent. For example, highlighted text:

```
- This has [highlighted text]{highlight} in it
```

Or custom attributes on a span of text:

```
- Price is [lo]{pizza}
```

Bike uses standard markdown syntax when it can (`**bold**`, `*italic*`, etc.) and falls back to `[text]{attrs}` only when there's no markdown equivalent.

### Frontmatter

Files can optionally begin with a JSON metadata block between `---` delimiters:

```
---
{"bikemd":true,"root-id":"c43J5daN"}
---

- First row
```

The frontmatter preserves document metadata like the root ID and spell-checker ignore words. Bike manages this automatically — you don't need to edit it by hand.

### Escaping

Characters that would normally be interpreted as row type prefixes are escaped with a backslash:

```
- \# This is not a heading
- \> This is not a blockquote
- \+ This is not a list item
```

Standard markdown escaping also applies for inline formatting characters like `*`, `` ` ``, `[`, `]`, etc.

### Example

Here's a complete example showing several features together:

```
---
{"bikemd":true,"root-id":"root123"}
---

- # Project Notes {#notes}
	- This has **bold** and *italic* text
	- A row with [highlighted words]{highlight}
	- > An important quote
	- [ ] Review the [documentation](https://example.com)
	- [x] Write initial draft {done}
	1. First step
	2. Second step
	+ A bullet point
	- `console.log("hello")`
- ---
- # Another Section
	- Regular body text {created="2024-02-26"}
```
