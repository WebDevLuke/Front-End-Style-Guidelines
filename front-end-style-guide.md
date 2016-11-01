# Introduction
This style guidelines proposes a high-level approach to front-end development which aims to make our front-end code consistent, easily readable and readily maintainable by multiple developers. Front-end build is moving towards componentised UIs, so instead of building page-based websites, we’ll be building design systems and UI Toolkits that come together to form the resulting pages.

It is based upon my own experience working on large-scale development projects over the years and should be learned, understood, and implemented on new projects moving forwards. I want this to be a collaborative effort, so if deviations from this document can be justified then I'm happy to filter those back into the guidelines.


# Global Structure

A project's directory structure is built around the use of a build tool such as [Gulp](http://gulpjs.com/). `dev` contains sources files whilst `dist` contains the compiled.

~~~~
- dev
	- html
	- js
	- sass
- dist (Built on compile, not present by default)
	- css
	- js
	- index.html
	- etc.html
~~~~

# CSS

How your CSS is structured during those first days of a front-end build has a direct impact on its maintainability and scalability, so it's important that we structure our CSS in such a way that developers 6 months down the line can quickly jump on board and start being productive straight away.

### Structure

We're going to be using a CSS methodology called ITCSS (Inverted Triangle CSS). Quoting a [post](https://www.xfive.co/blog/itcss-scalable-maintainable-css-architecture/) on the subject:

> ITCSS helps you to organize your project CSS files in such way that you can better deal with (not always easy-to-deal with) CSS specifics like global namespace, cascade and selectors specificity.

The full article contains an indepth look at the methodology. In laymans terms, we essentially seperate our CSS into different layers (which we later compile into a single stylesheet via SASS) with increasing selector specificity as each layer progresses. This creates a more organic increase in specificity and explicitness so we avoid challenging situations where CSS higher up the stylesheet is overriding newly added CSS.

These layers in question are as follows:

- Settings – used with preprocessors and contain font, colors definitions, etc.
- Tools – globally used mixins and functions. It’s important not to output any CSS in the first 2 layers.
- Generic – reset and/or normalize styles, box-sizing definition, etc. This is the first layer which generates actual CSS.
- Elements – styling for bare HTML elements (like H1, A, etc.). These come with default styling from the browser so we can redefine them here.
- Objects – class-based selectors which define undecorated design patterns, for example media object known from OOCSS
- Components – specific UI components. This is where majority of our work takes place and our UI components are often composed of Objects and Components
- Utilities – helper classes with ability to override anything which goes before in the triangle, eg. hide helper class

Therefore we structure our CSS directory in the following way, with each layer represented as a subfolder:

~~~~
- sass
	- 01_settings
	- 02_tools
	- 03_generic
	- 04_elements
	- 05_objects
	- 06_components
	- 07_utilities
~~~~

Within each layer, new CSS should be seperated into mutiple files according to its area of concern. This means that we can quickly deduce which file may contain which CSS from simply looking at its filename and layer. For example a more fleshed out ITCSS project may look like:

~~~~
- sass
	- 01_settings
		- _settings.breakpoints.scss
		- _settings.grid.scss
		- _settings.typography.scss
	- 02_tools
		- _tools.clearfix.scss
		- _tools.responsive-breakpoints.scss
	- 03_generic
		- _generic.box-sizing.scss
		- _generic.reset.scss
	- 04_elements
		- _elements.headings.scss
		- _elements.images.scss
		- _elements.lists.scss
	- 05_objects
		- _objects.grid-system.scss
		- _objects.media.scss
	- 06_components
		- _components.buttons.scss
	- 07_utilities
		- _utilities.show-hide.scss
	- style.scss (Main SCSS file which pulls everything else in)
~~~~

### Naming

When it comes to naming our CSS classes, we're going to draw from various popular existing naming conventions. These are [BEM](http://getbem.com), [BEMIT](http://csswizardry.com/2015/08/bemit-taking-the-bem-naming-convention-a-step-further), [OOCSS](http://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces), and a little from [SMACSS](https://smacss.com/).

A typical classname for a button component and some of its variants would therefore be the following.


```
/*
[1] An example of a base class for a button. Since a button would belong on the component layer, we prefix it with a 'c'.
[2] This would be an element class for the button component, hench the 2 underscores.
[3] An example of a modifier for the button component. This is signified by the 2 dashs. 
[4] An example of a stateful modifier. These are used when elements require style changes for changes of state (eg: hover, click etc).
*/

.c-btn {
	/* [1] */ 
}

.c-btn__inner {
	/* [2] */ 
}

.c-btn--secondary {
	/* [3] */ 
}

.c-btn.is-active {
	/* [4] */ 	
}
```

A basic bootstrap-esq grid system may have the following classes:

```
/*
[1] A simple container object pattern, so therefore we prefix this with an 'o'.
[2] A typical column class.
[3] A responsive column using the BEMIT naming convention. The '@md' refers to the breakpoint which this styles would become active.
*/

.o-container {
	/* [1] */ 	
}

.o-col-4 {
	/* [2] */ 
}

@media (min-width: 740px) {
	.o-col-4@md {
		/* [3] */ 		
	}
}
```

Whilst a handy utility class may have the following structure:

```
/*
[1] Using the same principles of the previous example, this utility class would only activate once the 'lg' breakpoint has been reached. As this is a utility class, we use a 'u' prefix.
*/

@media (min-width: 925px) {
	.u-hide@lg {
		/* [1] */
	}
}

```

Using these naming coventions allows us to create classnames which share information regarding form, function and placement within the codebase. It also encourages consistancy and makes for clean, DRY stylesheets.

# Code Documentation



# Further Reading

WIP