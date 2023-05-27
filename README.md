# LP-pool-monitoring-robot
Monitor LP creation action

_在本项目中，我将记录学习制作区块链监控机器人的过程_

## 1 工具
### 1.1 [Infura工具](https://www.infura.io/zh)
Infura 是一个以太坊的基础设施提供商，它为开发者提供了访问以太坊网络的简单和可靠的方式。以下是 Infura 的一些主要用途和功能：

  以太坊节点托管：Infura 提供了托管的以太坊节点，开发者可以通过 Infura 连接到以太坊网络，而无需自行搭建和维护节点。这样，开发者可以省去节点同步和运维的复杂性，直接使用 Infura 提供的接口进行开发。

  以太坊 API 提供商：Infura 提供了简单易用的 API 接口，开发者可以通过 API 访问以太坊网络，执行各种操作，如查询区块链数据、发送交易、获取合约事件等。这使得开发者可以直接与以太坊进行交互，构建以太坊应用程序或集成以太坊功能到自己的应用中。

  无需本地节点：使用 Infura，开发者无需自行运行和同步以太坊节点。他们可以通过 Infura 提供的节点访问以太坊网络，从而在开发和测试阶段快速进行应用程序的迭代和调试。

  多网络支持：Infura 支持多个以太坊网络，包括主网（Mainnet）和各种测试网络（如Rinkeby、Ropsten、Kovan等）。这使得开发者可以在不同的环境中测试和部署他们的应用程序。

  可靠性和稳定性：Infura 提供高度可靠和稳定的基础设施，具备高可用性和弹性扩展性，以确保开发者的应用能够在以太坊网络上持续运行。
  
### 1.2 [Web3.py库](https://web3py.readthedocs.io/en/stable/)
当涉及到与以太坊区块链进行交互的 Python 开发时，Web3.py 是一个常用且功能强大的库。Web3.py 提供了一个用于与以太坊节点通信的接口，使开发者能够构建基于以太坊的应用程序和工具。

以下是 Web3.py 库的一些主要特点和功能：

与以太坊节点通信：Web3.py 提供了与以太坊节点进行通信的功能，包括连接到以太坊节点、发送交易、调用智能合约等。

智能合约交互：Web3.py 提供了对智能合约的本地和远程调用支持。你可以使用智能合约的 ABI（Application Binary Interface）来与智能合约进行交互，包括调用函数、获取状态和事件等。

交易管理：通过 Web3.py，你可以创建、签名和发送以太坊交易。你可以设置交易的各种参数，如发送地址、接收地址、转账金额等。

事件过滤：Web3.py 允许你订阅和过滤以太坊网络中的事件。你可以设置过滤器以监听特定事件的发生，并在事件发生时执行相应的操作。

以太坊网络管理：Web3.py 提供了一些与以太坊网络和区块链状态相关的功能，如获取块信息、查询交易状态、获取 gas 价格等。

助记词和私钥管理：Web3.py 支持生成助记词、派生私钥以及对私钥进行签名和验证等操作。

## 2 流程
可以使用区块链数据源和智能合约事件监听来监视 Uniswap 上新创建的超过一定金额的池子。以下是一种可能的实现方法：

1、选择一个适合的区块链数据源：你可以使用以太坊的公共区块链浏览器或者专门的区块链数据服务作为你的数据源。一些常用的数据源包括 Etherscan、Infura、Alchemy等。

2、订阅事件：使用所选的数据源，你可以订阅 Uniswap 智能合约的事件。具体来说，你可以订阅 PairCreated 事件，该事件在新的交易对创建时触发。

3、过滤事件：在接收到 PairCreated 事件后，你可以检查交易对的初始资金池规模。你可以获取交易对的合约地址，并使用合约调用方法获取资金池的初始资金量。

4、检查资金池规模：根据你的要求，你可以设置一个金额阈值，例如超过一定数量的代币或以太币。如果资金池的规模超过了你设定的阈值，你可以将该交易对标记为满足监视条件。

5、通知或记录：一旦发现满足监视条件的交易对，你可以选择将结果通知给你的监视 bot用户，或者记录下来以后进行分析或处理。

需要注意的是，上述方法仅提供了一种基本的框架，实际实现中可能需要进行一些定制和调整，具体取决于你使用的数据源和开发工具。另外，对于大规模的监视和实时处理，你可能需要考虑性能和可扩展性方面的问题。

## 3 代码

### 3.1 订阅事件
```python
from web3 import Web3

# 连接到以太坊网络
web3 = Web3(Web3.HTTPProvider('https://rinkeby.infura.io/v3/your-infura-project-id'))

# 合约地址和ABI
contract_address = '0x...'  # Uniswap合约地址
contract_abi = [...]  # Uniswap合约ABI

# 创建合约实例
contract = web3.eth.contract(address=contract_address, abi=contract_abi)

def handle_event(event):
    # 处理PairCreated事件
    print(event)

# 订阅PairCreated事件
event_filter = contract.events.PairCreated.createFilter(fromBlock='latest')

while True:
    for event in event_filter.get_new_entries():
        handle_event(event)
```


