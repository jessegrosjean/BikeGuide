# Using the Choice Palette

Quickly find and select items in long lists. Bike's choice palette is used by the "Go > Focus Heading" and "Format > Add Link to Row" commands. In the future I expect the choice palette to show up in other places too.

<figure><img src="../.gitbook/assets/Focus Heading 1.png" alt=""><figcaption><p>Choice palette</p></figcaption></figure>

#### To make a choice

* Use up/down arrow keys (or mouse) to select an item
* Press Return key to select choice and close the palette
* Press Escape key to close the palette without selecting an item

#### To filter the available choices

Filtering is really fast, even in big outlines!

<figure><img src="../.gitbook/assets/Focus Heading 2.png" alt=""><figcaption><p>Choice palette while filtering</p></figcaption></figure>

* Start typing to filter the list
* Filtering is "fuzzy". Matches must have all searched letters, but they can also have unmatched letters between.
* Results are ordered by how well they match, this can be configured in the palettes settings.
* Acronym filtering (first letter of each word) works well when looking for shorter "topic" rows.
* Filter method can be customized with special characters:
  * Insert `!` at the start to invert the matching
  * Insert `'` at the start to use substring matching
  * Insert `^` at the start to use prefix matching
  * Insert `$` at the end to use suffix matching

When you filter your list the containing parent items of each match are always included in the results, even if those containing items don't match.

### Choice Palette Settings

To the right of the filter field is the settings button. Each choice palette has its own settings. The settings for "Focus Heading" can be different then the settings for "Add Link to Row".

#### Sort by match quality

When checked the best matches are shown first. Otherwise the outline is only filtered, leaving items in natural order.

#### Remove duplicate containing parent items

When the above sorting option is selected you might see duplicate containing parent items in your filter results.

<figure><img src="../.gitbook/assets/Filter Options.png" alt=""><figcaption><p>No filter, sorted matches, sorted matches + remove duplicates</p></figcaption></figure>

1. Unfiltered list
2. Filtered list, matches sorted.
   * Notice how item "one" shows up in the list twice. This is because the results are ordered and item "two > a" shows up after item "one > a", but before item "one > ba".
   *   This view is nice because you see all top matches at the top of your list. The drawback is that you may see duplicate containing items, sibling matches are not always listed together.


3. Filtered list, matches sorted, duplicates removed.
   * Notice that now the list contains no duplicates.
   * This view is nice because the sibling items "one > a" and "one > ba" are next to each other and there are no duplicates. The drawback is that while the item "two > a" is the second best match, it is shown in the third position.

#### Outline Path

Use the "outline path" setting to specify the rows that you'll see and filter.

For example the "Focus Heading" commands shows "heading" type and top level rows by default. If you would like to see different rows when you "Focus Heading" you can do that by customizing the outline path.

{% content-ref url="using-outline-paths.md" %}
[using-outline-paths.md](using-outline-paths.md)
{% endcontent-ref %}
