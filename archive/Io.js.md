# Io.js

# Io.js

A lot of people have asked me lately about io.js and its place among Node Forward, the Advisory Board, and npm.

有很多人向我打听 io.js，问我它和 Node Forward、Advisory Board（Node 委员会）和 npm 是什么关系。

This is my own personal point of view. However, I have shared early drafts of this post with the io.js technical committee to ensure that it’s at least a reasonable approximation of how the team sees things.

下面是我个人的看法。一个较早的版本我已经在 io.js 委员会里发过了，目的是让大家能够大体上达成一致的意见。

## What is io.js?

## 什么是 io.js？

Io.js is a collaborative fork of joyent/node. Io.js is created by Fedor Indutny, a long-time active node core team member responsible for some of the most important parts of the Node.js runtime.

[Io.js](https://github.com/iojs/io.js) 是 Fedor Indutny 创建的[ joyent/node](https://github.com/joyent/node) 的 fork 版。后者是一位长期活跃的node 核心团队的成员，负责 Node.js 运行时中一些重要部分的开发。

Io.js continues the work that was previously being done by Node Forward in the node-forward/node repository. We hope to merge with the original Node.js project at some point in the future.

Io.js 继续完成 Node Forward 之前在 node-forward/node 中的工作。我们希望在将来的某个时间点可以合并回原始的 Node.js 项目中。

## What is Node Forward?

## Node Forward 是什么？

Node Forward is an effort by Node.js core contributors, community members, and corporate interests, united in the desire to improve the Node.js project.

Node Forward 是 Node.js 核心贡献者、社区成员以及利益企业之间的一个尝试，大家联合在一起努力提升 Node.js 这个项目。

On July 11, Mikeal Rogers created a private node-forward repository under his personal GitHub account to discuss the future direction of Node.js. Many of us felt that it was time to pursue a consensus-seeking contributor-governed process for the project, in a neutral foundation.

今年7月11日，Mikeal Rogers 在自己的 Github 账号下创建了一个 node-forward 非公开的项目，用于讨论 Node.js 未来发展的方向。我们中的很多人意识到是时候给这个项目寻求一种大家能够接受的贡献者自治的流程，在一个中立基础之上。

## What is “node-forward/node”?

## node-forward/node？

After a month in Mikeal’s private node-forward repository, the discussion was moved to the Node Forward organization in August, where it could be continued in public. The Node Forward organization included a (GitHub fork-button-style) “fork” of Node.js at node-forward/node, created with the express intent of merging changes with the joyent/node repository.

一个月后，也就是8月份，我们的讨论从非公开的 node-forward 项目，迁移到了 [Node Forward ](https://github.com/node-forward)这个组织，这样可以把讨论公开化。Node Forward 这个组织包含了一个 Node.js fork 的项目 [node-forward/node](https://github.com/node-forward)。创建的目的就是希望可以和  joyent/node 项目之间互相 merge 更新。

At that time, Mikeal wrote:

彼时，Mikeal 如下写到：

> The first goal of the foundation is to house core development at a neutral organization that can support Node. Nobody prefers that work to be released as a fork and we will continue to work with Joyent to make them a member and even a leader of this foundation. Joyent may decide that the best thing for Node is to continue their own work in parallel to the work other contributors are taking under the foundation in a symbiotic manner that has propelled projects like Linux and BSD. In that case the contributors in the TC are committed to releasing as a “fork” although they do not find it preferable.


> 最根本的目的就是把核心开发放到这里，在一个可以支持 Node 的中立组织下。没有人愿意把成就作为一个 fork 的分支发布出来，我们还将继续与 Joyent 一同工作，把他们作为一个成员，甚至是作为这个组织的带头人。Joyent 可能会觉得对 Node 最好的做法就是他们自己的工作与组织下的其他贡献者的工作并行，以一种共生的方式继续推进项目，就像 Linux 和 BSD 的关系类似。这样的话， TC（技术委员会）的贡献者被迫承诺以 “fork” 的方式发布，不过这会让他们感觉不爽。

In the Node Forward repository, the core contributors involved in this effort formed a Technical Committee (TC), and came to consensus on governance process and broad technical direction. The TC consists of 6 of the top 8 Node.js core contributors. (Ryan Dahl has no current involvement with Node.js. TJ Fontaine was invited but declined to participate.)

在 Node Forward 项目中，核心贡献者被卷进来努力形成一个技术委员会（TC），并就管理流程和长远的技术方向达成一致。TC 由 [8 位 Node.js 的顶级贡献者](https://github.com/joyent/node/graphs/contributors)中的 6 名组成。（Ryan Dahl 已经不参与 Node.js 相关的事情了，而 TJ Fontaine 拒绝了我们的邀请。）

## Does Node Forward compete with Joyent or Node.js?

## Node Forward 与 Joyent 或者 Node.js 是竞争关系么？

No.

不是。

Joyent is a corporation that provides container infrastructure as a service solutions, including the Joyent Container Service, the Manta data storage and analytics platform, and the SmartDataCenter orchestration system. In 2010, Joyent purchased the Node.js copyright and trademark from Node’s original author, Ryan Dahl.

Joyent 是一个把容器基础设置作为服务解决方案提供的公司。包括 Joyent Container Service、Manta 数据存储以及分析平台和 SmartDataCenter 编排系统。2010年，Joyent 从 Node 的原作者，即 Ryan Dahl 手中购买了 Node.js 的版权和商标。

Node Forward is a group of community members and core committers who want to continue to work on improving Node.js as effectively as possible, using open self-governance in a neutral community foundation.

Node Forward 是一个由社区成员和核心贡献者组成的组织。目的是在一个中立的社区基础上，通过开放的自治，不断地尽可能地提高 Node.js 的性能。

The goal of Node Forward is to work with Joyent and the rest of the Node.js community in order to improve Node.js. We respect Joyent’s significant investment in Node.js over the years, and we believe that a combined effort is beneficial to Joyent and to Node.

Node Forward 的目的是与 Joyent 和其他 Node.js 社区一起联手提升 Node.js。感谢 Joyent 数年来对 Node.js 的大力资助，我们相信双方努力的联合，对 Joyent 和 Node 都大有好处。

## What is the Joyent Node Advisory Board?

## Joyent Node 委员会是什么？

On August 13, Joyent’s CEO Scott Hammond called to speak with me about the direction of Node.js. He indicated that he was also speaking to many other Node business, technical, and community leaders.

8月13日，Joyent 的 CEO Scott Hammond 打电话给我，讨论 Node.js 方向的问题。他暗示我说他已经和其他很多 Node 商业的、技术的、社区的头们聊过了。

On September 26, he held a meeting to discuss the creation of an advisory board. He said he wanted to address the issues surrounding the Node project, and the board would be a way to get that process started.

9月26号，他组织了一个会议，讨论成立一个咨询委员会。他说他希望可以解决 Node 这个项目产生的问题，而委员会就是一个良好的开端。

The first official meeting of the Advisory Board occurred on October 23.

第一个正式的委员会会议于10月23号召开。

More information about the Joyent Node Advisory Board can be found on Joyent’s website.

更多关于 Joyent Node 委员会的信息可以在[ Joyent 的网站](http://nodejs.org/about/advisory-board/)上找到。

## Why was node-forward/node made private?

## 为什么 node-forward/node 是非公开的？

On October 9th (that is, after the initial bootstrapping meeting and before the first official Advisory Board meeting), Scott Hammond called Mikeal Rogers and told him that the node-forward/node repository was a violation of Joyent’s trademark on Node.js.

10月9号，即动员会之后，正式的 Advisory Board 会议举行之前，Scott Hammond 打电话给 Mikeal Rogers，告知 node-forward/node 项目对 Joyent 的 Node.js 的商标造成了侵权。

Hammond expressed that he saw this as a sign of bad faith, undermining his efforts in the creation of an advisory board. We agreed to make the repository private in order to show our commitment to working with Joyent on improving Node.js.

Hammond 表示，他把这看做是不守信用的一个迹象，破坏了他对创建委员会的努力。我同意把这个项目变成非公开的，表明我们与 Joyent 合作提升 Node.js 的承诺。

I brought up the issue in the third Joyent Node Advisory Board meeting on November 20. Scott Hammond reiterated that it is a violation of Joyent’s trademark to release code “based on Joyent Node.js which is also called node”, and that he intended to ensure that their trademark was protected “by any and all legal means”. He requested that we choose a name other than “node” if we make this project public.

11月20日，我在第三届 Joyent Node Advisory Board 会议上提出了这个问题。Scott Hammond 重申，发布“基于 Joyent Node.js 同样叫做 node 的”代码是对 Joyent 商标的侵权，而且他打算通过“任何合法的手段”保护其商标。要求我们取另外一个不是“node”的名字，如果我们要把这个项目公开的话。

By that time, there was also clear indication of forward progress at the JNAB meetings. We had taken steps towards a consensus-based open governance model not dictated by any one corporation. There had been several discussions of relaxing use of the “Node.js” trademark by businesses providing products related to Node. There was a clearer understanding of the role that the community might play in future Advisory Board meetings.

那个时候，在 JNAB 会议上取得了一些显著的成果。我们已经迈向一种共同的开放的管理模型，而不是任由任何一个公司发号施令。对提供 Node 相关的商业服务行为放宽 “Node.js” 商标的使用已经有过数次讨论。对社区在 Advisory Board 会议未来扮演的角色也有的清晰的认识。

I am optimistic that the JNAB meetings will continue to be productive.

我很有信心，JNAB 会议会继续富有成效的。

## Why was io.js created?

## 为什么创建 [io.js](https://github.com/iojs/io.js)？

On November 26, Fedor Indutny, an extremely prolific Node.js core contributor, and an active participant in Node Forward, decided that he would create a fork of Node.js with a different name, so that the technical work started in Node Forward could continue in public without violating Joyent’s trademark.

11月26号，Fedor Indutny，一个高产的 Node.js 核心贡献者，同时也是一个积极的 Node Forward 参与者，提议说他可以创建一个 Node.js 的 fork 版，使用另外一个名字，以此继续公开地进行 Node Forward 开发工作，而不对 Joyent 的商标造成侵权。

The Technical Committee that had previously been working on node-forward/node decided to move to the io.js repository. The other non-technical discussion repositories remain on the Node Forward organization at this time.

之前一直在 node-forward/node 项目上开发的技术委员会决定迁移到 [io.js](https://github.com/iojs/io.js) 项目上。同时其他非技术的讨论项目还留在 [Node Forward](https://github.com/node-forward) 这个组织里。

## Does io.js compete with Joyent or Node.js?

## io.js 与 Joyent 或者 Node.js 是竞争关系么？

No.

不是。

The intent of io.js is to provide a space for the Node core team to continue to do the work of improving Node.

io.js 的目的，是为了给 Node 核心团队一个地方，继续开发，提升 Node。

Io.js continues the efforts of Node Forward. We are committed to making forward progress and serving the Node.js community, both in technical and non-technical issues.

Io.js 延续了 Node Forward 做出的努力。我们承诺继续推进，继续为 Node.js 社区服务，技术和非技术方面都有。

The work being done in the Joyent Node Advisory Board appears to be heading towards shared goals for the good of the Node community. I expect that this will continue.

Joyent Node Advisory Board 已经完成的事情正朝着分享对 Node 社区有意义的目标推进。我希望这可以继续下去。

The Node community as whole is endeavoring to make a change.  The transformation is in progress and we expect to come out better for it.  io.js is an ingredient in the process of change, a spark. The future is large, and we continue exploring it.

整个 Node 社区正在努力转变。迁移正在进行，我们希望能够有更好的结果。io.js 是转变过程中不可或缺的一部分，一个星星之火。前途一片光明，我们继续探索前行。

## What are the goals of io.js?

## 什么是 io.js 的目标？

In no particular order:

与顺序无关：

- Continuous integration
- 100% passing tests as a normal state of affairs
- Strict SemVer-compliant versioning
- Contributor ownership, outside of corporate control
- Transparent consensus-seeking governance
- Weekly releases
- Supported versions of V8
- Active development
- Predictable roadmap
- Community engagement
- Assuming the Joyent Node Advisory Board continues to make progress towards these goals, we expect to merge the projects in the future.


- 持续集成
- 正常情况下必须100%测试通过
- 严格兼容 SemVer 标准的版本号管理
- 贡献者自治，不受任何公司控制
- 透明的民主制度
- 每周发布
- 支持 V8 更新
- 活跃地开发
- 可见的 roadmap
- 集体约定
- 假定 Joyent Node Advisory Board 也是继续朝着这些目标推进，希望将来项目和合并。


## What is npm’s position on Node forks?

## 在 Node 社区中 npm 是什么？

npm is the package manager for JavaScript. We support all nodes that are widely used in the JavaScript community, and do not recommend or prefer any of them over any other.

npm 是一个 JavaScript 包管理器。我们支持各种平台，在 JavaScript 社区被广泛使用，不支持谁谁谁比谁谁谁优先级更高。

Node’s community has always been its best feature. With over 100,000 modules today, covering a breathtaking array of use cases, used every day by millions of developers around the world, it is clear that the primary value Node provides is a small core enabling userland innovation.

Node 社区是它最好的一个特性。现在在上面的包已经超过了10万，覆盖了繁多的使用场景，每天被全世界数以百万的人使用，很显然 Node 提供的主要价值作为一个核心，正在促使着用户群的革新。

npm remains committed to its mission of reducing friction for all JavaScript developers.

npm 仍然会致力于自己的使命——减少 JavaScript 开发者阻力。

## Will npm be renaming to “ipm” since it’s the io.js package manager?

## npm 会不会更名为 “ipm”，既然有 io.js package manager？

No.

不会。

"npm" does not stand for Node Package Manager. It is officially an abbreviation for “npm is not an acronym”. We do enjoy novel pun-making though.

“npm” 不是指代 Node Package Manager。它是“npm 不是首字母缩写词”的[官方缩写词](https://docs.npmjs.com/misc/faq#if-npm-is-an-acronym-why-is-it-never-capitalized-)。我们的确喜欢这种文艺的[双关语](https://github.com/npm/npm-expansions)。

—————

*Edit 2014-12-11 – At the decision of the last TC meeting, it’s “io.js” or “Io.js” when capitalized, but not “IO.js*”.

*2014-12-11 更新——根据最新一次 TC 会议的决定，使用 “io.js” 或 “Io.js”（大写版），不用“IO.js*”。

原文：http://blog.izs.me/post/104685388058/io-js
