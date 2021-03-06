# Responsive Images from Scratch with SCSS/Sass

## 2/17/16

If you're not using a framework like Bootstrap or Foundation, but you'd like your images to automatically resize while keeping their aspect ratios, use the following SCSS:

```scss
#container-div {
  overflow: hidden;
  img {
    width: auto;
    height: auto;
    @media (min-width: 768px) and (max-width: 992px) { /* small to medium screens */
      max-width: 800px; 
    }  
    @media (max-width: 767px) { /* xs screens */
      max-width: 300px;
    }  
  }
}
```
The media queries and max widths are of course adjustable depending on your particular situation. (I also learned you can target only img tags preceded by a p, for example, using the selector `p + img`)

And that's it!
