# Super Awesome Bootstrap
Super Awesome Bootstrap (SA Bootstrap) is a Front End Framework that extends the core functionality of Bootstrap, it's like Bootstrap on steroids! It uses BEM for the naming conventions and changes the core functionality of Bootstrap to Opt-in, preventing bloated CSS. This makes the setting up, developing and managing your projects easier.

This project is still in its infancy and so are the docs. Please feel free to highlight any mistakes in the code or the docs so the project can be improved.

### Single Class Rule
When using Bootstrap the modular class system can leave your HTML littered with classes. To prevent this SA Bootstrap utilizes a single class rule. The idea behind the rule is that no HTML element may contain more than one class. This can be extremely powerful when combined with the SASS @extend rule for importing classes or placeholders, and SASS Variables.

#### Usage
HTML
```html
<div class="page-heading">
  <h1>This is a title</h1>
</div>
```
SASS
```SCSS
.page-heading {
    @extend .page-header;
    color: $brand-primary;
}
```

### BEM
The Block Element Modifier (BEM) naming convention can be used to break down the elements of our web pages into reusable components. Best practice is to use one scss partial file for each Block, these should be saved in the components folder. This means that the class ".blog__heading" would be in the "_blog.scss" partial, this makes your code much easier to read for other developers and helps keep the code maintainable over time.

The default layout for BEM elements is block__element--modifier. SA Bootstrap has builtin mixins to generate the classes automatically. The definition of what is a Block, Element or Modifier is talked about in detail on the BEM website: https://bem.info/ but can be implemented however works best for you. The most important thing is that you're consistent in your interpretation.

#### Example
```scss
.my-block {
    @include element(my-element) {
        @include modifier(my-modifier) {

        }
    }
}
```

#### Usage
HTML my-modal.php
```html
<div class="my-modal fade">
  <div class="my-modal__dialog">
    <div class="my-modal__content">
      <div class="my-modal__header">
        <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
        <h4 class="my-modal__title--primary">Modal title</h4>
      </div>
      <div class="my-modal__body--dark">
        <p>One fine body&hellip;</p>
      </div>
    </div><!-- /.modal-content -->
  </div><!-- /.modal-dialog -->
</div><!-- /.modal -->
```
SASS _my-modal.scss
```scss
.my-modal {
    @extend .modal;

    @include element(dialog) {
        @extend .modal-dialog;
    }

    @include element(content) {
        @extend .modal-content;
    }

    @include element(header) {
        @extend .modal-header;
    }

    @include element(title) {
        @extend .modal-title;

        @include modifier(primary) {
            @extend .my-modal__title;
            color: $brand-warning;
        }
    }
    @include element(body) {
        @extend .modal-body;

        @include modifier(dark) {
            @extend .my-modal__body;

            background-color: $gray-darker;
            color: white;
        }
    }
}
```
### Directory Structure

#### Root

##### style.scss
Main import file, generates the .css file.

##### components.scss
Each new componenet should be added here as an import line. You can set up your sass globbling to point to this file and automatically populate the file with the component names. If your using ruby sass you can use the global import command.
```SCSS
@import 'components/*';
```

##### shame.scss
Used just like a shame.css file.

##### wordpress.scss
This file contains the Wordpress Header for naming your theme and also the core wordpres styles. This isn't imported by default, if your using wordpress you can uncomment the import for this in the style.scss file.

#### Base
The base styling the project is built on. This folder contains the styling for the base HTML elements, the css reset, the colours used in the project and a copy of the Bootstrap variables that we can customize. It's also used to define any custom variables for use in the project.

#### Components
Components are the block elements in our BEM css styling. All Components code and styling should be built in a way that is independant of other modules. The idea is to build not only maintainable code but reuseable code across projects using variables when possible.

#### Utilities
The Utilities folder contains our 3rd party CSS imports such as bootstrap and all other helpers and functions.

##### Lib
Third party Libs go in this Folder, including Bootstrap. Within the Bootstrap folder is a bootstrap overrides partial to override any of the default bootstrap behaviour you don't want. Super Awesome Bootstrap ships with the latest version of Bootstrap, animate.css, jQuery UI & Owl Carousel.

##### helpers.scss
Used to store global CSS classes. See the helpers section for more info.

##### lib.scss
Loads in all 3rd party libs. By defualt only Bootstrap is imported. If you want to use and of the other libs you need to uncomment them here.

