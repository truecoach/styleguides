# Pixel Perfection

Pixel perfection is regarded by some to be controversial ([meaningless](https://www.designernews.co/comments/222544) or [madness](http://blog.kylegawley.com/pixel-perfection-is-madness/) as described by some) in the sense that designs [are not rendered the same](http://dowebsitesneedtolookexactlythesameineverybrowser.com/) in all browsers or devices. Nevertheless, with the right planning and process, it is possible to approach pixel perfection, for all practical purposes.

#### Why Strive for Pixel Perfection?

* Brand identity preservation
* Avoid compounding layout errors
* Reliable and reusable modularity
* Improved UX
* Improved Design Quality and aesthetics.

## Suggested Reading

Achieving pixel perfection starts with planning. Designers must deliver mockups that support mutually agreed-upon design systems.

* [Handoffs Guide for Pixel Perfect Design. Part II](https://medium.com/pixelpoint/handoffs-guide-for-pixel-perfect-design-part-ii-d91999742dd9)
* [Handoffs Guide for Pixel Perfect Design. Part III](https://medium.com/pixelpoint/handoffs-guide-for-pixel-perfect-design-part-iii-3acc5a93d3a2)

## Support Designers

Designers may benefit from tools to achieve the goals of the project and navigate constraints.

* Consider offering tools or guides that help designers to adhere to design systems (grid bookmarklets, living style guides, change logs, notifications, [audit tools](https://cssstats.com), etc.).  
* Work to consolidate typography and colors styles to a smaller standard set.
 * For colors, consider using [color functions](https://css-tricks.com/the-power-of-rgba/) to produce variations from the standard set.
 * For typography, investigate if the standard set will work responsively.
* Assist designers in matching grid systems between the browser and their design tools.

## Precision UXD

Hand measurements and distance-based rules are unreliable and can compound errors. 

### Pixel Perfection Tools

  * [GluePrint](http://glueprintapp.com/) (Free)
  * [xScope](https://xscopeapp.com/) ($49.99)
  * [PixelSnap](https://getpixelsnap.com/) ($15)

## Caveats

### Mockups are not always correct.

At times, designs may have errors or deviate slightly from existing conventions, grids and standards. Use your best judgement to determine when it makes sense to adhere to, or deviate from, mockups. When it doubt, seek design direction (cc'ing project managers) and assess the risks at scale. 

### Consider unintended consequences

There may be issues that result in standardizing distances between baselines versus distance between modules or character maps. For instance, `line-height` may not be static across locales. Or two different modules may introduce different vertical flows. Do your best to consider these unintended consequences.

### Be flexible and weight risks

It may make UX sense for some elements to intentionally deviate from established grids when the risk to scale is low. At the same time, designers should be cognizant that the established design system is what permits the site to easily scale.

## Avoid Over-engineering

There are times you may be tempted to over-engineer a solution in order to achieve pixel perfection. However, this is in and of itself a risk. Err on the side of caution. Achieving pixel perfection is not always worth the risks if it undermines the existing libraries or systems that allow a site to scale. Keep your solutions simple.
