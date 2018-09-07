# # In context

`in-context` is a SASS utility. Currently it's a fork of the Foundation for Sites [`breakpoint`](https://foundation.zurb.com/sites/docs/media-queries.html#the-breakpoint-mixin). The `+breakpoint(x)` mixin has been aliased to `+in-context(x)`.

## Usage

Install npm package:

`yarn add in-context` / `npm install --save in-context`

Use in `.sass` files:

```
@import "~in-context"

.album
	img
		+in-context(small only)
			width: 100%
		+in-context(medium)
			width: calc(100% / 3)
```

Customize breakpoints:

```
$breakpoints: (small: 0, medium: 640px, large: 1024px)

@import "~in-context"
```

## Fork idea

The idea is to take the best responsive design utility from [ZURB Foundation for Sites](https://github.com/zurb/foundation-sites), and make it stand-alone & more flexible. Instead of reflecting on screen space, the mixin can reflect on dynamic contexts defined by parent nodes. Here is an API draft:

Projects.jsx:

```
import './Projects.sass'
export default ({ images }) => (
  <div className='Projects'>
    <div className='Project Project-1'>
      {images.map((src, index) => (
        <Thumbnail
          key={`thumbnail-${index}`}
          src={`/images/{src}`}
          description={`Image ${index + 1}/${images.length}`}
        />
      ))}
    </div>
  </div>
)
```

Thumbnail.jsx:

```
import './Thumbnail.sass'
export default ({ src, description }) => (
  <div className='Thumbnail'>
    <img src={ this.props.src } />
    <p className='description'>{ this.props.description }</p>
  </div>
)
```

index.sass:

```
body
  @media (screen and max-width: 34rem)
    +set-child-context(small)
  @media (screen and (min-width: 34rem and max-width: 54rem))
    +set-child-context(medium)
  @media (screen and min-width: 54rem)
    +set-child-context(large)
```

Projects.sass:

```
.Projects
  
  +in-context(medium down)
          
    .Project
      width: 100%
  
    .Thumbnail.description
      +set-context(small)

  +in-context(large up)
  
    +set-child-context(medium)
  
    .Project
      width: 33.33%
```

Thubmnail.sass:

```
.Thumbnail
  
  .description
    
    +in-context(small)
      font-size: small
    
    +in-context(medium up)
      font-size: normal
```