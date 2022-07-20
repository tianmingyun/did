# DID V1.0核心架构、数据模型和表示序言

W3C 2021 年 8 月 3 日提案

相关链接及作者

本版本链接:

[Decentralized Identifiers (DIDs) v1.0](https://www.w3.org/TR/2021/PR-did-core-20210803/ "Decentralized Identifiers (DIDs) v1.0")

最新稿:

[Decentralized Identifiers (DIDs) v1.0](https://w3c.github.io/did-core/ "Decentralized Identifiers (DIDs) v1.0")

测试包及实现报告:

[DID Core Specification Test Suite and Implementation Report](https://w3c.github.io/did-test-suite/ "DID Core Specification Test Suite and Implementation Report")

以前的版本:

[Decentralized Identifiers (DIDs) v1.0](https://www.w3.org/TR/2021/CRD-did-core-20210730/ "Decentralized Identifiers (DIDs) v1.0")

编辑:

[Manu Sporny](http://manu.sporny.org/ "Manu Sporny") ([Digital Bazaar](https://digitalbazaar.com/ "Digital Bazaar"))

[Amy Guy](https://rhiaro.co.uk/ "Amy Guy") ([Digital Bazaar](https://digitalbazaar.com/ "Digital Bazaar"))

[Markus Sabadello](https://www.linkedin.com/in/markus-sabadello-353a0821 "Markus Sabadello") ([Danube Tech](https://danubetech.com/ "Danube Tech"))

[Drummond Reed](https://www.linkedin.com/in/drummondreed/ "Drummond Reed") ([Evernym](https://www.evernym.com/ "Evernym"))


作者:

[Manu Sporny](http://manu.sporny.org/ "Manu Sporny") ([Digital Bazaar](https://digitalbazaar.com/ "Digital Bazaar"))

[Dave Longley](https://github.com/dlongley "Dave Longley") ([Digital Bazaar](https://digitalbazaar.com/ "Digital Bazaar"))

[Markus Sabadello](https://www.linkedin.com/in/markus-sabadello-353a0821 "Markus Sabadello") ([Danube Tech](https://danubetech.com/ "Danube Tech"))

[Drummond Reed](https://www.linkedin.com/in/drummondreed/ "Drummond Reed") ([Evernym](https://www.evernym.com/ "Evernym"))

[Orie Steele](https://www.linkedin.com/in/or13b/ "Orie Steele") ([Transmute](https://www.transmute.industries/ "Transmute"))

[Christopher Allen](https://www.linkedin.com/in/christophera "Christopher Allen") ([Blockchain Commons](https://www.blockchaincommons.com/ "Blockchain Commons"))

参与者:

[GitHub w3c/did-core](https://github.com/w3c/did-core/ "GitHub w3c/did-core")

[File an issue](https://github.com/w3c/did-core/issues/ "File an issue")

[Commit history](https://github.com/w3c/did-core/commits/main "Commit history")

[Pull requests](https://github.com/w3c/did-core/pulls/ "Pull requests")

相关文档

[DID Use Cases and Requirements](https://www.w3.org/TR/did-use-cases/ "DID Use Cases and Requirements")

[DID Specification Registries](https://www.w3.org/TR/did-spec-registries/ "DID Specification Registries")

[DID Core Implementation Report](https://w3c.github.io/did-test-suite/ "DID Core Implementation Report")

[Copyright](https://www.w3.org/Consortium/Legal/ipr-notice#Copyright "Copyright") © 2021 [W3C](https://www.w3.org/ "W3C")® ([MIT](https://www.csail.mit.edu/ "MIT"), [ERCIM](https://www.ercim.eu/ "ERCIM"), [Keio](https://www.keio.ac.jp/ "Keio"), [Beihang](https://ev.buaa.edu.cn/ "Beihang")). W3C [liability](https://www.w3.org/Consortium/Legal/ipr-notice#Legal_Disclaimer "liability"), [trademark](https://www.w3.org/Consortium/Legal/ipr-notice#W3C_Trademarks "trademark") and [permissive document license](https://www.w3.org/Consortium/Legal/2015/copyright-software-and-document "permissive document license") rules apply.




# 摘要

去中心化标识符 (DID) 是一种新型标识符，用于实现可验证的去中心化数字身份。 DID 指的是由 DID 控制器（DID controller）确定的任何主体（DID subject）（例如人、组织、事物、数据模型、抽象实体等）。 与典型的联合标识符相比，DID 的设计使其可以独立于集中式注册表、身份提供者和证书颁发机构进行实施。 具体来说，虽然其他方也可能被用于发现与 DID 相关的信息，但该设计使 DID 控制器能够证明对它的控制，而无需任何其他方的许可。 DID 是将 DID 主体与 DID 文档（DID document）相关联的 URI，允许与该主体可信交互。

每个 DID 文档都会包含加密材料、验证方法或服务，它提供了一组机制，使 DID 控制器能够证明对 DID 的控制。 服务实现与 DID 主体相关的可信交互。 如果 DID 主体是诸如数据模型之类的信息资源，则 DID 就会返回 DID 主体本身。

本文档指定了 DID 语法、通用数据模型、核心属性、序列化表示、DID 操作以及将 DID 解析为它们所代表的资源的过程的说明。

# 文档状态

*本节描述了本文档在出版时的状态。 其他文件可能会取代本文件。 当前 W3C 出版物列表和本技术报告的最新版本可在 W3C 技术报告索引中找到，网址为 https://www.w3.org/TR/。*

W3C 去中心化标识符工作组已将本文档作为 W3C 提议的建议书发布，并要求相关方在 2021 年 8 月 31 日之前审查本规范。

在发布时，存在 103 个实验性 DID 方法规范、32 个实验性 DID 方法驱动程序实现、一个确定给定实现是否符合本规范的测试套件以及提交给一致性测试套件的 46 个实现。 建议读者注意 DID 核心问题和 DID 核心测试套件问题，每个问题都包含最新的关注点列表和可能导致对本规范进行更改的建议更改。 在发布时，预计不会出现额外的实质性问题、更改或修改。

欢迎对本文档提出意见。 请直接在 GitHub 上提交问题，或将它们发送到 public-did-wg@w3.org（订阅，存档）。

本文件由去中心化标识符工作组作为建议书发布。 本文档旨在成为 W3C 推荐标准。

讨论本规范时首选 GitHub 问题。 或者，您可以将评论发送到我们的邮件列表。 请将它们发送到 public-did-wg@w3.org（订阅，档案）。

作为提议的建议发布并不意味着得到 W3C 成员的认可。

这是一份草稿文件，可能随时被其他文件更新、替换或废止。 除了正在进行的工作之外，引用本文件是不恰当的。

邀请 W3C 成员和其他相关方在 2021 年 8 月 31 日之前审阅该文件并发送意见。咨询委员会代表应查阅他们的 WBS 问卷。 请注意，预计在 2021 年 7 月 13 日结束的候选建议审查期间会提出实质性技术意见。

本文档由一个根据 [W3C 专利政策](https://www.w3.org/Consortium/Patent-Policy/)运营的小组制作。 W3C 维护与该小组的可交付成果相关的任何[专利披露的公开列表](https://www.w3.org/groups/wg/did/ipr)； 该页面还包括披露专利的说明。 实际了解个人认为包含[基本权利要求](https://www.w3.org/Consortium/Patent-Policy/#def-essential)的专利的个人必须根据 [W3C 专利政策第 6 节](https://www.w3.org/Consortium/Patent-Policy/#sec-Disclosure)披露该信息。

本文档受 [2020 年 9 月 15 日 W3C 流程文档](https://www.w3.org/2020/Process-20200915/)的约束。

