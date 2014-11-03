# Best Practices for Chinese Layout——10 Principles to let long-form articles more readable

If you are going to expand your service into Asia. First, you may see the language wall (and then the Great Fire Wall). CJK have different layout rules. Requirements for Japanese Text Layout is a good reference, but it might be too long to read it over. Requirements for Hangul Text Layout and Typography is ok. And I'm still writing the draft of Requirement for Chinese Text Layout.

Before I get the document done, there are 10 principles for you to do Chinese layout well.

## 1, Punctuations differ.

For some reasons, Tradition Chinese readers in Taiwan and HongKong do not like to read Simplified Chinese at all, on the other hand, Mainland China readers are hard to read Traditional characters.

As a designer, there's lots of Chinese fonts, how can you tell them Traditional or Simplified?

It's easy. Type a full-width comma and period. If the punctuations in the middle, that's Traditional; and in the left-down corner as Japanese, that's Simplified.

And remember, all punctuations should be full-width.

## 2, Use the right fonts
Once you can tell a font Traditional or Simplified. It's time for you to assign right fonts in CSS or App l10n. Sometimes I see people only do Japanese l10n, such as, use OS X/ iOS Hiragino Mincho as body text. That's really troublesome:

- Punctuations not for Traditional Chinese.
- Adobe Japan 1-6 do not contain some characters in Traditional, and lots of them in Simplified. Fallback will let your article looks like a black mail.
- 漢字(Hanzi/Kanji) is used in Japan, China, Taiwan/HongKong and Vietnam. They have differences in between.

Glyphs differ between Tradition and Simplified Chinese.

I'll list system fonts for OS X / iOS / Windows and Android for you to choose the right font. Some of them not available for old version systems, and in iOS App, you have to do extra works to download them from Apple.

Chinese default fonts for latest OSs.
For Android, Droid Sans Fallback is pain for CJK. If you want to get typography better, Noto Sans will be good solution, unfortunately the CJK fonts are so huge that you have to subset them for decrease loading time. You can also try dynamic subsetting webfont service like iFontCloud. Or just forget Android *sigh*.

## 3, Proper line-height.

Not only line-height, but font size also matter. But hardly I can tell you the magic number for font size. In movable type age, proper font size for Chinese body text is 10.5pt（please calculate to actual size, not use this in CSS）, and not smaller than 9pt. Depends on your design style. But remember, people do not like to read small characters, especially on screen.

But line-height has the magic number. Usually between 1.5 to 2.0em. you can just type:

    p { line-height: 1.7em; }

and it's done.

## 4, Justify is elixir.

Here is a old Chinese movable type(by wood!), and shows a important Chinese Layout principle: all components are square characters.

But in 20th century, punctuations are used in publishing. And modern desktop publishing tools applied “prohibition rules(禁則處理)” from Japan to avoid period, comma… cannot appear in start of the line. And English words are written together. So that we cannot aligh everything horizontally and vertically. But at least, Start and End of the line should to aligned well. That’s why justify is important for eBook and long-form articles.

In CSS, you can try:

    p { text-align: justify; text-justify: ideographic;

it will make Chinese layout more beautiful immediately.

## 5, No italic/oblique.

In China, ink brushes were used for writing more than one thousand years. It's "Cursive style" is not shown as latin text in italic style, but cursive. There's several types like Kai, Xing (semi-cursive), cao(cursive).

Cursive script in Chinese

Not till the digital age, you never see Chinese characters in italic/oblique style. And it should not exist. Some times, default style for certain tags like <em>, <blockquote> may use default italic style for Chinese characters, please correct it by CSS:

    em { font-style: normal; }

<em> can be bold, sans-serif, but not italic/oblique.

## 6, Paragraph separation.

Paragraph separation is very important. Two ways to go:

### Bookish

In Chinese print books, there’s always no blank between paragraphs, and two characters indent is nessesary. That is:

    p { margin: 0; text-indent: 2em;

You can also use Japanese way, two U+3000 [ideographic space] to replace text-indent. Especially when some tools like Safari’s reader that may override your style, this way will keep the indent.

This style is suitable for eBooks.

### WEBish

But when people skim on web, bookish layout may let them stressful. You can just add margin-after(or margin-bottom) to separate paragraphs. That is:

    p { margin-after: 0.5em;

Remember, don’t leave too much blank in between. More than 0.5em, less than 1em is nice.

### 7, Kai is more bookish.

For body text, serif fonts like Ming (明體)/Sung(宋體) are generally used. But sans-serif fonts are so modern, less in the print book world.

Kai is used for citation in a Chinese poem textbook
Usually for blockquote, quotation, citation, dialogue, monologue…we used "Kai(楷書)" . Most desktop operation system contains default font for Kai, so use it if you like your content more bookish to readers.

Otherwise, sans-serif is also acceptable.

### 8, Break-all v.s. prohibition rules.

Justification is the digital method to solve Chinese layout principle, but sometimes not perfect. An obvious problem is easy to reproduce:

Justify with long latin word with Chinese

- short column like magazine layout, and also on your phone;
- there is a long latin word(or a lot of them) inline with Chinese characters;
- justify.

You can see the left picture, letter-spacing is expanded by justification, even more than 1em! It happens in layout software also on web browsers.

For a quick solution, just add :

    p { word-break: break-all; }

could help a lot. But consequently, break-all will also break prohibition rule, let comma, period go to the start of lines. But that's fine for Traditional Chinese, but not for the Simplified.

With Word-break: break-all. It's better but break some rules.
Why? Because punctuations in Simplified Chinese is on the left-bottom as in Japanese. When they appear in the line start, just weird.

## 9, Careful about letter-spacing.

Usually for Chinese body text, you can just do nothing about letter-spacing. There are some sites in HongKong, they will use letter-spacing but that's not good practice.

> No Kerning, no letter-spacing for body text but heading.

## 10, Simplified to Traditional not ok, but Traditional to Simplified works.

Here's a table for you to know the frequently used words in Traditional and Simplified Chinese. There's some problem that characters may have different code point from one to another. But most tools that let you convert just work with table to match.

But there’s 267 characters in Traditional Chinese will be altered from SC→TC. Here’s the example:

    TC→SC：皇后、後世→皇后、后世
    SC→TC：皇后、后世→皇后、后世
    TC→SC：呂布→呂布
    SC→TC：呂布→呂佈

So if you want to do the simple convert, you must use a dictionary-based converter for that. Or you have to do proof reading.

That's all, Not quite hard to complete it, just do.