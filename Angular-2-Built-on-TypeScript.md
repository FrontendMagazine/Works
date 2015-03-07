# Angular 2: Built on TypeScript

# Angular 2：基于TypeScript

We're excited to unveil the result of a months-long partnership with the Angular team.

非常兴奋的为大家揭开我们与 Angular 团队合作数月之久的成果！

This partnership has been very productive and rewarding experience for us, and as part of this collaboration, we're happy to announce that Angular 2 will now be built with TypeScript.  We're looking forward to seeing what people will be able to do with these new tools and continuing to work with the Angular team to improve the experience for Angular developers.

这次合作不但硕果累累，还获馈赠给我们了非常多的经验。作为合作的一部分，我们非常愉快地宣布 Angular2 将基于 TypeScript 来开发。我们期待基于新工具或产生怎样的火花，并且将会持续地与 Angular 团队协作，提升 Angular 开发者的开发体验。

The first fruits of this collaboration will be in the upcoming TypeScript 1.5 release.

合作的第一个成果将在出现在即将释出的 TypeScript 1.5 中。

We have worked with the Angular team to design a set of new features that will help you develop cleaner code when working with dynamic libraries like Angular 2, including a new way to annotate class declarations with metadata.  Library and application developers can use these metadata annotations to cleanly separate code from information about the code, such as configuration information or conditional compilation checks.

我们与 Angular 团队一起设计出了一系列的新的特性，这些特性有助于你在使用像 Angular 2 这样的动态类库时，保持代码清晰。还包含一种新方式，使用元数据来注解类的申明。类库和应用的开发者可以使用这些元数据注释来把代码信息和代码清晰地分开，比如配置信息或者条件检查等等。

We've also added a way to retrieve type information at runtime.  When enabled, this will enable developers to do a simple type introspection.  To verify code correctness with additional runtime checks.  It also enables libraries like Angular to use type information to set up dependency injection based on the types themselves.

我们还添加了一种在运行时中获取类型信息的方式。开启时，开发者可以非常方便地做类型检测。利用额外的运行时检查验证代码的正确性。它还允许像 Angular 中这样的类库使用类型信息来设置依赖注入。

## TodoMVC for Angular 2 in TypeScript

At ng-conf, we are previewing this work by showing a TodoMVC example, based on [David East’s Angular 2 TodoMVC](https://github.com/davideast/ng2do). You can try this example out for yourself. If you’re new to TypeScript, you can also learn TypeScript through our [interactive playground](http://www.typescriptlang.org/Playground).

在 ng-conf 中，我们已经通过一个 TodoMVC 例子预览过这些工作了，这个例子基于 [David East 的 Angular 2 TodoMVC](https://github.com/davideast/ng2do)。即可以自己试试这个例子。如果你原来不熟悉 TypeScript，你可以通过我们交互式的[练操场]((http://www.typescriptlang.org/Playground))学习 TypeScript。

We’d love to hear your feedback.

期待你的反馈！

![TypeScript autocomplete in Sublime 3 for Angular 2](http://blogs.msdn.com/resized-image.ashx/__size/550x0/__key/communityserver-blogs-components-weblogfiles/00-00-01-56-67/0820.Sublime_5F00_Intellisense.png)

*TypeScript autocomplete in Sublime 3 for Angular 2*

*Sublime 3 中，TypeScript 针对 Angular 2 的自动补全效果*

We’re looking forward to releasing a beta of TypeScript 1.5 in the coming weeks, and along with it, growing TypeScript’s tooling support to include more development styles and environments.  We'd also like to give a huge thanks to Brad, Igor, Miško on the Angular team for being great partners.  Special shout out to Yehuda Katz, who helped us design the annotation+decorator proposal which helped make this work possible. 

期待就在几周内发布 TypeScript 1.5 的 beta 版，随之而来的，还有更多的 TypeScript 工具支持，包含更多的开发模式和环境。非常感谢来自 Angular 团队的 Brad、Igor 和 Miško，非常棒的合作伙伴。特别感谢 Yehuda Katz，它协助我们设计了注解装饰范式，让这些特性成为可能。
