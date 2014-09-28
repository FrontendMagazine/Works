# Built-in Browser Support for Responsive Images
# 浏览器内建支持的响应式图像
## Introducing the picture element
## picture元素介绍
The picture element offers a declarative approach towards image resource loading. Web developers will no longer need CSS or JavaScript hacks to handle images in responsive designs. And users benefit from natively-optimized image resource loading—especially important for users on slower mobile internet connections.

对于图像资源加载，picture元素提供了一种声明的方式。在响应式设计中，Web开发者将不再需要CSS或者JavaScript写hack去控制图片。用户也将受益于这种原生优化过的图片资源加载方式——对那些速度更慢的移动网络连接用户来说更为重要。

Alongside the newer srcset and sizes attributes recently added to img, thepicture element gives web developers more flexibility in specifying image resources. Write clear HTML markup and let the browser do the work of detecting any of the following scenarios, alone or in combination, to support responsive designs and improve web page load times:

除了img元素新增的srcset和sizes属性，picture元素也在指定一个图片资源时为web开发者提供了便利。仅仅写下HTML标记，让浏览器去识别下列任意一种情况，单个的或者成组，去支持响应式设计，并且提升web页面的加载时间：

#### Art direction-based selection
Is this a mobile device held in a portrait orientation or a wide desktop monitor? Load an image that is optimized for the given screen dimensions.

