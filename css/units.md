# Units in CSS

- **px: do not scale**. Use only for borders and the base font size on the html element.
- **em, rem: scale with the font-size**.
    - 1.5em is 150% of the font size of the current element.
        - Use em wisely for elements relative to the font-size (icons as a font for instance) and use it for media query breakpoints.
    - 0.75rem is 75% of the font size of the html element.
        - Use rem for typography and everything vertical like margins and paddings.
- **%, vh, vw: scale with the resolution**.
    - vh and vw are percentages of the viewport height and width.
    - perfect for layouts or in a calc to compute the remaining space available (`min-height: calc(100vh - #{$appBarHeight})`).

