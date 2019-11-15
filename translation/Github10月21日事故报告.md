# Github10月21日事故报告

## **前言**

国际标准时间下午10:52，大约是国内7点多的时候，Github网站出现了问题，网上描述为：无法登录，无法创建或删除，无法创建issue等等。我下午登录后显示的页面则是502错误提示页面。

## **官方报告**

- 原文

> At 10:52 pm Sunday UTC, multiple services on [GitHub.com](http://GitHub.com) were affected by a network partition and subsequent database failure resulting in inconsistent information being presented on our website. Out of an abundance of caution we have taken steps to ensure the integrity of your data, including pausing webhook events and other internal processing systems.

> We are aware of how important our services are to your development workflows and are actively working to establish an estimated timeframe for full recovery. We will share this information with you as soon as it is available. During this time, information displayed on [GitHub.com](http://GitHub.com) is likely to appear out of date; however no data was lost. Once service is fully restored, everything should appear as expected. Further, this incident only impacted website metadata stored in our MySQL databases, such as issues and pull requests. Git repository data remains unaffected and has been available throughout the incident.

> We will continue to provide updates and an estimated time to resolution via our status page.

- 译文

> 国际标准时间下午10:52，Github网站的多个服务因为网络分区和备份数据库和网站所呈现出的信息不一致导致异常。出于谨慎我们必须采取措施去确保数据的完整性，包括暂停网络连接以及关闭一些其他的内部信息处理系统。

> 我们知道我们的服务对于开发者有多么重要,所以我们积极地建立服务完全恢复的时间表并将有用信息及时地反馈给大家。在此期间，网站上显示的信息可能会是过期信息；不管怎样数据并没有丢失。一旦服务恢复则所有信息都会像往常一样呈现。详细地说,这次事故只对网站MySQL数据库内存储的相关的元数据产生影响，例如issues和pull requests。而Git仓库实体数据则不受影响并且始终处于可用状态。

> 我们将继续通过状态页面提供更新和估计解决时间

## **说明**

从官方给的报告来看，其实就是数据库服务这一块出现问题，所以人为将相关服务断开网络连接；导致我们无法对数据进行操作，但是又能对Github仓库的文件系统进行同步操作。下图是Github给出的状态信息说明，我们可以从中看到，北京时间晚上20点左右故障已经基本移除，并在一小时后所有数据存储恢复正常。

## **扩展**

当一个大型网站遇到问题时的做法

以前很少关注哪个网站挂了或者怎样，因为以前都是公司自己购买服务器然后自己组建数据中心，当系统出现问题时，只需要启动备案计划即可；但是云的广泛化，带来的是IT基础设施的服务化，当我们组建系统时，都会默认云服务不会挂，即云服务具备可靠性，所以当我们的基础设施真的挂了后，我们会手足无措，并且没有能力去处理，毕竟所有数据都在云服务公司那边。而云服务出现故障后，带来的是无数网站不可用，甚至数据丢失。

例如2018年6月27日阿里云出现的[故障](https://bbs.aliyun.com/read/581590.html?spm=5176.10695662.1996646101.searchclickresult.711b46f42U3bhk)：

所以这里面涉及到两个**问题**：

- **是否应该使用云服务**

- **如何降低云服务带来的风险**

  > 在我看来，云是趋势，我们只能调整方向，但是不能逆着趋势，因为逆着趋势阻力太大了。所以我们应该通过云服务降低自己的运维成本，但是本着“鸡蛋不能放在同一个篮子里”的原则，我们还应该与多个云服务厂商建立联系，毕竟云服务不间断地出现问题这是真，而两个云服务同时出现故障则基本没有。