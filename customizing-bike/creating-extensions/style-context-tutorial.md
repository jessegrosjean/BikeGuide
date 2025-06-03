# Style Context Tutorial

Use the style context to create or modify Bike outline styles.

#### Style Context Summary

* [Style Context API](https://github.com/jessegrosjean/bike-extension-kit/tree/main/api/style).
* Entry point `style/main.ts`
* Fun custom styles, but complex!
* Use to define custom stylesheets for Bike’s outline editor.
* Import bike/style context API using `import { SYMBOL } from 'bike/style'`.

## Outline Styles Overview

Styles are powerful, but also quite complex.

This tutorial will show you how styles work and what they can do. If you decide to create your own style, you should also see the default outline style that's [included](https://github.com/jessegrosjean/bike-extension-kit/tree/main/src/!bike.bkext/style) in the Bike extension kit.

Each outline style is an ordered list of rules, organized into layer groups. A rule is composed of a relative [outline path](../../using-bike-2/using-outline-paths.md) and a callback function. The callback function is passed the editor state and a style object to modify. The purpose of layer groups is to allow rules to be inserted into (or imported from) existing outline styles.

To style an outline element:

1. A default style object is created.
2. A list of the rules that match the element is created.
3. The style object is passed to each matching rule and may be modified.
4. Through this process, the default style object is transformed into a specific style.

The order that you define your rules is important, because each rule can read the current state of the style object when deciding what styles it will apply. Generally, you want generic style rules listed first and refinements listed later.

The rule callbacks must be pure functions. Given the same editor state and style state, they must always generate the same end style state. They should only read values from the editor and style parameters when deciding what style state to set.

Styles are not inherited from parents as they are in CSS. Instead, you should create a generic rule that matches all elements and sets defaults there. Put this generic rule in the base layer of your theme and then follow it with more specific rules.

## Setup

Tutorial assumes that you have run the `npm run watch` command. Your extension should automatically build and install when you save changes.

## Create Outline Style

We will start by creating an empty outline style in `style/main.ts`:

<pre class="language-typescript"><code class="lang-typescript"><strong>import { defineOutlineStyle } from 'bike/style'
</strong>
let style = defineOutlineStyle('tutorial', 'Tutorial')
</code></pre>

Save and then select your style: Bike > Window > Style Sheets > Tutorial.

Notice that your outline editor now shows no indentation or formatting. Those things are defined by outline style rules, and this style has no rules.

Outline editing behavior is still there. You can still expand/collapse lines. You can still insert text. You can still select and edit text, though it will be difficult because you won't see selection marks.

## Define Visual Structure

In `style/main.ts` add back visual structure:

```typescript
import { defineOutlineStyle, Insets } from 'bike/style'

let style = defineOutlineStyle('tutorial', 'Tutorial')

style.layer('base', (row, run, caret, viewport, include) => {
  row(`.*`, (editor, row) => {
    row.padding = new Insets(10, 10, 10, 28)
  })
})
```

Save, to rebuild and install your changes. You should now see some indentation.

When adding rules we always add them to a layer. This helps to organize them, and makes it possible to modify existing styles by adding new rules to a specific layer.

Replace `style/main.ts` to see the structure more clearly:

```typescript
import { defineOutlineStyle, Insets, Color } from 'bike/style'

let style = defineOutlineStyle('tutorial', 'Tutorial')

style.layer('base', (row, run, caret, viewport, include) => {
  row(`.*`, (editor, row) => {
    row.padding = new Insets(10, 10, 10, 28)

    row.decoration('background', (background, layout) => {
      background.border.width = 1
      background.border.color = Color.systemBlue()
    })

    row.text.padding = new Insets(5, 5, 5, 5)
    row.text.decoration('background', (background, layout) => {
      background.border.width = 1
      background.border.color = Color.systemGreen()
    })
  })
})
```

The visual structure of your outline is now apparent. Rows (blue border) contain text (green border) and potentially other child rows.

The blue and green rectangles are created by attaching decorations to the row and to the row's text. Decorations can also be attached to runs of the row's text. Decorations are attached to the underlying text layout, but they don't affect it. If you need to make space for a decoration, add padding to the element you are decorating.

## Show Selection

Notice that when you select text in the outline editor, you can't see any selection.

In `style/main.ts`, add a selection rule:

```typescript
style.layer('selection', (row, run, caret, viewport, include) => {
  run(`.@view-selected-range`, (editor, run) => {
    run.backgroundColor = Color.textBackgroundSelected()
  })
})
```

Now you should see text selections when you select within a single paragraph. They will disappear when you select multiple paragraphs, but we'll fix that eventually.

How would you even know about that `@view-selected-range` attribute we just used? This is where the outline path explorer is useful. Select Window > Outline Path Explorer. Then make sure that "Show View Attributes" is selected. Then make some selections.

You should see the `@view-selected-range` attribute show up in the outline path explorer when you select a range of text. You can also type `.@view-selected-range` into the outline path explorer’s search field, and then the selected range of text will be highlighted green.

You can use the outline path explorer to find the attributes that you can use when styling, and you can also see which elements will be selected by your outline paths when the editor is in various states.

### Improving the selection

In our current selection rule, we are setting the run's background color. This is setting Core Text’s background color attribute. This works, but it's more flexible to use decorations.

In `style/main.ts`, replace the selection layer with:

```typescript
style.layer('selection', (row, run, caret, viewport, include) => {
  run(`.@view-selected-range`, (editor, run) => {
    run.decoration('selection', (selection, layout) => {
      selection.zPosition = 1
      selection.color = Color.textBackgroundSelected().withAlpha(0.5)
    })
  })
})
```

This is similar to how we used decorations to draw boxes around rows and row text. One difference is that in this case, we had to set the `zPosition` property. We want to make sure that the text selection draws behind the run's text.

In Bike, there are two selection modes–text selection mode and block selection mode. So far, we are only showing text selections. When you extend the selection beyond a single paragraph, Bike starts using block selection mode.

In `style/main.ts`, replace the selection layer to also show block selections:

```typescript
style.layer('selection', (row, run, caret, viewport, include) => {
  run(`.@view-selected-range`, (editor, run) => {
    run.decoration('selection', (selection, layout) => {
      selection.zPosition = 1
      selection.color = Color.textBackgroundSelected().withAlpha(0.5)
    })
  })
  
  row(`.selection() = block`, (editor, row) => {
    row.text.color = Color.white()
    row.text.decoration('background', (background, layout) => {
      background.color = Color.selectedContentBackground()
      background.border.color = Color.systemRed()
    })
  })
})
```

A few interesting things are happening here.

1. The matching outline path is calling the `selection` function with a `block` parameter value. This function will return true if the current row has block selection.
2. For this example, I am reusing the "background" decoration and changing its style. If I give the background rounded corners in my first rule, then the block selection will also have rounded corners. Alternatively, I could have created a new decoration with a new ID to indicate block selection.

## Show Inline Formatting

In `style/main.ts`, add support for bold and italic text:

```typescript
style.layer(`run-formatting`, (row, run, caret, viewport, include) => {
  run('.@emphasized', (editor, text) => {
    text.font = text.font.withItalics()
  })
  
  run(`.@strong`, (editor, text) => {
    text.font = text.font.withBold()
  })
})
```

While these rules are simple, you can also add decorations to text runs, just like you can add them to rows and rows’ text. Try creating a rule to show text with the `@highlight` attribute. You can add/remove that attribute from text by using Format > Highlight.

## More on Decorations

So far we've used decorations as simple backgrounds, but they can do more. They can be placed and sized. They can contain images, symbols, and text. Let's use decorations to style a task item with a checkbox.

In `style/main.ts` start by adding this rule:

```typescript
style.layer('row-formatting', (row, run, caret, viewport, include) => {
  row(`.@type = task`, (editor, row) => {
    row.text.decoration('mark', (mark, layout) => {
      mark.color = Color.systemRed()
    })
  })
})
```

With that rule in place, when you create a task in your outline, it will have a red background. Decorations can be more than just colors; they can also show images using the contents property.

In `style/main.ts`, modify the above rule to show an image:

```typescript
style.layer('row-formatting', (row, run, caret, viewport, include) => {
  row(`.@type = task`, (editor, row) => {
    row.text.decoration('mark', (mark, layout) => {
      mark.contents.gravity = 'center'
      mark.contents.image = Image.fromSymbol(
        new SymbolConfiguration('square')
          .withHierarchicalColor(Color.text())
          .withFont(Font.systemBody())
      )
    })
  })
})
```

We are creating a SF Symbol configuration. Then we use that configuration to create an image. Finally, we assign that image to the decorations’ contents. When you save this rule, you should see an empty checkbox centered over all task rows.

In `style/main.ts`, position the checkbox in a better location:

```typescript
style.layer('row-formatting', (row, run, caret, viewport, include) => {
  row(`.@type = task`, (editor, row) => {
    row.text.decoration('mark', (mark, layout) => {
      let lineHeight = layout.firstLine.height;
      mark.x = layout.leading.offset(-28 / 2);
      mark.y = layout.firstLine.centerY;
      mark.width = lineHeight;
      mark.height = lineHeight;
      mark.contents.gravity = 'center’;
      mark.contents.image = Image.fromSymbol(
        new SymbolConfiguration('square')
          .withHierarchicalColor(Color.text())
          .withFont(Font.systemBody())
      )
    })
  })
})
```

Decorations have `x`, `y`, `width`, and `height` properties of type `LayoutValue`. You get layout values from the passed-in `layout` parameter. These are logical values that are resolved later in the layout process to position the decoration.

In `style/main.ts`, add another rule for unchecked "done" tasks:

```typescript
style.layer('row-formatting', (row, run, caret, viewport, include) => {
  row(`.@type = task`, (editor, row) => {
    row.text.decoration('mark', (mark, layout) => {
      let lineHeight = layout.firstLine.height;
      mark.x = layout.leading.offset(-28 / 2);
      mark.y = layout.firstLine.centerY;
      mark.width = lineHeight;
      mark.height = lineHeight;
      mark.contents.gravity = 'center’;
      mark.contents.image = Image.fromSymbol(
        new SymbolConfiguration('square')
          .withHierarchicalColor(Color.text())
          .withFont(Font.systemBody())
      )
    })
  })

  row(`.@type = task and @done`, (editor, row) => {
    row.text.decoration('mark', (mark, layout) => {
      mark.contents.image = Image.fromSymbol(
        new SymbolConfiguration('checkmark.square')
          .withHierarchicalColor(Color.text())
          .withFont(Font.systemBody())
      )
    })
  })
})
```

## Editor

Each rule callback takes two parameters—editor and style object. So far, we've just been modifying the style object. We can also read from the editor settings that we can use in our style.

For example, you might add these lines to the first match-all `.*` rule:

```typescript
row.text.font = editor.theme.font
row.text.lineHeightMultiple = editor.theme.lineHeightMultiple
```

Now when you View > Text Size > Zoom In/Out, the outline text size will change in your theme. In addition to user settings, the editor also includes system state such as `isKey` or `isTyping`.

## Building Complex Outline Styles

Creating a full style that supports all of Bike's features is quite complex. Here are some ways to offload that work with existing outline styles, such as the default style that ships with Bike:

### Add Rules

It may be that you don't need to create a whole new style; maybe you just want to add a few rules to an existing style(s). You can do this using the `defineOutlineStyleModifier` API. This allows you to insert rules into existing outline styles.

### Import Rules

It may be that you do want to create a whole new style, but you want to import rules into that style from other outline styles. For example, maybe you want to include the "run-formatting" rules from the standard style.

You can include rules from other styles like this:

```typescript
let style = defineOutlineStyle('tutorial', 'Tutorial')
style.layer('row-formatting', (row, run, caret, viewport, include) => {
  include('bike', 'run-formatting')
})
```

### Copy / Modify

Last, you can copy an existing style. Rename it, and make modifications.

## Next Steps

* Study the extensions that come with the [Bike Extension Kit](https://github.com/jessegrosjean/bike-extension-kit).
* Read through the API's documentation in the [Bike Extension Kit](https://github.com/jessegrosjean/bike-extension-kit).
* Ask questions in the [Support Forums](https://support.hogbaysoftware.com/c/bike/22).
