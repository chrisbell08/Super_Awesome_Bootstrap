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