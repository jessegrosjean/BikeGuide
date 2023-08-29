# Text Formatting

Bike supports bold, italic, code, highlight, and strikethrough text formatting. You can also insert links.

Bike's rich text editing should be familiar, but also has a few innovations. My goal is to make Bike formatting precise like Markdown, but without all the syntax characters. Please watch this Rich Text Intro movie to see how the new features work:

{% embed url="https://vimeo.com/758067700" %}
Rich Text Intro
{% endembed %}

#### Link Buttons

There is often an overlap between commands that activate links and commands that edit links. This can make both tasks more difficult.

Bike solves this problem with link buttons. After each link a button is added. Click the button to activate the link. Click and edit the text normally without fear of activating the link.

To edit the URL associated with a link right click on the link text or link button and choose "Edit Link" from the popup menu.

#### Typing Affinity

Rich text editing can be frustrating at formatting boundaries. There is no precise way to specify what formatting to apply when inserting text at a boundary.

Bike solves this problem with a new concept: _Typing Affinity_. When your text caret is at a formatting boundary Bike attaches a new affinity bar to the bottom of the caret. Point that bar at the formatting you want applied to inserted text.

#### Formatting Popover

Rich text formatting is command based, to make text bold you need to use the bold command. This is strait forward, but can become slow if you can't remember the right keyboard shortcuts.

Bike's formatting popover makes this easier. You only need to learn one keyboard shortcut (`Command-E`) to show the formatting popover. Once the popover is showing you can use single key shortcuts to apply formatting commands. These shortcuts are shown in the popover so you don't need to memorize them.

#### Visible Typing Attributes

Normally when you type, the text is formatted the same as surrounding text. But there are some cases where this isn't true. For example if you have an empty selection and choose "Bold" then the text you type will be different then the sourounding text.

Bike indicates this hidden formatting state by showing the hidden attributes as part of the text caret. For example in the above example the bold "B" icon would show above the text caret.
