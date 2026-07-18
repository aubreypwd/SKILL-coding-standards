# Accessibility Coding Standards

> Snapshot source: https://developer.wordpress.org/coding-standards/wordpress-coding-standards/accessibility/
> Retrieved: 2026-07-18
> Conversion: HTML to Markdown with Pandoc.

<div id="wp--skip-link--target"
class="wp-block-group alignwide is-layout-flow wp-block-group-is-layout-flow"
role="main">

# Accessibility Coding Standards

<div class="wp-block-wporg-sidebar-container is-layout-flow wp-block-wporg-sidebar-container-is-layout-flow"
breakpoint="1300px">

<div class="wp-block-wporg-table-of-contents">

<div class="wporg-table-of-contents__header">

## In this article

<span class="screen-reader-text">Table of Contents</span>

</div>

[About WCAG A, AA, and AAA Conformance
Levels](#about-wcag-a-aa-and-aaa-conformance-levels)

[Applying WCAG Conformance Levels](#applying-wcag-conformance-levels)

- [Principles](#principles)
- [Guidance](#guidance)
- [Success Criteria](#success-criteria)
- [Techniques: Sufficient, Advisory, and
  Failures](#techniques-sufficient-advisory-and-failures)

[Authoritative Resources](#authoritative-resources)

- [Technical and / or specific
  topics](#technical-and-or-specific-topics)

</div>

[<span class="wp-exclude-emoji" aria-hidden="true">↑</span> Back to
top](#wp--skip-link--target)

</div>

<div class="entry-content wp-block-post-content is-layout-flow wp-block-post-content-is-layout-flow">

Code integrated into the WordPress ecosystem – including WordPress core,
WordPress.org websites, and official plugins, is expected to conform to
the Web Content Accessibility Guidelines (WCAG), version 2.2, at level
AA.

New or updated interfaces are encouraged to incorporate the Authoring
Tool Accessibility Guidelines (ATAG) 2.0. The most significant way that
ATAG 2.0 guidelines can be incorporated is by emphasizing choices that
help people make more accessible content: encouraging alternative text,
captions, and semantic structures, for example.

Official information about web accessibility standards can be divided
into two groups: “normative” and “informative” documents. Only the
guidelines themselves are normative, and establish the actual
requirements for conforming to WCAG 2.2. Other documents should be
considered to be informational, and offer help in interpreting the
guidelines, but are not definitive.

The WordPress A11y team is in the process of developing a library of
recommended accessibility patterns to help describe the WordPress
recommended way to accomplish a variety of interfaces. These may not be
the only reasonable way to create an accessible example of the pattern,
but are preferred for the sake of consistency across WordPress.

Normative Documents:

- [W3C WCAG 2.2](https://www.w3.org/TR/WCAG22)
- [W3C ATAG 2.0](https://www.w3.org/TR/ATAG20/)
- [W3C WAI ARIA 1.1](https://www.w3.org/TR/wai-aria/)

Informative Documents:

- [W3C Understanding WCAG
  2.2](https://www.w3.org/WAI/WCAG22/Understanding/)
- [W3C Using ARIA](https://www.w3.org/TR/using-aria/)
- [W3C WAI-ARIA Authoring Practices Guide (accessible design
  patterns)](https://www.w3.org/WAI/ARIA/apg/)
- [W3C Introduction to
  ATAG](https://www.w3.org/WAI/standards-guidelines/atag)

## [About WCAG A, AA, and AAA Conformance Levels](#about-wcag-a-aa-and-aaa-conformance-levels)

The WordPress commitment is to conform to all WCAG 2.2 Level A and Level
AA guidelines. Conformance to level AAA success criteria is encouraged
where relevant, as is exceeding the accessibility of any of these
guidelines.

**Level A** success criteria address concerns considered to be
accessibility barriers on a very wide scale that will prevent many
people from accessing the site and the minimum set of accomplished goals
required for the majority of web-based interfaces.

**Level AA** success criteria address concerns that are generally
somewhat more complicated to address and may impact smaller groups of
people, but are still common needs with broad reach.

**Level AAA** success criteria are mostly targeted at very specific
needs and may be quite difficult to implement effectively.

[W3C Quick Reference to WCAG 2.2 Level A and Level AA
Requirements](https://www.w3.org/WAI/WCAG22/quickref/?versions=2.2&currentsidebar=%23col_customize&levels=aaa)

## [Applying WCAG Conformance Levels](#applying-wcag-conformance-levels)

WCAG 2.2 consists of 4 layers:

- Principles
- Guidance
- Success criteria
- Sufficient and advisory techniques

### [Principles](#principles)

When applying WCAG 2.2, the guidance and success criteria are organized
around 4 principles. These principles place emphasis on how people
interact with content and must be:

- **Perceivable** – interacting with the content using the medium that
  they are familiar with. For example, providing text alternatives for
  those who are blind.
- **Operable** – finding and using content is accessible. For example,
  being able to use a keyboard or a screen reader.
- **Understandable** – content uses clear language and is
  understandable. For example, use meaningful labels, explain all
  abbreviations.
- **Robust** – content can be interpreted in a range of ways. For
  example, assistive technologies are able to interpret and parse
  content.

### [Guidance](#guidance)

Each principle is supported by a list of guidelines to ensure that
content is more accessible and presentable across the different devices
that meet a user’s disability. The guidelines are listed below, the full
detail can be found in the WCAG 2.2.

#### [Principle: Perceivable](#principle-perceivable)

**Guideline 1.1 Text Alternatives**  
Provide text alternatives for any non-text content so that it can be
changed into other forms people need, such as large print, braille,
speech, symbols or simpler language.

**Guideline 1.2 Time-based Media**  
Provide alternatives for time-based media. For example, include captions
and transcripts for audio or video clips.

**Guideline 1.3 Adaptable**  
Create content that can be presented in different ways (for example
simpler layout) without losing information or structure.

**Guideline 1.4 Distinguishable**  
Make it easier for users to see and hear content including separating
foreground from background.

#### [Principle: Operable](#principle-operable)

**Guideline 2.1 Keyboard Accessible**  
Make all functionality available from a keyboard.

**Guideline 2.2 Enough Time**  
Provide users enough time to read and use content.

**Guideline 2.3 Seizures and Physical Reactions**  
Do not design content in a way that is known to cause seizures or
physical reactions.

**Guideline 2.4 Navigable**  
Provide ways to help users navigate, find content, and determine where
they are.

**Guideline 2.5 Input Modalities**  
Make it easier for users to operate functionality through various inputs
beyond keyboard.

#### [Principle: Understandable](#principle-understandable)

**Guideline 3.1 Readable**  
Make text content readable and understandable.

**Guideline 3.2 Predictable**  
Make Web pages appear and operate in predictable ways.

**Guideline 3.3 Input Assistance**  
Help users avoid and correct mistakes.

#### [Principle: Robust](#principle-robust)

**Guideline 4.1 Compatible**  
Maximize compatibility with current and future user agents, including
assistive technologies.

### [Success Criteria](#success-criteria)

Each guidance has a [specific list requirements that must be met for
your content to be accessible](https://www.w3.org/WAI/WCAG21/quickref/).
These tests can be carried out using automated software and or human
testers. You can find more information on how to meet the success
criteria in [Understanding Levels of
Conformance](https://www.w3.org/WAI/WCAG21/Understanding/conformance#levels).
Whilst these criteria are important, usability testing is still
important and should be carried out alongside any accessibility testing.

### [Techniques: Sufficient, Advisory, and Failures](#techniques-sufficient-advisory-and-failures)

Techniques (code examples, resources, and tests) for guidance and
success criteria that can help in making content more accessible, are
divided into three categories:

- Sufficient – required and help meet the success criteria
- Advisory – suggestions and go beyond what is required
- Failures – cause problems and fail to meet the success criteria

For more information on techniques, visit [Understanding Techniques for
WCAG Success
Criteria](https://www.w3.org/WAI/WCAG21/Understanding/understanding-techniques).

## [Authoritative Resources](#authoritative-resources)

- [WebAIM: Web Accessibility In Mind](https://webaim.org/) (see Articles
  and Resources)
- [UK Government Digital Service](https://gds.blog.gov.uk)
- [Accessibility in Government Blog
  (UK)](https://accessibility.blog.gov.uk/)
- [Create Accessible Software & Websites – Section 508
  (US)](https://www.section508.gov/develop/software-websites/)
- [Blog \| TPGi](https://www.tpgi.com/blog/)
- [Web Accessibility Blog (Deque)](https://www.deque.com/blog/)
- [Tink – Léonie Watson](https://tink.uk) (Léonie Watson)
- [Adrian Roselli](https://adrianroselli.com)
- [Scott O’Hara](https://www.scottohara.me)
- [Joe Dolson](https://www.joedolson.com/blog)
- [Sarah Higley](https://sarahmhigley.com/)
- [Marco’s Accessibility Blog](https://www.marcozehe.de/)
- [Karl Groves](https://karlgroves.com/)
- [Inclusive Components](https://inclusive-components.design) (Heydon
  Pickering)
- [Accessibility London (London, United
  Kingdom)](https://www.meetup.com/London-Accessibility-Meetup/) (London
  accessibility meetup: they live stream meetups on youtube)
- [24 Accessibility](https://www.24a11y.com/)
- [Mozilla Accessibility – Users first, no matter their
  abilities](https://blog.mozilla.org/accessibility/)
- [WordPress Accessibility
  Meetup](https://www.meetup.com/wordpress-accessibility-meetup-group/)
- [Equalize Digital Blog](https://equalizedigital.com/resources/)
- [WordPress Accessibility Day Conference](https://wpaccessibility.day)

### [Technical and / or specific topics](#technical-and-or-specific-topics)

- [Accessibility Support](https://a11ysupport.io/) (Will your code work
  with assistive technologies?)
- [Accessibility APIs: A Key To Web
  Accessibility](https://www.smashingmagazine.com/2015/03/web-accessibility-with-accessibility-api/)
  (by Léonie Watson)
- [How accessibility trees inform assistive
  tech](https://hacks.mozilla.org/2019/06/how-accessibility-trees-inform-assistive-tech/)
  (by Hidde de Vries)
- [What is this thing and what does it
  do?](https://www.youtube.com/watch?v=YLihNhn_MO4) (presentation by
  Karl Groves)
- [The Browser Accessibility
  Tree](https://www.tpgi.com/the-browser-accessibility-tree/) (by Steve
  Faulkner)
- [Brief history of browser accessibility
  support](https://www.tpgi.com/brief-history-of-browser-accessibility-support/)
  (by Steve Faulkner)
- [ARIA Landmarks Example: General
  Principles](https://www.w3.org/TR/wai-aria-practices/examples/landmarks/)
- [ARIA Landmarks Example: HTML Sectioning
  Elements](https://www.w3.org/TR/wai-aria-practices/examples/landmarks/HTML5.html)
- [Mozilla Developer Docs –
  Accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility)

</div>

<div class="wp-block-group entry-meta is-content-justification-left is-nowrap is-layout-flex wp-container-core-group-is-layout-9c958f0d wp-block-group-is-layout-flex"
style="border-top-color:var(--wp--preset--color--light-grey-1);border-top-width:1px;margin-top:var(--wp--preset--spacing--40);margin-bottom:var(--wp--preset--spacing--40);padding-top:var(--wp--preset--spacing--40)">

<div class="wp-block-group is-layout-constrained wp-container-core-group-is-layout-979f74b1 wp-block-group-is-layout-constrained">

First published

<div class="wp-block-post-date">

April 25, 2019

</div>

</div>

<div class="wp-block-group is-layout-constrained wp-container-core-group-is-layout-979f74b1 wp-block-group-is-layout-constrained">

Last updated

<div class="wp-block-post-date__modified-date wp-block-post-date">

September 3, 2024

</div>

</div>

<div class="wp-block-group is-layout-constrained wp-container-core-group-is-layout-979f74b1 wp-block-group-is-layout-constrained">

Edit article

[Improve it on GitHub<span class="screen-reader-text">: Accessibility
Coding
Standards</span>](https://github.com/WordPress/wpcs-docs/edit/master/wordpress-coding-standards/accessibility.md)

</div>

<div class="wp-block-group is-layout-constrained wp-container-core-group-is-layout-979f74b1 wp-block-group-is-layout-constrained">

Changelog

[See list of changes<span class="screen-reader-text">: Accessibility
Coding
Standards</span>](https://github.com/WordPress/wpcs-docs/commits/master/wordpress-coding-standards/accessibility.md)

</div>

</div>

<div class="wp-block-group alignfull is-content-justification-space-between is-nowrap is-layout-flex wp-container-core-group-is-layout-11b2f7b6 wp-block-group-is-layout-flex"
style="border-top-color:var(--wp--preset--color--light-grey-1);border-top-style:solid;border-top-width:1px;margin-top:var(--wp--preset--spacing--20);margin-bottom:var(--wp--preset--spacing--20);padding-top:var(--wp--preset--spacing--30);padding-bottom:var(--wp--preset--spacing--30)">

<div class="post-navigation-link-previous wp-block-post-navigation-link">

<a
href="https://developer.wordpress.org/coding-standards/wordpress-coding-standards/"
rel="prev"><span class="post-navigation-link__label"
aria-hidden="true">Previous</span> <span
class="post-navigation-link__title" aria-hidden="true">WordPress Coding
Standards</span> <span class="screen-reader-text">Previous: WordPress
Coding Standards</span></a>

</div>

<div class="post-navigation-link-next wp-block-post-navigation-link">

<a
href="https://developer.wordpress.org/coding-standards/wordpress-coding-standards/css/"
rel="next"><span class="post-navigation-link__label"
aria-hidden="true">Next</span> <span class="post-navigation-link__title"
aria-hidden="true">CSS Coding Standards</span> <span
class="screen-reader-text">Next: CSS Coding Standards</span></a>

</div>

</div>

</div>

