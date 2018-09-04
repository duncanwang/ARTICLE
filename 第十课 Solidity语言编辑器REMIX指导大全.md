![](https://upload-images.jianshu.io/upload_images/1190574-3943f2aa688230f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 1\. 文章摘要 

**【本文目标】**
 本文翻译和实践Solidity语言编辑器REMIX官网帮助文档，但并不拘泥于逐字翻译，而时根据实践用中文语言讲透编辑器相关功能。
**【前置条件】**
了解以太坊ETH和Solidity语言。
**【技术收获】**
1).  使用REMIX编辑器进行Solidity语言编程，编译，运行，调试。
2). REMIX界面介绍
**【实操课程列表】**
[第一课 如何在WINDOWS环境下搭建以太坊开发环境](https://www.jianshu.com/p/683ea7d62a39)
[第二课 如何实现以太坊最简智能合约“Hello World”的运行](https://www.jianshu.com/p/2a5d60744988)
[第四课 以太坊开发框架Truffle从入门到实战](https://www.jianshu.com/p/2e2b3b12eb0e)
[第六课 技术小白如何开发一个DAPP区块链应用(以宠物商店为例)](https://www.jianshu.com/p/9844d97ef1d0)
[第七课 技术小白如何在45分钟内发行通证(TOKEN)并上线交易](https://www.jianshu.com/p/a7880be9b46e)
[第八课 如何调试以太坊官网的智能合约众筹案例](https://www.jianshu.com/p/c7c87c7ad74f)
[第九课 如何在Remix环境下进行Solidity代码单步调试](https://www.jianshu.com/p/a2502274b092)
[第十课 Solidity语言编辑器REMIX指导大全](https://www.jianshu.com/p/2110ed61d2cc)

【说明】未列出的课程为知识普及的非实操类课程，所有区块链文章参考[“区块链入口”](https://www.jianshu.com/c/93efec13d95f)专栏。

#2. 使用REMIX编辑器执行"HELLO WORLD"的智能合约
REMIX是一个智能合约编程语言Solidity的集成开发环境。
最简单的入门就是进行"HELLO WORLD"的程序运行。

##2.1 智能合约编译
在CHROME浏览器上点击链接[REMIX官网浏览器版编辑器](http://remix.ethereum.org)即可打开REMIX编辑器。部分不能正常访问官网的，可以访问笔者的[国内REMIX编辑器](http://remix2.ju3ban.net)。
新增一个命名为“HelloWorld.sol”的文件，复制以下代码：
```
pragma solidity ^0.4.17;

contract Hello         
{
    address creator;     
    string greeting;     

    function Hello(string _greeting) public   
    {
        creator = msg.sender;
        greeting = _greeting;
    }
    

    function greet() public constant returns (string)           
    {
        return greeting;
    }
    
    function setGreeting(string _newgreeting) public
    {
        greeting = _newgreeting;
    }
    
     /**********
     Standard kill() function to recover funds 
     **********/
    
    function kill() public
    { 
        if (msg.sender == creator)
            suicide(creator);  // kills this contract and sends remaining funds back to creator
    }

}
```
点击"Start to compile"按钮，会得到一下结果：![0.智能合约编译.png](https://upload-images.jianshu.io/upload_images/1190574-6be07b22277aad5f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
除一些函数的告警提示外，结果表明该智能合约编译成功。

###2.1.1 编译告警
如果编写的程序有告警，REMIX会有提示。一般来说，提示不必修改。![REMIXD编译告警](https://upload-images.jianshu.io/upload_images/1190574-b1477fa2e8dc2ceb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###2.1.2 编译错误
为了说明编译错误案例，把该文件的第17行"return greeting;"改为"return _greeting;", 重新编译，得到以下结果![REMIXD编译错误](https://upload-images.jianshu.io/upload_images/1190574-f68e38c77ff72eeb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果编写的Solidity程序存在语法错误，Remix编译器会以粉红色底色提示显示错误原因，并在编辑器窗口提示位置。
编译错误演示完后，恢复第17行代码为"return greeting;"
###2.1.3 详情信息
点击"Detail"按钮，可以看到详细信息。其中WEB3DEPLOY部分的内容就是在案例[《第二课 如何实现以太坊最简智能合约“Hello World”的运行》](https://www.jianshu.com/p/2a5d60744988)中geth控制台使用的编译后代码。![DETAIL的内容](https://upload-images.jianshu.io/upload_images/1190574-be13aea60022b9fe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##2.2 智能合约运行
**1. 创建智能合约**
选择“Run”的页面，选择Environment环境为Javascript VM,在“Create”按钮前输入框内输入"Hello World!"[注意：输入字符串一定要有英文双引号]，点击“Create”按钮。![创建智能合约](https://upload-images.jianshu.io/upload_images/1190574-0567a193df7fdee8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**2. 运行greet函数**
点击浅蓝色函数"greet"按钮，可以看到输出结果为“Hello World!”
![运行函数greet](https://upload-images.jianshu.io/upload_images/1190574-c6a007250d30c5d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**说明：**浅蓝色按钮函数表示该交易函数运行时不需要消耗GAS的。

**3. 运行setGreeting函数**
在setGreeting函数按钮的输入框输入内容"Hello Duncan!", 运行后在点击执行"greet"函数，发现该函数的输出内容已变为"Hello Duncan!"了，说明setGreeting执行正常。
![运行函数SetGreeting](https://upload-images.jianshu.io/upload_images/1190574-534b8249a2e08e9b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**说明：**粉红色按钮函数表示该交易函数运行时是需要消耗GAS的。

#3. REMIX界面详解
第2章以“Hello World”为例，介绍了REMIX集成环境的基本使用方法。本章介绍REMIX编辑器的详细功能描述。
##3.1 REMIX集成环境概述
REMIX是一个智能合约编程语言Solidity的集成开发环境，它集成了一个调试器和测试环境。
如果你对以下内容感兴趣，REMIX是一个不错的学习方式：
1，开发智能合约（REMIX集成了SOLIDITY编辑器）
2，调试智能合约
3，获取已运行合约的状态和属性
4，调试已经提交的合约
5，分析SOLIDITY代码，以便减少编码错误和加强最佳实践
6，配合Mist(或者任何植入web3的工具)，REMIX可用于测试和调试DAPP分布式程序。（参考链接[Debugging a Dapp using Remix - Mist - Geth ](https://remix.readthedocs.io/en/latest/tutorial_mist.html)）
详解介绍使用的程序案例为"ballot.sol"文件的代码，完整代码如下：
```
pragma solidity ^0.4.0;
contract Ballot {

    struct Voter {
        uint weight;
        bool voted;
        uint8 vote;
        address delegate;
    }
    struct Proposal {
        uint voteCount;
    }

    address chairperson;
    mapping(address => Voter) voters;
    Proposal[] proposals;

    /// Create a new ballot with $(_numProposals) different proposals.
    function Ballot(uint8 _numProposals) public {
        chairperson = msg.sender;
        voters[chairperson].weight = 1;
        proposals.length = _numProposals;
    }

    /// Give $(toVoter) the right to vote on this ballot.
    /// May only be called by $(chairperson).
    function giveRightToVote(address toVoter) public {
        if (msg.sender != chairperson || voters[toVoter].voted) return;
        voters[toVoter].weight = 1;
    }

    /// Delegate your vote to the voter $(to).
    function delegate(address to) public {
        Voter storage sender = voters[msg.sender]; // assigns reference
        if (sender.voted) return;
        while (voters[to].delegate != address(0) && voters[to].delegate != msg.sender)
            to = voters[to].delegate;
        if (to == msg.sender) return;
        sender.voted = true;
        sender.delegate = to;
        Voter storage delegateTo = voters[to];
        if (delegateTo.voted)
            proposals[delegateTo.vote].voteCount += sender.weight;
        else
            delegateTo.weight += sender.weight;
    }

    /// Give a single vote to proposal $(toProposal).
    function vote(uint8 toProposal) public {
        Voter storage sender = voters[msg.sender];
        if (sender.voted || toProposal >= proposals.length) return;
        sender.voted = true;
        sender.vote = toProposal;
        proposals[toProposal].voteCount += sender.weight;
    }

    function winningProposal() public constant returns (uint8 _winningProposal) {
        uint256 winningVoteCount = 0;
        for (uint8 prop = 0; prop < proposals.length; prop++)
            if (proposals[prop].voteCount > winningVoteCount) {
                winningVoteCount = proposals[prop].voteCount;
                _winningProposal = prop;
            }
    }
}
```
REMIX主界面截图如下，具体说明参考各个子章节。![REMIX主界面截图](https://upload-images.jianshu.io/upload_images/1190574-d6cfc2a1875bf1d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**具体界面介绍章节分类如下：**
3.2 [文件浏览器](http://remix.readthedocs.io/en/latest/file_explorer.html)
3.3 [Solidity语言编辑器](http://remix.readthedocs.io/en/latest/solidity_editor.html)
3.4 [终端输出器](http://remix.readthedocs.io/en/latest/terminal.html)
3.5 分页控制面板
3.5.1 [编译合同](http://remix.readthedocs.io/en/latest/compile_tab.html)
3.5.2 [运行](http://remix.readthedocs.io/en/latest/run_tab.html)
3.5.3 [调试](http://remix.readthedocs.io/en/latest/debugger_tab.html)
3.5.4 [设置](http://remix.readthedocs.io/en/latest/settings_tab.html)
3.5.5 [分析](http://remix.readthedocs.io/en/latest/analysis_tab.html)
3.5.6 [支持](http://remix.readthedocs.io/en/latest/support_tab.html)

##3.2 文件浏览器
![文件浏览器](https://upload-images.jianshu.io/upload_images/1190574-c66f4bb6e9747f9c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**1）增加新文件(Create new File)**
点击在浏览器上增加一个新的智能合约文件
![9.增加新文件.png](https://upload-images.jianshu.io/upload_images/1190574-8f1f91d90bb09f84.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**2）打开文件(Add Local File)**
![打开存在文件](https://upload-images.jianshu.io/upload_images/1190574-fb6925a1f859d4ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**3） 匿名发布到GitHub（Publish to Gist）**
![11. 匿名发布到GitHub.png](https://upload-images.jianshu.io/upload_images/1190574-0d01418765a49a21.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**4） 复制文件列表到其他浏览器(Copy to another instance)**
例如，把“http://remix2.ju3ban.net”的文件列表复制到另一个浏览器“http://remix.ju3ban.net”
修改输入目标浏览器的网址：![12.复制到别的浏览器.png](https://upload-images.jianshu.io/upload_images/1190574-4d6c01d88bd546d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开新的浏览器，文件列表已复制过来了：![13.别的浏览器的文件列表.png](https://upload-images.jianshu.io/upload_images/1190574-e0dbf58ab1f824ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**5）链接本地服务器（Connect to Localhost）**
在Remix文件浏览器上的文件并不会同步到本地文件系统，对浏览器文件列表的新增/修改/删除并不会同步本地文件。一般操作时，调试成功的代码需要在本地文件系统新建文件保存。Remix浏览器提供了一种方法，就是使用Remixd，允许保存和同步浏览器文件到你的本地计算机中。因为根据官网文档，笔者没有能够在Windows 10系统或者Ubuntu中安装成功Remixd，本文不再详细介绍该功能。有兴趣的朋友欢迎根据[官网文档](http://remix.readthedocs.io/en/latest/tutorial_remixd_filesystem.html)尝试使用REMIXD同步。成功者欢迎跟辉哥反馈交流，补充完整这个文档。

![14. 链接本地文件系统.png](https://upload-images.jianshu.io/upload_images/1190574-1ed3c2a6962512cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##3.3 [Solidity语言编辑器](http://remix.readthedocs.io/en/latest/solidity_editor.html)
![15. Solidity 浏览器.png](https://upload-images.jianshu.io/upload_images/1190574-0dcd8f365e2e1f15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Solidity语言编辑器重要的功能列表：
1，TAB页面显示打开的文件
2，编译告警或者错误会显示在编辑器最左侧边
3，Remix自动保存当前文件（修改后5秒内）
4，+/- 表示增加/减少编辑器的字体大小

##3.4 [终端输出器](http://remix.readthedocs.io/en/latest/terminal.html)
![终端输出器窗口](https://upload-images.jianshu.io/upload_images/1190574-a4e7091ae1f9e963.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**终端输出器功能说明：**
1，点击1按钮可隐藏终端输出器
2，点击2按钮可情况输出器内容
3，点击3按钮可复制地址信息等到缓存，便于粘贴
4，点击Details按钮可展开交易详细内容
5， 点击Debug按钮可调试该交易合约
6， 点击复选框，可增加选择显示所有的交易还是只显示智能合约相关的交易程序。默认只显示后者。
![17. 终端显示内容.png](https://upload-images.jianshu.io/upload_images/1190574-a061d0f26b864f8a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
7，输入关键字可搜索定位输出内容
例如输入"Hello "关键字，相关的内容就显示出来了。
![18. 搜索Hello关键字](https://upload-images.jianshu.io/upload_images/1190574-ad128d0d84f5ec33.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
8，监听网络（Listen to network）
勾选后，REMIX不仅监听REMIX编辑器中创建的交易，还监听当前环境中所有的交易。

##3.5 分页控制面板
###3.5.1 [编译合约](http://remix.readthedocs.io/en/latest/compile_tab.html)
![20. Compile控制面板.png](https://upload-images.jianshu.io/upload_images/1190574-08ae63a5d05f09c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1，发布到Swarm网络(Publish on Swarm)
点击“Publish on Swarm”按钮可把智能合约源码和对应的ABI信息发布到网络上。这个合约智能是非抽象类的才能发布。
合约发布后，你可以点击Detail按钮，在弹出窗口的“SWARM LOCATION”
位置找到对应的MetaDATA信息。
信息中的SWARM LOCATION位置有对应网址，在能科学上网的计算机就可找到该合同的MetaData信息 “bzzr://165fad4c3d8ead3a7fe28296777b4bedafb09bb57de2e9ba39c1547866437182”

###3.5.2 [运行](http://remix.readthedocs.io/en/latest/run_tab.html)
![21. Run控制面板.png](https://upload-images.jianshu.io/upload_images/1190574-c12018a7514b70d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**重点特性说明**
**1） 环境选择**
**JavaScript VM**: 所有交易在浏览器的沙盒中被执行。这以为着没有结果是永久保存的。当页面重载后，一个新的区块链会被从零开始运行，老的会被丢弃。
**Injected Provider**: Remix将连接被植入的Web3对象。例如Mist或者Metamask钱包是提供植入的Web3对象的例子。例如[《第八课 如何调试以太坊官网的智能合约众筹案例》](https://www.jianshu.com/p/c7c87c7ad74f)就是使用Meta的账号。
**Web3 Provider**: Remix将连接到一个远程节点。你可以用这个地址来选择钱包客户端：geth, parity or any Ethereum client.

**2） 初始化合约**
**1] 创建合约**
“Create”按钮用于创建合约。合约创建时，需要参考左边构建函数定义（智能合约的同名函数为构建函数）的类型来输入参数。
![25. 创建合约.png](https://upload-images.jianshu.io/upload_images/1190574-c914756468a524f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
注意：如果输入参数为地址或者字符串，都需要使用英文双引号("")囊括起来，使用英文(",")表示输入参数分割。

**2] 运行合约**
假设给出的地址是一个选择合同的实例。通过这种方法就可以跟已经运行的合同进行交互。要特别谨慎使用该功能，因为系统不做验证。运行时要确认信任这个地址的合同。

**3] 等待(pending)合约**
![26，挂起合约.png](https://upload-images.jianshu.io/upload_images/1190574-ff2fe3bc992f7e37.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

等待合约表示还没有完成执行过程的合约。按钮功能描述参考上述附件。


####3.5.1.1 Web3 Provider远程节点配置
1） 启动Ganache客户端。不熟悉的，可参考该篇文章的对应关键字[“在WINDOWS上安装Ganache”](https://www.jianshu.com/p/683ea7d62a39)安装好客户端。![22. 启动Ganache客户端.png](https://upload-images.jianshu.io/upload_images/1190574-be3c12bb56e74f5e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2） 输入Web3 Provider远程节点，例如Ganache的签宝地址为“HTTP://127.0.0.1:7545” ![23. 输入WEB3 privider的服务器地址.png](https://upload-images.jianshu.io/upload_images/1190574-fdbcee0dcf92c9bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3） 输入完成后，Remix的账号列表更新为Ganache的账号列表了。
![24. 账号为Ganache的账号列表.png](https://upload-images.jianshu.io/upload_images/1190574-65bc3b9e72354d5e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###3.5.3 [调试](http://remix.readthedocs.io/en/latest/debugger_tab.html)
调试/回滚和单步调试是一个很重要的功能，点击[《第九课 如何在Remix环境下进行Solidity代码单步调试》](https://www.jianshu.com/p/a2502274b092)文章查看。

###3.5.4 [设置](http://remix.readthedocs.io/en/latest/settings_tab.html)
![27. Settiongs.png](https://upload-images.jianshu.io/upload_images/1190574-0ee7056cc99e180d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**重要特性说明：**
（1）Solidity version：编译SOLIDITY版本，不调整的话系统会默认选择最新正式发布的版本。
（2）Always use Ethereum VM at Load：勾选此项会在重新加载时默认选择JavaScript VM环境。
（3）Enable Optimization：勾选此项会节省运行GAS，JVM等测试环境可不用勾选。

###3.5.5 [分析](http://remix.readthedocs.io/en/latest/analysis_tab.html)
分析面板显示的是最后一次编译的信息，默认情况下每一次编译都会产生一个新的分析。
这个分析面板显示的是合约代码的详细信息，它可以帮助你避免代码错误或者增强编码能力。![28. Analysis页面.png](https://upload-images.jianshu.io/upload_images/1190574-581584d53c74b5ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**Security:**
Transaction origin: 如果tx.origin被使用则会告警；
Check effects: 避免潜在的可重入性BUG；
Inline assembly:  使用植入汇编方式；
Block timestamp: Semantics maybe unclear；
Low level calls: Semantics maybe unclear；
Block.blockhash usage: 自毁调用；
**Gas & Economy:**
Gas costs: 函数的GAS消耗太高会告警。
This on local calls: 勾选后本地函数会被唤醒；
**Miscellaneous:**
Constant functions: 检查潜在的常量函数
Similar variable names: 检查变量名是否太相似

Miscelllaneous下方显示的是本次编译的优化告警，供程序员优化代码使用。
![28. Analysis页面2.png](https://upload-images.jianshu.io/upload_images/1190574-4eb33f4b77ea1d0a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###3.5.6 [支持](http://remix.readthedocs.io/en/latest/support_tab.html)
支持页面，可点击需求技术支持和交流。
![remix_supporttab](https://upload-images.jianshu.io/upload_images/1190574-9e60f59cb726837d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#4.典型错误介绍
#### 4.1 REMIX编译器官网链接打不开
**提示内容：**点击官网链接（http://remix.ethereum.org），如果打不开或者有错误提示“mock compiler: source not found”
**原因分析：** 这个主要是不能科学上网引起官网浏览器不能下载完全。
**解决办法：**
1，参考欧阳哥哥的文章[《【以太坊开发】Remix IDE本地部署与配置个性风格》](https://www.jianshu.com/p/afdee36ccbc0)完成本地REMIX的部署；
2，采用一个已国内部署的REMIX浏览器链接：http://remix2.ju3ban.net

#### 4.2 参数转换失败
**提示内容：** 终端输出器显示为“transact to Donation.moveFund errored: Error encoding arguments: Error: Assertion failed ”
**原因分析：** 
Solidity函数定义为"function moveFund(address _to, uint _amount)"
RUN面板的函数输入参数如下，希望转移5个ETH给目标账号。
> "0x14723a09acff6d2a60dcdf7aa4aff308fddc160c",500000000000000

结果提示错误。
这个原因主要是因为”5 ＊10^18”的值超过了计算器的范围，需要增加""来让程序进行转换。Solidity的默认单位为wei，1 ETH = 1*10^18 wei。
**解决办法：**
输入参数改为2个参数均有双引号则不会有错误提示。
> "0x14723a09acff6d2a60dcdf7aa4aff308fddc160c","500000000000000"

在remix中，任何输入参数都可以带""表示。

#### 4.3 import 导入依赖编译失败
**提示内容：** 
编译一个智能合约，有以下依赖导入代码
```
import "./SafeERC20.sol";
import "../../ownership/Ownable.sol";
import "../../math/SafeMath.sol";
```
REMIX编译失败，提示为“Unable to import "math/SafeMath.sol": File not found”

![](https://upload-images.jianshu.io/upload_images/1190574-dd6e2da6187cb105.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**原因分析：** 
REMIX支持打开本地文件，目前还不支持本地文件导入。
**解决办法：**
直接从GitHub上导入文件。改为如下代码即可编译成功。
```
import 'https://github.com/OpenZeppelin/openzeppelin-solidity/contracts/token/ERC20/SafeERC20.sol';
import 'https://github.com/OpenZeppelin/openzeppelin-solidity/contracts/math/SafeMath.sol';
import 'https://github.com/OpenZeppelin/openzeppelin-solidity/contracts/ownership/Ownable.sol';
```
注意哦，import的地址为`https://github.com/OpenZeppelin/openzeppelin-solidity/contracts/token/ERC20/SafeERC20.sol`，而不是浏览器能打开的`https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/token/ERC20/SafeERC20.sol`地址。

#5.参考文档
1)，[REMIX英文官网帮助文档](http://remix.readthedocs.io/en/latest/)
2),   [【以太坊开发】Remix IDE本地部署与配置个性风格](https://www.jianshu.com/p/afdee36ccbc0)
3),  [SOLIDITY语言官网中文版](http://solidity-cn.readthedocs.io/zh/develop/solidity-in-depth.html)

*尊重知识输出，如需引用，敬请说明本文链接和作者-笔名辉哥。*
