# In context

`in-context` is a SASS utility. Currently it's a fork of the Foundation for Sites [`breakpoint`](https://foundation.zurb.com/sites/docs/media-queries.html#the-breakpoint-mixin). The `+breakpoint(x)` mixin has been aliased to `+in-context(x)`.

## Usage

Install npm package:

`yarn add in-context` / `npm install --save in-context`

Use in `.sass` files:

```sass
@import "~in-context"

.album
  img
    +in-context(small only)
      width: 100%
    +in-context(medium)
      width: calc(100% / 3)
```

Customize breakpoints:

```sass
$breakpoints: (small: 0, medium: 640px, large: 1024px)

@import "~in-context"
```

## Fork idea

The idea is to take the best responsive design utility from [ZURB Foundation for Sites](https://github.com/zurb/foundation-sites), and make it stand-alone & more flexible. Instead of reflecting on screen space, the mixin can reflect on dynamic contexts defined by parent nodes. Here is an API draft:

_Projects.jsx:_

```jsx
import './Projects.sass'

export default ({ images }) => (
  <div className='Projects'>
    <div className='Project Project-1'>
      {images.map((src, index) => (
        <Thumbnail
          key={`thumbnail-${index}`}
          src={`/images/{src}`}
          alt={`Image ${index + 1}/${images.length}`}
        />
      ))}
    </div>
  </div>
)
```

_Thumbnail.jsx:_

```jsx
import './Thumbnail.sass'

export default ({ src, alt }) => (
  <div className='Thumbnail'>
    <img src={ src } />
    <p className='alt-text'>{ alt }</p>
  </div>
)
```

_index.sass:_

```sass
@import '~in-context'

body

  @media (screen and max-width: 34rem)
    +set-child-context(small)

  @media (screen and (min-width: 34rem and max-width: 54rem))
    +set-child-context(medium)

  @media (screen and min-width: 54rem)
    +set-child-context(large)
```

_Projects.sass:_

```sass
@import '~in-context'

.Projects
  
  +in-context(medium down)
          
    .Project
      width: 100%
  
    .Thumbnail.alt-text
      +set-context(small)

  +in-context(large up)
  
    +set-child-context(medium)
  
    .Project
      width: 33.33%
```

_Thubmnail.sass:_

```sass
@import '~in-context'

.Thumbnail
  
  .alt-text
    
    +in-context(small)
      font-size: small
    
    +in-context(medium up)
      font-size: normal
```

--

Licensed under MIT License
