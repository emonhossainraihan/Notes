## [bootstrap](https://www.jquery-az.com/bootstrap-margin-padding-classes-spacing-explained-5-examples/)

- The classes are named using the format `{property}{sides}-{size}` for xs and `{property}{sides}-{breakpoint}-{size}` for sm, md, lg, and xl.
- Where sides is one of:
  - **t** for classes that set margin-top or padding-top
  - **b** for classes that set margin-bottom or padding-bottom
  - **l** for classes that set margin-left or padding-left
  - **r** for classes that set margin-right or padding-right
  - **x** for classes that set both _-left and _-right
  - **y** for classes that set both _-top and _-bottom
  - **blank** for classes that set a margin or padding on all 4 sides of the element
- react-bootstrap uses functions and Hooks
- reactstraps documentation uses state in a lot of its code

## [BEM](http://getbem.com/)

Block Element Modifier is a methodology that helps you to create reusable components and code sharing in front-end development

##  7–1 Pattern

The architecture known as the 7–1 pattern (7 folders, 1 file), is a widely-adopted structure that serves as a basis for large projects. You have all your partials organised into 7 different folders, and a single file sits at the root level (usually named main.scss) to handle the imports — which is the file you compile into CSS.

```
sass/
|
|– abstracts/ (or utilities/)
|   |– _variables.scss    // Sass Variables
|   |– _functions.scss    // Sass Functions
|   |– _mixins.scss       // Sass Mixins
|
|– base/
|   |– _reset.scss        // Reset/normalize
|   |– _typography.scss   // Typography rules
|
|– components/ (or modules/)
|   |– _buttons.scss      // Buttons
|   |– _carousel.scss     // Carousel
|   |– _slider.scss       // Slider
|
|– layout/
|   |– _navigation.scss   // Navigation
|   |– _grid.scss         // Grid system
|   |– _header.scss       // Header
|   |– _footer.scss       // Footer
|   |– _sidebar.scss      // Sidebar
|   |– _forms.scss        // Forms
|
|– pages/
|   |– _home.scss         // Home specific styles
|   |– _about.scss        // About specific styles
|   |– _contact.scss      // Contact specific styles
|
|– themes/
|   |– _theme.scss        // Default theme
|   |– _admin.scss        // Admin theme
|
|– vendors/
|   |– _bootstrap.scss    // Bootstrap
|   |– _jquery-ui.scss    // jQuery UI
|
`– main.scss              // Main Sass file
```

## Designing: 

- **Responsive design**
  - Fluid layout
  - Media queries
  - Responsive image
- **Maintainable and scalable code**
  - Component driven Design
  - Block element modifier
- **Web performance**
  - Less http requests
  - compress code 
  - css preprocessor
  - less image/compressed image
