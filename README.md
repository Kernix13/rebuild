# README

- HTMLImageElement: [fetchPriority property](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/fetchPriority)
- illustrations as webp instead of svg!!!
- look at freelance/index.html

I Need:

1. Intro text for Services/Packages section - AND - the services titles
2. Intro text for Testimonials section - do I need it?
3. Intro text for FAQ section
4. I need an FAQ page and pages for the individual services

PROBLEM SECTION:

- Use brief, impactful statements with accompanying icons or visuals to make the problems stand out.
- End the section with a transition statement like: "If any of these sound familiar, you're not alone! The good news? We specialize in solving these exact problems—so you can focus on growing your business."

BENEFITS SECTION:

- List them with short descriptions and icons to make them visually engaging
  - On Service Pages: Tie each benefit to a specific service, reinforcing the value of what you offer
  - Throughout the Site: Sprinkle them in testimonials, CTA sections, or feature highlights

## Tools

- Color schemes generator: https://mycolor.space/
- Color schemes generator: https://color.adobe.com/create/color-wheel

## My Theme

Lucid1 - username = lucid1, PW = lucid1

- Nav: https://learn.wordpress.org/lesson-plan/how-to-create-a-menu-with-the-navigation-block/

## Problems and solutions

- https://developer.wordpress.org/themes/global-settings-and-styles/
- https://developer.wordpress.org/themes/global-settings-and-styles/settings/
- https://developer.wordpress.org/themes/global-settings-and-styles/styles/