#### mixins.scss
For our mixins. SA Bootstrap ships with some handy mixins, more info can be found in the mixins sections.

### Opt In Components
To keep our css file as small as possible the main bootstrap import has been changed to 'opt in', meaning all components are not imported by default. Instead you need to uncomment the components you wish to import from the utilities > lib > bootstrap (version) > bootstrap.scss file. By default it will look like this:
```SCSS
// Core variables and mixins
@import "bootstrap/mixins";

// Reset and dependencies
// @import "bootstrap/normalize";
// @import "bootstrap/print";
// @import "bootstrap/glyphicons";

// Core CSS
@import "bootstrap/scaffolding";
@import "bootstrap/type";
@import "bootstrap/code";
@import "bootstrap/grid";
@import "bootstrap/tables";
@import "bootstrap/forms";
@import "bootstrap/buttons";

// Components
// @import "bootstrap/component-animations";
// @import "bootstrap/dropdowns";
// @import "bootstrap/button-groups";
// @import "bootstrap/input-groups";
// @import "bootstrap/navs";
// @import "bootstrap/navbar";
// @import "bootstrap/breadcrumbs";
// @import "bootstrap/pagination";
// @import "bootstrap/pager";
// @import "bootstrap/labels";
// @import "bootstrap/badges";
// @import "bootstrap/jumbotron";
// @import "bootstrap/thumbnails";
// @import "bootstrap/alerts";
// @import "bootstrap/progress-bars";
// @import "bootstrap/media";
// @import "bootstrap/list-group";
// @import "bootstrap/panels";
// @import "bootstrap/responsive-embed";
// @import "bootstrap/wells";
// @import "bootstrap/close";

// Components w/ JavaScript
// @import "bootstrap/modals";
// @import "bootstrap/tooltip";
// @import "bootstrap/popovers";
// @import "bootstrap/carousel";

// Utility classes
//@import "bootstrap/utilities";
//@import "bootstrap/responsive-utilities";
```
You can go into the file an uncomment each component as you want to use it.


### Declaration Order
It's stated in the Code Guide created by Mark Otto (The creator of Bootstrap) that css / sass should have a consistant declarative order. The order stated in the code guide only really covers basic CSS so I've extened that to the declarative order shown below. The full code guide can be found here: http://mdo.github.io/code-guide/.

#### Dependancies
Any usage of the @extend SASS directive is classed as a dependancy. This is a global programming concept, a class that extends another class is by definition dependant on that class. Mixins however should NOT be treated as dependancies. they should be placed in the section relative to the output.

#### Positioning
Positioning comes next because it can remove an element from the normal flow of the document and override box model related styles.

#### Box Model
The box model comes next as it dictates a component's dimensions and placement.

#### Typograpy
Anything relating to the style and presentation of the text on the page.

#### Visual
Rules that control the visual style of the element NOT the typography.

#### Misc
Any other classes such as tranistions, transforms, opacity etc are placed here.

#### Pseudo Classes
Pretty self explanitory, Puesdo classes are things like :last-of-type, :nth-child(1) and :after.

#### States
This is the "State" of the element, this can be :hover, :active, :visited or :focus.

#### Media Queries
After all the styles have been declared we then declare our responsive stuff.

#### Nested Elements
Finally we need nest all the child elements.

#### Example
```SCSS
.declaration-order {
  /* Dependencies */
  @extend .xyz;

  /* Positioning */
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 100;

  /* Box-model */
  display: block;
  float: right;
  width: 100px;
  height: 100px;
  @include spacing(padding, all);

  /* Typography */
  font: normal 13px "Helvetica Neue", sans-serif;
  line-height: 1.5;
  color: #333;
  text-align: center;

  /* Visual */
  background-color: #f5f5f5;
  border: 1px solid #e5e5e5;
  border-radius: 3px;

  /* Misc */
  opacity: .8;

  /* Pseudo Classes*/
  &:first-of-type {}

  /* States */
  &:hover {}

  /* Media Queries */
  @include screen(max, 1200px) {}

  /* Nesting  */
  a {}

  /* Elements */
  @include element(wrapper) {

    /* Dependencies */
    @extend .container;

    /* Modifiers */
    @include element(fluid) {

       /* Dependencies */
       @extend .container-fluid;
    }
  }
}
```

