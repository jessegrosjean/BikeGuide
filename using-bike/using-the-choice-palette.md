# Using the Choice Palette

Bike's choice palette allows you to quickly find and select items in long lists. The choice palette is used by the Go > Focus Heading and Format > Add Link to Row commands. In the future I expect it will show up in other places too.

#### Show choice palette

When the choice palette is shown it lists all items. Use the arrow keys or mouse to select an item. Press Return to choose that item and close the palette.

Here you can see I've just selected Go > Focus Heading in my Moby Dick test document. In this example I have adjusted the palettes settings (see below) so that all rows are listed, instead of just headings and top level rows.

<figure><img src="../.gitbook/assets/Focus Heading 1.png" alt=""><figcaption><p>Choice palette before filtering</p></figcaption></figure>

#### Filter choice palette

To filter the list of items start typing. The search is "fuzzy", and by default the results show ordered by how well they match.

Here you can see that I've typed "md" and that has matched "Moby Dick" as my best match. Using acronyms in the filter field works well for short items.

Filtering is really fast, even in big outlines.

<figure><img src="../.gitbook/assets/Focus Heading 2.png" alt=""><figcaption><p>Choice palette while filtering</p></figcaption></figure>

#### Use special characters to control filtering

* Insert `!` at the start to invert the matching
* Insert `'` at the start to use substring matching
* Insert `^` at the start to use prefix matching
* Insert `$` at the end to use suffix matching

#### Choice Palette Settings

To the right of the filter field is a settings button. Each choice palette has it's own settings. For example the settings for "Focus Heading" can be different then the settings for "Add Link to Row".

* _Sort by match quality_: When checked the list isn't just filtered, matching items are also ordered by how well they match your query.
* _Remove duplicate parents_: When matches are ordered (if they have parents) it can be the case that parents get listed in the results multiple times. Check this option to remove those duplicate parents, but doing this will also mean that the results are no longer strickly sorted by match quality.
*   _Outline Path_: When the list of items is the rows in your outline you can use an "outline path" to select the set of rows that you'll see and filter.



    For example the "Go to heading" commands shows "heading" and top level rows by default. Maybe you don't use heading typed rows in your outline, and woud like to see level 1, 2, and 3 rows listed in the "Focus Heading" list.  You can do that by customizing the outine path.



    It's never that simple right... outline paths are a new in progress feature, and there's no documentation yet. Polishing and documenting outine path syntax will be a focus of the next Bike release.
