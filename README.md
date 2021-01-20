# proj47-tee-os

### 项目名称

面向可信执行环境的OS。

### 项目描述

机密计算（Confidential Computing）是近几年新兴的一项安全技术，它利用可信执行环境（简称TEE）的强隔离和内存加密来保护TEE内的可信应用程序，确保了用户数据的机密性和完整性。机密计算一举解决了诸多应用场景中“信任”难题，包括但不限于公有云、区块链、多方数据分析等。

目前，所有主流的体系架构都已经推出了各自的TEE实现，比如Intel SGX、ARM TrustZone、AMD SEV、Intel TDX和RISC-V Keystone等。TEE中要运行各种应用程序当然离不开OS的支撑；而OS的设计也必须充分考虑TEE的需求。本课题希望参赛团队能针对TEE的场景设计和实现一个安全而高效的OS（详见“特性”）。

### 所属赛道

2021全国大学生操作系统比赛的“OS功能设计”赛道

### 参赛要求

- 以小组为单位参赛，最多三人一个小组，且小组成员是来自同一所高校的本科生（2021年春季学期或之后本科毕业的大一~大四的学生）
- 如学生参加了多个项目，参赛学生选择一个自己参加的项目参与评奖
- 请遵循“2021全国大学生操作系统比赛”的章程和技术方案要求

### 项目导师

田洪亮（蚂蚁集团）
* Github： https://github.com/tatetian
* Email： tate.thl@antgroup.com
* WeChat：tatetian

### 难度

* 基础特性的难度：中等
* 高级特性的难度：较高

### 特征

必选的基础特性：

* 支持至少一种TEE，包括但不限于
    * Intel SGX
    * AMD SEV
    * ARM TrustZone
    * Intel TDX
    * RISC-V Keystone
* 使用Rust语言开发
* 基于微内核架构
* 兼容Linux syscall ABI
* 可运行复杂应用程序

可选的高级特性：

* 兼容多种TEE；
* 实现高性能的加密文件系统；
* 利用语言级的隔离机制实现高效RPC；
* 支持基于io_uring的switchless I/O。

关于高级特性的说明，详见预期目标部分。

### 文档

无

### License

* [Apache-2.0](https://opensource.org/licenses/Apache-2.0)

## 预期目标

### 注意：下面的内容是建议内容，不要求必须全部完成。选择本项目的同学也可与导师联系，提出自己的新想法，如导师认可，可加入预期目标

前述的高级特性都具备一定的新颖性和学术价值，也没有成熟的实现可供参考。因此，这里提供更多的背景信息。

**兼容多种TEE.**  不同体系架构的TEE具备不同的特点。比如，Intel SGX是用户态的，因此给SGX用的TEE实际上是Library OS。AMD SEV和Intel TDX都是VM级的，需要运行“真正”的OS。而ARM TrustZone的运行环境虽然也是特权级，但不是VM。目前，业界尚未出现可跨多种TEE的通用OS。我们希望你能解决这个“架构通用TEE OS”的技术挑战。

**利用语言级的隔离机制实现高效RPC.**  RPC数量多导致性能开销大，这是长期困扰微内核OS的一个技术难题。OSDI‘20的[RedLeaf工作](https://www.usenix.org/conference/osdi20/presentation/narayanan-vikram)提出了一种基于Rust语言的轻量级隔离机制，有望从根本上解决安全隔离和RPC开销之间的矛盾。但这个工作并没有开源，而且我们认为这个方向还有可以挖掘的创新潜力。所以我们给你的挑战是：在你的TEE OS中复现RedLeaf提出的隔离机制，进而延伸，甚至超越它。

**高性能加密文件系统.** TEE OS是要防止来自Host OS的攻击的，保护TEE OS的文件I/O的机密性和完整性。因此，加密文件系统是TEE OS的“标配”。加密FS并不是普通FS加上加密这么简单。加密FS必须保证FS整体的完整性，目前标准的做法是采用merkel hash tree，这意味着对一个数据块的更新需要更新多个hash块——这就是“写放大”的问题，尤其对随机写。所以，我们给你的挑战是设计一种新型的加密FS，在保证安全性的前提下，达到很好的性能。

**高效的host call机制.** TEE总是依附于一个host环境的，因此TEE OS的功能是可以部分依赖于host OS的。当TEE OS需要请求host OS提供某些服务时，TEE OS就需要做host call：在Intel SGX这叫OCall，在AMD SEV上就是hypercall。由于CPU在TEE和host之间的切换成本较高，所以普通host call如果被频繁调用，则会产生比较大的性能影响。因此，我们给你的挑战是：能不能设计一种更高效的host call机制？（提示：Linux的io_uring特性）

如果有更多关于TEE OS的创新想法或需要TEE的硬件支持，欢迎参赛团队联系导师。