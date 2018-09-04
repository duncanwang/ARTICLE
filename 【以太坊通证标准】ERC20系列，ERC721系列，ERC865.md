![](https://upload-images.jianshu.io/upload_images/1190574-af166f57c438ac8a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#1，摘要
**【本文目标】**
通过本文学习，了解ERC定义及知名ERC协议标准，如ERC20以及ERC223、ERC621,ERC827协议，ERC721以及 ERC875,ERC1155,ERC998协议，ERC 865等描述和定义。

#2，ERC的定义及标准列表
ERC代表“Etuereum Request for Comment"，这是Ethereum版的意见征求稿 （RFC），RFC是由互联网工程任务组制定的一个概念。 RFC中的备忘录包含技术和组织注意事项。 对于ERC，意见征求稿中包括一些关于以太坊网络建设的技术指导。

ERC是Ethereum开发者为以太坊社区编写的。 因此，ERC的创建流程中包括开发人员。 为了创建一个以太坊平台的标准，开发人员应当提交了一个以太坊改进方案（EIP）， 改进方案中包括协议规范和合约标准。 一旦EIP被委员会批准并最终确定，它就成为ERC。 EIP的完整列表可以在[GITHUB网址](https://github.com/ethereum/EIPs)找到。

**EIP有4种状态：**
**草稿(Draft)** -  处于打开状态，便于考察讨论；
**接受(Accepted)** - 即将被接受，例如将包含在下一个硬分叉中；
**定稿（Final）**- 在上一个硬分叉中被接受，已定稿；
**延期(Deferred)**- 不会马上被接受，但也许在将来的硬分叉版本会考虑。

最终确定的EIP为以太坊开发者提供了一套可实施的标准。 这使得智能合约可以遵循这些通用的接口标准来构建。

ERC-20是整个加密社区中最为人熟知的标准，在Ethereum平台之上发布的大多数通证（token）都使用它。

#3，ERC20系列 - ERC20、ERC223、ERC621,ERC827
##3.1 ERC20标准
* **状态**：
定稿（Final）
* **提交记录**：
https://github.com/ethereum/EIPs/issues/20
* **标准说明**：
https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md
* **推荐样例**：
https://github.com/OpenZeppelin/openzeppelin-solidity/tree/master/contracts/token/ERC20

ERC-20标准中定义了以下函数接口：
* **totalSupply()**：
返回代币供给总量
* **balanceOf(address _owner)**：
返回_owner的帐户余额
* **transfer(address _to,uint256 _value)**：
并将数量为_value的代币转入地址_to并触发transfer事件
* **transferFrom(address _from,address _to,uint256_value)**：
将地址_from中的_value数量的代币转入地址_to ，并触发transfer事件
* **approve(address _spender,uint256 _value)**：
允许_spender提取限额_value的代币
* **allowance(address _owner,address _spender）**：
返回_spender可从_owner提款的代币数量上限

ERC-20于2015年提出并于2017年9月正式实施。这是代币标准化的一个很好的起点。 然而，开发者社区 已经注意到它存在一些缺陷和漏洞，此外，还有一些场景它不能很好的满足。因此陆续提出了其他的ERC标准。

##3.2 ERC223
* **状态**：
草稿(Draft)
* **提交记录**：
https://github.com/ethereum/EIPs/issues/223
* **标准说明**：
https://github.com/Dexaran/ERC223-token-standard
* **推荐样例**：
 https://github.com/Dexaran/ERC223-token-standard/tree/Recommended

开发人员Dexaran在一篇文章中详细描述了ERC20不适合的两种场景：
“在ERC20中执行交易有两种方式：transfer函数，approve + transferFrom机制，通证余额只是通证合约中的一个变量。
通证的交易是合约内部变量的变化。 转出账户的余额将减少，转入账户的余额将增加。
交易发生时， transfer()函数不会通知转入账户。 因此转入账户将无法识别传入的交易！ 我写了一个例子，可以展示这一导致未处理的交易和资金损失的过程 。
因此，如果接收账户是合约账户，那么必须使用approve + transferFrom机制来发送通证。
如果接受账户是外部帐户，则必须通过transfer函数发送通证。 如果选择了错误的机制，通证将卡在合约内（合约将不会识别交易），没有办法来提取这些卡壳的通证。“
他对这个问题提出的解决方案包含在ERC-223中 。 它与ERC-20标准非常相似，但解决了上述问题。当通证转移到智能合约账户时，该合约的特殊函数tokenFallback() 允许接收方合约拒绝令牌或触发进一步的操作。 大多数情况下，这可以用来代替approve()函数。
函数接口：
**transfer(address _to, uint _value)**：
会区分代币是发往外部账户地址还是发往智能合约地址。
**transfer(address _to, uint _value, bytes _data)**：
会区分代币是发往外部账户地址还是发往智能合约地址，还可以传送数据。
* 1、如果_to是合约，则此函数必须传输令牌并调_to中的函数tokenFallback(address,uint256,bytes)。
* 2、如果_to（接收方合同）中没有实现tokenFallback函数，则事务必须失败，并且不会发生令牌的传输。
* 3、如果_to是外部拥有的地址，则必须发送事务，而不尝试在_to中执行tokenFallback。
* 4、_data可以附加到这个令牌交易中，它将永远保持在块状（需要更多的gas）。 _data可以是空的。

*注意：检查_to是合约还是地址的推荐方法是组装_to的代码。 如果_to中没有代码，那么这是一个外部拥有的地址，否则就是一个合约。*
```
  //assemble the given address bytecode. If bytecode exists then the _addr is a contract.
  function isContract(address _addr) private view returns (bool is_contract) {
      uint length;
      assembly {
            //retrieve the size of the code on target address, this needs assembly
            length := extcodesize(_addr)
      }
      return (length>0);
    }
```


**function tokenFallback(address _from, uint _value, bytes _data)**
_from是令牌发送者，_value是传入令牌的数量，_data是附加的数据，类似于Ether事务中的数据。 适用于以太交易的回退功能，并且不返回任何内容。
tokenFallback -- 令牌持有者发送令牌时处理从令牌合同所调用的令牌传输的功能

*注意：msg.sender将是tokenFallback函数内的令牌合同。 过滤哪些令牌（通过令牌契约地址）发送可能很重要。 令牌发送者（谁发起了代币交易的人）在_from thetokenFallback函数内。*

**示例代码:**
ERC223_Interface.sol
```
pragma solidity ^0.4.9;

 /* 新的 ERC23 contract 接口文件 */

contract ERC223 {
  uint public totalSupply;
  function balanceOf(address who) constant returns (uint);

  function name() constant returns (string _name);
  function symbol() constant returns (string _symbol);
  function decimals() constant returns (uint8 _decimals);
  function totalSupply() constant returns (uint256 _supply);

  function transfer(address to, uint value) returns (bool ok);
  function transfer(address to, uint value, bytes data) returns (bool ok);
  function transfer(address to, uint value, bytes data, string custom_fallback) returns (bool ok);
  event Transfer(address indexed from, address indexed to, uint value, bytes indexed data);
}
```

Receiver_Interface.sol
```
pragma solidity ^0.4.9;

 /*
 * Contract that is working with ERC223 tokens
 */

 contract ContractReceiver {

    struct TKN {
        address sender; //调用合约的人
        uint value;
        bytes data;
        bytes4 sig; //签名
    }


    function tokenFallback(address _from, uint _value, bytes _data){
      TKN memory tkn;
      tkn.sender = _from;
      tkn.value = _value;
      tkn.data = _data;
      uint32 u = uint32(_data[3]) + (uint32(_data[2]) << 8) + (uint32(_data[1]) << 16) + (uint32(_data[0]) << 24);
      tkn.sig = bytes4(u);

      /* tkn变量是Ether交易的msg变量的模拟
      *  tkn.sender是发起这个令牌交易的人（类似于msg.sender）
      *  tkn.value发送的令牌数（msg.value的类比）
      *  tkn.data是令牌交易的数据（类似于msg.data）
      *  tkn.sig是4字节的功能签名
      *  如果令牌事务的数据是一个函数执行
      */
    }
}

```

ERC223_Token.sol
```
pragma solidity ^0.4.9;

import "./Receiver_Interface.sol";
import "./ERC223_Interface.sol";

 /**
 * ERC23 token by Dexaran
 *
 * https://github.com/Dexaran/ERC23-tokens
 * https://github.com/LykkeCity/EthereumApiDotNetCore/blob/master/src/ContractBuilder/contracts/token/SafeMath.sol
 */
contract SafeMath {
    uint256 constant public MAX_UINT256 = 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF;

    function safeAdd(uint256 x, uint256 y) constant internal returns (uint256 z) {
        if (x > MAX_UINT256 - y) throw;
        return x + y;
    }

    function safeSub(uint256 x, uint256 y) constant internal returns (uint256 z) {
        if (x < y) throw;
        return x - y;
    }

    function safeMul(uint256 x, uint256 y) constant internal returns (uint256 z) {
        if (y == 0) return 0;
        if (x > MAX_UINT256 / y) throw;
        return x * y;
    }
}


//示例的智能合约代码
contract ERC223Token is ERC223, SafeMath {

  mapping(address => uint) balances;

  string public name;
  string public symbol;
  uint8 public decimals;
  uint256 public totalSupply;


  // 获取token的名称
  function name() constant returns (string _name) {
      return name;
  }
  // 获取token的符号
  function symbol() constant returns (string _symbol) {
      return symbol;
  }
  // 获取token精确到小数点后的位数
  function decimals() constant returns (uint8 _decimals) {
      return decimals;
  }
  // 获取token的发布总量
  function totalSupply() constant returns (uint256 _totalSupply) {
      return totalSupply;
  }


  // 当用户或其他合同想要转移资金时调用的功能。
  function transfer(address _to, uint _value, bytes _data, string _custom_fallback) returns (bool success) {
    //如果to是合约
    if(isContract(_to)) {
        if (balanceOf(msg.sender) < _value) throw; //如果当前的余额不够就抛出
        balances[msg.sender] = safeSub(balanceOf(msg.sender), _value);//发送者的余额做减法
        balances[_to] = safeAdd(balanceOf(_to), _value); //接收者的余额做加法
        ContractReceiver receiver = ContractReceiver(_to);   //初始化接收合约，构造函数参数为接收者的合约地址
        receiver.call.value(0)(bytes4(sha3(_custom_fallback)), msg.sender, _value, _data);
        Transfer(msg.sender, _to, _value, _data);
        return true;
    }
    else {
        return transferToAddress(_to, _value, _data);
    }
}


  // 当用户或其他合同想要转移资金时调用的功能。
  function transfer(address _to, uint _value, bytes _data) returns (bool success) {

    if(isContract(_to)) {
        return transferToContract(_to, _value, _data);
    }
    else {
        return transferToAddress(_to, _value, _data);
    }
}

  // 类似于ERC20传输的标准功能传输，没有_data。
  // 由于向后兼容性原因而增加。
  function transfer(address _to, uint _value) returns (bool success) {

    //类似于没有_data的ERC20传输的标准功能传输
    //由于向后兼容性原因而增加
    bytes memory empty;
    if(isContract(_to)) {//如果是合约
        return transferToContract(_to, _value, empty);
    }
    else {
        return transferToAddress(_to, _value, empty);
    }
}

  //组装定地址字节码。 如果存在字节码，那么_addr是一个合约。
  function isContract(address _addr) private returns (bool is_contract) {
      uint length;
      assembly {
            //检索目标地址上的代码大小，这需要汇编
            length := extcodesize(_addr)
      }
      return (length>0);
    }

  //当传递目标是一个地址时调用函数
  function transferToAddress(address _to, uint _value, bytes _data) private returns (bool success) {
    if (balanceOf(msg.sender) < _value) throw;
    balances[msg.sender] = safeSub(balanceOf(msg.sender), _value);
    balances[_to] = safeAdd(balanceOf(_to), _value);
    Transfer(msg.sender, _to, _value, _data);
    return true;
  }

  //当传递目标是一个合约时调用函数
  function transferToContract(address _to, uint _value, bytes _data) private returns (bool success) {
    if (balanceOf(msg.sender) < _value) throw;
    balances[msg.sender] = safeSub(balanceOf(msg.sender), _value);
    balances[_to] = safeAdd(balanceOf(_to), _value);
    ContractReceiver receiver = ContractReceiver(_to);
    receiver.tokenFallback(msg.sender, _value, _data); //必须要调用这个回调
    Transfer(msg.sender, _to, _value, _data);
    return true;
}


  //得到_owner的余额
  function balanceOf(address _owner) constant returns (uint balance) {
    return balances[_owner];
  }
}

```



##3.3 ERC621
* **状态**：
草稿(Draft)
* **提交记录**：
https://github.com/ethereum/EIPs/issues/621
* **标准说明**：

* **推荐样例**：
https://github.com/skmgoldin/EIP621Token/blob/master/contracts/EIP621Token.sol

ERC-621是ERC-20标准的扩展。 它增加了两个额外的功能， increaseSupply和decreaseSupply 。这可以增加和减少流通中的令牌供应。 ERC-20只允许单一的通证发放事件。 这将供应量限制在一个固定的不可改变的数目。 ERC-621建议totalSupply应当是可修改的。

接口函数说明：
**increaseSupply(uint value, address to)**：
可以给特定账户`to`增加`value`值的供应量，代币总供应量`totalSupply`也同步增加；
**decreaseSupply(uint value, address from)**：
可以给特定账户`to`减少`value`值的账户余额，代币总供应余额`totalSupply`也同步减少；

##3.4  ERC827
* **状态**：
草稿(Draft)
* **提交记录**：
https://github.com/ethereum/EIPs/issues/827
* **标准说明**：

* **推荐样例**：
https://github.com/windingtree/erc827/blob/master/contracts/ERC827/ERC827Token.sol

 它允许转让通证并允许持有人允许第三方使用通证。 以太坊上的通证可以被其他应用程序重复使用，这其中也包括钱包和交易所。 当需要支持第三方动态消费限额调整时这一点非常有用。
该标准目前处于草稿状态，并被证明有使用安全风险，建议暂不使用。

#4，ERC721系列
#4.1 ERC721
* **状态**：
定稿（Final）
* **提交记录**：
https://github.com/ethereum/EIPs/issues/721
* **标准说明**：
https://github.com/ethereum/EIPs/blob/master/EIPS/eip-721.md
* **推荐样例**：
1）https://www.cryptokitties.co/
2）https://github.com/OpenZeppelin/openzeppelin-solidity/tree/master/contracts/token/ERC721

ERC-721与ERC-20和ERC-223都大不相同。 它描述了一个不可互换的通证，官方简要解释是Non-Fungible Tokens，简写为NFTs，多翻译为非同质代币。 这意味着每个通证是完全不同的，并且每个通证对不同的用户都有不同的价值。 理解这种通证的一个方法就是回忆CryptoKittes。 每一个数字猫都是独立的，其价值取决于其稀缺性和用户的购买欲。

ERC-721令牌可用于任何交易所，但通证价值是“每个通证的唯一性和稀缺性所决定的结果”。标准中规定的接口函数包括name、symbol、totalSupply、balanceOf、ownerOf、approve、takeOwnership 、 transfer 、tokenOfOwnerByIndex和tokenMetadata 。 它还定义了两个事件： Transfer和Approval 。 Gerald Nash的 这篇文章很好地解释了可互换性的概念。

函数接口说明：
* **balanceOf(address _owner)**: 
返回由_owner 持有的NFTs的数量。
* **ownerOf(uint256 _tokenId)**: 
返回tokenId代币持有者的地址。
* **exists(uint256 _tokenId)**：
tokenId代币是否存在，返回BOOL值；
* **approve(address _to, uint256 _tokenId)**: 
授予地址_to具有_tokenId的控制权，方法成功后需触发Approval 事件。
* **getApproved(uint256 _tokenId)**：
获得_tokenId授权的地址；
* **setApprovalForAll(address _operator, bool _approved)**: 
授予地址_operator具有所有NFTs的控制权，成功后需触发ApprovalForAll事件。
* **isApprovedForAll(address _owner, address _operator)**：
 用来查询授权。
* **transferFrom(address _from, address _to, uint256 _tokenId)**
不建议使用本函数，建议使用safeTransferFrom函数。
* **safeTransferFrom(address _from, address _to, uint256 _tokenId)**:
把`_tokenId`代币从`_from`账户安全转移到`_to`账户，安全是指如果目标地址为合约地址，则执行`onERC721Received`进行资产转移，否则的话交易会回滚；如果目标地址为外部账号地址，则正常转移。

更深度分析的文章参考[《【基于ERC721的区块链游戏】迷恋猫从玩耍到开发》。](https://www.jianshu.com/p/684490db252c)

#4.2 ERC875
* **状态**：
还处于pull request下（issue）
* **提交记录**：
https://github.com/ethereum/EIPs/issues/875
* **标准说明**：

* **推荐样例**：
https://github.com/alpha-wallet/ERC875-Example

AlphaWallet自主开发了ERC875协议族。该协议不仅会让数字资产变得具有收藏价值，同时也能帮助现实世界中不可拆分替代、具有物权唯一性的资产上链，这就能为线下服务的链上操作提供了可能性。
虽然另一种协议ERC721也能实现token的不可置换性，但其存在需要交易双方支付gas费用、无法简单实现原子化交易等一些不易于用户使用的问题。
ERC875内置了两个密码学协议，  一方面能够简单实现原子化交易（atomic swap）——直接搭建去中心化市场、降低普通用户使用门槛，卖家无需持有以太币，买家支付一次gas即能完成；另外一方面可以简单打包处理大量交易。
拿基于ERC721的加密猫来说，换用ERC875协议的话，能够实现。用户在商家网站法币购猫，通过MagicLink免费把猫导入用户的钱包，之后用户还可以在不需要持有以太币的情况下，通过MagicLink把猫售出或者免费转让，全部过程都是无中心的原子化交易。另外商家可以一次批发100只猫给分销商。

接口函数或者变量说明
* **totalTickets**:
* **inventory**:
* **ticketIndex**:
* **expiryTimeStamp**:
* **owner**:
* **admin**:
* **transferFee**:
* **numOfTransfers**:
* **name**:
* **symbol**:
* **decimals**:
* **Token(uint256[] numberOfTokens, string evName,uint expiry, string eventSymbol, address adminAddr)**:
构建函数，样例如example: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15], "MJ comeback", 1603152000, "MJC", "0x007bEe82BDd9e866b2bd114780a47f2261C684E3"]
* **getDecimals()**:
返回代币的小数点后位数值；
* **trade(uint256 expiry,uint256[] tokenIndices,uint8 v,bytes32 r,bytes32 s)**:
交易函数，例如：0, [3, 4], 27, "0x2C011885E2D8FF02F813A4CB83EC51E1BFD5A7848B3B3400AE746FB08ADCFBFB", "0x21E80BAD65535DA1D692B4CEE3E740CD3282CCDC0174D4CF1E2F70483A6F4EB2" 
* **encodeMessage(bytes12 prefix, uint value,uint expiry, uint256[] tokenIndices)**:
* **name()**:
返回代币名称；
* **symbol()**:
返回代币表示；
* **getAmountTransferred()**:
返回已传输的数量；
* **isContractExpired()**：
合约是否过期；
* **balanceOf(address _owner)**：
返回账户余额；
* **myBalance()**：
* **transfer(address _to, uint256[] tokenIndices)**：
资产转账；
* **transferFrom(address _from, address _to, uint256[] tokenIndices)**：
* **endContract()**：
结束合约；
* **getContractAddress()**：
获取合约地址；

#4.3 ERC998
* **状态**：
草稿（Draft）
* **提交记录**：
https://github.com/ethereum/EIPs/issues/998
* **标准说明**：
https://github.com/ethereum/EIPs/blob/master/EIPS/eip-998.md

* **推荐样例**：
https://github.com/mattlockyer/composables-998

不同的ERC代币可以被兼容的ERC-998代币既代表一组相似资产（ERC-20代币），一种独特资产的分类（ERC-721代币），或者内嵌于单比交易的混合体。
这个协议是以不可以替代代币为基础的全新复杂型数字资产。一个ERC-998代币可以作为一个数字资产投资组合。

**中心资产和辅助资产的结合**
第一个使用案例是可以和不同数字资产和零件结合外加附加增值功能的中心NFT币。增值可以通过个人花费时间和精力，或者购买来实现。
![](https://upload-images.jianshu.io/upload_images/1190574-5cfd2860eda91014.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在奢侈品行业里，这些可以作为应用于不同时尚品牌的“外表”和“包装”。 比如说，中心的不可替代货币是外表，可以辅助鞋子和钱包，每一个都代表了他们自己的NFT。这一整个套装都作为一部分组合到ERC-998代币里面。
**单一资产的几个组成部分**
作为附加债券的一部分，单一的数字资产可以被ERC-998代币群组所代表。因为每一个部分都是由自己的NFT被独特的体现出来的，NFT代币组合可以保证货品的绝对真实性。然而，除非依附于实际商品的形式，每一个NFT都没有自己的价值。这是一个可以附加在类似手表，珠宝这样价值商品上的非常强势的防伪手段。 
![](https://upload-images.jianshu.io/upload_images/1190574-503dd2519d966dd3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
上图所示为劳力士的三个组件，每个组件都有一个单独的序列号 - 表壳、表面和指针。单独来看，这些部件几乎没有任何价值，但它们一起构成了一个真正的劳力士手表，由一个ERC-998代币所代表。
**分组集合**
通常被认为是组合的任何东西，例如一副牌，一本历史邮票或罕见的硬币收集等，可以在一个组合型代币下结合在一起。因此，完整的组合可以由单个数字资产表示。
大多数奢侈品牌都有很大众的产品，每年都会使用重新设计的型号，这些产品往往成为收藏品。在奢侈品战略下，消费者购买经典和品牌，这些都是通过多年来产品的演变出来的。作为上图中的示例，该品牌在三年内发布了其第一、二、三代模型，这些模型被分组为一个组合代币。
同时，这也更为品牌加强与老客户的联系。例如，如果用户可以通过Smart-Links这样可组合的代币，显示他们对某个品牌的所有收藏品，那么该品牌将能够通过独家邀请和优惠来奖励这位客户。


#4.4 ERC1155
* **状态**：
草稿（Draft）
* **提交记录**：
https://github.com/ethereum/EIPs/issues/1155
* **标准说明**：
https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1155.md

* **推荐样例**：
* https://enjincoin.io/

ERC-1155是一种定义token的新标准，1155是这种方法的编号。1155标准定义了一种解决上述问题的新方法。现在『物品』（可能包含ERC20的token或ERC721的token或两者都有）可以被单一的一个合约（打包处理）来定义了。合约里包含区别token们所需的最小量的数据。合约的状态包含了每个token ID的配置信息和管理收集的所有行为。ERC-1155的灵活性更强，它使得开发者可以自行选择是批量生成某一种特定的token，还是构建不可被复制的惟一元数据。
ERC-1155支持原子交换，“原子交换”是指不通过中介物而将一种token换成为另外一种token。
ERC-1155的最大进步就是可以融合不同token（可能是“可替换”的代币与“不可替换”的代币的混合体）进行“打包处理”。

函数接口说明：
* **name(uint256 _itemId)**：

* **symbol(uint256 _itemId)**：

* **decimals(uint256 _itemId)**：

* **totalSupply(uint256 _itemId)**：

* **balanceOf(uint256 _itemId, address _owner)**：

* **allowance(uint256 _itemId, address _owner, address _spender)**：


* **transfer(uint256[] _itemId, address[] _to, uint256[] _value)**：

* **transferFrom(uint256[] _itemId, address[] _from, address[] _to, uint256[] _value)**：

* **approve(uint256[] _itemId, address[] _spender, uint256[] _value)**：

* **increaseApproval(uint256[] _itemId, address[] _spender, uint256[] _addedValue)**：

* **decreaseApproval(uint256[] _itemId, address[] _spender, uint256[] _subtractedValue)**：

* **ownerOf(uint256 _itemId)**：

* **itemURI(uint256 _itemId)**：

* **itemByIndex(uint256 _itemId, uint256 _index)**：

* **itemOfOwnerByIndex(uint256 _itemId, address _owner, uint256 _index)**：


#5，ERC865
* **状态**：
还处于pull request下（issue）
* **提交记录**：
https://github.com/ethereum/EIPs/issues/865
* **标准说明**：

* **推荐样例**：
https://github.com/lavawallet/lava-wallet

以太坊开发者Bhaskar Sharma在Medium网站上发表了一篇文章，提出了一个在以太坊中使用对用户更友好的代币的提案，这一提案将允许用户来采用去中心化应用程序（DApps）时使用代币（tokens）来支付交易费用，而不是用以太币ETH来支付。
他认为，以太坊区块链中的费用支付问题为新手带来了麻烦，这些费用用于支付矿工将交易打包到区块并确保区块链的安全。用户必须认识以太坊的工作原理，这样才能理解燃料（gas）价格和成本。用户还需要获得必要的以太币来支付这两种费用。
函数接口说明：
* **transferPreSigned(bytes _signature, address _to, uint256 _value, uint256 _gasPrice, uint256 _nonce)**：

* **approvePreSigned(bytes _signature, address _spender, uint256 _value, uint256 _gasPrice, uint256 _nonce)**：

* **approveAndCallPreSigned(bytes _signature, address _spender, uint256 _value, bytes _extraData, uint256 _gasPrice, uint256 _nonce) **：

* **increaseApprovalPreSigned(bytes _signature, address _to, uint256 _amount, uint256 _gasPrice, uint256 _nonce)**：

* **decreaseApprovalPreSigned(bytes _signature, address _to, uint256 _amount, uint256 _gasPrice, uint256 _nonce)**：

* **revokeSignature(bytes _signature)**：

#6. 其他ERC协议
#6.1 ERC165
* **状态**：
已定稿（Final）
* **提交记录**：
https://github.com/ethereum/EIPs/issues/165
* **标准说明**：
https://github.com/ethereum/EIPs/blob/master/EIPS/eip-165.md

ERC165定义以下功能：
1） 如何识别接口；
2） 一个智能合约如何发布它执行的接口；
3） 如何检测一个智能合约是否执行了ERC-165协议；
4） 如何检测一个智能合约是否执行了一个给定的接口；

#7，参考
（1）[以太坊官网ERC列表](https://github.com/ethereum/EIPs/tree/master/EIPS)
（2）[以太坊官网EIP提出列表](https://github.com/ethereum/EIPs/issues)
***
**知识对接服务:**
辉哥和欧阳哥哥在知识星球开通了区块链入门专栏，用于存放简书区块链入门专栏文章的工程源码等内容，并建立专项微信群用于技术交流，欢迎加入。

![](http://upload-images.jianshu.io/upload_images/1190574-d8c733deb0ffb69b?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

