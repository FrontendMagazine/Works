# Built-in Browser Support for Responsive Images
# 浏览器内建支持的响应式图像

## Introducing the picture element
## picture元素介绍

The picture element offers a declarative approach towards image resource loading. Web developers will no longer need CSS or JavaScript hacks to handle images in responsive designs. And users benefit from natively-optimized image resource loading—especially important for users on slower mobile internet connections.

对于图像资源加载，picture 元素提供了一种声明的方式。在响应式设计中，Web 开发者将不再需要 CSS 或者 JavaScript 写 hack 去控制图片。用户也将受益于这种原生优化过的图片资源加载方式——对那些速度更慢的移动网络连接用户来说更为重要。

Alongside the newer srcset and sizes attributes recently added to img, the picture element gives web developers more flexibility in specifying image resources. Write clear HTML markup and let the browser do the work of detecting any of the following scenarios, alone or in combination, to support responsive designs and improve web page load times:

除了 img 元素新增的 srcset 和 sizes 属性，picture 元素也在指定一个图片资源时为 web 开发者提供了更多的灵活性。仅仅写下HTML标记，让浏览器去识别下列任意一种情况，单个的或者组合，去支持响应式设计，并提升 web 页面的加载时间：

- **Art direction-based selection**: Is this a mobile device held in a portrait orientation or a wide desktop monitor? Load an image that is optimized for the given screen dimensions.

- **Device-pixel-ratio-based selection**: Does the device have a high DPI display? Load a higher resolution image.

- **Viewport-based selection**: Is the image meant to always fill a fixed proportion of the viewport? Load images relative to the viewport.

- **Image format-based selection**: Can the browser support additional image file types that offer performance boosts such as smaller file sizes? Load an alternative image file such as WebP.

