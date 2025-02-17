# Full Site Editing

- download or view course material from https://github.com/carolinan/fullsiteediting.

## Full site editing for theme developers

> https://fullsiteediting.com/courses/full-site-editing-for-theme-developers/

### What is full site editing?

> https://fullsiteediting.com/lessons/what-is-full-site-editing/

1. [What is full site editing?](https://fullsiteediting.com/lessons/what-is-full-site-editing/)

- Check out the heading "Default colors and typography with global styles" - the `Style Book`

> you can select default colors, typography (including font families), width, and spacing for the website and different block types. This is also where you can add custom CSS

- WordPress saves the content, markup, and styles in custom post types
  - Template (wp_template)
  - Template part (wp_template_part)
  - Styles (wp_global_styles)
  - Navigation menus (wp_navigation)
  - Synced block patterns (and formerly reusable blocks) (wp_block)
- **`Export option`**: The export contains a copy of the active theme with the changes applied, and you can use it as a base for a new theme
- Full site editing is production-ready and it works especially well paired with custom block development
- Site Editor: the editor where you edit templates

. . . . . . . . . .

### Preparations -Quick start guide

> https://fullsiteediting.com/lessons/preparations-quick-start-guide/

Step 2, Optional: Enable development mode

- By enabling the theme development mode in `wp-config.php`, you prevent caching of the theme.json configuration file and pattern files
- `WP_DEVELOPMENT_MODE`: possible values are `core`, `plugin`, `theme`, `all`, and `""`
- Most usage today relates to theme.json caching
- `define( 'WP_DEVELOPMENT_MODE', 'theme' );`
- `WP_ENVIRONMENT_TYPE` (string) defines whether the site is a `local`, `development`, `staging`, or `production` environment

Step 4: Install a block theme:

- WordPress enables different features depending on what your active theme supports
- A block theme is required to enable the new Site Editor

. . . . . . . . . .

### Creating WordPress block themes

> https://fullsiteediting.com/lessons/creating-block-based-themes/

- The Site Editor: For most block theme projects, this is where you will assemble and style blocks to create your templates, parts, and patterns

Workflow:

1. Install the Create Block Theme plugin
2. Create a blank theme from the plugin options
3. Add a color palette and fonts in the Site Editor Styles panel
4. Create block templates based on the desired design
5. Export the changes to a theme .zip file
6. Tweak the theme files in your code editor
7. Repeat from step 4

> You will likely have a **library of pattern and template designs** that you can reuse in your themes

Step 1: Create the theme folder and style.css

- ✅

Step 2: Add your first block template

- create `/templates/index.html`

Step 3: Create the theme.json configuration file

- version | settings > layout > contentSize & wiseSize
- contentSize -> 680px = 75ch - I changed to 645px

Step 4: Include functions.php and theme support (optional)

- You can include functions.php in your block theme, if you need to enqueue stylesheets or JavaScript, add custom block styles and image sizes, or use hooks
- enqueue the style.css file for later use with `wp_enqueue_style()`
- The following theme support is automatically enabled for block themes: post-thumbnails, editor-styles, responsive-embeds, automatic-feed-links, html5 styles, and html5 scripts
- You do not need to include add_theme_support( 'title-tag' ) in the setup function, because WordPress already renders the title tag for full site editing themes
- Similarly, you do not need to register widget areas and menus and add theme support for a custom logo, custom header, or colors
- Adding theme support for `wp-block-styles` is optional. This file includes the combined CSS from the `theme.scss` file that some blocks use. For example, the padding when you add a background color to a group block and the default border of the quote block. Because this CSS is so opinionated and now considered legacy, my recommendation is not to include it

> how to make updates to the meta tags in the head of the theme

- When you create templates you will find that the header template part does not include a `<!DOCTYPE>`, `<html>`, or `<head>`. There is also no `<body>` element - WordPress already adds the necessary HTML elements and hooks

```php
<!DOCTYPE html>
<html <?php language_attributes(); ?>>
<head>
	<meta charset="<?php bloginfo( 'charset' ); ?>" />
	<?php wp_head(); ?>
</head>

<body <?php body_class(); ?>>
  <?php wp_body_open(); ?>

// (template)

  <?php wp_footer(); ?>
</body>
</html>
```

- If you need to change the meta tags, you need to use hooks and filters
- On the front, your templates and all your blocks are loaded in the `<body>` element, inside a `<div>` with the class `wp-site-blocks`. There is no way to remove this div

. . . . . . . . . .

### Block grammar, attributes, and supports

> https://fullsiteediting.com/lessons/block-grammar-basics/

- `<!-- wp:blockname /-->`
- `<!-- wp:blockname -->` content `<!-- /wp:blockname -->`
- Columns block example

```html
<!-- wp:columns -->
<div class="wp-block-columns">
  <!-- wp:column -->
  <div class="wp-block-column"></div>
  <!-- /wp:column -->

  <!-- wp:column -->
  <div class="wp-block-column"></div>
  <!-- /wp:column -->

  <!-- wp:column -->
  <div class="wp-block-column"></div>
  <!-- /wp:column -->
</div>
<!-- /wp:columns -->
```

- When you change block settings, WordPress stores the settings as a JSON object inside the block comment
- The block repeats the JSON data from the HTML comment as a CSS class and inline style

```html
<!-- wp:columns {"style":{"color":{"background":"#88bbdd"}}} -->
<div
  class="wp-block-columns has-background"
  style="background-color:#88bbdd"
></div>
```

- `{"style":{"color":{"background":"#88bbdd"}}}`
- `class="wp-block-columns has-background"` | `style="background-color:#88bbdd"`

The JSON data includes more than block attributes

- Calling it block attributes is a simplification: When WordPress registers a block, it can add both `attributes` and `block supports`
- Attributes are often unique to the block. For example, the audio block registers `autoplay` and `loop` as block attributes
- Settings for the attributes are created in the block’s `edit.js` file using basic components like the toggle control:

```jsx
<ToggleControl
	label={ __( 'Autoplay' ) }
	onChange={ toggleAttribute( 'autoplay' ) }
	checked={ autoplay }
	help={ getAutoplayHelp }
/>
<ToggleControl
	label={ __( 'Loop' ) }
	onChange={ toggleAttribute( 'loop' ) }
	checked={ loop }
/>
```

- if you wanted to remove these controls manually in your template markup, you would remove the keywords:

```html
<!-- wp:audio {"id":821} -->
<figure class="wp-block-audio">
  <audio
    controls
    src="http://example.com/wp-content/uploads/2008/06/originaldixielandjazzbandwithalbernard-stlouisblues.mp3"
    autoplay
    loop
  ></audio>
</figure>
<!-- /wp:audio -->
```

- In comparison, block supports are settings that are reused by many blocks. The blocks can register if they want full or partial support and if the control should be visible by default in the editors
- Alignments: There are two types of alignments, divided between supports and attributes:
  - Block alignments like wide and full width are registered as block supports
  - Text alignment is a block attribute
- Blocks that do not have wide/full-width block alignments use “align” to align text, e.g. with paragraph
- Some blocks have both wide/full-width block alignments and text alignments. These blocks use "`textAlign`" to align text and "`align`" to align the block

. . . . . . . . . .

### Introduction to templates and template parts

> https://fullsiteediting.com/lessons/templates-and-template-parts/

- Templates are your base files
- A template part is an `1)` HTML file, `2)` a custom post type (wp_template_part), and `3)` a block
- `parts/footer.html`

#### Combining templates and template parts

- You use the block markup for template part blocks to include them inside a template file
- Template parts are always self-closing because the content is inside the HTML files: `<!-- wp:template-part {"slug":"header"} /-->`
- think of that as the equivalent of `get_header();` with one important exception - The block markup adds a wrapping HTML element
- The value of the slug parameter is your file name without the file ending
- Just like blocks, templates and template parts are self-containing. The opening tag and the closing tag must be in the same template
- The default HTML element for template parts is a `<div>`. You need to update the header and footer template parts to use the correct HTML elements using `tagName`

```html
<!-- wp:template-part {"slug":"header","tagName":"header"} /-->
<!-- wp:template-part {"slug":"footer","tagName":"footer"} /-->
```

- If you want to be able to identify and target the header and footer with CSS, add a custom CSS className

#### Adding the blog section

- create the blog using a query loop and a post template block
- But first, you need to add a `<main>` element as a wrapper for the list of blog posts. By including the `<main>` element you also enable the automatic skip link
- In index.html, between the two template parts, add a group block with the tagName attribute with the value main. Next, update the `<div>` to `<main>`
- There are three layout types
  - Constrained: The blocks inside the group are centered horizontally. The width of the blocks is the value you added to contentSize in theme.json.
  - Default: The blocks inside the group are left aligned and fill the width of the container. In other words, this block is basically unstyled
  - Flex: Used with the row and stack group block variations.

#### Query loop block

- The query loop block has many advanced features

> On the front page template and on custom page templates, you need to make sure that the "`Inherit query from template`" setting is toggled `off`, or nothing will show

- Each query block uses an inner block called a post template, a container block for post blocks like the title and content. WordPress passes data from the query to the post template and then repeats the blocks inside the post template for every post
- The title block (previously called post title) must be a link. Otherwise, visitors can not reach the individual posts from the blog. Including `"isLink":true` will add the correct link for each post inside the loop
- You can use the post terms block to display any taxonomy terms; the category is only one variation
- Pagination: place the pagination block inside the query but outside the post template

#### Creating a single post template

- Save a copy of index.html as single.html
- you only want to show the current post, so you need to remove the loop
- Since this is a single post, remove the `isLink` attribute from the post title

#### Next and previous post navigation

- The post navigation link block adds a link to the next post, and the link to the previous post is a block variation.
- If there is no next or previous post to link to, then the block is not visible
- The markup for the two block variations is:

```html
<!-- wp:post-navigation-link {"type":"previous"} /-->
<!-- wp:post-navigation-link /-->
```

#### Creating the single page template

- save single.html as page.html
- Next, remove the unsupported blocks: post navigation, category, and tags

#### Archive templates

- You can create an archive.html file used for all taxonomies or combine it with different templates for categories (category.html) or tags (tag.html).
- If you want to create a template for a specific category, use the file name `category-{slug of the category}.html`
- If you don’t include the archive block templates in your theme, WordPress will use index.html, and your website will still show the correct content. The reason why I am suggesting that you create separate templates is to allow for customization and to display an archive title
- save index.html as archive.html - add an archive title, a variation of the query title block, above the query

```html
<!-- wp:query-title {"type":"archive"} /-->
<!-- wp:term-description /-->
```

#### Customizing the search results page

With the addition of the "no results" block in Gutenberg version 12.9, you can finally customize the search results page to display a message instead of a blank page when there are no search results. Like the post and comment template, it is used inside a query and is a container block where you place other blocks

- Copy index.html and save it as search.html
- add a search results title block above the query to show users they are on the search page.
- This block is a variation of the query title and is available from WordPress version 6.1. “Level: 2” means the heading level two is used (H2)
- WordPress shows the blocks inside the no results block instead of the post template, so it matters less where you position it as long as it is inside the query
- You can add a basic message, a search form, a list of the latest posts, or even use a second query

. . . . . . . . . .

### Global Styles & theme.json

> https://fullsiteediting.com/lessons/global-styles/

Global Styles have low CSS specificity: Styles added to individually selected blocks, including styles in a block template’s HTML markup, override Global Styles - only block themes use the new Styles interface

#### Main sections

- You separate the theme.json file into two main sections: settings and styles
- Settings:
  - The editor configuration. Enables or disables features like custom font sizes, custom padding, and colors
  - Defines preset values such as color palettes
- Styles:
  - Applies styling to the website or the blocks, with or without the generated CSS custom properties

#### Settings

- you can change: color, typography, layout, spacing, border, shadow, dimensions, position
- Typography:
  - FontFamilies contains an array with the following keys: fontFamily, slug, name, fontFace
  - FontSizes: slug, size, name
- Layout and content width: The layout category replaces `add_theme_support( 'align-wide' );` and defines the width of the content

Custom presets:

- When you create a theme.json file and add custom presets, you are not limited to using properties supported by the block. Compared to standard presets, they do not influence the block controls in the editor?

Presets for blocks

- With block presets, you can add settings to a specific block type?

#### Enable or disable features

> **Reminder**: When a feature is enabled, the editor control is available, but only for the blocks that have registered support

- AppearanceTools: enables various features
- Typography features:
  - customFontSize: to select a custom size value. Enabled by default
  - dropCap: Let the user add a drop cap to paragraphs. Enabled by default
  - fontWeight: Enabled by default
  - `lineHeight`: Opt-in, set to `true` to enable
  - fontStyle: Enabled by default
  - textDecoration: add an underline or line through the text. Enabled by default
  - textTransform: Let the user select uppercase, capitalized, and lowercase text. Enabled by default
  - writingMode: Let the user change the direction of the text to vertical. `Disabled by default`

> It is not possible to choose which font weights should be available in the editor, and the weights do not change depending on the font family

- Color:
  - custom: Let the user select custom colors using the color picker. Enabled by default
  - customGradient: Let the user select custom gradients using the color picker. Enabled by default
  - customDuotone: Let the user select custom highlights and shadows for the duotone. Enabled by default
  - `link`: Let the user select a link text color. Used for all link states. Opt-in, set to `true` to enable
  - background: Let the user select background colors. Enabled by default
  - text: Let the user select text colors. Enabled by default

#### Styles

Objects at the top level of the styles section are styles that affect the website. Objects inside "blocks" are styles that affect the named block type

Block Styles:

- Block styles override the default styles for blocks. WordPress applies the styles to all instances of the block.
- For block styles, the category name is always an existing property
- see [Styles](https://developer.wordpress.org/block-editor/how-to-guides/themes/global-settings-and-styles/#styles) and [Blocks](https://fullsiteediting.com/blocks/)
- You can also reference previously registered styles. If you have registered a style for the root of the site, you can reuse the same style for a block using the term ref: `"text": { "ref": "styles.color.background" }`

#### Template parts

With full site editing, there are three optional template part areas: header, footer, and uncategorized (general).

The section name is `templateParts`, followed by the template part slug (name) and the area. The title is the visible name in the editor

- `wp_template_part_area` is a taxonomy for the `wp_template_part` post type

#### Custom templates

With full site editing, you can list templates in theme.json.
The section name is `customTemplates`

- The first key is the name and slug of the template
- title is the template name that is visible in the editor
- `postTypes` is an array of supported post types

#### Patterns

- It is in the root level of theme.json and uses an array of block pattern slugs
- The slug of the pattern is the last part of the URL

#### Theme.json schema

```json
// Schema for theme.json version 2:
"$schema": "https://schemas.wp.org/wp/6.5/theme.json"
// Schema for theme.json version 3:
"$schema": "https://schemas.wp.org/trunk/theme.json"
```

#### Style priority

theme.json creates custom CSS properties and overrides default styles for blocks - specificity:

1. Styles on an individual block that users add in the block editor
2. Stylesheets that you enqueue: Specificity and use of `!important`
3. Styles that are added in the theme.json file and via the Styles interface in the Site Editor
4. Default block styles from WordPress

. . . . . . . . . .

### Creating theme.json

> https://fullsiteediting.com/lessons/creating-theme-json/

Styles

- To change the body link color, you need to use elements
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

.......................................................

## 2. Full site editing for site creators

> https://fullsiteediting.com/courses/full-site-editing-for-site-creators

- Choose from more premade designs with patterns
- Patterns will become more valuable to site creators with full site editing. You can choose from premade designs made by the theme developer and from the WordPress.org pattern directory

### How to switch your website to full site editing

> SKIP - READ IF A NEW CLIENT WANTS TO SWITCH

### Introduction to the Site Editor

- Creating new templates:
- Default styles for the website: Typography, Colors, Background, Shadows, Layout, Blockss -
- triple dot menu
  - Resetting Styles
  - Additional CSS:
- Stylebook: displays blocks per category: Text, Media, Design, Widgets, and Theme

### Editing with a purpose: Which WordPress editor should I use?

- How much content can I add to the templates: Templates are structural, they are meant for creating page layouts, and _NOT_ meant for writing new content
- Content suitable for templates can include initial branding, slogans, and similar types of shorter introductions, opening hours, current offers, and contact information

### How to create WordPress themes without coding

> SKIP - very long page: https://fullsiteediting.com/site-creators/how-to-create-wordpress-themes/

### Managing color palettes

How to create a custom palette with solid colors

- Appearance > Editor > Styles > Edit btn > Colors > Palette > Color Tab > Custom > Click the `+` symbol to add custom colors using the color picker
- The default name of the color is Color 1 - You can click on the name field to add your own color name

How to create a custom gradient palette

- Appearance > Editor > Styles > Edit btn > Colors > Palette > Gradient Tab > Custom > Click the `+` symbol to add custom colors using the color picker
- The gradient starts with two default colors. Clicking on the white circles will open the color modal, where you can use the color picker or enter your color values
- Clicking anywhere between the white circles will add a new color
- You can reposition the colors horizontally along the gradient by clicking, holding down the left mouse button, and dragging the circle
- In the type dropdown, you can select between linear or radial
- You can adjust the angle by entering a numeric value or by using your mouse and dragging the small blue circle inside the larger white circle
- You can add as many gradients as you need, name them, and remove them

Custom duotone filter

- You can change the duotone on a per-image basis
- Select your image block in the editor. In the block toolbar, select the "Apply duotone filter"
- Instead of selecting a duotone from the palette, either click on the color field or on the two texts that say Shadows and Highlights

### Theme style variations

> SKIP

### How to create a sticky header with a group block

1. Step 1: Add a new group block. Move the block to the top of the editor
2. Step 2: Select and drag the header template part inside the new group. Make the header full-width
3. Step 3: Select the group block and open the position panel. Select the "Sticky" option
4. Step 4: Select the styles panel in the block settings sidebar and add a background color to the group. Without this, the site header will be transparent, and the blocks in the header will overlap your content, making it difficult to read

### Creating sidebars with blocks

There are a couple of different ways to create sidebars using blocks

1. You can use the columns block and add one or more sidebars
2. create the sidebar as a template part
3. or add it as part of the content in the block editor

If you want to use the same sidebar in multiple places, I recommend using the template part method because it will save you time. Editing a template part is similar to adding block widgets to a traditional sidebar. The difference is that you now choose exactly where you want the sidebar to show without depending on a developer to add the code.

There are two questions that I recommend that you ask yourself before creating your sidebars:

- Should the content (the blocks) in the sidebar be the same on every page or post?
- Where should the sidebar content show on small screens like mobile?

#### Using the same sidebar on every page:

If you want every post or page to use the same sidebar, then you need to edit the default template for single posts and pages or any other template where you want a sidebar. In this case, your sidebar should be a template part

#### Using sidebars on only some posts or pages

If you only want to have a single sidebar but only on some posts or pages, you must create a new custom template in the Site Editor and assign it to your posts or pages in the block editor. In this case, your sidebar should also be a template part

#### Using different sidebars on different posts and pages

If you plan to use different blocks and create a unique sidebar depending on the post, I recommend adding the columns block directly into your post content using the block editor. Next, move all your content blocks inside the widest column. That way, you can view and edit the sidebar at the same time as you are editing your content.

#### Sidebars on small screens

The most common layout is to display the sidebar above or below the content on small screens. In a left-to-right environment, a sidebar that is to the left of the content will be above the content. And a sidebar on the right will be below. Because of that, I recommend only using a left sidebar if it has a minimal amount of content or if it contains the post title; because you want your title to come before the content

#### Creating the sidebar

**Adding a sidebar template part**:

Select the template that you want to add the sidebar to. Insert a new template part block. You can place it anywhere in the editor because you will move it inside the columns block in a minute. Select the option “Start blank” - In the modal, enter a suitable name

**Adding the columns block and choosing the layout**:

> The post template in Twenty Twenty-Three uses a **group** for the content located between the site header and site footer. Then it has a **second group** that contains the featured image and post title. Below the title is the post content block, and finally, the post meta information and the comments area.

- Inside the main group block, add a columns block
- Use a two-column layout. You can select the 33/66 variation for a left sidebar or the 66/33 variation for a right sidebar
- Next, move all the blocks related to the post content into the wide column. Move your new template part into the narrow column
- The first block you must add inside the sidebar column is a group block. Using a group will give you more styling options to choose from
- For example, the column has basic border settings but not border-radius, which is available for groups
- **But you also need the group because the column block can not use other HTML elements other than a `<div>`**
- Add the rest of your blocks to the sidebar inside the group
- In the column settings, you can adjust the width of the content area and the sidebar. Add a numeric value...
- By selecting the parent columns block, you can adjust the space between the content blocks and the sidebar - adjust the block spacing

**Accessibility concerns and tips**:

- In a left-to-right environment, users of assistive technology can read the content from the top left to the bottom right of the page
- Your post or page title should be a heading that describes the content, so it is important that you do not place the title after the content
- The same rules apply to right-to-left environments, except for the left side. If you use the table of contents, time to read, or navigation blocks, these should also be placed before the content
- If your sidebar contains complementary content like lists of links to other posts, category archives, advertising, or latest comments; then your sidebar should use the `<aside>` HTML element
- **You can update it by selecting the `group` inside the sidebar `column` and opening the `Advanced` panel in the block settings sidebar. There, find the HTML element option, and select `<aside>` in the select list**

**Wide and full-width content will not look the same in the editor and front**:

If you are using a block theme template with a sidebar, then the sidebar is not visible in the block editor, only on the front and in the site editor. That means that the sidebar does not take up space in the block editor. So when you add wide or full-width blocks, they make look wider in the block editor than on the front of the website. There is no good way around this mismatch

............... Lessons: Templates and template parts ...............

## 3. Creating blocktemplates for custom post types

> https://fullsiteediting.com/lessons/creating-block-templates-for-custom-post-types/

### How to add file-based templates for custom post types

The difference between creating a template file for a default post type and a custom post type is the naming of the file

- `single-{cpt-name}.html`, e.g. `single-book.html`
- A template for a specific book would be named `single-book-{book-title}.html`, and the archive would be called `archive-book.html`

> Use an HTML file if you want the Site Editor to list the template

> Use a PHP file if you need to: 1. Use conditionals, 2. Display post meta in the content, 3. Check if a plugin is active before adding a plugin block to the template, 4. Support a plugin that is not compatible with the Site Editor

Remember to place HTML templates in the templates folder and PHP templates in the theme’s root folder

> I think the rest of the page is off because it is old

.......................................................................

## 4. How to add blocks to user-created templates

> https://fullsiteediting.com/lessons/how-to-add-blocks-to-user-created-templates/

...how to change the blocks that WordPress adds when the user creates a custom template...

When a user creates a new template through the Site Editor, WordPress prompts them asking if the user wants to use a premade template.

This premade template is the `single` post template of the active theme (or the `index`, if the post template is missing). This means that both the theme developer and users with the correct permission are able to change the default blocks.

However, when the user wants to create a template for the post or page that they are currently editing inside the block editor, they are not presented with this option

When the user selects the button with the template name “Single” the editor opens a modal where the user can create a new template by clicking on the plus icon

Inside this new template, WordPress loads a few basic blocks: groups, site title, tagline, separator block, post title, and post content. It does not include a site footer

> Other sections: How to replace the default blocks | Creating an empty template | Using markup directly in the filter

### Using a file as the default template

You can add a template by including an HTML file with your block markup inside

.......................................................................

5. [WooCommerce and full site editing themes](https://fullsiteediting.com/lessons/woocommerce-and-full-site-editing/)

> SKIP FOR NOW

............... Lessons: Theme.json ...............

## 6. Theme.json color options

> https://fullsiteediting.com/lessons/theme-json-color-options/

The WordPress block editor has many different types of color options:

- Background color: Solid and gradient
- Text color
- Link text color, link hover color
- Border color
- Inline text color (Highlight)
- Duotone
- Cover block overlay

All three color options: the palette, duotone, and gradients use the same JSON structure with key and value pairs

- `slug`: The unique identifier. Also used in the CSS custom property.
- `name`: The visible name of the color in the editor.
- color/gradient/duotone: The CSS color value.

There are three types of color palettes:

1. The default palette added by WordPress,
2. a palette that can be added and customized by the user,
3. and the theme palettes

all WordPress core blocks with link color support:

- Author, Author Biography, Author Name
- BLOCKS: Column, Columns, Group, Media & Text
- Calendar, Content
- all Comment area links
- Date, Details
- File, Footnotes
- Heading, Paragraph, Verse
- List, List item
- Post Navigation Link, Post Template, Post Terms, Latest Posts, Excerpt, Pagination, Term Description
- Login/out, No results, Pullquote, Quote, Site Title, Table of Contents, Title

all WordPress core blocks with gradient color support:

- Author, Author Biography, Author Name
- Button, Buttons, Code
- BLOCKS: Column, Columns, Group, Media & Text
- Content
- Date, Details, File, Gallery
- Heading, Paragraph, Verse
- List, List item
- Post Navigation Link, Post Template, Post Terms, Latest Posts, Excerpt, Pagination, Term Description, Next Page, Page Numbers, Previous Page
- Login/out, No results, Preformatted, Pullquote, Query Title, Quote, Read More, Search, Separator, Site Tagline, Site Title, Social Icons, Table, Table of Contents, Time To Read, Title

all WordPress core blocks with duotone support:

- Site Logo, Featured Image, Author, Image, Cover, Avatar

.......................................................................

## 7. Theme.json typography: Font family

> https://fullsiteediting.com/lessons/theme-json-typography-options/

You can set a default font family for the site and any text-based block using the theme.json styles section

Once you have registered a font family, it is available in the Font family control in the block’s typography panel: `settings.typography.fontFamilies` with the settings [fontFamily, slug, name]

- fontFamily can include one font or several, including your fallback font

### Using a custom web font

The most common scenario is to use a combination of web fonts and system fonts. To use a web font you need to download and place the font files in your theme folder: `assets/fonts/`. To reference the font files, you will add an additional setting to fontFamily called `fontFace`

fontFace params:

- fontFamily. The name of the font.
- src. Source, the path to the file in the theme folder.
- fontWeight. Optional. A list of available font weights, separated by a space.
- fontStyle. Optional. Select a font style like normal or italic.
- fontStretch. Optional. You can for example use this with font families that have a condensed version.

If you want to load two separate font files, for example, one for normal and one for italic, you include them as separate arrays inside fontFace, separated by curly brackets and a comma

### How to add a font family for a specific block

You can use different font families for different blocks. A common use case is to offer one set of fonts for paragraphs and one for headings or the site title. You add typography options for individual blocks under `settings.blocks.blockname`

> How to disable the font family control: leave the fontFamilies array empty

### Downloading font files using the Font Library

- Open the Typography panel in the Site Editor. Click on “Manage fonts” to open the Font Library
- Click on Install Fonts. If you have not already enabled Google Fonts, you will be asked to approve the connection
- Search for a font family and install it
- WordPress downloads the files to the wp-content/uploads/fonts folder on your WordPress installation, so you need to copy the files from this folder into your themes font folder

### Downloading font files manually

- you want to download font files in `.woff2` format

> If you choose a font family on https://fonts.google.com/ and then select the “Download family” option, you will only receive a zip file with .ttf files. To get the .woff2 files, you can use a font converter script or an online converter. I recommend using this time-saving tool called [Google webfonts helper](https://gwfh.mranftl.com/fonts) to download the files

.......................................................................

## 8. Theme.json typography: Font size

> https://fullsiteediting.com/lessons/theme-json-font-size/

### Font size presets

If you register more than five sizes, the block editor shows the sizes in a dropdown since they will not fit on one row

### Custom sizes

Besides selecting from your theme’s font size presets, users can set a custom size. The custom sizes are not site-wide; users can only apply them to single blocks

### How to add a font size for a specific block

Place your font size array inside `settings.blocks.blockname.typography.fontSizes` to make the size available for a specific block. This way, you can limit which font sizes that are selectable for that block

### **Fluid font sizes > How to add fluid font sizes to theme.json**

- you can use a typography scale to reduce or increase the font size depending on the browser window width using clamp()
- Fluid font sizes are disabled by default, and you enable them by adding “fluid": true to settings.typography
- Besides the boolean, there are three optional properties. – Setting either of these properties also enables fluid font sizes without needing the boolean. Use one format or the other
  - minFontSize
  - maxViewportWidth
  - minViewportWidth
- The root font size for the calculations is 16px. It is not possible to change this value. If you change it in your custom CSS, the calculations will still use 16px

> Example: As you can see, the clamp is not applied to the small size, which is set to `13px`, because it is below the minimum font size

### How to add fluid font sizes to theme.json

When you have enabled fluid font sizes in the typography setting, WordPress will use your size and calculate the clamp() values for you.

Relying on WordPress for the calculations is one option, but you can also customize the minimum and maximum values for each size by adding min and max values to each size

Note: The size value is the preferred size in the clamp()

### How to apply default font sizes

- You add a font size for the `body` at the top level of the theme.json styles section: `styles.typography.fontSize`
- You can add styles to block types under styles.blocks.blockname.typography

.......................................................................

## Theme.json typography: Line height, font weight, and text decoration

> https://fullsiteediting.com/lessons/theme-json-typography-font-styles/

Line height: SKIP

Drop cap: SKIP

The font appearance setting: SKIP

Font weight - you can select between:

- Thin: 100
- Extra Light: `200`
- Light: 300
- Regular: `400`
- Medium: `500`
- Semi Bold: `600`
- Bold: `700`
- Extra Bold: 800
- Black: 900

Unfortunately, it is not possible to limit the font weights to those supported by the current font family. Many users have requested this feature, but the development team has not built it into Gutenberg yet

Text transform: SKIP

Letter-spacing: SKIP

Text decoration: SKIP

Orientation: writing mode and vertical text:

- lets users add vertical text
- `writingMode` is disabled by default, set it to `true` to enable it

Text columns:

- The `textColumns` setting is disabled by default
- to enable it, you have to set it to `true`

Text align: SKIP

### Applying typography styles with theme.json

- You can add typography styles for the website and as defaults for all blocks in the root level of the styles section in theme.json
- or you can add that for individual blocks

```json
"styles": {
	"typography": {
		"fontStyle": "value",
		"fontWeight": "value",
		"lineHeight": "value",
		"textDecoration": "value",
		"textTransform": "value"
	}
}
```

.......................................................................

## Theme.json layout and spacing options

> https://fullsiteediting.com/lessons/theme-json-layout-and-spacing-options/

- contentSize
- wideSize

### The block editor layout controls

The following blocks use the inner block width control: Query loop, group, post content, cover, media & text, and column. However, different blocks also start with different default values

- A group block placed in the Site Editor will have the setting “Inner blocks use content width” enabled
- While the post content block disables the setting
- You can specify the width of the inner blocks using the input fields in the layout panel
- You can justify the blocks to the left, center, or right
- When Inner blocks use content width is toggled off:
  - The content width options in the layout panel are unavailable
  - The inner blocks are always justified to the left (on ltr) and fill the width of the parent block

> SKIP: Custom units | Margin and padding | How to disable custom spacing

### How to add default margin and padding to blocks

- `styles.blocks.blockname.spacing.margin`
- `styles.blocks.blockname.spacing.padding`
- inside margin &/or padding object you can set, top, right, bottom, &/or left

### BlockGap: What is blockGap

The `blockGap` setting in theme.json sets the vertical spacing between blocks by adjusting the margins depending on the position of the block:

- The first block on the webpage and the first inner block or child block have no top or bottom margin
- The following blocks have a top margin corresponding to the value in the theme.json setting, and no bottom margin

The block editor also uses blockGap for horizontal spacing between columns, gallery items, buttons, and social icons. The control for blockGap in the editor is called Block spacing, and you can find it in the dimensions panel

### Spacing presets

With spacing presets, both developers and users can select from predefined values for padding, margin and block spacing. The purpose is to make it easier to use consistent spacing throughout the website

#### How to use the default spacing scale

- later

#### How to add your own custom spacing scale

> Look into this (maybe not)

#### Using spacing preset values without a scale

- I did it this way

#### Padding aware alignments

> REREAD THIS

- Since the introduction of full width blocks, theme developers have tried different ways to adjust the width of the content that is placed inside full width container blocks.
- For example, when you used a full width group or columns block with paragraphs inside, the text would touch the edge of the browser window. - Unless you added padding or a background to each block.
- Secondly, if we added a padding to the root level in the theme.json styles section (styles.spacing.padding), it meant that the blocks were no longer full width.
- useRootPaddingAwareAlignments solves that which uses the value from styles.spacing.padding - You must use separate values for the padding; using a single value does not work (top, right, bottom, left)

#### Minimum height

the group and post content blocks support setting a minimum height via the dimensions panel. This theme.json setting is opt-in. You enable it by setting appearanceTools to true or with settings.dimensions.minHeight

#### Position

The position setting is limited to the group block and to two options; default, and sticky. Position is disabled by default and can be enabled by enabling appearanceTools or with `position: {sticky: true}`

> NOT WORKING

.......................................................................

11. [Theme.json elements](https://fullsiteediting.com/lessons/theme-json-elements/)

> Only 11 elements/blocks: link, h1-h6, heading, button, caption, cite with a future possible form elements

.......................................................................

12. [How to add hover and focus styles using theme.json](https://fullsiteediting.com/lessons/how-to-add-hover-and-focus-styles-using-theme-json/)

From WordPress 6.3, every block that supports link color has a setting with two tabs: one for the default link state, and one for hover

- There is no interface for the `:focus`, `:active`, or `:visited` link states
- Button blocks do not support link color and as such, does not have a setting for hover styles yet
- From Gutenberg version 13.6 you can style `:hover`, `:focus `and `:active` states on link elements.
- From Gutenberg version 14.1 you can style `outline` and `:visited`

The keys that you use to styles these states in theme.json are the same as the CSS pseudo classes:

- styles.elements.link.:hover
- styles.elements.link.:focus
- styles.elements.link.:active
- styles.elements.link.:visited
- styles.elements.link.outline

> As with most theme.json settings, you can use this method to add site wide styles or style individual block types

Examples: elements.link:hover | blocks.core/site-title.elements.link:hover | elements.button:focus

```json
{
  "version": 2,
  "styles": {
    "elements": {
      "button": {
        ":focus": {
          "outline": {
            "offset": "3px",
            "width": "3px",
            "style": "solid",
            "color": "blue"
          }
        }
      },
      "link": {
        "typography": {
          "textDecoration": "none"
        },
        "border": {
          "bottom": {
            "width": "2px",
            "color": "currentColor",
            "style": "solid"
          }
        },
        ":hover": {
          "typography": {
            "textDecoration": "none"
          },
          "border": {
            "bottom": {
              "width": "2px",
              "color": "#111",
              "style": "dotted"
            }
          }
        }
      }
    },
    "blocks": {
      "core/site-title": {
        "elements": {
          "link": {
            "spacing": {
              "padding": ".5rem"
            },
            "typography": {
              "textDecoration": "none"
            },
            "border": {
              "width": "2px",
              "color": "transparent",
              "style": "solid"
            },
            ":hover": {
              "typography": {
                "textDecoration": "none"
              },
              "border": {
                "width": "2px",
                "color": "#111",
                "style": "solid"
              }
            }
          }
        }
      }
    }
  }
}
```

**Styling the navigation block links is trickier**:

- You can choose to style the navigation block, or style the inner navigation link, page list, home link and submenu blocks individually
- This example is for the navigation block, and only works if the text decoration setting and the “Submenus: Open on click” settings are not used
  - The text decoration setting adds a class name that increases the specificity, and the styles from theme.json are overriden.
  - The “Open on click” setting uses an HTML button element, and not a link.

```json
{
  "version": 2,
  "styles": {
    "blocks": {
      "core/navigation": {
        "elements": {
          "link": {
            "typography": {
              "textDecoration": "none"
            },
            "border": {
              "bottom": {
                "width": "4px",
                "color": "transparent",
                "style": "solid"
              }
            },
            ":hover": {
              "typography": {
                "textDecoration": "none"
              },
              "border": {
                "bottom": {
                  "width": "4px",
                  "color": "currentColor",
                  "style": "solid"
                }
              }
            }
          }
        }
      }
    }
  }
}
```

.......................................................................

13. [How to add box-shadows with theme.json](https://fullsiteediting.com/lessons/how-to-add-box-shadows-with-theme-json/)

In the editor, the shadow settings are in the same styles panel as the border settings. All blocks can have a default shadow applied in theme.json. The following WordPress core blocks supports the shadow control in the Site Editor and block editor:

- Button,
- image,
- featured image,
- cover,
- columns,
- and column

Shadow presets are very similar to the color palette and belong under the theme.json settings section: settings.shadow.

WordPress includes a set of shadow presets that it enables by default. To disable the shadow presets, set settings.shadow.defaultPresets to `false`

```json
"settings": {
	"shadow": {
		"defaultPresets": false
	}
}
```

You can add custom shadows by adding a presets object with the following key and value pairs:

- name: The visible name in the editor.
- slug: The unique identifier, used in the CSS custom property.
- shadow: The CSS value for the box shadow.

```json
"settings": {
	"shadow": {
		"presets": [
			{
				"name": "Natural",
			  "slug": "natural",
				"shadow": "6px 6px 9px rgba(0, 0, 0, 0.2)"
			},
			{
				"name": "Crisp",
				"slug": "crisp",
				"shadow": "6px 6px 0px rgba(0, 0, 0, 1)"
			}
		]
	}
}
```

You apply the box shadows under styles.blocks.blockname.shadow. The setting accepts a CSS variable for one of the presets; or a string with a shorthand box-shadow: offset-x, offset-y, blur-radius, spread-radius, and color

```json
"styles": {
	"blocks" :{
		"core/post-featured-image": {
			"shadow": "var(--wp--preset--shadow--natural)"
		},
		"core/post-featured-image": {
			"shadow": "10px 10px 5px 0px rgba(0,0,0,0.66)"
		}
	}
}
```

You can add shadows to both blocks and elements:

```json
"styles": {
	"elements": {
		"button": {
			"shadow": "10px 10px 5px 0px rgba(0,0,0,0.66)"
		}
	}
}
```

.......................................................................

## 14. How to use custom CSS in theme.json

> https://fullsiteediting.com/lessons/how-to-use-custom-css-in-theme-json/

WordPress 6.2 added two new custom CSS options to the Styles sidebar in the Site Editor:

- The site-wide Additional CSS option, which is the Site Editing equivalent of the Additional CSS Customizer option.
- The per-block CSS option, where you add CSS to all copies of a single block type. This option uses the block’s unique CSS class as the pre-determined CSS selector. The CSS can be edited and viewed in the Styles sidebar > Blocks > Block > Advanced > Additional CSS.

The CSS options have been added as replacements for Additional CSS option in the Customizer, which is hidden when you activate a block theme.

It is not a replacement for enqueueing CSS files if you need to add a lot of custom CSS. You can use it to add a small amount of CSS to your theme when block options are not enough but you don’t want to add an extra stylesheet only to solve a small issue

On the front, the CSS is printed in the `<head>` in the global-styles-inline-css `<style>` tag. Since you are working with a JSON file, the CSS needs to be written on one line, without line breaks. The CSS is enclosed in double quotes ", so you also need to escape any double quotes that you include in your CSS

### Site-wide custom CSS

You add the site-wide custom CSS as a string at the top level of the styles section in theme.json. The string can contain any valid CSS and the CSS selectors of your choice

### Per-block custom CSS

You add the per-block custom CSS as a string to styles.blocks.blockname.css. With this option, the CSS selector is already included for you

```json
{
  "version": 3,
  "styles": {
    "blocks": {
      "core/table": {
        "css": "color:#333"
      }
    }
  }
}
```

```css
/* Output: */
.wp-block-table > table {
  color: #333;
}
```

### Custom CSS for theme.json elements

```json
{
  "version": 3,
  "styles": {
    "elements": {
      "link": {
        "css": "color: red"
      }
    }
  }
}
```

> SKIP: Custom CSS for variations

### Extending custom CSS with the & selector

The new CSS options also introduce the possibility of using the `&` CSS selector to target nested elements and use pseudo selectors. For example, you could use it with `:not`, `:before` or `:after`

```json
{
  "version": 3,
  "styles": {
    "blocks": {
      "core/table": {
        "css": "color:#333 & th{ background:#f5f5f5; }"
      }
    }
  }
}
```

```css
.wp-block-table > table {
  color: #333;
}

.wp-block-table > table th {
  background: #f5f5f5;
}
```

When you want to target inner blocks, for example, the img element inside the figure in the image block, you can use a single space or & followed by a space

```json
{
  "version": 3,
  "styles": {
    "blocks": {
      "core/image": {
        "css": " img{border:2px solid #ffffff;}"
      }
    },
    "blocks": {
      "core/image": {
        "css": "& img{border:2px solid #ffffff;}"
      }
    }
  }
}
```

```css
/* Output: */
.wp-block-image img {
  border: 2px solid #ffffff;
}
```

A common example is setting the default width of the separator block

```json
{
  "version": 3,
  "styles": {
    "blocks": {
      "core/separator": {
        "css": "&:not(.is-style-wide):not(.is-style-dots):not(.alignwide):not(.alignfull){width: 100px}"
      }
    }
  }
}
```

.......................................................................

15. [Style variations](https://fullsiteediting.com/lessons/global-style-variations/)

> SKIP > NOT INTERESTED FOR MY THEME

.......................................................................

16. [How to filter theme.json with PHP](https://fullsiteediting.com/lessons/how-to-filter-theme-json-with-php/)

> SKIP - PHP FUNCTIONS

.......................................................................

17. [Cheat sheet: Every setting that you can remove using theme.json](https://fullsiteediting.com/lessons/remove-settings-in-theme-json/)

> SKIP - NOT IMPORTANT

............... Lessons: Block Patterns ...............

## 15. Introduction to block patterns

> https://fullsiteediting.com/lessons/introduction-to-block-patterns/

> HUUUUUGE PAGE

You will learn how to create patterns via:

1. the user interface,
2. the patterns folder,
3. or a PHP function called `register_block_pattern()`

### What is a block pattern?

A block pattern is a group of blocks that you combine to create reusable layout elements and page sections...Block patterns help you select pre-made designs that you can customize using infinite color options and media

They help you speed up your template and page creation, and they can be as small as a single block or as large as a full-page layout. They can also be part of a template, template part, or even nested inside other patterns

By combining blocks with custom block styles and block variations, you can style and create patterns with different purposes for your theme. By creating your own pattern library, you can create time-saving layouts that look and feel unique with only minor changes

### What is a synced pattern?

A synced pattern behaves like one unit. Like a pattern block with nested inner blocks. It stays grouped and is labeled with the pattern name. It also has a different color, which makes it easier to find in the editor

### Patterns can be created with or without using the database

before version 6.3, WordPress made patterns available through a block pattern registry class and private functions. Plugins and themes registered the pattern, and the markup of the blocks was provided through a PHP function or in a file in the theme’s patterns folder

From WordPress 6.3, users can create patterns directly in their WordPress installation. These patterns are site-specific, and WordPress needs to store them in the database. To make this possible, the post type used for reusable blocks; wp_block, was re-purposed for patterns. Not only for synced patterns but all user-created patterns

Patterns that are not created using the wp_block post type can not be synced

### Patterns that use the ‘wp_block’ post type

Patterns registered with the wp_block post type works similarly to other WordPress post types. They can be managed with code using a plugin but are primarily managed through the WordPress interface. They can be either synced or not synced. The sync status is saved to a post meta called wp_pattern_sync_status

Where a standard pattern places the markup of the individual blocks into the editor, synced patterns use a reference to a post ID

```html
<!-- wp:block {"ref":1874} /-->
```

### Creating patterns in the block editor

creating a new pattern for your site is as easy as selecting the block, opening the options menu (right-click) in the toolbar, and selecting “Create pattern”

- In the next step, you enter a name for the pattern and decide if it should be synced or not
- When you save your content and reload the editor, WordPress transforms your blocks into a pattern block

> When you have multiple blocks in your pattern, you want to place them inside a “container block” like a group or row, and then use the “Create pattern” option on this outer block. This way all your blocks will stay together

> SKIP Classic themes: Creating and managing patterns in the WordPress admin

### Block themes: Creating and managing patterns using the Site Editor

Go to Appearance > Editor > Patterns to open the new patterns list. You can choose between different pattern categories in the Site Editor sidebar. The sidebar lists all user-created patterns under the heading “My patterns”.

You can edit, copy, and delete user-created patterns. But you can not edit patterns from themes and plugins directly, because they are not saved in the WordPress database. You must first duplicate the pattern

You can create a new pattern by selecting the plus icon at the top of the Site Editor sidebar. This opens the same modal that WordPress uses to create patterns in the block editor. You can add a name and category and select the sync status. The form requires adding a name

### What happens when you delete a user-created pattern

If you have placed a user-created pattern in your content and then trash it or set it as a draft, the pattern will stop showing on the front.

WordPress does not remove the pattern from your content, and the pattern will continue to show in the block editor. If you delete the trashed item, the pattern will show an error message in the editor

### How to export and import block patterns

Users can export and import patterns as JSON files. This is useful if you have created a pattern on one site and want to use it on a different site. There are plans to add this feature to the Site Editor in WordPress 6.4. Until then, you can use the classic pattern management screen

- Open the pattern management screen. Focus on or hover over the pattern you want to export. Select “Export as JSON”.
- This will download a file called `patternname.JSON`. Example file
- To import a pattern, select the button “Import from JSON” and choose your pattern JSON file
- When you select the “Import” button, you will receive a notification confirming that the import was successful. Refresh the page to see your new pattern in the pattern list

### The Pattern Explorer

Click on the block inserter in the top left of the editor, select the Patterns tab, and the button “Explore all Patterns”.
In Pattern Explorer, you can search for patterns provided by the theme, plugins, WordPress core, and featured items from the WordPress.org pattern directory...it is not possible to edit, create, or delete block patterns from this screen

### Registering block patterns

...registering block patterns without using the `wp_block` post type

> SKIP this section
> SKIP How to use `register_block_pattern()`
> SKIP How to add block markup to the pattern
> SKIP How to register block patterns using the theme patterns folder

### Block pattern categories

This section refers to registering pattern categories for block patterns without using the wp_block post type. For categories, you have two options: Either use a category provided by WordPress or register your own

The available categories are:

- Buttons: Patterns that contain buttons and calls to action.
- Columns: Multi-column patterns with more complex layouts.
- Featured: A set of high-quality curated patterns.
- Gallery: Different layouts for displaying images.
- Text: Patterns containing mostly text.

#### New block pattern categories in WordPress 6.2

- About: Introduce yourself,
- Call to action: Sections whose purpose is to trigger a specific action. Use as call-to-action.
- Contact: Display your contact information.
- Footers: A variety of footer designs displaying information and site navigation.
- Headers: A variety of header designs displaying your site title and navigation. Replaces the old header category.
- Media: Different layouts containing video or audio.
- Portfolio: Showcase your latest work.
- Posts: Display your latest posts in lists, grids, or other layouts. Replaces to old query category.
- Services: Briefly describe what your business does and how you can help.
- Team: A variety of designs to display your team members.
- Testimonials: Share reviews and feedback about your brand/business.

> Skip notes on "register_block_pattern_category"

> SKIP User-created block pattern categories

### How to add a CSS class to a block pattern

You can add a CSS class to the wrapper element and use that selector to style your pattern

- First, add the className attribute inside the block comment, where the value is the CSS class
- You need to duplicate this for the wrapping div

```html
<!-- wp:cover {"className":"prefix-contact-form", ... -->
<div class="wp-block-cover prefix-contact-form"></div>
```

> SKIP How to remove block patterns

### How to prevent loading block patterns from the pattern directory

You can prevent loading patterns from the WordPress.org pattern directory with a filter called `should_load_remote_block_patterns`:

```php
add_filter( 'should_load_remote_block_patterns', '__return_false' );
```

> SKIP Removing block pattern categories

### How to place patterns in block templates

If you are using a block theme, you can load patterns in templates, template parts, and other patterns with `wp:pattern`. In this markup example, the slug is the same parameter and value used to register the pattern

```html
<!-- wp:pattern {"slug":"prefix/pattern-slug"} /-->
```

You can only use the prefix and slug combination to reference patterns added by themes or plugins. It does not work with user-created patterns because they can only be referenced by their post ID.

If the template you are creating is specific to one site and not for a theme that is distributed to many sites, you can find the post ID for the pattern and reference it using `wp:block` instead of `wp:pattern`

```html
<!-- wp:block {"ref":77} /-->
```

### How to include a pattern from the pattern directory

To use a pattern from the WordPress pattern directory in your block theme, you first need to add the pattern to theme.json.

At the root level of theme.json, add a new section called patterns. Then, add an array of pattern slugs. – The pattern slug is the same as the path to the pattern in the directory

> Confusing

> SKIP How to display the pattern selection when creating a new page
> SKIP How to display the pattern selection when creating a new template
> SKIP How to hide a block pattern from the inserter
> SKIP How to use a template part inside a block pattern
> SKIP the rest of the page

........ Lessons: Classic themes, Hybrid themes & Child themes ........

18. [Full site editing child themes](https://fullsiteediting.com/lessons/child-themes/)

> SKIP - ONLY NEED FOR CLIENTS though child themes are interesting

.......................................................................

19. [Adding full site editing features to classic themes](https://fullsiteediting.com/lessons/adding-full-site-editing-features-to-classic-themes/)

> SKIP - ONLY NEED FOR CLIENTS

.......................................................................

20. [How to use PHP templates in block themes](https://fullsiteediting.com/lessons/how-to-use-php-templates-in-block-themes/)

> GOOD INFO - LATER

...if WordPress can not find a matching .html file, it tries to find a .php version of that file

create a new .php file in the root directory called example-page.php. At the top of the file, add the template name file header - add the hooks directly in the file

```php
<?php
/*
Template Name: Example
*/
?>
<!doctype html>
<html <?php language_attributes(); ?>>
<head>
	<meta charset="<?php bloginfo( 'charset' ); ?>">
	<?php wp_head(); ?>
</head>

<body <?php body_class(); ?>>
  <?php wp_body_open(); ?>
  <div class="wp-site-blocks">


  </div>
<?php wp_footer(); ?>

</body>
</html>
```

### How to render blocks in PHP templates

To render block markup as blocks and not as HTML comments, you need to wrap the code inside a PHP function called [do_blocks();](https://developer.wordpress.org/reference/functions/do_blocks/)

The function uses one parameter, which is the string that contains the content that the function needs to parse. You can use do_blocks() to render any block or a combination of blocks.

You can render more than one block in the same call.

Note that you can not split blocks: The opening and closing tag must be in the same do_blocks() function call

WordPress also has functions specifically for rendering the content of template parts:

- [block_template_part()](https://developer.wordpress.org/reference/functions/block_template_part/) which accepts one parameter, $part, the slug of the selected template part. This function is a wrapper for do_blocks(), and echoes the content of the template part.
- A wrapper for block_template_part() called [block_header_area()](https://developer.wordpress.org/reference/functions/block_header_area/), which prints a template part called 'header'.
- A wrapper for block_template_part() called [block_footer_area()](https://developer.wordpress.org/reference/functions/block_footer_area/), which prints a template part called 'footer'.

The key differences that I recommend you to remember is that:

- `do_blocks()` returns the block markup that you place inside the function, and you need to output the result yourself, for example with echo.
- `block_template_part()` echoes the content of the template part. It fetches the content of the template part, runs it through do_blocks(), and then echoes it.

```php
<body <?php body_class(); ?>>
<?php wp_body_open(); ?>
  <div class="wp-site-blocks">
    <header class="wp-block-template-part site-header">
    <?php block_header_area(); ?>
    </header>

    <footer class="wp-block-template-part site-footer">
    <?php block_footer_area(); ?>
    </footer>
  </div>
  <?php wp_footer(); ?>
</body>
```

### Making sure that WordPress loads the block CSS

> This section is confusing

### Using template parts inside do_blocks()

If you want to include a template part with the wrapper element directly inside do_blocks(), instead of using the block template part function, you need to add the theme attribute to the markup. Use your theme slug as the value

```html
<!-- wp:template-part {"slug":"comments","theme":"fse"} /-->
```

### Extending the template with PHP

Now you have an example template that displays the site header and footer and the block content of the assigned page. You can combine this with any PHP functionality that you need, for example, displaying block content conditionally, display data from post meta fields, or support a plugin. You could display different blocks for logged in users, or for users with a specific user role

> Remember that when you add HTML elements that are not blocks to the page, you need to load additional CSS to style them

.......................................................................

## How to re-enable the Customizer in a block theme

> https://fullsiteediting.com/lessons/how-to-re-enable-the-customizer/

...activating block themes removes the Customizer from the menus in the WordPress admin. The Customizer is still a part of WordPress, and you can re-enable the links with just one line of codes

### Re-enable the Customizer with the customize_register hook

Add in functions.php:

```php
add_action( 'customize_register', '__return_true' );
```

............... Lessons: Accessibility ...............

## Accessibility in full site editing themes

> https://fullsiteediting.com/lessons/accessibility-in-full-site-editing-themes/

> EXCELLENT

- it is up to the theme developer to use the blocks correctly and not break the block’s accessible features

### Keyboard Navigation

Provide visual keyboard focus highlighting in navigation menus and for form fields, submit buttons, and text links. All controls and links must be reachable using the keyboard

### Navigation menus

- The navigation block is keyboard accessible on the front of the website by default
- You can open submenu items on hover and by using a button with an aria-label, the aria-expanded attribute, and an SVG icon with `aria-hidden=true and focusable="false"`
- The block has an option called “Open on click”. The option turns the parent menu item into a button that expands the submenus
- You can choose between always showing the menu, hiding it on small screens, and hiding it on all screen sizes. The buttons that open and closes the navigation are keyboard accessible; you can also close the navigation by using the escape key.
- You can choose to use the menu button with a two line icon, or display the text “Menu”. The text is translatable, but not editable

> Potential issues: When you enable the option “Open on click” option and open the responsive navigation, the parent menu item is still a clickable button. This button does not do anything, because the overlay menu already expands all the submenu items

### Forms

- There are only two types of WordPress core blocks that use forms: Search and comments.
- The category and archive blocks use a select list and a label. They do not use a form element, submit buttons or editable input fields.
- The blocks have a visible focus outline, but the color contrast will depend on the active theme

### Controls

All theme features that behave as buttons or links must use `<button>`, `<input>`, or `<a>` elements, to ensure native keyboard accessibility and interaction with screen reader accessibility APIs.

- All controls must also have machine-readable text to indicate the nature of the control
- The buttons in the navigation block, search block, and comments are HTML buttons or submit buttons and have machine-readable text
- The button block is a link element (`<a>`) and this is correct because they do not perform an action; they lead to a different part of the website

### Skip links

WordPress adds the skip link automatically if the page contains a `<main>` element. The link is visually hidden until you focus on it, and it is the first focusable item on the page. It always skips to the `<main>` element

### Headings

- Headings do not skip levels when descending
- Multiple H1 is acceptable but highly discouraged. Having one H1 is beneficial for screenreader users
- The block editor has a built-in document outline that warns if there are heading levels that are in the wrong order.
- This tool is not available in the Site Editor, and theme authors need to ensure that they use the correct heading structure in the theme’s default templates

### ARIA Landmark Roles

Assign landmark roles to the main content areas of your site. All content on your site must be wrapped in at least one landmark role; any content not wrapped in a landmark role is ‘orphaned’, and may not be found by screen reader users

- Theme authors create landmarks by changing the HTML element of their container blocks: `group`, `template part`, `query`, and `comments`
- You can change the tag by updating the `tagName` attribute in the markup or selecting the HTML element in the Advanced settings panel in the block settings sidebar

### Aria labels

If a particular role appears more than once on a page, provide an ARIA label for that role

- Adding an aria-label in the markup of a block will sometimes cause a block validation error
- Having a descriptive aria-label when there are multiple navigation blocks is important for any user who navigates via landmarks
- The navigation block automates this in WordPress 6.0, by using the menu name as the aria-label
- The aria-label does not have a separate input field and can not be different from the name. It is important that the user adds a descriptive name instead of the default menu name
  - There is a potential issue if you duplicate the navigation block in the editor, since the editor does not update the name

> Because the navigation block requires a stored navigation ID to add the aria-label, you can not add the aria-label in the HTML block markup, therefore, there is nothing the theme author can do to add the aria-label

### Content Links

> LINKS - THIS IS IMPORTANT

- Links within large sections of text must be underlined
- When links appear within a larger body of block-level content, they must be clearly distinguishable from surrounding content
- The underline is the only accepted method of indicating links within content.
- Bold, italicized, or color-differentiated text is ambiguous and will not pass
- Links in navigation-like contexts (e.g. menus, lists of upcoming posts in widgets, grouped post meta data) do not need to be specifically distinguished from surrounding content
- Links for headings and paragraph blocks have underlines by default, and it is up to the theme developer to not break this requirement by removing the underlines
- Most blocks do not have controls for removing or adding underlines, the exceptions are the navigation block and the read more block

### Repetitive Link Text

- Links must avoid repetitive non-contextual text strings such as ‘read more…’ and must make sense if taken out of context.
- Bare URLs must not be used as links
- Editors can update the text to pass the requirements, or choose to not use the block

### Contrasts

Theme authors must ensure that all background/foreground color contrasts for plain content text are within the level AA contrast ratio (4.5:1)

If a change in color is the only visible change when the link state changes:

- The text color on hover must have a contrast ratio of 4.5:1 against the default link text color and against the focused link text color
- The text color on focus must have a contrast ratio of 4.5:1 against the default link text color and against the hovered link text color
- If there is no text decoration on linked text, there must be a 3:1 color contrast between the link text color and the surrounding non-link text color, in addition to the other color contrast requirements

> This requirement may first seem difficult, but it mostly applies to links that have no underline; as the theme developer, you can prevent failure by not removing underlines on links - It requires additional styling to some blocks including the navigation block

### Images

Where theme authors add non-decorative images to template markup or provide a method for end users to add images, theme authors MUST incorporate an appropriate alt attribute or the means for an end user to provide one

- All decorative images must be marked up such that they will be ignored by screen readers
- Not all image blocks have alt text settings in the editor, but alt texts can be provided in the block markup or via the media library

> Full site editing and the block editor do not support inserting custom svg icons out of the box, and if the theme author implements it, they need to make sure it passes the requirements

### Media

Media resources must NOT auto start or change without user action as a default configuration. This includes resources such as audio, video, or image/content sliders and carousels. The video block has an option for autoplay, and theme authors and editors can follow this requirement by not enabling the option

### Screen Reader Text

If you are making text available only for screen reader users, there are two valid ways to do this. You can either use text hidden in a valid screen-reader-text class or use aria-label or aria-labelledby attributes to provide an accessible name for what you’re labeling

The block editor does not provide an interface for adding screen reader texts. Theme authors can implement screen reader texts by adding a custom CSS class and the appropriate styling.

But be careful when deciding what to show in the editor and what to show on the front, especially if you intend for users to customize the texts

### Conclusion

The four main things you need to focus on to create accessible block themes are:

1. Using the correct semantic elements in templates (header, main, footer)
2. Color contrast
3. Underlining links
4. Heading order

Please also be careful to not break the functionality of the navigation block when you want to use custom CSS _beyond spacing and colors_

......... Lessons: Block Styles .........

22. [Block style variations and section styles](https://fullsiteediting.com/lessons/custom-block-styles/)

> SKIP - CONFUSING

The custom block styles, often called block style variations, are options in the Styles section of the block settings sidebar. You use them to quickly apply styles without having to adjust individual settings. Common examples of block styles are the filled and outlined buttons and the rounded image

### What is the difference between block styles, section styles, and patterns

- Block styles in this context are style variations for one or more blocks. To use the style, you insert the block and then select the style option in the block’s settings sidebar.
- Section styles is exactly the same thing, except the styles are also applied to nested blocks and elements.
- Block styles are options that adds styling to existing blocks, compared to patterns that insert new blocks into your content or template, with or without styling

You can combine patterns and section styles. That way, users can insert patterns and then quickly change the colors, spacing and typography

### How do custom block styles work

When you create your block style you can choose between adding custom CSS, using a theme.json style variation, or registering an array of style data using PHP

Block styles work by generating and adding a CSS class to the block and applying the styles to the new selector. When you have selected a block style in the editor, you can open the Advanced section of the block settings sidebar and see that WordPress adds the class in the Additional CSS class(es) field. WordPress automatically loads the CSS in the editors and the front

It is important to know that these block styles are only an extra layer of CSS on top of the block’s default style. It does not remove the default style, it overrides it using a higher CSS specificity

Example: If you register and select a style that adds a background color to a block, then it will not be reflected in the block’s background color control.
If you decide to use the background color control to add a new color, this setting will override both the default block style and the style variation.

You are not limited to styling blocks from WordPress core. If you know the prefix and the slug, you can also style blocks from plugins.

### How to register custom block styles

You can register the custom block style with PHP, JavaScript or JSON - Which method you choose depends on personal preference and how you wish to organize your theme files

#### Registering block styles using JSON

You will need to create a new JSON file with the same formatting as theme.json, but with some modifications. You will need one file per style, and you need to place it in the themes `styles` folder, or in a subfolder of styles

The key and value pairs that are unique for the block style variations are:

- `slug`: The unique identifier that WordPress uses to create the CSS class.
- `blockTypes`: The list of blocks that you want to apply the style to

> SKIP Registering block styles using PHP
> SKIP Registering block styles using JavaScript
> SKIP How to unregister block styles

............... Lessons: Block variations ...............

23. [Block variations](https://fullsiteediting.com/lessons/block-variations/)

### What are block variations?

One example of a block variation is the columns block, where you can select the number of columns

> A full-width group block is one of the blocks that will be used most with full site editing because they work great as wrappers for other blocks

### How to register a block variation

Block variations can only be registered using JavaScript. The JavaScript file needs to be enqueued in the editors, but is not needed on the front of your website

> SKIP the rest of the page

.......................................................................

24. [Block variation examples for full site editing](https://fullsiteediting.com/lessons/block-variation-examples-for-full-site-editing/)

> SKIP - maybe later with the link above

............... Lessons: Block locking ...............

25. [How to lock blocks and templates](https://fullsiteediting.com/how-to-lock-blocks-and-templates/)

The primary use for block locking is to prevent the accidental removal of important blocks or to prevent specific users from removing blocks

> WHY LOCK BLOCKS? USE FOR CLIENTS WHO WANT IT

............... Lessons: Miscellaneous ...............

26. [Community resources & How to stay up to date](https://fullsiteediting.com/lessons/bonus-how-to-stay-up-to-date/)

- Look into this course, `$397`: [Block Theme Academy](https://wpdevelopment.courses/courses/building-block-themes/)
- check out [WebMan Design](https://profiles.wordpress.org/webmandesign/)
- great design, [Ollie theme](https://olliewp.com/)

- SKIP but check their blog:

> https://wordpress.com/reader/subscriptions - https://developer.wordpress.org/news/

.......................................................................

27. [Block reference](https://fullsiteediting.com/blocks/)

> REFERENCE - `EXCELLENT`

.......................................................................

28. [Troubleshooting block themes](https://fullsiteediting.com/lessons/troubleshooting-block-themes/)

> SKIP

.......................................................................

29. [How to remove default block styles](https://fullsiteediting.com/lessons/how-to-remove-default-block-styles/)

> SKIP, PHP - WHY DO YOU NEED TO DO THIS?

.......................................................................

30. [How to add default blocks to the block editor](https://fullsiteediting.com/lessons/how-to-add-default-blocks-to-the-block-editor/)

> SKIP, PHP - CONFUSING - WHY DO YOU NEED TO DO THIS?

.......................................................................

31. [Learn together by building block themes in public](https://fullsiteediting.com/lessons/learn-together-build-in-public/)
    1. [community-themes](https://github.com/WordPress/community-themes)

> SKIP FOR NOW - CONTRIBUTE LATER
