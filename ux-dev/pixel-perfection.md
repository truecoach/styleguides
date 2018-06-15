# Pixel Perfection

Pixel perfection is regarded by some to be controversial (either [meaningless](https://www.designernews.co/comments/222544) or [madness](http://blog.kylegawley.com/pixel-perfection-is-madness/)) in the sense that designs [are not rendered the same](http://dowebsitesneedtolookexactlythesameineverybrowser.com/) in all devices. Nevertheless, many clients and their brands require a very high level of UX precision. With the right planning and process, it is possible to approach pixel perfection, for all practical purposes.

#### Why Strive for Pixel perfection?

* Brand identity preservation
* Avoid compounding layout errors
* Reliable and reusable modularity
* Improved UX
* Improved design quality and aesthetics

## Suggested reading

Achieving pixel perfection starts with planning. Designers must deliver mockups that support mutually agreed-upon design systems.

* [Handoffs Guide for Pixel Perfect Design. Part II](https://medium.com/pixelpoint/handoffs-guide-for-pixel-perfect-design-part-ii-d91999742dd9)
* [Handoffs Guide for Pixel Perfect Design. Part III](https://medium.com/pixelpoint/handoffs-guide-for-pixel-perfect-design-part-iii-3acc5a93d3a2)
* [How to achieve Pixel Perfect front end practically?
](https://blog.prototypr.io/how-to-achieve-pixel-perfect-front-end-practically-bd990390588)

Generally speaking, brands that want pixel perfection should consider grid-based layouts with established breakpoints. Designers should be well versed in these design systems.

## Support designers

Designers may benefit from tools to achieve the goals of the project and navigate constraints.

* Consider offering tools or guides that help designers to adhere to design systems (grid bookmarklets, living style guides, change logs, notifications, [audit tools](https://cssstats.com), etc.).  
* Work to consolidate typography and colors styles to a condensed standard set.
  * For colors, consider using [color functions](https://css-tricks.com/the-power-of-rgba/) to produce many variations from the standard set.
  * For typography, investigate if the standard set will work responsively. Consider using a web font API to normalize type across appropriate devices.
* Assist designers in matching grid systems between the browser and their design tools.

## Precision UXD

Hand measurements and distance-based rules are unreliable and can compound errors. 

### Pixel perfection tools

* [GluePrint](http://glueprintapp.com/) (Free)
* [xScope](https://xscopeapp.com/) ($49.99)
* [PixelSnap](https://getpixelsnap.com/) ($15)

## Caveats

### Mockups are not always correct.

At times, designs may have errors or deviate slightly from existing conventions, grids and standards. Use your best judgement to determine when it makes sense to adhere to, or deviate from, mockups. When it doubt, seek design direction (cc'ing project managers) and assess the risks at scale. 

### Consider unintended consequences

There may be issues that result from standardizing distances between baselines versus distance between modules or character maps. For instance, `line-height` may not be static across locales. Or two different modules may introduce different vertical flows. Do your best to consider these unintended consequences.

### Be flexible and weigh risks

It may make UX sense for some elements to intentionally deviate from established grids, when the risk to scale is low. At the same time, designers should be cognizant that the established design system is what permits the site to easily scale.

## Common pitfalls

#### Over-engineering

There are times you may be tempted to over-engineer a solution in order to achieve pixel perfection. However, this is in and of itself a risk. Err on the side of caution. Achieving pixel perfection is not always worth the risks if it undermines the existing libraries or design systems that allow a site to scale. Keep your solutions simple.


#### Pixel-nudging

If you find yourself using pixel-nudging techniques, such as `top: -1px` or negative margins, to match to designs, you should stop and re-evaluate your approach. Such techniques may not scale well, and they suggest that either the mockups may contain minor errors or the design systems are flawed. When established libraries and design systems are consistent with with mockups, there is typically no need for pixel-nudging.