- **基于布局做选择**：是一个纵向手持的移动设备还是一个宽大的桌面显示器？[加载一张为指定显示器尺寸适配的图片](http://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-art-direction)

- **基于像素比率设备的选择**：是否设备有一个很高的 DPI 显示？[加载一个更高分辨率的图像。](http://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-pixel-density-descriptors)

- **基于视窗的选择**：是否图像总是要填充一个固定比例的视窗？[相对于视窗去加载图像](http://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-width-descriptors)

- **基于格式的图片选择**：浏览器能否支持额外的文件类型，可以提供性能上的提升，比如使用更小的文件？[加载一个可替代的图像文件，比如使用 WebP 格式的图片](http://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-file-type)

### Use for art direction
The most common use of the picture element will be for "art direction" in responsive designs. Instead of having one image that is scaled up or down based on the viewport width, multiple images can be designed to more appropriately fill the browser viewport.

### 用于页面的布局
在响应式设计中，picture 元素最普遍的使用是“布局”。不再通过将一张图片基于视口宽度来放大或缩小，取而代之的是，设计多幅图像来恰如其分地填充浏览器视口。![响应式图片](http://img.china.alibaba.com/cms/upload/2014/672/160/2061276_975966031.png)

### Improve resource loading performance
When using picture, or img with the srcset and sizes attribute, the browser will only download the image explicitly stated for the matching scenario. This native implementation is compatible with HTML parsers and can take advantage of the browser's image caching and preloading abilities.

### 提升资源载入性能
当使用了带有 srcset 和 sizes 属性的 picture 或 img 标签时，浏览器只会去加载那些与场景明确匹配的图像。实现兼容 HTML 解释器，可以充分利用浏览器的图像缓存和预加载方面的能力。

## View a live demo
It's a fact that the Internet was created to host cat images. Using picture we can emulate the amazing ability of cats to adjust to the space given to them no matter how small or large.

## 看一个在线实例
事实是，互联网的创建就是为了服务猫的图像。使用 picture 我们可以模拟出令人惊讶的表现，这只猫适应了给定的空间，无论这个空间是大是小。![喵星人](http://www.html5rocks.com/en/tutorials/responsive/picture-element/cat-stretching@2X.png)
Cat illustrations by @itsJonQ

Open the demo in a new tab with Chrome 38 or higher. Resize the viewport to see the cat in action.
As a starting point, this demo only shows off the bare minimum of features thatpicture has to offer. Let's dig into the syntax now.

使用 Chrome 38+ 打开一个新标签页[打开这个 demo](http://googlechrome.github.io/samples/picture-element/)。调整视口的大小然后去观察这只猫的行为。
作为一个开始，这个实例仅仅能展示 picture 元素提供的最基本的功能。现在我们钻研一下语法。

### The picture syntax
The following HTML and CSS snippet is everything that was used to implement the demo:

### picture 的语法
下面的 HTML 和 CSS 代码段就是实现 demo 完整的代码：

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

Note how there is no JavaScript involved and no-third party libraries. The CSS style block is used only to style the image element and does not contain media queries. The native implementation of the picture element means that you can declare your responsive images using only HTML.

注意没有 JavaScript，也没有第三方的类库。CSS 样式块仅仅为 image 元素添加样式，并没有包含 Media Queris。picture 元素原生实现让你可以仅使用 HTML来声明响应式图像。

### Use with source elements
The picture element has no unique attributes of its own. The magic happens when picture is used as a container for source.
The source element, which is used for loading media such as video and audio, has been updated for image loading and these new attributes have been added:

### 使用 source 元素
picture 元素没有它自己特有的属性。当 picture 被用于 source 的外层容器时，奇妙的事情就发生了。
source 元素通常用来载入像 video 和 audio 这样的媒体资源，现在也可以用来载入图片，并且加入了下面的一些新属性：

#### srcset (required)
- Accepts a single image file path (e.g. srcset="kitten.png").
- Or a comma-delimited list of image file paths with pixel density descriptors (e.g. srcset="kitten.png, kitten@2X.png 2x") where a 1x descriptor is assumed when it is left off.
- Refer to Combine with pixel density descriptors for this in use.

#### srcset (optional)
- 接受一个图片文件路径（e.g. srcset="kitten.png"）。
- 或者一个使用逗号分隔的图像文件路径列表，并且带有像素密度描述符(e.g. srcset="kitten.png, kitten@2X.png 2x")。1x 描述符可以省略。
- 参考[结合像素密度描述符](http://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-pixel-density-descriptors)查看应用实例。

#### media (optional)
- Accepts any valid media query that you would normally find in a CSS @mediaselector (e.g. media="(max-width: 30em)").
- Refer to the previous picture syntax example for this in use.

#### media (optional)
- 接收任何你可能在 CSS @media 选择器里面出现的有效 media query(e.g. media="(max-width: 30em)")。
- 参阅前面的[picture 语法](#toc-syntax)应用实例。


#### sizes (optional)
- Accepts a single width descriptor (e.g. sizes="100vw") or a single media query with width descriptor (e.g. sizes="(max-width: 30em) 100vw").
- Or a comma-delimited list of media queries with a width descriptor (e.g.sizes="(max-width: 30em) 100vw, (max-width: 50em) 50vw, calc(33vw - 100px)") in which the last item in the list is used as the default.
- Refer to Combine with width descriptors for this in use.

#### sizes (optional)
- 接受一个单独的 width 描述符(e.g. sizes="100vw") 或者一个带有 width 描述符的media query(e.g. sizes="(max-width: 30em) 100vw")。
- 或者一个带有 width 描述符使用逗号分隔的 media queries 列表(e.g.sizes="(max-width: 30em) 100vw, (max-width: 50em) 50vw, calc(33vw - 100px)") ，列表中的最后一项作为默认值。
- 参阅[结合 widt h描述符](http://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-width-descriptors)查看应用实例。

#### type (optional)
- Accepts a supported MIME type (e.g. type="image/webp" ortype="image/vnd.ms-photo").
- Refer to [Load alternative image file formats](http://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-file-type) for this in use.

#### type (optional)
- 接受一个 MIME 支持的类型(e.g. type="image/webp" ortype="image/vnd.ms-photo")。
- 参阅[加载可替代的图片文件格式](http://www.html5rocks.com/en/tutorials/responsive/picture-element/#toc-file-type)查看应用实例。

The browser will use the hints passed in as attribute values to load the most appropriate image resource. The listing order of tags matter! The browser will use the first source element with a matching hint and ignore any followingsource tags.

浏览器将会提取传入的属性值作为依据去加载最合适的图片资源。这与列表的顺序还有关系！浏览器将会使用第一个 匹配的 source 元素，并且忽略后面的 source 标签。

## Add a final img element
The img element has also been updated to be used within picture as the fallback in case a browser does not support the picture element or if no source element tags are matched. Using img within picture is a requirement—if you forget it, no images will show up.

## 末尾添加一个 img 元素
当浏览器不支持 picture 元素或者没有匹配的 source 元素标记时，可以放一个升级了的 img 标签在 picture 标签内作为备用。picture里面包含一个 img 是**必要的**——如果你忘记了，图片将不会呈现。

Use img to declare the default image to be used within a picture block. Place img as the last child of picture since the browser will ignore anysource declarations that occur after an img tag is found. The image tag is also where you should attach alternative text using the image element's alt attribute.

使用 img 声明一个包含在 picture 代码块内的默认图片。把 img 放在 picture 的最后一个子节点的位置，因为当浏览器解析到 img 标签时会忽略掉所有的source 声明。使用图片元素的 alt 属性，图片标签也可以用来显示替代文本。

## Combine with pixel density descriptors 
Add support for high resolution displays using pixel density descriptors such as 1x, 1.5x, 2x, and 3x. The new srcset attribute applies to both img andsource elements.
The example below supports 1x, 1.5x, and 2x resolution screens:

## 结合像素密度描述符
为高分辨率的显示器添加了支持，这种显示器使用了像1x，1.5x，2x和3x的像素密度描述符。[新的 srcset](https://developers.google.com/web/fundamentals/media/images/images-in-markup#enhance-imgs-with-srcset-for-high-dpi-devices) 属性被应用于 img 和 source 元素。
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
## 结合 width 描述符
Web Fundamentals covers the the new sizes attribute for the img element indepth:

*Web Fundamentals* 更深层次地介绍了 img 元素全新的[sizes 属性](https://developers.google.com/web/fundamentals/media/images/images-in-markup#relative-sized-images)

"When the final size of the image isn't known, it can be difficult to specify a density descriptor for the image sources. This is especially true for images that span a proportional width of the browser and are fluid, depending on the size of the browser.
Instead of supplying fixed image sizes and densities, the size of each supplied image can be specified by adding a width descriptor along with the size of the image element, allowing the browser to automatically calculate the effective pixel density and choose the best image to download."

“当图片最终尺寸未知时，为图片资源指定密度描述符会变得很困难。尤其对于图片占据了浏览器特定比例的宽度并且是不固定，依赖于浏览器的尺寸的时候。
取代了提供固定图片尺寸和密度的方式，给定图片的尺寸可以通过添加一个带有图片元素尺寸的 width 描述符来指定，允许浏览器自动的计算有效的像素密度然后选择最佳的图片进行加载。”

Here's an example of using the sizes attribute to set the proportion of an image to always fill 80% of the viewport. It is combined with the srcset attribute to supply four versions of the same lighthouse photo in widths of 160px, 320px, 640px, and 1280px wide:

下面这个例子通过使用 sizes 属性使得图片的比例总能填满视口的 80%。把它与 srcset 属性结合起来将会提供同一张灯塔图像的四种版本，包括了160px, 320px, 640px和 1280px 宽：

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

比如，左边的视口宽度大约 800px。浏览器将会加载 lighthouse-640.jpg 除非设备的像素比率是 2x——此时，lighthouse-1280.jpg 将会被下载。

With the addition of picture, the sizes attribute can be applied to both imgand source elements:

通过添加 picture 元素，sizes 属性可以应用于 img 和 source 元素：

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

建立在上一个例子上，当视口在 800px 或者超过 800px，浏览器将会提供 landscape 版本代替 lighthouse:
![landscape](http://www.html5rocks.com/en/tutorials/responsive/picture-element/lighthouse-example-picture@2X.png)

The viewport on the left is above 800px wide so a landscape version of the lighthouse photo will be displayed.

左侧的视口超过 800px 宽所以 landscape 版本的灯塔照片将被呈现。

## Load alternative image file formats
The type attribute of source can be used to load alternative image file formats that might not be supported in all browsers. For example, you can serve an image in WebP format to browsers that support it, while falling back to a JPEG on other browsers:

## 加载可供选择的图像文件格式
source 元素的 type 属性可被用于去加载一个可选择的图像文件格式，这可能不会被所有的浏览器支持。比如，对于支持 WebP 的浏览器可以提供一张WebP 格式图片，在其他浏览器上再降级到 JPEG:

    <picture>
        <source type="image/webp" srcset="images/butterfly.webp">
        <img src="images/butterfly.jpg" alt="a butterfly">
    </picture>

## Additional code examples
Refer to [Responsive Images: Use Cases and Documented Code Snippets to Get You Started](http://dev.opera.com/articles/responsive-images/) on the Dev.Opera blog for an exhaustive list of examples combiningpicture and img with the srcset, media, sizes, and type attributes.

## 额外的代码实例
参阅Opera开发博客中[响应式图片：从用例和文档化的代码片段开始](http://dev.opera.com/articles/responsive-images/) 有一个结合了picture 和 img，使用 srcset、media、sizes和 type 属性实例的详细列表。

## Try it out today
The picture element is currently available Chrome 38. Try it out with screen emulation in the Chrome DevTools.

If you have feedback on this new feature, drop us a line on the Chrome bug tracker.

If you're ready to start implementing picture today but also want to add support for responsive images in additional browsers, refer to [this picture element sample](https://google-developers.appspot.com/web/fundamentals/resources/samples/media/images/media) for using picture with a polyfill.

Be sure to also check out [the guide to images on Web Fundamentals](https://developers.google.com/web/fundamentals/media/images/) for best practice examples of implementing images on the web.

## 现在就来试一试
picture 元素在[Chrome38 下有效](http://www.chromestatus.com/feature/5910974510923776)。可以通过 Chrome 开发工具中的屏幕模拟器来尝试。

如果如果对于这个新特性有什么反馈，请在[ Chrome bug tracker](http://crbug.com/)给我们留下信息。

如果你现在就想去实现 picture 元素而且还想在其他的浏览器上为响应式图片添加支持，可以查阅[ picture 元素例子](https://google-developers.appspot.com/web/fundamentals/resources/samples/media/images/media)使用带有 polyfill 的picture元素。

也一定要看看[ Web Fundamentals 中的图片指南](https://developers.google.com/web/fundamentals/media/images/)中关于 web 中实现图片的最佳实践的例子。

原文：[Built-in Browser Support for Responsive Images](http://www.html5rocks.com/en/tutorials/responsive/picture-element/)