> [r/ProWordPress](https://www.reddit.com/r/ProWordPress/)

1. Problem 1: I could not get my font sizes to apply.

- Solution 1: ✅ `defaultFontSizes`

2. Problem 2: my button and link styles do not apply? `WHY`? Same for hover and focus

- Solution 2: [How to add hover and focus styles using theme.json](https://fullsiteediting.com/lessons/how-to-add-hover-and-focus-styles-using-theme-json/)
- still can't style button hover
- what about the navigation block - try `core/navigation-link`

3. Problem 3: How do I actually get my buttons to work? They are considered links so they should go to a different page

- Solution 3: click the link icon in the menu bar and type the page name without using `/`
- How to get it to act like a button to perform an action

4. Problem 4: How do I make some words a different color in a heading?

- Solution 4: ✅ Down arrow > Highlight > Choose color (`<span>`)
- don't think this is solved

5. How do I add icons and where should I get the icons? How do you place inline images, or icons or copy paste emojis?

- Solution 5: look at Premium Blocks – Gutenberg Blocks for WordPress

6. Problem 6: How do I add page links without the navigation block?

- Solution 1:

7. Problem 6: Sticky is not an opttion for me under Position

- Solution 1: Toggle off "Inner blocks use content width" for the group

8. Problem 6: Do I add all those font files in theme.json

- Solution 6:

9. Problem 7: ssytem fonts: `system-ui` for sans-serif, `ui-serif` for serif fonts, `ui-monospace` for monospaced fonts

- Solution 7:

10. Problem 6: Why are all of my paraphaphs 16 or 14 px when they should be 18px?

- Solution 1:

11. problem 11: Why are headings and P's in a group kept at the contentSize?

### Questions & Answers

1. Question 1: IS https://schemas.wp.org/trunk/theme.json needed for v3 or is that I have (https://schemas.wp.org/wp/6.6/theme.json) good enough?

- Answer 1:

2. Question 2: is this correct: Group toggle off "inner blocks use content width" > Group with manual 1200 Content Width

- Answer 2:

3. Question 3: How to add title attr or aria labels to section tags?

- Answer 3:

3. Question 3: How to add a large double-quote symbol for testimonials? add a class and use `content`?

- Answer 3:

3. Question 3: How to implement `::selection`

- Answer 3:

4. Question 4: How do you adjust things? My footer links are not wanted, especially on the blog image, the link hover color is blue bit the text remains white,

- Answer 4:

......................

Monospace Font Stacks:

```css
/* 
from cssfontstack.com:
ui-monospace, 'Cascadia Code', 'Source Code Pro', Menlo, Consolas, 'DejaVu Sans Mono', monospace;

from tailwindcss.com:
ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, 'Liberation Mono', 'Courier New', monospace
*/
pre,
code {
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas,
    'Liberation Mono', 'Courier New', Courier, 'Lucida Console',
    'Lucida Sans Typewriter', monospace;
}
/* default system fonts */
pre,
code {
  font-family: ui-monospace, Consolas, Monaco, Courier, 'Courier New',
    'Lucida Sans Typewriter', monospace;
}
```

freelancewebprogrammer.com

- WordPress Development & Maintenance: Get a new site built, or your existing site rebuilt. Plus maintenance and managements services to keep it running fast, optimized, secure & error free, all of the time.

goldenoakwebdesign.com

- WordPress Services: Our WordPress Services are designed to cover every aspect of your website’s needs. From crafting custom designs to developing powerful e-commerce platforms, our solutions are built to help businesses of all sizes. Whether you need help designing a new website, maintaining an existing one, or creating custom features, our team has the expertise to deliver exceptional results.

ledetailwp.com

- We know WordPress: We’re a partner in your business journey – Let’s make it happen!

startupproduction.com

- Web Design & Internet Marketing: A professional website is the beginning of your internet marketing journey … we build strategies that increase traffic to your website and help convert those visitors into customers. With over 25 years experience, a large portfolio of web design, logo & graphic design, content marketing, internet marketing training & consulting, we provide affordable, hands-on and user-friendly assistance in creating or enhancing your online presence for your brand.

webnus.net

- We Are Webnus Team: Our focus is on crafting affordable, reliable, and user-friendly tools to help business owners and professionals succeed.

wpsiteplan.com

- Our monthly, flat-fee WP SitePlan service includes all of the maintenance & security measures that every WordPress website requires on a regular basis. With optional add-ons, our WordPress site maintenance services can even help you update content, troubleshoot issues, enhance landing pages, and optimize hosting and load times. Focus on your business while we will fully-manage your website.

```

```

..........................................................................

### About

- wpbuffs.com: - 5 - only 1 good section with 3 cards: `Who we are | Our mission | What we do`
- freelancewebprogrammer.com: - 6 - bio section, Why Hire Me section, Developer vs Web Designer vs Graphic Designer section, CTA found above footer
- matthewsdesign.co: - 7 - 3 good sections - Why we exist (should be Our Mission), What we do, How we do it, CTA found above footer
- goldenoakwebdesign.com: - 6 - Who Is... section, ...our work history section, What Is WordPress section, the Benefits of Using WordPress section, What Does All of This Mean for You section, review section, big contact form CTA
- jackandbean.com: - 5 - Our Purpose, Our Approach, Bio, CTA

- `Who we are` | `Our mission` | What we do
- Why we exist | What we do | How we do it
- Who Is.. | our work history || What Is WordPress | Benefits of Using WP
- Our Purpose | Our Approach | Bio

1. WHO WE ARE like goldenoakwebdesign.com: ...James Kernicky, a freelance WordPress designer and developer... based in Kentucky...`we provide a range of WordPress Services` - `we offer services that cater to all project sizes and industries. Our offerings include WordPress Development, WordPress Consulting, WordPress Maintenance Plans, and`
2. OUR MISSION: To create beautiful and functional websites enabling our clients to enhance their online presence
3. WHAT WE DO:
4. HOW WE DO IT: Clarify Your Message | Create Compelling Content | Design and Build - custom-tailored to our clients' needs - `Quality is the key` - like matthewsdesign.co home page: planning & content | design & revisions | build | deploy - if if redesign or fixes, ...
5. CTA

.........................................................................

### ✅ Contact

- modernwebstudios.com: no text, 2 col: contact info | form
- goldenoakwebdesign.com: large text top section | then repeat of the 2-col but with map | CTA | Review | 2-col form
- emilyjourney.com: 3 intro sentences, form below - about sidebar
- freelancewebprogrammer.com: - NO -
- wpassist.ca: 2 col - CTA | Form
- wpsiteplan.com: - NO -
- matthewsdesign.co: 2 col section | full-width map
- ledetailwp.com: info section | 2 col with text, form | google calendar section
- jackandbean.com: 2 col: form, p with map t hen info | reviews
- totalwpsupport.com: map section | form section | info sections |

1. intro p's like emilyjourney.com | bio in right col
2. 2 col like matthewsdesign.co
3. google map
4. Note like emilyjourney.com

.........................................................................

### ✅ Footer

- emilyjourney.com: 3 cols + bottom bar - Mission | Links | Links
- totalwpsupport.com: 4 cols + bottom bar - info | recent posts | Links | Logo
- thewebfactory.us: 4 cols + bottom bar - Info | Links | Links | Newsletter
- wpassist.ca: 4 cols + bottom bar - heading, cta, socials | Links x 3

- freelancewebprogrammer.com: 2 cols + bottom bar - Heading, p | Info
- goldenoakwebdesign.com: 3 cols + bottom bar - Info | Links | Links
- ledetailwp.com: 4 cols + bottom bar - logo | Text | Links | Socials + info
- matthewsdesign.co: 4 cols + bottom bar - Logo + p + socials | info | Links | Map
- wpsiteplan.com: 3 cols + bottom bar - logo + headline | Links | Socials

- jackandbean.com: 4 cols + bottom bar - logo, info, CTA | Links | Links | Links
- webnus.net: - NO -

3 Col

1. mission statement: To create websites our clients love so that can concentrate on their mission! - + contact info - city, ST address, 800 #, link to contact form, maybe logo
2. recent posts
3. links
4. links - OR - contact info if can't fit in col 1
5. bottom bar: copyright, links: privacy policy, terms of service, sitemap

```html
<ul class="wp-block-page-list has-large-font-size">
  <li class="wp-block-pages-list__item current-menu-item">
    <a
      class="wp-block-pages-list__item__link"
      href="http://lucid1.luna/services/"
      aria-current="page"
      >Services</a
    >
  </li>
</ul>
```

> Twenty25, username = Twenty25, PW = Twenty25
