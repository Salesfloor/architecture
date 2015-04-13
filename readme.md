# CSS Coding Standards

## Contents 

* [Philosophy](#philosophy)
* [Architecture](#architecture)
* [Coding Style](#coding-style)
	* [Selectors](#selectors)
	* [Values](#values)
	* [Formatting](#formatting)
	* [Magic numbers and absolutes](#magic-numbers-and-absolutes)
	* [Commenting](#commenting)
	* [Important](#important)
* [Resources](#resources)
* [Credits](#credits)

## Philosophy

* The easiest way to keep CSS maintainable is by writing less of it.
	* Write object oriented CSS ([OOCSS](https://github.com/stubbornella/oocss/wiki)) that allows common design solutions to be extended and re-used repeatedly. This methodology is scalable, maintainable, and standards-based.
	* Writing less CSS means there is a less chance of things going wrong, resulting in a more robust front-end.
* [Specificity](http://css-tricks.com/specifics-on-css-specificity/) is the biggest problem in CSS, but we can work around it by respecting the cascade and building complete styles progressively through extending common styles.
	* All styles should follow the principle of keeping specificity as low as possible.


## Architecture

* The source order is designed to take advantage of inheritance in the cascade with the goal of  writing DRYer code.
	* Reset
	* Variables (used throughout the project)
	* Mixins (used throughout the project)
	* Patterns/Objects (any repeatable or extendable patterns/skeletons)
	* Components (combinations of patterns/objects + extended styles)
	* Sections (combinations of components)
	* Utilities

 The source order:
 	* Settings
 		* Global variables
 		* Configs
 	* Tools
 		* Mixins
 		* Functions
	* Base
		* Reset
	* Patterns
		* Forms
		* Buttons
		* Alerts
		* Etc.
	* Components
		* Video Player
		* Newsletter Signup
		* Etc.
	* Sections (should have a very small amount of content)
		* Confirmation page
		* Rep listing page
	* Themes
		* retailers overrides
	* Trumps
		* Others overrides, helper

## Coding Style

### Selectors

* IDs are not to be used in any stylesheets.
	* They are far too high in specificity. It would take 256 classes to override one ID.
	* Anything an ID can do, a class can do, however classes are more useful since they are reusable.
	* You can read arguments for this rule from [Chris Coyier](http://css-tricks.com/a-line-in-the-sand/), [Stubbornella](https://github.com/stubbornella/oocss/wiki/FAQ#should-i-use-ids-to-style-my-content), [Harry Roberts](http://csswizardry.com/2011/09/when-using-ids-can-be-a-pain-in-the-class/)
* Avoid over-qualifying selectors. For example: ```p.intro {}``` could have been ```.intro {}```
	* Makes the selector non-reusable.
	* Increases specificity unnecessarily.
* Avoid chaining selectors unnecessarily. For example: ```.msg.error {}``` could have been ```.error-msg {}```. Chaining two selectors makes it doubly specific with no real benefit. It is less clear for a new developer to understand at a glance what exactly a generic error class does.
* No camel case. Selectors should be lowercase with dash-separated words.

### Values
The line-height should always be a multiple of the vertical rhythm variable. All other values should be as relative as possible. (see [magic numbers and absolutes](#magic-numbers-and-absolutes))
* All vertical spacing around elements should consider the vertical rhythm.
* Use [single direction](http://csswizardry.com/2012/06/single-direction-margin-declarations/) (downward) margin declarations. This would mean only use margin-bottom, no margin-top, to keep a constant downward flow of elements.
	* Easier to define vertical rhythm in one fell swoop.
	* More confidence in (re)moving components if you know their margins all push in the same direction.
	* Components and elements donÃƒÂ¢Ã¢â€šÂ¬Ã¢â€žÂ¢t have to necessarily live in a certain order if their margins arenÃƒÂ¢Ã¢â€šÂ¬Ã¢â€žÂ¢t dependent on adjoining sides.
	* Not being concerned with collapsing margins means one less thing to worry about.

### Formatting

* Single-line styles should be formatted as follows:
```
[selector(s)] { [property]: [value]; }
```
* Multiple-line styles should be formatted as follows:
```
[selectors] {
	[property]: [value];
	[property]: [value];
}
```
* Use soft-tabs with a two space indent. Spaces are the only way to guarantee code renders the same in any person's environment.

* Avoid [unnecessary nesting](http://thesassway.com/intermediate/avoid-nested-selectors-for-more-modular-css) in SCSS. At most, aim for two levels. It is perfectly reasonable to use nesting when creating components, however limit the amount of levels as much as possible.
	* Increases specificity unnecessarily.
	* Limits portability.
	* Less efficient.
	* If you cannot help it, step back and rethink your overall strategy (either the specificity needed, or the layout of the nesting).

### Naming conventions

Follow a very simple naming convention: hyphen (-) delimited strings, with BEM-like naming for more complex pieces of code.

#### Hyphen Delimited

All strings in classes are delimited with a hyphen (-), like so:
```
.page-head {}

.sub-content {}
```
Camel case and underscores are not used for regular classes; the following are incorrect:
```
.pageHead {}

.sub_content {}
```

### BEM like Naming

BEM, meaning Block, Element, Modifier, is a front-end methodology coined by developers working at Yandex. Whilst BEM is a complete methodology, here we are only concerned with its naming convention. Further, the naming convention here only is BEM-like; the principles are exactly the same, but the actual syntax differs slightly.

BEM splits componentsâ€™ classes into three groups:

* Block: The sole root of the component.
* Element: A component part of the Block.
* Modifier: A variant or extension of the Block.

This gives:

```
<div class="box  profile  profile--is-pro-user">

    <img class="avatar  profile__image" />

    <p class="profile__bio">...</p>

</div>
```

### Magic numbers and absolutes

* A magic number is a number which is used because it just work. These are bad
because they rarely work for any real reason and are not usually very
futureproof or flexible/forgiving. They tend to fix symptoms and not problems.
	* For example, using `.dropdown-nav li:hover ul { top: 37px; }` to move a dropdown
	to the bottom of the nav on hover is bad, as 37px is a magic number. 37px only
	works here because in this particular scenario the `.dropdown-nav` happens to be
	37px tall.
	* Instead you should use `.dropdown-nav li:hover ul { top: 100%; }` which means no
	matter how tall the `.dropdown-nav` gets, the dropdown will always sit 100% from
	the top.
	* Every time you hard code a number think twice; if you can avoid it by using
	keywords.
	* Every hard-coded measurement you set is a commitment you might not necessarily
	want to keep.


### Commenting

* Write more comments! Write as much information as you possibly can. If you integrate a hacky IE9 fix from stack overflow, it wouldn't hurt to briefly explain it and even provide a link so future devs understand the justification of our choices. We can then safely remove code later if we drop support for that particular browser.
* Write comments to [quasi-qualify](http://csswizardry.com/2012/07/quasi-qualified-css-selectors/) selectors.
	* Let's say we have an unqualified ```.breadcrumb {}``` selector. It's not immediately apparent what type of element that is expected to style (is it an ol, ul, li, ... ?).
	* What we can do to help other developers identify unqualified selectors is to quickly add a comment inline to indicate that. 
	Ex: ```.breadcrumb {} // ul```
* Don't be shy to include HTML code snippets within comments to indicate how the style will be used. Comments are removed at build time so they will never be seen by the end-user. For example:
```
//	<ul class="nav">
//		<li><a href="/">Home</a></li>
//	</ul>
.nav {}
	.nav > li {}
		.nav > li > a {}
```
* Use ```//``` for comment blocks (instead of ```/* */```).

### !important

* Because we maintain control of specificity within our cascade, it should never be necessary to use an ```!important``` on any property values. Favor refactoring the troublesome code before resorting to importants. Low specificity is our precious commodity, so importants should be very rare in our stylesheets.

## Resources
* [How Specificity Works](http://css-tricks.com/specifics-on-css-specificity/)
* [Specificity Diagram](http://cssspecificity.com/)
* [Avoid nested selectors for more modular CSS](http://thesassway.com/intermediate/avoid-nested-selectors-for-more-modular-css)

### Credits
This style guide is a living document based on a mixture of ideas from [GitHub](https://github.com/styleguide), [The Sass Way](http://thesassway.com/), [Chris Coyier](http://css-tricks.com/) and [Harry Roberts](https://twitter.com/csswizardry).