## 物联网平台概览 - Amazon, Microsoft, IBM IoT 解决方案概述
  
>最近研究了一些物联网平台技术资料，以做选型参考。脑子里积累大量信息，便想写出来做一些普及。作为科普文章，力争通俗易懂，不确保概念严谨性。我会给考据癖者提供相关英文链接，以便深入研究。  
>>>>        —— 冯立超 HiwebFrank

亚马逊、微软、IBM 等云计算厂商都在布局物联网。作为平台厂商，他们各自基于自己的云计算大数据平台，提出一套完善的物联网体系构架和构建与开发工具。  

作为物联网整体构架，可以简要描述为：将设备联入云平台；存储设备数据；进行设备信息管理；设备状态数据监控及对设备进行控制、管理、运维；数据分析与展示；提供多种业务功能。

本系列文章将从如下几方面分别描述：  
1. 概览  
2. 设备  
3. 连接  
4. 平台  
5. 安全  
6. 案例与参考文档  

## 1. 概  览
亚马逊、微软、IBM等都给出了物联网解决方案概念构架，下面暂以亚马逊物联网解决方案概念构架为例，做简要说明。  
亚马逊给出的物联网解决方案体系构架图如下：  
![亚马逊物联网解决方按构架](https://d0.awsstatic.com/IoT/assets/awsiot_how_it_works_diagram.png)    
  
其大致描述如下：  
>设备利用本身的功能或软件开发包SDK进行定制开发，连接到物联网系统中；为确保安全，设备需要验证、授权、注册等措施；一些不能直接接入的设备则需要通过设备网关接入；在云平台中，通过设备状态数据缓存机制，保存设备最新状态等信息，从而应用程序或其他设备可以读取设备消息并与设备交互；通过规则引擎，构建物联网应用程序，这些程序将收集、处理、分析设备数据并执行操作；同时，通过大数据分析，提供业务支持与决策。而各类数据处理，则通过云平台的各种计算服务、存储服务得以实现。  
  
    
## 2.  设 备
任何可以连接到网络的物体，如温度传感器、火灾监测设备、发动机、手环、汽车、钻井、机器人、火星车、小猫小狗、冰箱空调洗碗机等，即所谓 物联网 的“物”，Internet of Things 的“Things”。  
  
设备可以很简单，也可以很复杂。对于简单设备，可能不能直接联入互联网，则需要通过设备网关连接；对于复杂智能设备，则可以通过物联网操作系统（以前称嵌入式操作系统）进行深度开发管理。  

目前各厂商都推出自己的设备端系统，如亚马逊的 [Greengrass](https://aws.amazon.com/greengrass/)，微软的 [Windows 10 IoT](https://www.microsoft.com/en-us/windowsforbusiness/windows-iot)，华为的 [LiteOS](http://www.huawei.com/minisite/liteos/en/index.html) 等。这些系统使得智能设备有了强大的本地计算能力和安全性。  

根据场景不同，各家对设备又有一些分类。微软 Windows IoT Core 针对有头和无头设备（就是有没有显示器）有[不同的内存要求](https://developer.microsoft.com/en-us/windows/iot/Explore/IoTCore)。IBM 把设备分为[可管理设备和不可管理设备](https://console.ng.bluemix.net/docs/services/IoT/iotplatform_overview.html#about_iotplatform)，即是否可安装管理代理以对设备进行管控。

作为跃跃欲试的技术狂热者，搭建测试环境进行学习，什么软件/平台都好说，没设备就会一筹莫展。  

为了让开发者尽快掌握相关技术、搭建测试环境，各家也是想尽了办法。各自给出了软件模拟设备，微软Azure IoT案例中的一堆模拟温湿度计，IBM 红点模拟器 [Node-RED device simulator](https://console.ng.bluemix.net/catalog/starters/node-red-starter/)（永远的红点，永远的IBM）  

对于微软平台，用Windows计算机即可，但总感觉不像真的。所以最新的树莓派3可以安装 Windows 10 Core，然后进行各种操作，[具体点击这里](https://developer.microsoft.com/en-us/windows/iot/explore/deviceoptions)。  
亚马逊给出的最简单设备是[亚马逊按钮](https://aws.amazon.com/iotbutton/)（记不记得此前吵了一段的你买个洗衣液送你个按钮，下次没洗衣液了就把这个按钮按一下就自动下单了的新闻），以及[一堆物联网设备](https://aws.amazon.com/iot-platform/getting-started/#kits)。  
关于设备，有很多细节值得探讨，不再赘述。  

## 3. 连 接
从物理连接的角度，有大量的底层技术，包括网线、WiFi、GPRS、3G / 4G / 4.5G、Bluetooth、Zigbee、RFID 以及正吵的 NB-IoT、LiTRA 等等，此不赘述。  

对于协议，一般都主流支持 MQTT, HTTP, WebSockets。具体细节可点击：  
>- [MQTT](http://www.mqtt.org/) ;   
>- [亚马逊协议清单](http://docs.aws.amazon.com/iot/latest/developerguide/protocols.html)；
>- [微软协议网关](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-protocol-gateway)；
>- [IBM MQTT详述](https://console.ng.bluemix.net/docs/services/IoT/reference/mqtt/index.html#ref-mqtt)

由于物联网各种设备所处环境复杂，如室内、井下、隧道、甚至月球火星，很难保证链接的可靠性和持续性，这对应用系统的开发和使用带来挑战。  
于是，大家想出来一个办法（我估计是亚马逊先想出来的，因为我喜欢亚马逊方案的完备性和文档的完整易读性（什么逻辑）），就是在云平台上把设备最新的状态数据缓存起来，设备和缓存交互，而应用程序只和这个缓存的数据打交道。这样，应用程序就可以假设设备是永远在线的了。  

这个缓存数据，其实就是一个JSON文件，而亚马逊给其取了一个好听的名字：设备影子 [Device Shadow](https://aws.amazon.com/iot-platform/how-it-works/#shadows)；  
微软嘛，好吧，你叫 Shadow，我就另想一个名字吧，嗯——，设备孪生 [Device Twins](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-device-twins) ，（亏你想的出来）  
IBM西装革履职业一些，老老实实，就叫 设备最后事件缓存 [Device Last Event Cache](https://console.ng.bluemix.net/docs/services/IoT/feature_overview.html#feature_overview) 。  

这是一个很好的思路，这个影子孪生缓存设备，可以使应用系统更加高效、设置比设备更多的元数据及属性、预置设备状态、处理长时间工作流业务等等。  

对于物联网设备连接到云端，需要解决很多问题，包括设备到云/云到设备的通信，如消息传送、文件传输、请求响应方法；消息路由；设备元数据存储检索及设备状态信息同步；通信安全与访问控制；设备连接性监控及设备标识管理等等。各家都有自己的解决方案。  

微软比较清晰地提出一个专门的服务：[IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-what-is-iot-hub)，对海量物联网设备与云端解决放案之间提供可靠、安全的双向通信。  

微软给出的 IoT Hub 概念示意图如下，供参考：  

![Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/media/iot-hub-what-is-iot-hub/hubarchitecture.png)  


    
## 4. 平 台

### 4.1 平台概述
由于物联网的地域分布广、设备数量众多的特点，物联网解决方案必须借助公有云平台来实现。  
物联网解决方案须具备如下功能：  
>- 从设备收集数据  
>- 分析运行中的数据流  
>- 存储和查询大型数据集  
>- 实时和历史数据可视化  
>- 与后端业务系统集成  
>- 管理设备  

我们用一个简单的图示来说明物联网解决方案构架，下图是微软给出的一个与厂商无关的[物联网解决方案通用示意构架](https://docs.microsoft.com/en-us/azure/iot-suite/iot-suite-what-is-azure-iot)：  

![Generic IoT solution architecture](https://docs.microsoft.com/en-us/azure/includes/media/iot-azure-and-iot/iot-reference-architecture.png)  


该简化构架包括 **设备连接**、**数据处理与分析**、**信息呈现** 三个层面。  

该构架中，物联网设备收集数据并与云网关交互；云网关使其它后端服务可以处理这些数据，从云网关，数据提交给其它业务应用程序或仪表板操作人员及其它展示设备。  

#### 4.1.1 设备连接
如何让设备安全可靠地连接到解决方案后端，是物联网解决方案所面临的巨大挑战，相比于其它系统，物联网设备有如下一些特点：  
>- 通常是无人操作的嵌入式系统甚至是没有操作系统的设备  
>- 可能部署到物理访问成本高昂的远程位置等各种部署场景  
>- 可能无法通过其他方式来与设备交互，而只能通过解决方案后端来访问  
>- 供电及运算资源可能都有限  
>- 网络连接可能不稳定、缓慢或高成本  
>- 可能需要使用专属、自定义或行业特定的应用层协议  
>- 可以使用大量常见的硬件和软件平台来创建  

除上述特点之外，物联网解决方案还必须考虑可扩展性、安全性和可靠性。传统的技术如Web容器或消息传送代理等不足以支撑这样的需求。  

为此，物联网平台厂商都提出自己的解决方法，如微软的 Azure IoT Hub 及 Azure IoT SDK 等。关于 Azure IoT Hub 请参见上一讲相关内容。  

有关设备连接及配置，微软给出了非常详细的教程，包括使用模拟设备、使用模拟网关、使用物理设备等，并针对使用 C、Node.js、Python 利用树莓派搭建环境给出详细的教程，具体可参见[Connect Raspberry Pi to Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started)  
以下是该教程中的截图：   
  
<img src="https://docs.microsoft.com/en-us/azure/iot-hub/media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg" width="500" />
 
#### 4.1.2 数据处理与分析
在物联网解决方案中，数据处理与分析主要交由在公有云上的后端服务进行，包括设备数据筛选、汇总、路由到其他服务等等。后端服务主要负责如下工作：  
>- 接收来自设备的大规模数据，并确定如何处理和存储这些数据  
>- 必要时可以从云端向设备发送数据或指令  
>- 设备注册、预配置及安全连接控制  
>- 跟踪设备状态并监控设备活动  
>- 存储和分析设备历史数据，从而实现设备预见性维护  
>- 与设备进行交互，实现反馈控制等  

当然，并不是所有的业务都必须交由云端处理，例如一些必须及时响应的操作比如紧急刹车，这些处理及操作都必须在设备端直接进行；另外，设备也可以进行一些预处理，从而提高效率、降低数据传输量。  

为此，设备本身应该具备一些处理能力。  

微软的 Windows 10 IoT Core，提供了强大的设备端计算能力；  

而亚马逊的 Greengrass，则使物联网设备可以运行 AWS Lambda 函数、同步设备数据以及与其他设备安全通信，甚至无需连接互联网。Greengrass 可确保物联网设备快速响应本地事件、运行时采用间歇性连接，并最大程度地降低将物联网数据传输到云的成本。  

华为的 LiteOS，则强调轻量级、低功耗（一个纽扣电池用好几年）、快速启动（毫秒级）及响应（纳秒级）、多种连接协议（广域/局域）等特性。  

另外，一些物联网设备，也可以通过完全自主研发的芯片，实现相关数据采集与处理。  
比如图像火灾监测设备，可以将图像火灾分析算法集成在设备芯片中，由设备芯片中的算法进行判断，仅将是否发生火灾的判定结果传输到云端。  
而对于较复杂的图像、或者误判图像，则将图像数据及其设备端判定结果传到云端，通过机器学习，逐步改进算法，提高图像处理能力，并通过物联网对设备进行升级操作。  

#### 4.1.3 信息呈现和业务连接
信息呈现和业务连接层 用于展示和操控从设备收集的数据。它可让用户查看和分析从其设备收集的数据。 这些视图可以采用仪表板或 BI 报表的格式，以显示历史数据和/或接近实时的数据。  

此层还可实现物联网解决方案与现有业务应用程序的集成，以连接企业业务流程或工作流。 例如，图像火灾监测系统，在发现监测设备故障信息后，通过与维护服务商现有的运维计划系统集成，可以预约工程师到现场进行检查。  

下图是微软提供的一个物联网工厂的应用界面样例。  
在该界面中，仪表板左侧展示了生产线细节信息。如仪表板左侧第一条，提示机械臂严重警告，而在仪表板中部的模拟界面中，同样用红色标出机械臂警告位置。在右侧的报警栏，列出不同时间点的警告信息，如机械臂温度警告等。而在仪表板下方，则展示该条生产线的整体效率信息。  

![MSConnectedFactory](http://azureiotclicks01.azureedge.net/demos/scenario3.task3/03.gif?b=1.0.1.10)      


### 4.2 微软物联网解决方案构架

下图是微软物联网解决方案的[较详细的构架图](http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf)：  

![MicrosoftIoTArch](https://docs.microsoft.com/en-us/azure/includes/media/iot-security-architecture/iot-security-architecture-fig2.png)  

微软的物联网解决方案平台，通过微软的公有云 Azure 实现，即 微软物联网套件 Azure IoT Suite。设备通过 Azure IoT Hub 注册和接入，然后可以使用微软公有云 Azure 的各种强大的数据处理、存储、分析、机器学习能力，构建所需的各类物联网业务。  

微软物联网解决方案通常至少会使用到如下服务：  
>- [Azure IoT Hub](https://azure.microsoft.com/documentation/services/iot-hub/)： 该服务提供设备到云和云到设备的消息传送功能，并充当云和其他主要 IoT 套件服务的网关。 该服务使得可以从大量设备接收消息，并将命令发送给设备。 使用该服务，还能够管理设备，例如，可以配置、重启连接到 Azure IoT Hub 的设备，或对其执行恢复出厂设置操作。  
>- [Azure Stream Anzlytics](https://azure.microsoft.com/documentation/services/stream-analytics/)： 流分析提供运行数据分析。 该服务处理传入遥测数据、执行聚合以及检测事件。 以及处理包含元数据或来自设备的命令响应的信息消息。 解决方案使用流分析来处理设备消息，并将这些消息传送给其他服务。  
>- [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) 和 [Azure DocumentDB](https://azure.microsoft.com/documentation/services/documentdb/)： Azure 存储和 Azure DocumentDB 提供数据存储功能。 解决方案使用 Blob Storage 来存储设备遥测数据并使其可用于分析；使用 DocumentDB 来存储设备元数据，以及启用解决方案的设备管理功能。  
>- [Azure Web App](https://azure.microsoft.com/documentation/services/app-service/web/) 和 [Microsoft Power BI](https://powerbi.microsoft.com/)： 提供数据可视化功能。 借助 Power BI 的灵活性，可以快速生成针对具体业务要求的交互式仪表板。  


微软还给出了一些预配置解决方案和相关演示案例，有兴趣的读者可以点击如下链接，通过实际操作了解上面截图中的工厂物联网使用场景：<http://www.microsoftazureiotsuite.com/demos/connectedfactory> 。  

### 4.3 亚马逊物联网解决方案构架
而[亚马逊给出的构架](http://docs.aws.amazon.com/iot/latest/developerguide/aws-iot-how-it-works.html)如下图所示：  
![How AWS IoT Works](http://docs.aws.amazon.com/iot/latest/developerguide/images/aws_iot_data_services.png)    
亚马逊物联网解决方案当然强调其公有云平台 AWS，而最受其推崇的，自然是当下正热的 Serverless 构架的 AWS Lambda。  
亚马逊物联网解决方案至少用到如下 AWS 服务：  
>- [Amazon Simple Storage Service](https://aws.amazon.com/s3/)：亚马逊简单存储服务，简称 Amazon S3,提供可扩展的存储服务。
>- [Amazon DynamoDB](https://aws.amazon.com/dynamodb/)：提供受管理的 NoSQL 数据库服务。
>- [Amazon Kinesis](https://aws.amazon.com/kinesis/)：提供大规模流数据实时处理能力。
>- [AWS Lambda](https://aws.amazon.com/lambda/)：在亚马逊公有云平台发布运行自定义代码。  
>    ZStack创始人张鑫的文章[《Serverless,后端小程序的未来》](http://geek.csdn.net/news/detail/195964) 比较透彻地讲解了 AWS Lambda 及 Serverless 的原理、构架、应用场景、优势和不足，并讲述了Serverless与PaaS的区别、与容器技术的区别及联系等，推荐一读。
>- [Amazon Simple Notification Service](https://aws.amazon.com/sns/)：亚马逊简单通知服务，简称 Amazon SNS，负责处理通知发送与接收。
>- [Amazon Simple Queue Service](https://aws.amazon.com/sqs/)：亚马逊简单队列服务，简称 Amazon SQS，将数据存储到队列，由应用程序获取。

    （看看人家，做那么大的生意，本星球/本星系第一啊，啥都谦称为 Simple 简单啥啥啥，另外，包括现在 IT 运维领域最重要的 SNMP 简单网络管理协议、电子邮件的 SMTP 简单邮件传输协议......；看看我们，动辄 皇家、擎天、至尊，汗颜啊！惭愧啊！）

### 4.4 IBM 物联网解决方案构架
IBM 的物联网解决方案名为 [Watson IoT Platform](https://www.ibm.com/internet-of-things/platform/watson-iot-platform/)，（永远的沃森）。其构架图如下所示：  
![Watson IoT Platform](https://s1.51cto.com/wyfs02/M02/94/F6/wKioL1kQLkDiCexMAADU7H1wLqA301.png)   

IBM 物联网解决方案基于 IBM 公有云平台 Bluemix,涉及到的服务至少包括：
>- [IBM Cloudant NoSQL DB](https://console.ng.bluemix.net/docs/services/IoT/cloudant_connector.html#cloudant_main)：用于存储和访问设备数据。
>- [IBM Message Hub](https://console.ng.bluemix.net/docs/services/MessageHub/index.html)：为实时数据提供低延迟、可扩展的、高吞吐量的消息总线。
>- [IBM Blockchain 集成](https://console.ng.bluemix.net/docs/services/IoT/bl_blockchain_integration.html#gettingstartedtemplate)：对于特定领域，可以集成 IBM 区块链服务，以符合特定的商业规则要求。
>- [仪表板](https://console.ng.bluemix.net/docs/services/IoT/data_visualization.html#boards_and_cards)：利用 Bulemix 提供的数据可视化仪表板，可以可视化展示设备状态数据，提供 BI 功能。

IBM给出了一个简单教程，可以快速搭建物联网测试环境，可以参见<https://console.ng.bluemix.net/docs/services/IoT/getting_started/quickstart/index.html#quickstart>   


## 5. 安 全

### 5.1 物联网安全通述
    我想用遥控器把隔壁邻居家的电视给关了！
    不知是否可以，但至少，我拿着一个空调遥控器可以到各个房间开关空调。

如果物联网设备没有任何安全措施，那么状况将无法设想。  

如何做到全方位安全？  

微软早在2003年就提出了可信赖的计算 Trustworthy Computing 和基于 STRIDE 网络安全风险模型的纵深防御 Defence in Depth 理念。  
这些理念并不是针对微软产品及其解决方案的，而是被业界充分认可的通用的理念。  
我们将根据这些理念与模型，为大家展开讨论物联网安全。  

微软基于上述网络安全模型与理念，给出了通用的 物联网安全构架 和 物联网安全最佳实践。具体信息请参见：
>- [物联网安全架构](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-security-architecture)
>- [物联网安全最佳实践](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-security-best-practices)

在上述资料中，微软对物联网的物理安全层面进行了细分，将其分为 设备、现场网关（Field Gateway）、云网关 及 服务等多个分区Zone。指出各区域都是分离的信任边界，每个分区都应有自己的数据安全、验证、授权机制。  

在这些分区之间数据与信息的传输都应考虑 [STRIDE]https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) 各种风险，即：
>- Spoofing   欺骗
>- Tampering  篡改
>- Repudiation 抵赖
>- Information disclosure  信息泄露
>- Denial of service  拒绝服务
>- Elevation of privilege  特权提升

微软给出的物联网安全构架示意图如下所示：图中红色虚线框为不同的分区Zone，红色虚弧线则为信任边界。  

![MicrosoftIoTTrustBoundary](https://docs.microsoft.com/en-us/azure/includes/media/iot-security-architecture/iot-security-architecture-fig1.png)  

下图是用微软威胁建模工具所建立的数据流原理模型：  

![Data Flow Diagram model](https://docs.microsoft.com/en-us/azure/includes/media/iot-security-architecture/iot-security-architecture-fig3.png)  


关于上述模型的详细信息，原文在 [Internet of Things security architecture](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-security-architecture)，中文可以参见 [物联网安全体系结构](https://docs.microsoft.com/zh-cn/azure/iot-hub/iot-hub-security-architecture)，这篇文档好像是人翻译的:)，中文可读。（谢谢[Sunny Deng](https://github.com/SunnyDeng)和[v-cchen](https://github.com/v-cchen)）  

### 5.2 微软 Azure IoT Suite 物联网安全

具体到微软自己的 Azure IoT Suite 物联网部署构架，则主要包括如下三个安全领域：
>- 设备安全：在实际部署物联网设备时，保护设备安全
>- 连接安全：确保物联网设备和 Azure IoT Hub 之间数据传输的机密性和防篡改性
>- 云安全：确保数据在云中传输、移动和存储时的安全

下图给出微软 Azure IoT 物联网安全相关概念：  
![Securing the Azure IoT deployment](https://docs.microsoft.com/en-us/azure/includes/media/iot-secure-your-deployment/overview.png)  

#### - 设备预配与验证安全
>微软 Azure IoT Suite 使用两种方法确保设备安全：
>- 为每个设备提供唯一标识密钥（安全令牌），设备可使用该密钥与 IoT Hub 通信。
>- 使用设备内置 X.509 证书和私钥作为一种向 IoT Hub 验证设备的方式。 此身份验证方式可确保任何时候都无法在设备外部获知设备上的私钥，从而提供更高级别的安全性。

#### - IoT Hub 安全令牌
>IoT Hub 使用安全令牌对设备和服务进行身份验证，而不在网络上发送密钥。，安全令牌的有效期和范围有限。 Azure IoT SDK 无需任何特殊配置即可自动生成令牌。 但在某些情况下，需要用户生成并直接使用安全令牌。 包括 MQTT 、AMQP 或 HTTP 应用层协议的直接使用，以及令牌服务模式的实现。

#### - 连接安全
>使用传输层安全性 (TLS) 标准来保护 IoT 设备和 IoT 中心之间的 Internet 连接安全。 Azure IoT 支持 TLS 1.2、TLS 1.1 和 TLS 1.0

#### - 云安全
>Azure IoT Hub 为每个安全密钥定义访问控制策略。 它使用设备注册表读写、服务连接、设备连接等权限，向每个 IoT Hub 的终结点授予访问权限。
>另外，在云端的各种服务中，如 Azure DocumentDB、Azure Stream Analytics、Azure App Service、Logic App、Azure Blob Storage等服务，都应设置响应的安全权限配置。

### 5.3 亚马逊 AWS 物联网安全
亚马逊同样有一套完善的物联网安全方案。  
所连接的每台设备必须拥有凭证才能访问消息代理或物联网设备影子服务。对于往返 AWS 物联网的所有流量，都必须通过传输层安全性 (TLS) 进行加密。必须保证设备凭证的安全，以便安全地将数据发送到消息代理。数据到达消息代理后在 AWS IoT 和其他设备或 AWS 服务之间移动时，AWS 云安全机制可为数据提供保护。  

下图是亚马逊物联网安全与标识示意图：  
![AmazonIoTSec](http://docs.aws.amazon.com/iot/latest/developerguide/images/thunderball-overview.png)    

>- 我们需要管理设备上的设备凭证（X.509 凭证、AWS 凭证）及 AWS IoT 中的策略。将唯一身份分配给每台设备并管理设备或设备组的权限。
>- 设备将按照 AWS IoT 连接模式使用所选身份（X.509 证书、IAM 用户和组，或者 Amazon Cognito 身份）来建立安全连接。
>- AWS IoT 消息代理 针对账户内的所有操作执行身份验证和授权。消息代理负责对设备执行身份验证、安全地接收设备数据，以及支持通过策略授予设备的访问权限。
>- AWS IoT 规则引擎 根据所定义的规则将设备数据转发到其他设备和其他 AWS 服务。利用 AWS 访问管理系统将数据安全地传输到最终目标。

AWS IoT 消息代理和设备影子服务利用传输层安全 TLS 对所有通信进行加密。TLS 用于确保受 AWS IoT 支持的应用程序协议（MQTT、HTTP）的保密性。  
对于 MQTT，TLS 可将设备与代理之间的连接加密。AWS IoT 使用 TLS 客户端身份验证来识别设备。对于 HTTP，TLS 可将设备与代理之间的连接加密。身份验证工作委派给 AWS Signature Ver. 4 执行。  


### 5.4 IBM Watson IoT 物联网安全
同样，IBM Watson IoT 物联网解决方案也包含完备的安全方案。  
在 [IBM Watson IoT Platform Security](https://console.ng.bluemix.net/docs/services/IoT/reference/security/index.html#sec-index) 一文中， 首先强调了安全性的合规性、验证、授权、加密四各方面，并指出 Watson IoT Platform 以 Bluemix 为基础，而 Bluemix 基础构架的安全性与可靠性是 Watson IoT Platform 的基石。  

然后，IBM 像个学生一样老老实实认认真真地道出他的安全方案。  
![ISO27001](https://s1.51cto.com/wyfs02/M00/95/1F/wKiom1kRxqnQlzm-AABKcBCVlzY091.png)     

首先，掏出证书。指出 Watson IoT Platform 获得了 ISO 27001:2013 认证。  

然后，在 Watson IoT Platform 构架图的基础上，分别讲述了如下几个方面： 
>- 如何实现物联网信息管理安全
>- 如何实现设备与应用程序凭据安全
>- 如何实现设备连接安全
>- 如何防止设备数据在设备之间泄露
>- 如何防止数据在组织之间泄露  

上面每一方面，都有较详细的描述和示意图，此不一一列出，仅以下图设备与应用程序凭据安全为例供大家参考：  
![IBMIoTSec](https://s3.51cto.com/wyfs02/M01/95/1E/wKioL1kRyhbA1whzAACee_pR69Y511.png)  

另外，IBM 给出了安全风险管理建议，包括客户端证书、组织规划与安全策略、连接策略、黑白名单策略等。  

总之，物联网安全是一个重要的、同时也是复杂的问题。不仅仅包括物联网特定的设备与构架，还牵涉到网络安全及软件开发安全的各个方面。需要在构架设计、开发、部署、运维、应用的各个方面确保安全与合规。
    
## 6. 案例与参考文档

#### 亚马逊物联网相关文档及网址
>- [亚马逊 AWS IoT 官网](https://aws.amazon.com/iot-platform/)  
>- [亚马逊 Greengrass 官网](https://aws.amazon.com/greengrass/)  
>- [亚马逊 Greengrass Blog](https://aws.amazon.com/cn/blogs/aws/aws-greengrass-ubiquitous-real-world-computing/)    
>- [AWS IoT Developer Guide](http://docs.aws.amazon.com/iot/latest/developerguide/what-is-aws-iot.html) 
>- [AWS IoT Button](https://aws.amazon.com/iotbutton/)  

#### 微软物联网相关文档及网址
>- [微软物联网官网](https://www.microsoft.com/en-us/internet-of-things)
>- [Azure IoT Suite 官网](https://azure.microsoft.com/en-us/suites/iot-suite/)
>- [Microsoft Azure IoT Reference Architecture PDF](http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf) 
>- [IoT Suite Documentation](https://docs.microsoft.com/en-us/azure/iot-suite/)
>- [Microsoft Windows 10 IoT Core](https://developer.microsoft.com/en-us/windows/iot)

#### 微软演示案例
微软给出了几个较直观的演示案例，通过场景介绍及互动操作，可以了解不同场景的物联网应用：  
>- [物联网工厂](http://www.microsoftazureiotsuite.com/demos/connectedfactory)  
>- [远程监控](http://www.microsoftazureiotsuite.com/demos/remotemonitoring)  
>- [预测性维护](http://www.microsoftazureiotsuite.com/demos/predictivemaintenance)  

#### IBM 物联网相关网址
>- [IBM IoT 官网](https://www.ibm.com/internet-of-things/)
>- [IBM Watson IoT](https://www.ibm.com/internet-of-things/platform/watson-iot-platform/)
>- [IBM Bluemix Docs 之 Watson IoT Platform](https://console.ng.bluemix.net/docs/services/IoT/index.html)
>- [IBM IoT 动手演练 QuickStart](https://quickstart.internetofthings.ibmcloud.com)

## 总结
本文通过对亚马逊、微软、IBM 等公有云及物联网领域领先的解决方案的简要介绍与探讨，简单给出了物联网整体解决方案的框架，希望能让读者对物联网解决方案有一些宏观的了解。也对于我们选择国内公有云平台构建自己的物联网行业解决方案提供参考。  
鉴于个人经验有限及文章篇幅所束，错误偏颇之处在所难免，敬请诸位网友批评指正。
