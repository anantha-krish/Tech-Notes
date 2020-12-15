Responsive design

 for Desktop first approach use
max-width 

for Mobile first approach use min-width

```scss
@media (max-width : 600px)
{
 // style codes
}
```

with normal css you have to repeat the code every where

its better to define mixins for responsive code

rem does not work properly in media queries

1em = 16px

```scss

@mixin respond($breakpoint)
{ @if $breakpoint == phone
  {
      @media (max-width: 37.5 em){   //600px
      @content
      }
    }
    
    @if $breakpoint == tab-port
  {
      @media (max-width: 56.25 em){   //900px
      @content
      }
    } 
    
       @if $breakpoint == tab-land
  {
      @media (max-width: 75 em){   //1200px
      @content
      }
    } 
    
  @if $breakpoint == big-desktop
  {
      @media (min-width: 112.5 em){   //1800px
      @content
      }
    } 
    
}

```

