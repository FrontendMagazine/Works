原文地址：[Twitter’s Heart Animation in Full CSS](https://medium.com/@OxyDesign/twitter-s-heart-animation-in-full-css-b1c00ca5b774#.pndd8brke)

# Twitter’s Heart Animation in Full CSS
# CSS 实现Twitter 的“爱心动画”

A few weeks ago, as everybody, I saw the Twitter Star turned into a Heart. The favorite into a like.

几周前，我发现 twitter 的“喜欢”不再使用星星的图标，而是变成一颗爱心。将“最爱”变成了”喜欢“。

twitter 官方也[发推](https://blog.twitter.com/2015/hearts-on-twitter)说了这事儿

> You can say a lot with a heart. Introducing a new way to show how you feel on Twitter: https://blog.twitter.com/2015/hearts-on-twitter ... pic.twitter.com/G4ZGe0rDTP
  爱心，一种新的表达你对一条 twitter 的方式：


That was a huge source for debates for sure … but the only thing that I had in mind was … is it possible to make it with only CSS (not a single picture or SVG) ?

这一改动肯定是经过了各方拉锯，不过我关心的只是，爱心动画可不可以用 CSS 实现呢？我指的是“纯 css”，不是一张图片或者 SVG。

I know it’s not a matter of life and death but when something like that gets in my head it’s too late, I can’t sleep until I have a viable answer.

我知道这并不是一个事关生死的问题，但是既然我有了这个想法，就一定要实现出来，不然根本睡不着觉啊。

After a few trials on this challenge I finally have my answer. The result is not perfect (and it’s a lot of SCSS / CSS — almost 400 lines) but it’s satisfying (based on my expectations at least).