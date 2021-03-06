---
title: 应用签名
date: 2021-05-20 11:19:01
tags:
    - 应用签名
categories:
    - 逆向
---


# 代码签名

代码签名是对`可执行文件或脚本`进行`数字签名`，用来确认软件在签名后未被修改或损坏的措施。和数字签名原理一样,只不过签名的数据是代码。

## 简单的代码签名

在iOS出来之前，以前的主流操作系统(Mac/Windows)软件随便从哪里下载都能运行，系统安全存在隐患，盗版软件、病毒入侵、静默安装等等，那么苹果希望解决这样的问题，要保证每一个安装到 iOS 上的 APP 都是经过苹果官方允许的，为了保证都是经过苹果官方允许的，就有了`代码签名`。

如果要实现验证，其实最简单的方式就是通过苹果官方生成`非对称加密的一对公私钥`,在iOS的系统中内置一个`公钥`，`私钥`由苹果后台保存，我们传APP到AppStore时，苹果后台用私钥对APP数据进行签名，iOS系统下载这个APP后，用公钥验证这个签名，若签名正确，这个APP肯定是由苹果后台认证的，并且没有被修改过，也就达到了苹果的需求：保证安装的每一个APP都是经过苹果官方允许的。

如果我们iOS设备安装APP只从App Store这一个入口这件事就简单解决了，没有任何复杂的东西，一个数字签名搞定。

但是实际上iOS安装APP还有其他渠道。比如对于我们开发者而言,我们是需要在开发APP时直接真机调试，而且苹果还开放了企业内部分发的渠道，企业证书签名的APP也是需要顺利安装的。

苹果需要开放这些方式安装APP,这些需求就无法通过简单的代码签名来办到了。


# 双重签名

为了解决上述的问题，苹果采用了双层签名。

首先我们需要两个设备，一个iOS系统的设备，一个Mac系统的设备。iOS开发是在Mac环境下进行的，这也就是双层签名的基础。

1. 在Mac上生成非对称加密算法的公钥、私钥。我们称之为公钥M、私钥M。M=Mac
2. 苹果自己有一套公钥、私钥，与简单签名中的逻辑一致，私钥在苹果后台，公钥在每一个iOS系统的设备中，称之为公钥A，私钥A，A=Apple
3. 公钥M打包开发者的信息，上传到苹果后台（通过CSR请求文件申请证书）。苹果后台用私钥A去签名公钥M，得到一份包含了公钥M的Hash值（签名）以及开发者信息等。这个东西就是证书。然后下载到本地
4. 在开发时，编译完一个APP，用本地的私钥M（p12文件），对这个APP进行签名，其实就是对Mach-O文件签名，同时步奏3得到的证书也一同打包，生成.ipa文件。
5. 这个ipa文件也就是我们的APP，安装到手机时，手机上有公钥A，通过公钥A解密步奏3得到的证书，可以拿到公钥M。
6. 然后再用公钥M验证步奏4对APP的签名。如果签名验证通过，则APP就是合法的。

但是这样还是会有问题，我们只要有了证书，任何设备都可以安装，而且没有设备数限制。

# 描述文件

苹果为了解决上面的问题，就又加了限制，引入了描述文件。

1. 对开发者来说，只有添加了udid的设备才能安装。
2. 只能安装某一个，或者某一类的APP。（指定了bundle id，或者是通配类型：com.apple.*）
3. 还指定了APP的权限(Entitlements)，比如：iCloud、Push、后台运行等等权限，由xcode生成。

苹果把这些内容，放在一个文件中，叫做描述文件（Provisioning Profile）。后缀名为mobileprovision

描述文件一般包括三样东西：证书、App ID、设备信息、权限信息。当我们在真机运行或者打包一个项目的时候，证书用来证明我们程序的安全性和合法性。

所以我们再来看看真正定双重签名。

# 真正的双重签名、验证


1. 在Mac上生成非对称加密算法的公钥、私钥。我们称之为公钥M、私钥M。M=Mac
2. 苹果自己有一套公钥、私钥，与简单签名中的逻辑一致，私钥在苹果后台，公钥在每一个iOS系统的设备中，称之为公钥A，私钥A，A=Apple
3. 公钥M打包开发者的信息，上传到苹果后台（通过CSR请求文件申请证书）。苹果后台用私钥A去签名公钥M，得到一份包含了公钥M的Hash值（签名）以及开发者信息，这个东西就是证书（CRT。可以下载到本地，如果多开发者协同开发，需要把证书的密钥导出来，生成p12文件）。同时打包bundle id，设备的udid、APP权限等和证书一起生成描述文件。然后下载到本地。（多开发者协同开发，需要把对应的p12文件、描述文件一起给开发人员）
4. 在开发时，选择对应的描述文件，用本地的私钥M（p12文件），对Mach-O文件签名，同时步奏3得到的描述文件也一同打包，生成.ipa文件。
    权限文件为`embedded.mobileprovision`(由xcode生成)，也在ipa内。包含了设备udid的数组、权限数组、app id、有效时间等信息
5. 这个ipa文件也就是我们的APP，安装到手机时，通过公钥A解密步奏3得到的证书，可以拿到公钥M。
6. 然后再用公钥M验证步奏4对APP的签名。如果签名验证通过，则APP就是合法的。


![](双重签名-验证.jpg)


# 总结：

* 苹果的双重签名验证流程
* 生成的证书，需要导出p12文件和描述文件一起发给其他开发人员。






关于证书相关的配置这里推荐一篇来自知乎的文章，[iOS证书配置](https://zhuanlan.zhihu.com/p/69162456)。写的比较详细了。

