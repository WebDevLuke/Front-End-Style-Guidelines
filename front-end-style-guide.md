# Introduction
This style guidelines proposes a high-level approach to front-end development which aims to make our front-end code consistent, easily readable and readily maintainable by multiple developers.

It is based upon my own experience working on large-scale development projects over the years and should be learned, understood, and implemented on new projects moving forwards. I want this to be a collaborative effort, so if deviations from this document can be justified then I'm happy to filter those back into the guidelines.

# Global Directory Structure
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

### Structure

How CSS is structured during those first days of a front-end build has a direct impact on its maintainability and scalability, so it's important that we structure our CSS in such a way that developers 6 months down the line can quickly jump on board and start being productive straight away.

We're going to be using an ever so slightly tweaked version of a CSS methodology called ITCSS (Inverted Triangle CSS). Quoting a [post](https://www.xfive.co/blog/itcss-scalable-maintainable-css-architecture/) on the subject:

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

# Further Reading

WIP