#### 业内基于方向的选择
是一个纵向手持的移动设备还是一个宽大的桌面显示器？
[加载一张为指定显示器尺寸适配的图片](http://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-art-direction)

#### Device-pixel-ratio-based selection
Does the device have a high DPI display? Load a higher resolution image.

#### 基于像素比率设备的选择
是否设备有一个很高的DPI显示？
[加载一个更高分辨率的图像。](http://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-pixel-density-descriptors)

#### Viewport-based selection
Is the image meant to always fill a fixed proportion of the viewport? Load images relative to the viewport.

#### 基于视口的选择
是否图像总是要填充一个固定比例的视口？
[相对于视口去加载图像](http://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-width-descriptors)

Image format-based selection
Can the browser support additional image file types that offer performance boosts such as smaller file sizes? Load an alternative image file such as WebP.

#### 基于格式的图片选择
浏览器能否支持额外的文件类型，可以提供性能上的增长，像更小的文件大小？
[加载一个可替代的图像文件，像Webp](http://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-file-type)

## Use for art direction
The most common use of the picture element will be for "art direction" in responsive designs. Instead of having one image that is scaled up or down based on the viewport width, multiple images can be designed to more appropriately fill the browser viewport.
## 醉了。。。
在响应式设计中，picture元素最普遍的使用是“art direction”。不再是通过将一张图片基于视口宽度来放大或缩小，取而代之的是，设计了多幅图像来恰如其分地填充浏览器视口。
![响应式图片](http://img.china.alibaba.com/cms/upload/2014/672/160/2061276_975966031.png)

## Improve resource loading performance
When using picture, or img with the srcset and sizes attribute, the browser will only download the image explicitly stated for the matching scenario. This native implementation is compatible with HTML parsers and can take advantage of the browser's image caching and preloading abilities.

## 提升资源载入性能
当使用了带有srcset和sizes属性的picture或img标签时，浏览器只会去加载那些与场景明确匹配的图像。本地的接口会兼容HTML解释器，并且可以充分利用浏览器的图像缓存和预加载方面的能力。

## View a live demo
It's a fact that the Internet was created to host cat images. Using picture we can emulate the amazing ability of cats to adjust to the space given to them no matter how small or large.

## 看一个在线实例
网络连接被建立去承载了猫的图像。使用picture我们可以模拟出令人惊讶的表现，这只猫适应了给定的空间无论是多小或者大。
![喵星人](http://www.html5rocks.com/en/tutorials/responsive/picture-element/cat-stretching@2X.png)
Cat illustrations by @itsJonQ

Open the demo in a new tab with Chrome 38 or higher. Resize the viewport to see the cat in action.
As a starting point, this demo only shows off the bare minimum of features thatpicture has to offer. Let's dig into the syntax now.

使用Chrome 38+打开一个新标签页[打开这个demo](http://googlechrome.github.io/samples/picture-element/)。调整视口的大小然后去观察这只猫的行为。
作为一个起始点，这个实例仅仅能展示picture元素提供的最差的表现。现在我们钻研一下语法。

    <style>
      img {display: block; margin: 0 auto;}
    </style>

    <picture>
        <source 
          media="(min-width: 650px)"
          srcset="images/kitten-stretching.png">
        <source 
          media="(min-width: 465px)"
          srcset="images/kitten-sitting.png">
        <img 
          src="images/kitten-curled.png" 
        alt="a cute kitten">
    </picture>

Note how there is no JavaScript involved and no-third party libraries. The CSSstyle block is used only to style the image element and does not contain media queries. The native implementation of the picture element means that you can declare your responsive images using only HTML.

注意没有JavaScript，也没有第三方的类库。CSSstyle代码块仅仅被用于为image元素添加样式，并没有包含Media Queris。picture元素的本地实现让你可以仅使用HTML来声明响应式图像。

## Use with source elements
The picture element has no unique attributes of its own. The magic happens when picture is used as a container for source.
The source element, which is used for loading media such as video and audio, has been updated for image loading and these new attributes have been added:

## 使用source元素
picture元素没有它自己特有的属性。当picture被用于source的外层容器时，奇妙的事情就发生了。
source元素通常用来载入像video和audio这样的媒体资源，现在也可以用来载入图片，并且加入了下面的一些新属性：

#### srcset (required)
- Accepts a single image file path (e.g. srcset="kitten.png").
- Or a comma-delimited list of image file paths with pixel density descriptors (e.g. srcset="kitten.png, kitten@2X.png 2x") where a 1x descriptor is assumed when it is left off.
- Refer to Combine with pixel density descriptors for this in use.

#### srcset (required)
- 接受一个单独的图片文件路径（e.g. srcset="kitten.png"）。
- 或者一个使用逗号分隔的图像文件路径列表，并且带有像素密度描述符(e.g. srcset="kitten.png, kitten@2X.png 2x")。当它中断时这里的 1x描述符将会生效。
- 参考[结合像素密度描述符](http://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-pixel-density-descriptors)查看应用实例。

#### media (optional)
- Accepts any valid media query that you would normally find in a CSS @mediaselector (e.g. media="(max-width: 30em)").
- Refer to the previous picture syntax example for this in use.

#### media (optional)
- 接收任何你可能在CSS @media选择器里面出现的有效media query(e.g. media="(max-width: 30em)")。
- 参阅前面的[picture语法](#toc-syntax)应用实例。


#### sizes (optional)
- Accepts a single width descriptor (e.g. sizes="100vw") or a single media query with width descriptor (e.g. sizes="(max-width: 30em) 100vw").
- Or a comma-delimited list of media queries with a width descriptor (e.g.sizes="(max-width: 30em) 100vw, (max-width: 50em) 50vw, calc(33vw - 100px)") in which the last item in the list is used as the default.
- Refer to Combine with width descriptors for this in use.

#### sizes (optional)
- 接受一个单独的width 描述符(e.g. sizes="100vw") 或者一个带有width描述符的media query(e.g. sizes="(max-width: 30em) 100vw")。
- 或者一个带有width描述符使用逗号分隔的 media queries列表(e.g.sizes="(max-width: 30em) 100vw, (max-width: 50em) 50vw, calc(33vw - 100px)") ，列表中的最后一项作为默认值。
- 参阅[结合width描述符](http://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-width-descriptors)查看应用实例。

#### type (optional)
- Accepts a supported MIME type (e.g. type="image/webp" ortype="image/vnd.ms-photo").
- Refer to [Load alternative image file formats](http://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-file-type) for this in use.

#### type (optional)
- 支持的MIME类型(e.g. type="image/webp" ortype="image/vnd.ms-photo")。
- 参阅[加载可替代的图片文件格式](http://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-file-type)查看应用实例。

The browser will use the hints passed in as attribute values to load the most appropriate image resource. The listing order of tags matter! The browser will use the first source element with a matching hint and ignore any followingsource tags.

浏览器将会把传入的属性值当做提示去加载最合适的图片资源。和列表的顺序相关！浏览器将会使用第一个source元素作为一个匹配提示并且忽略后面的source标签。

## Add a final img element
The img element has also been updated to be used within picture as the fallback in case a browser does not support the picture element or if no source element tags are matched. Using img within picture is a requirement—if you forget it, no images will show up.

## 末尾添加一个img元素
新的img标签可以被用于picture标签内作为备用，当浏览器不支持picture元素或者没有匹配的source元素标记。picture里面包含一个img是必要的—如果你忘记了，图片将不会呈现。

Use img to declare the default image to be used within a picture block. Place img as the last child of picture since the browser will ignore anysource declarations that occur after an img tag is found. The image tag is also where you should attach alternative text using the image element's alt attribute.

使用img声明一个包含在picture代码块内的默认图片。把img放在picture的最后一个子节点的位置，因为当浏览器解析到img标签时会忽略掉所有的source声明。图片标签也可能是作为替代文本在图片元素附加在alt属性。

## Combine with pixel density descriptors 
Add support for high resolution displays using pixel density descriptors such as 1x, 1.5x, 2x, and 3x. The new srcset attribute applies to both img andsource elements.
The example below supports 1x, 1.5x, and 2x resolution screens:

## 结合像素密度描述符
为高分辨率的显示器添加了支持，这种显示器使用了像1x，1.5x，2x和3x的像素密度描述符。新的srcset属性被应用于img和source元素。
下面的例子是支持1x，1.5x和2x分辨率的屏幕：

    <picture>
        <source 
          media="(min-width: 650px)" 
          srcset="images/kitten-stretching.png,
                  images/kitten-stretching@1.5x.png 1.5x,  
                  images/kitten-stretching@2x.png 2x">
        <source 
          media="(min-width: 465px)" 
          srcset="images/kitten-sitting.png,
                  images/kitten-sitting@1.5x.png 1.5x
                  images/kitten-sitting@2x.png 2x">
        <img 
          src="images/kitten-curled.png" 
          srcset="images/kitten-curled@1.5x.png 1.5x,
                  images/kitten-curled@2x.png 2x"
          alt="a cute kitten">
    </picture>

## Combine with width descriptors
## 结合width描述符
Web Fundamentals covers the the new sizes attribute for the img element indepth:

Web基础更深层次地为img元素包含了全新的[sizes 属性](https://developers.google.com/web/fundamentals/media/images/images-in-markup#relative-sized-images)

"When the final size of the image isn't known, it can be difficult to specify a density descriptor for the image sources. This is especially true for images that span a proportional width of the browser and are fluid, depending on the size of the browser.
Instead of supplying fixed image sizes and densities, the size of each supplied image can be specified by adding a width descriptor along with the size of the image element, allowing the browser to automatically calculate the effective pixel density and choose the best image to download."

“当图片最终尺寸未知时，为图片资源指定密度描述符会变得很困难。尤其对于图片占据了浏览器特定比例的宽度并且是不固定，依赖于浏览器的尺寸的时候。
取代了提供固定图片尺寸和密度的方式，给定图片的尺寸可以通过添加一个带有图片元素尺寸的width描述符来指定，允许浏览器自动的计算有效的像素密度然后选择最佳的图片进行加载。”

Here's an example of using the sizes attribute to set the proportion of an image to always fill 80% of the viewport. It is combined with the srcset attribute to supply four versions of the same lighthouse photo in widths of 160px, 320px, 640px, and 1280px wide:

下面这个例子通过使用sizes属性使得图片的比例总能填满视口的80%。把它与srcset属性结合起来将会提供同一张灯塔图像的四种版本，包括了160px, 320px, 640px和 1280px 宽：

    <img src="lighthouse-160.jpg" alt="lighthouse"
     sizes="80vw"
     srcset="lighthouse-160.jpg 160w, 
             lighthouse-320.jpg 320w,        
             lighthouse-640.jpg 640w,
             lighthouse-1280.jpg 1280w">

The browser will use these hints to choose the most appropriate image resource to serve up based on the viewport width and hardware display resolution:

浏览器将会使用这些提示去选择最合适的图像资源去显示，依据了视口的宽度和硬件显示器分辨率：
![ligihthouse](http://www.html5rocks.com/en/tutorials/responsive/picture-element/lighthouse-example-img@2X.png)

For example, the viewport on the left is approx. 800px wide. The browser will load lighthouse-640.jpg unless the device pixel ratio is 2x—in which case, lighthouse-1280.jpg will be loaded instead.

比如，左边的视口宽度大约800px。浏览器将会加载lighthouse-640.jpg除非设备的像素比率是2x——此时，lighthouse-1280.jpg将会会下载。

With the addition of picture, the sizes attribute can be applied to both imgand source elements:

通过添加picture元素，sizes属性将被应用于img和source两个元素：

    <picture>
        <source 
          media="(min-width: 650px)" 
          srcset="images/kitten-stretching.png,
                  images/kitten-stretching@1.5x.png 1.5x,  
                  images/kitten-stretching@2x.png 2x">
        <source 
          media="(min-width: 465px)" 
          srcset="images/kitten-sitting.png,
                  images/kitten-sitting@1.5x.png 1.5x
                  images/kitten-sitting@2x.png 2x">
        <img 
          src="images/kitten-curled.png" 
          srcset="images/kitten-curled@1.5x.png 1.5x,
                  images/kitten-curled@2x.png 2x"
          alt="a cute kitten">
    </picture>

Building on the previous example, when the viewport is at 800px and above, the browser will serve up a landscape version of the lighthouse version instead:

建立在上一个例子上，当视口在800px或者超过800px，浏览器将会提供landscape版本代替lighthouse:
![landscape](http://www.html5rocks.com/en/tutorials/responsive/picture-element/lighthouse-example-picture@2X.png)

The viewport on the left is above 800px wide so a landscape version of the lighthouse photo will be displayed.

左侧的视口超过800px宽所以landscape版本的灯塔照片将被呈现。

## Load alternative image file formats
The type attribute of source can be used to load alternative image file formats that might not be supported in all browsers. For example, you can serve an image in WebP format to browsers that support it, while falling back to a JPEG on other browsers:

## 加载可供选择的图像文件格式
source元素的type属性可被用于去加载一个可选择的图像文件格式，这可能不会被所有的浏览器支持。比如，对于支持Webp的浏览器可以提供一张Webp格式图片，在其他浏览器上再回到JPEG:

    <picture>
        <source type="image/webp" srcset="images/butterfly.webp">
        <img src="images/butterfly.jpg" alt="a butterfly">
    </picture>

## Additional code examples
Refer to [Responsive Images: Use Cases and Documented Code Snippets to Get You Started](http://dev.opera.com/articles/responsive-images/) on the Dev.Opera blog for an exhaustive list of examples combiningpicture and img with the srcset, media, sizes, and type attributes.

## 额外的代码实例
参阅Opera开发博客中[响应式图片：从用例和文档化的代码片段开始](http://dev.opera.com/articles/responsive-images/) 有一个结合了picture 和 img 的srcset, media, sizes和 type 属性实例的详细列表。

## Try it out today
The picture element is currently available Chrome 38. Try it out with screen emulation in the Chrome DevTools.

If you have feedback on this new feature, drop us a line on the Chrome bug tracker.

If you're ready to start implementing picture today but also want to add support for responsive images in additional browsers, refer to [this picture element sample](https://google-developers.appspot.com/web/fundamentals/resources/samples/media/images/media) for using picture with a polyfill.

Be sure to also check out [the guide to images on Web Fundamentals](https://developers.google.com/web/fundamentals/media/images/) for best practice examples of implementing images on the web.

## 现在来试一试
picture元素在[Chrome38下有效](http://www.chromestatus.com/feature/5910974510923776)。可以通过Chrome开发工具中的屏幕模拟器来尝试。

如果如果对于这个新特性有什么反馈，请在[Chrome bug tracker](http://crbug.com/)给我们留下信息。

如果你现在就想去实现picture元素而且还想在其他的浏览器上为响应式图片添加支持，可以查阅[picture元素例子](https://google-developers.appspot.com/web/fundamentals/resources/samples/media/images/media)使用带有polyfill的picture元素。

也一定要看看[web基础中的图片指南](https://developers.google.com/web/fundamentals/media/images/)中关于web中实现图片的最佳实践的例子。









