# Best of

- contentSize -> 680px = 75ch - I changed to 645px
- [Block reference](https://fullsiteediting.com/blocks/)
- [Google webfonts helper](https://gwfh.mranftl.com/fonts)
- [Core Blocks Reference](https://developer.wordpress.org/block-editor/reference-guides/core-blocks/)
- [github gutenberg src folder](https://github.com/WordPress/gutenberg/tree/trunk/packages/block-library/src)
- BEST OF BEST OF at bottom of page

...........................................................

## FSE Best Of

1. [Blocks](https://fullsiteediting.com/blocks/)

## HEADING: Preparations -Quick start guide

- By enabling the theme development mode in `wp-config.php`, you prevent caching of the theme.json configuration file and pattern files: `define( 'WP_DEVELOPMENT_MODE', 'theme' );`

## HEADING: Creating WordPress block themes

- The Site Editor: is where you will assemble and style blocks to create your templates, parts, and patterns
- You will likely have a **library of pattern and template designs** that you can reuse in your themes
- **You can include functions.php in your block theme, if you need to `enqueue stylesheets or JavaScript`**
- On the front, your templates and all your blocks are loaded in the `<body>` element, inside a `<div>` with the class `wp-site-blocks`. There is no way to remove this div

## HEADING: Block grammar, attributes, and supports

- `<!-- wp:blockname /-->`
- `<!-- wp:blockname -->` content `<!-- /wp:blockname -->`
- When you change block settings, WordPress stores the settings as a JSON object inside the block comment
- The block repeats the JSON data from the HTML comment as a CSS class and inline style
- When WordPress registers a block, it can add both `attributes` and `block supports`
- Attributes are often unique to the block. For example, the audio block registers `autoplay` and `loop` as block attributes
- In comparison, block supports are settings that are reused by many blocks.

## HEADING: Introduction to templates and template parts

- Templates are your base files
- A template part is an `1)` HTML file, `2)` a custom post type (wp_template_part), and `3)` a block

## HEADING: Combining templates and template parts

- Template parts are always self-closing because the content is inside the HTML files: `<!-- wp:template-part {"slug":"header"} /-->`
- The value of the slug parameter is your file name without the file ending
- If you want to be able to identify and target the header and footer with CSS, add a custom CSS className
- **The default HTML element for template parts is a `<div>`. You need to update the header and footer template parts to use the correct HTML elements using `tagName`**

```html
<!-- wp:template-part {"slug":"header","tagName":"header"} /-->
<!-- wp:template-part {"slug":"footer","tagName":"footer"} /-->
```

## HEADING: Adding the blog section

- In index.html, between the two template parts, add a group block with the tagName attribute with the value `main`. Next, update the `<div>` to `<main>`
- There are three layout types: Constrained, Default, Flex

## HEADING: Query loop block | Creating a single post template

- **_On the front page template and on custom page templates, you need to make sure that the "`Inherit query from template`" setting is toggled `off`, or nothing will show_**
- Each `query` block uses an inner block called a `post template`, a container block for post blocks like the title and content. WordPress passes data from the query to the post template and then repeats the blocks inside the post template for every post
- The title block must be a link. Including `"isLink":true` will add the correct link for each post inside the loop
- you only want to show the current post, so you need to remove the loop
- Since this is a single post, remove the `isLink` attribute from the post title

## HEADING: Archive templates

- You can create an archive.html file used for all taxonomies or combine it with different templates for categories (category.html) or tags (tag.html).
- If you want to create a template for a specific category, use the file name `category-{slug of the category}.html`

```html
<!-- wp:query-title {"type":"archive"} /-->
<!-- wp:term-description /-->
```

## HEADING: Customizing the search results page

- You can finally customize the search results page to display a message instead of a blank page when there are no search results
- Add a `search results title` block above the query to show users they are on the search page.

## HEADING: Enable or disable features

- It is not possible to choose which font weights should be available in the editor, and the weights do not change depending on the font family
- You can also reference previously registered styles. If you have registered a style for the root of the site, you can reuse the same style for a block using the term ref: `"text": { "ref": "styles.color.background" }`

## HEADING: Theme.json schema

```json
// Schema for theme.json version 2:
"$schema": "https://schemas.wp.org/wp/6.5/theme.json"
// Schema for theme.json version 3:
"$schema": "https://schemas.wp.org/trunk/theme.json"
```

## HEADING: **Creating theme.json**

- **To change the body link color, you need to use elements**
- add elements and then select which element to target; in this case, link

```json
{
  "styles": {
    "color": {
      "background": "var(--wp--preset--color--base)",
      "text": "var(--wp--preset--color--contrast)"
    },
    "elements": {
      "link": {
        "color": {
          "text": "var(--wp--preset--color--accent-2)"
        }
      }
    }
  }
}
```

## HEADING: Editing with a purpose: Which WordPress editor should I use?

- Templates are structural, they are meant for creating page layouts, and _NOT_ meant for writing new content
- Content suitable for templates can include initial branding, slogans, and similar types of shorter introductions, opening hours, current offers, and contact information

## HEADING: How to create WordPress themes without coding

> Very long page (Good for clients): https://fullsiteediting.com/site-creators/how-to-create-wordpress-themes/

## HEADING: How to create a custom palette with solid colors

- Appearance > Editor > Styles > Edit btn > Colors > Palette > Color Tab > Custom > Click the `+` symbol to add custom colors using the color picker
- The default name of the color is Color 1 - You can click on the name field to add your own color name

## HEADING: How to create a custom gradient palette

- Appearance > Editor > Styles > Edit btn > Colors > Palette > Gradient Tab > Custom > Click the `+` symbol to add custom colors using the color picker
- The gradient starts with two default colors. Clicking on the white circles will open the color modal, where you can use the color picker or enter your color values
- Clicking anywhere between the white circles will add a new color
- You can reposition the colors horizontally along the gradient by clicking, holding down the left mouse button, and dragging the circle
- You can adjust the angle by entering a numeric value or by using your mouse and dragging the small blue circle inside the larger white circle

## HEADING: Custom duotone filter

- You can change the duotone on a per-image basis
- Select your image block in the editor. In the block toolbar, select the "Apply duotone filter"
- Instead of selecting a duotone from the palette, either click on the color field or on the two texts that say Shadows and Highlights

## HEADING: **How to create a sticky header with a group block**

1. Step 1: Add a new group block. Move the block to the top of the editor
2. Step 2: Select and drag the header template part inside the new group. Make the header full-width
3. Step 3: Select the group block and open the position panel. Select the "Sticky" option
4. Step 4: Select the styles panel in the block settings sidebar and add a background color to the group. Without this, the site header will be transparent, and the blocks in the header will overlap your content, making it difficult to read

> **Sticky is not an option**???

## HEADING: Creating sidebars with blocks

There are a couple of different ways to create sidebars using blocks

1. You can use the columns block and add one or more sidebars
2. **create the sidebar as a template part**
3. or add it as part of the content in the block editor

If you want to use the same sidebar in multiple places, I recommend using the template part method because it will save you time. Editing a template part is similar to adding block widgets to a traditional sidebar. The difference is that you now choose exactly where you want the sidebar to show without depending on a developer to add the code.

There are two questions that I recommend that you ask yourself before creating your sidebars:

- Should the content (the blocks) in the sidebar be the same on every page or post?
- Where should the sidebar content show on small screens like mobile?

## HEADING: **Adding a sidebar template part**:

Select the template that you want to add the sidebar to. Insert a new template part block. You can place it anywhere in the editor because you will move it inside the columns block in a minute. Select the option “Start blank” - In the modal, enter a suitable name

## HEADING: **Adding the columns block and choosing the layout**:

> The entire section is good as is the next 2 sections

## HEADING: How to add file-based templates for custom post types

- `single-{cpt-name}.html`, e.g. `single-book.html`
- Use an HTML file if you want the Site Editor to list the template
- Use a PHP file if you need to: 1. Use conditionals, 2. Display post meta in the content
- Remember to place HTML templates in the templates folder and PHP templates in the theme’s root folder

> The rest of the notes are good as well

## HEADING: Theme.json color options

> All notes good

## HEADING: Theme.json typography: Font family

> All notes good

## HEADING: Downloading font files manually

- If you choose a font family on https://fonts.google.com/ and then select the “Download family” option, you will only receive a zip file with .ttf files.
- To get the .woff2 files, you can use a font converter script or an online converter. I recommend using this time-saving tool called [Google webfonts helper](https://gwfh.mranftl.com/fonts) to download the files

## HEADING: How to add a font size for a specific block

- Place your font size array inside `settings.blocks.blockname.typography.fontSizes` to make the size available for a specific block

## HEADING: How to add fluid font sizes to theme.json

- The root font size for the calculations is 16px. It is not possible to change this value. If you change it in your custom CSS, the calculations will still use 16px
- When you have enabled fluid font sizes in the typography setting, WordPress will use your size and calculate the `clamp()` values for you.
- Relying on WordPress for the calculations is one option, but you can also customize the minimum and maximum values for each size by adding min and max values to each size
- You can add styles to block types under styles.blocks.blockname.typography

## HEADING: Theme.json typography: Line height, font weight, and text decoration

- it is not possible to limit the font weights to those supported by the current font family. Many users have requested this feature, but the development team has not built it into Gutenberg yet

- Thin: 100 | Light: 300
- Extra Light: `200`
- Regular: `400`
- Medium: `500`
- Semi Bold: `600`
- Bold: `700`
- Extra Bold: 800 | Black: 900

## HEADING: Orientation: writing mode and vertical text

- lets users add vertical text
- **`writingMode` is disabled by default, set it to `true` to enable it**

## HEADING: The block editor layout controls

- **A group block placed in the Site Editor will have the setting "Inner blocks use content width" enabled - While the post content block disables the setting**
- You can specify the width of the inner blocks using the input fields in the layout panel
- **You can justify the blocks to the left, center, or right**
- When Inner blocks use content width is toggled off:
  - **The content width options in the layout panel are unavailable**
  - **The inner blocks are always justified to the left (on ltr) and fill the width of the parent block**

> for padding/margin you can set top, right, bottom, &/or left

## HEADING: BlockGap: What is blockGap

- The `blockGap` setting in theme.json sets the vertical spacing between blocks by adjusting the margins depending on the position of the block
- The first block on the webpage and the first inner block or child block have no top or bottom margin
- The following blocks have a top margin corresponding to the value in the theme.json setting, and no bottom margin
- The block editor also uses blockGap for horizontal spacing between columns, gallery items, buttons, and social icons. The control for blockGap in the editor is called Block spacing, and you can find it in the dimensions panel

## HEADING: Minimum height

- **the group and post content blocks support setting a minimum height via the dimensions panel**.
- This theme.json setting is opt-in. You enable it by setting appearanceTools to true or with settings.dimensions.minHeight

## HEADING: Theme.json layout and spacing options > Position

- The position setting is limited to the group block and to two options; default, and sticky. Position is disabled by default and can be enabled by enabling appearanceTools or with `position: {sticky: true}` - NOT WORKING
- you have to toggle off "Inner blocks use content width" for the outermost group to see the Sticky option

## HEADING: How to add hover and focus styles using theme.json

> https://fullsiteediting.com/lessons/how-to-add-hover-and-focus-styles-using-theme-json/

> Try to style `core/navigation-link` instead

```json
"core/navigation": {
  "elements": {},
  "typography": {
    "fontWeight": "500"
  }
},
```

- **every block that supports link color** has a setting with two tabs: one for the default link state, and one for hover
- There is no interface for the `:focus`, `:active`, or `:visited` link states
- Button blocks do not support link color and as such, does not have a setting for hover styles yet
- From Gutenberg version 13.6 **you can style `:hover`, `:focus `and `:active` states on link elements**.
- From Gutenberg version 14.1 **you can style `outline` and `:visited`**

1. The keys that you use to styles these states in theme.json are the same as the CSS pseudo classes:

- styles.elements.link.:hover
- styles.elements.link.:focus
- styles.elements.link.:active
- styles.elements.link.:visited
- styles.elements.link.outline
- see examples for elements.link:hover | blocks.core/site-title.elements.link:hover | elements.button:focus

> CHECK: https://developer.wordpress.org/block-editor/reference-guides/core-blocks/

## HEADING: Extending custom CSS with the & selector

- The new CSS options also introduce the possibility of using the `&` CSS selector to target nested elements and use pseudo selectors. For example, you could use it with `:not`, `:before` or `:after`
- the rest is interesting

## HEADING: Creating patterns in the block editor

- creating a new pattern for your site is as easy as selecting the block, opening the options menu (right-click) in the toolbar, and selecting "Create pattern"
- In the next step, you enter a name for the pattern and decide if it should be synced or not
- When you save your content and reload the editor, WordPress transforms your blocks into a pattern block

## HEADING: How to re-enable the Customizer in a block theme

- **in functions.php: `add_action( 'customize_register', '__return_true' );`**

## HEADING: Accessibility in full site editing themes

- it is up to the theme developer to use the blocks correctly and not break the block’s accessible features
- Provide visual keyboard focus highlighting in navigation menus and for form fields, submit buttons, and text links.
- **You can open submenu items on hover and by using a button with an aria-label, the aria-expanded attribute, and an SVG icon with `aria-hidden=true and focusable="false"`. The block has an option called “Open on click”. The option turns the parent menu item into a button that expands the submenus**
- The buttons that open and closes the navigation are keyboard accessible; you can also close the navigation by using the `escape` key.
- You can choose to use the menu button with a two line icon, or display the text “Menu”.
- All theme features that behave as buttons or links must use `<button>`, `<input>`, or `<a>` elements, to ensure native keyboard accessibility
- **The button block is a link element (`<a>`) and this is correct because they do not perform an action; they lead to a different part of the website**
- **Tip**: Did you know that you can place inline images or copy paste emojis in the button?
- WordPress adds the skip link automatically if the page contains a `<main>` element.
- **Theme authors create (ARIA) landmarks by changing the HTML element of their container blocks: `group`, `template part`, `query`, and `comments`**
- **You can change the tag by updating the `tagName` attribute in the markup or selecting the HTML element in the Advanced settings panel in the block settings sidebar**
- _If a particular role appears more than once on a page, provide an ARIA label for that role (?)_ - `SEE BELOW`
- Adding an aria-label in the markup of a block will sometimes cause a block validation error
- Having a descriptive aria-label when there are multiple navigation blocks is important for any user who navigates via landmarks
- **The navigation block automates this in WordPress 6.0, by using the menu name as the aria-label**
- **The aria-label does not have a separate input field and can not be different from the name. It is important that the user adds a descriptive name instead of the default menu name**
- _Because the navigation block requires a stored navigation ID to add the aria-label, you can not add the aria-label in the HTML block markup, therefore, there is nothing the theme author can do to add the aria-label_
- **Links within large sections of text must be underlined**
- **When links appear within a larger body of block-level content, they must be clearly distinguishable from surrounding content**
- **The underline is the only accepted method of indicating links within content**.
- Bold, italicized, or color-differentiated text is ambiguous and will not pass
- **Links in navigation-like contexts (e.g. menus, lists of upcoming posts in widgets, grouped post meta data) do not need to be specifically distinguished from surrounding content**
- **Links for headings and paragraph blocks have underlines by default, and it is up to the theme developer to not break this requirement by removing the underlines**
- **Most blocks do not have controls for removing or adding underlines, the exceptions are the navigation block and the read more block**
- **The text color on hover must have a contrast ratio of 4.5:1 against the default link text color and against the focused link text color**
- **The text color on focus must have a contrast ratio of 4.5:1 against the default link text color and against the hovered link text color**
- If there is no text decoration on linked text, there must be a 3:1 color contrast between the link text color and the surrounding non-link text color, in addition to the other color contrast requirements
- This requirement may first seem difficult, but it mostly applies to links that have no underline; as the theme developer, **you can prevent failure by not removing underlines on links** - **It requires additional styling to some blocks including the navigation block**
- All decorative images must be marked up such that they will be ignored by screen readers
- **Not all image blocks have alt text settings in the editor, but alt texts can be provided in the block markup or via the media library**
- **Full site editing and the block editor do not support inserting custom svg icons out of the box, and if the theme author implements it, they need to make sure it passes the requirements**
- Media resources must NOT auto start or change without user action as a default configuration (audio, video, or image/content sliders and carousels)

```html
<!-- role="region" 
 1. A region landmark must have a label
 2. The HTML section element defines a region landmark when it has an accessible name (e.g. aria-labelledby, aria-label or title).
 -->
<section aria-labelledby="region1">
  <h2 id="region1">title for region area 1</h2>

  ... content for region area 1 ...
</section>

<section aria-labelledby="region2">
  <h2 id="region2">title for region area 2</h2>

  ... content for region area 2 ...
</section>
```

## HEADING: What are block variations?

- **A full-width group block is one of the blocks that will be used most with full site editing because they work great as wrappers for other blocks**

........................................................................

## BEST OF BEST OF

- **You can include functions.php in your block theme, if you need to `enqueue stylesheets or JavaScript`**
- **The default HTML element for template parts is a `<div>`. You need to update the header and footer template parts to use the correct HTML elements using `tagName`**
- **_On the front page template and on custom page templates, you need to make sure that the "`Inherit query from template`" setting is toggled `off`, or nothing will show_**
- **To change the body link color, you need to use elements**
- **create the sidebar as a template part**
- **But you also need the group because the column block can not use other HTML elements other than a `<div>`**
- **You can update it by selecting the `group` inside the sidebar `column` and opening the `Advanced` panel in the block settings sidebar. There, find the HTML element option, and select `<aside>` in the select list**
- you want to download font files in `.woff2` format
- use this time-saving tool called [Google webfonts helper](https://gwfh.mranftl.com/fonts) to download the files
- **`writingMode` is disabled by default, set it to `true` to enable it**
- **A group block placed in the Site Editor will have the setting "Inner blocks use content width" enabled - While the post content block disables the setting**
- **You can justify the blocks to the left, center, or right**
- When Inner blocks use content width is toggled off:
  - **The content width options in the layout panel are unavailable**
  - **The inner blocks are always justified to the left (on ltr) and fill the width of the parent block**
- **the group and post content blocks support setting a minimum height via the dimensions panel**.
- **every block that supports link color** has a setting with two tabs: one for the default _link_ state, and one for _hover_
- From Gutenberg version 13.6 **you can style `:hover`, `:focus `and `:active` states on link elements**.
- From Gutenberg version 14.1 **you can style `outline` and `:visited`**
- **in functions.php: `add_action( 'customize_register', '__return_true' );`**
- **You can open submenu items on hover and by using a button with an aria-label, the aria-expanded attribute, and an SVG icon with `aria-hidden=true and focusable="false"`. The block has an option called “Open on click”. The option turns the parent menu item into a button that expands the submenus**
- `The button block is a link element` (`<a>`) `and this is correct because they do not perform an action; they lead to a different part of the website`
- **Tip**: Did you know that you can place inline images or copy paste emojis in the button?
- **Theme authors create (ARIA) landmarks by changing the HTML element of their container blocks: `group`, `template part`, `query`, and `comments`**
- **You can change the tag by updating the `tagName` attribute in the markup or selecting the HTML element in the Advanced settings panel in the block settings sidebar**
- _If a particular role appears more than once on a page, provide an ARIA label for that role (?)_
- **The navigation block automates this in WordPress 6.0, by using the menu name as the aria-label**
- **The aria-label does not have a separate input field and can not be different from the name. It is important that the user adds a descriptive name instead of the default menu name**
- _Because the navigation block requires a stored navigation ID to add the aria-label, you can not add the aria-label in the HTML block markup, therefore, there is nothing the theme author can do to add the aria-label_
- **Links within large sections of text must be underlined**
- **When links appear within a larger body of block-level content, they must be clearly distinguishable from surrounding content**
- **The underline is the only accepted method of indicating links within content**.
- **Links in navigation-like contexts (e.g. menus, lists of upcoming posts in widgets, grouped post meta data) do not need to be specifically distinguished from surrounding content**
- **Links for `headings` and `paragraph` blocks have underlines by default, and it is up to the theme developer to not break this requirement by removing the underlines**
- **Most blocks do not have controls for removing or adding underlines, the exceptions are the `navigation` block and the `read more` block**
- **The text color on `hover` must have a contrast ratio of 4.5:1 against the default link text color and against the focused link text color**
- **The text color on `focus` must have a contrast ratio of 4.5:1 against the default link text color and against the hovered link text color**
- This requirement may first seem difficult, but it mostly applies to links that have no underline; as the theme developer, **you can prevent failure by not removing underlines on links** - **It requires additional styling to some blocks including the navigation block**
- **Not all image blocks have alt text settings in the editor, but `alt` texts can be provided in the block markup or via the media library**
- **Full site editing and the block editor do not support inserting `custom svg icons` out of the box, and if the theme author implements it, they need to make sure it passes the requirements**
- **A full-width group block is one of the blocks that will be used most with full site editing because they work great as wrappers for other blocks**

Look into the following:

1. [Component Reference](https://developer.wordpress.org/block-editor/reference-guides/components/)
2. [Component Reference: Animate](https://developer.wordpress.org/block-editor/reference-guides/components/animate/)

> how to add attributes to a group block in a wordpress block theme

..................................................................

- [github gutenberg src folder](https://github.com/WordPress/gutenberg/tree/trunk/packages/block-library/src)

- LAYOUT: columns, column, group, separator, spacer - where are row, stack, and grid?
- POST RELATED: archives, categories, calendar, post-author-biography, post-author-name, post-author, post-content, post-date, post-excerpt, post-featured-image, post-template, post-terms, `post-time-to-read`, post-title,
- PAGE RELATED: page-list-item, page-list - are these links?
- QUERY: query-no-results, query-pagination-next, query-pagination-numbers, query-pagination-previous, query-pagination, query-title, query-total, query
- MEDIA: audio, avatar, cover (also layout category), embed, gallery, image, media-text, site-logo, video
- LINKS: buttons, button, home-link, loginout, more, navigation-link, navigation-submenu, navigation, nextpage, post-navigation-link, read-more, social-link, social-links, table-of-contents,
- HTML ELEMENTS: details, html, table,
- SPECIAL: file, freeform (use the Classic editor), missing, rss, shortcode, `tag-cloud`,
- DESIGN: pattern, template-part,
- TEXT: code, footnotes, heading, list, list-item, paragraph, preformatted, `pullquote`, quote, site-tagline, site-title, verse
- FORM: form-input, form-submission-notification, form-submit-button, form, search,
- COMMENTS: comment- > author-avatar, author-name, content, date, edit-link, reply-link, template, pagination-next, pagination-numbers, pagination-previous, pagination, title, comments, latest-comments, post-comments-count, post-comments-form, post-comments-link,
- HUH: block-keywords-shortcuts, block,
- DEPRECATED: comment-author-avatar, post-comment, text-columns
- MISSING: row, stack, grid, page break, latest posts, title, modified date, tags, term description, archive title, search results title,

...........................................................

## Best Of ...something else