### Mixins
SA Bootstrap ships with some handy mixins to make our life a little easier.

#### Media Queryies
This is an extremely flexable mixin for creating media queries. The $screen variable can use any of the default bootstrap media queries (xs, sm, md, lg). We can also set up custom media queries using the 'min', 'max' or 'range' values. 'min' & 'max' use use $value1 as the min or max value in the media query, 'range' uses $value1 as the min and $value2 as the max in a min & max media query.
```SCSS
    // To set a media query just for the Bootstrap breakpoint
    @include screen(xs) {
        // Some styles here
    }

    // Here's how you do it using a min value of 480px
    @include screen(min, 480px) {
        // Some styles here
    }

    // a max width value would look like this
    @include screen(max, 480px) {
        // Some styles here
    }

    // And if we wanted to use a custom media query range between 480px - 1200px
    @include screen(range, 480px, 1200px ) {
        // Some styles here
    }

    // You can even pass in boostrap variables instead of px
    @include screen(min, $screen-xs) {
        // Some styles here
    }
```

#### Bem Selectors
This benefits of using this are convered in the BEM section above. Should you want to use BEM methodology this is how.

```SCSS

// Declare the block first
.block {

   // Nest the elements inside the block
    @include element(wrapper) {
        // Some styles here

        // And the modifers inside the elements
        @include modifier(first) {
        // Modifier Styles here
        }
    }
}

```

#### Global Spacing
One thing I have always struggled with in bootstrap is vertical spacing. Yes the horizontal spacing is kept tidy by the grid gutter but vertical spacing is kind of forgotten about. The spacing mixing attempt to address this issue, it uses the $spacing mixin found in the global vars file. By default this is set to 15px (The same as the bootstrap gutter width). The mixin accepts two agruments, type and direction. Type will be either 'padding' or 'margin', Direction can be any of('all', 'vertical', 'sides', 'top', 'left', 'right', 'bottom'). There is a 3rd argument of multiplier that will x by the $spacing variable, this is set to 1 by default.

```SCSS
    // A basic call would be
    @mixin spacing(padding, all);

    // To double the padding size we would call with a multiplier
    @mixin spacing(padding, all, 2);
```
#### Shorthand Cols
This mixin life much easier when delcaring the bootstrap .col-* grid classes. The first argument is the number of xs columns, it then works up to lg. We only have to declare as many as are needed however, For Example,

```SCSS
    // What would need to be wrote like this
    @extend .col-xs-12;
    @extend .col-sm-6;
    @extend .col-md-4;
    @extend .col-lg-2;

    // Could be shortened to this
    @include cols(12, 6, 4, 2);

    // But we dont have to pass all 4 args, it could just be
    @include cols(12, 6);

    // This would be
    @extend .col-xs-12;
    @extend .col-sm-6;

```

#### Headings
Pretty simple, sets styles on all headings h1 - h6.
```SCSS
@include headings(){
    // Some styles here
}
```

### Variables
Global variables should be kept in the variables file. Component specific variables should be kept in the appropriate sass file.

### Helpers
Helper styles (Global classes not already in Bootstrap) go here.

### Buttons
By default this file converts the standard btn classes of bootstrap into BEM classes.

````HTML
    <!-- This button -->
    <button class="btn btn-primary"></button>

    <!-- Can be declared like this -->
    <button class="btn__primary"></button>

    <!-- You can then build modifiers into the buttons like so -->
    <button class="btn__primary--outline"></button>
````
````SCSS
.btn {
    @include element(primary) {
        @include modifier(outline) {
            @extend .btn__primary;

            // Some styles here
        }
    }
}
````

### Updating
We use the standard version control numbering system. The version number can be found in the style.css file and will look like this (x.y.z). Z is a point release and just contains minor changes, Y is a major but none breaking change meaning you can update your project will no trouble, X is a breaking change meaning it is not compatable with older verisons.

#### Updating Bootstrap
If you would like to update bootstrap manually in your project this is fairly simple.
- 1. Create a new folder with the new version number (This allows us to rollback if we have any issues)
- 2. Copy the bootstrap folder from your bootstrap download into the the folder you just created.
- 3. Copy the _boootstrap.scss, _index.scss & _overrides.scss files from the old bootstrap folder to the new one.
- 4. Change the bootstrap version in the _lib.scss file.
- 5. Compile....
