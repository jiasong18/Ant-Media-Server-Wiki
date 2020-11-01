In this guide, I will explain how to setup Ant Media Server Clustering on Azure. When your load is high, one server instance is not enough for you and you can handle that load with a clustering solution.

For streaming applications, you will need a clustering solution when you have high numbers of publishers and viewers. Especially when you require ultra-low latency and adaptive bitrate because they need more processing power. Ultra-low latency is achieved by WebRTC and it is a CPU intensive protocol. Adaptive bitrating is downgrading video quality in bad networks if needed. It is also CPU intensive because there is video conversion. Luckily, Ant Media Server Enterprise Edition supports clustering, so that you can handle the high load in your streaming applications.

**Requirements:**

To set up media server, Having an Azure account and a ready AntMedia Server Image are needed.

The architecture of the cluster setup could be found here.
Introduction to clustering with AntMedia Server could be found here.

### Step 1: Create a Resource Group

Each resource created must be in the same resource group. For this, we will first create a resource group. Named **antmedia-cluster**

Click Resource groups in the portal which is on the left side then click **+Add**

[![Image](images/azure/create-resource-1.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/create-resource-1.png)

Enter "**Resource group**" then choose your zone.

[![Image](images/azure/create-resource-2.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/create-resource-2.png)

Proceed by clicking "**Create**" button

[![Image](images/azure/create-resource-3.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/create-resource-3.png)

### Step 2: Create a Virtual Network

We need to create a virtual network named antmedia-cluster-virtual-network, and then we will add gateway-subnet, origin-subnet and edge-subnet

Click Create a Resource in the portal which is on the upper left. Enter Virtual network in the Search the Marketplace box at the top of the New pane that appears. Click Virtual network when it appears in the search results.

[![Image](images/azure/virtual-network-1.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/virtual-network-1.png)

click "**Create**"

[![Image](images/azure/virtual-network-2.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/virtual-network-2.png)

Select the resource group that we created before, enter **antmedia-cluster-virtual-network** in the name field and click on the "**Next: IP Address**" button.

[![Image](images/azure/virtual-network-3.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/virtual-network-3.png)

Click on the Add subnet button and create the antmedia-origin-subnet, antmedia-edge-subnet and antmedia-gw-subnet as shown in the figure below.

[![Image](images/azure/virtual-network-4.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/virtual-network-4.png)

The subnets that you created will look like the following. If everything is ok, click the "Review + create" button and create your Virtual Network.

[![Image](images/azure/virtual-network-5.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/virtual-network-5.png)

The process is completed by clicking on the "**Create**" button.

[![Image](images/azure/virtual-network-6.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/virtual-network-6.png)

### Step 3: Create a MongoDB Virtual Machine

Click Virtual Machines on the left bar and then click "**+Add**"

[![Image](images/azure/mongodb-1.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/mongodb-1.png)

Enter the following values and click "**Next: Disks**"
```
Resource group "antmedia-cluster"
Image "Ubuntu 18.04 LTS"
```
[![Image](images/azure/mongodb-2.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/mongodb-2.png)

Enter the following values and click "**Next: Networking**"

[![Image](images/azure/mongodb-3.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/mongodb-3.png)

Select the Virtual Network that you created, click "**Advanced**" from "**Nic network security group**" and click "**Create new**"

[![Image](images/azure/mongodb-4.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/mongodb-4.png)

Click "**Add an inbound rule**" in the window that appears and "**Add inbound security rule**" as below.

[![Image](images/azure/mongodb-6.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/mongodb-6.png)

Enter the following values and click "**Next: Advanced**"

[![Image](images/azure/mongodb-7.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/mongodb-7.png)

Add the following lines to the "**Custom data**" area and click the "**Review + Create**" button to create a MongoDB instance.

```
#!/bin/bash
wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu `lsb_release -cs`/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
sudo apt-get update
sudo apt-get install -y mongodb-org
sed -i 's/bindIp:.*/bindIp: 0.0.0.0/g' /etc/mongod.conf
systemctl restart mongod
```
[![Image](images/azure/mongodb-8.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/mongodb-8.png)

The process is completed by clicking on the "**Create**" button.

[![Image](images/azure/mongodb-9.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/mongodb-9.png)

### Step 4: Create Application Gateway

Click Create a Resource in the portal which is on the upper left. Enter Application Gateway in the Search the Marketplace box at the top of the New pane that appears. Click the "**Application Gateway**" when it appears in the search results.

[![Image](images/azure/application-gateway-1.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/application-gateway-1.png)

Proceed by clicking "**Create**" button

[![Image](images/azure/application-gateway-2.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/application-gateway-2.png)

Enter the Resource Group, Application Gateway Name, Region, and Virtual Network settings as follows and click "**Next: Frontends**"

[![Image](images/azure/application-gateway-3.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/application-gateway-3.png)

Click on the "**Add new**" button and enter the public IP name then click "**Next: Backends**"

[![Image](images/azure/application-gateway-5.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/application-gateway-5.png)

Click on "**Add a backend pool**", create pools for both origin and edge as shown in the screenshot, and click "**Next: Configuration**".

[![Image](images/azure/application-gateway-7.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/application-gateway-7.png)

Click on "**Add a routing rule**" in the window that appears.

[![Image](images/azure/application-gateway-8.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/application-gateway-8.png)

Ekran goruntundeki gibi alanlari doldurup "Backend targets" 'i tiklayin.

[![Image](images/azure/application-gateway-9.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/application-gateway-9.png)

Backend target "Edge" poolunu secin ve HTTP settings icon Add new 'e tiklayin.

[![Image](images/azure/application-gateway-10.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/application-gateway-10.png)

Asagidaki gibi doldurun. Bu ayar hem origin hem de origin icin olacaktir. 

[![Image](images/azure/application-gateway-11.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/application-gateway-11.png)

Ayarlariniz asagidaki gibiyse HTTP istekler icin Edge yapilandirilmasi bitmistir.

[![Image](images/azure/application-gateway-12.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/application-gateway-12.png)

Tekrar "Add a routing rule" 'e tiklayip Origin icin HTTP ayarlarini yapalim. Asagidaki gibi ayarlari yapip "Backend target" tiklayin.

[![Image](images/azure/application-gateway-13.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/application-gateway-13.png)

Backend target olarak Origin poolunu secin ve HTTP settings olarakta onceden olusturdugumuz "BackendHttpSettings" i secin.

[![Image](images/azure/application-gateway-14.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/application-gateway-14.png)

Simdi sirada Origin icin HTTPS isteklerini yonlendirmeye geldi. Bunun icin asagidaki gibi ayarlari yapin. cluster.antmedia.io.pfx sertifikasi icin bu linki kullanabilirsiniz.

[![Image](images/azure/application-gateway-15.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/application-gateway-15.png)

Backend target olarak Origin ve HTTP Settings olarak "BackendHttpSetting" secin.

[![Image](images/azure/application-gateway-16.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/application-gateway-16.png)

Ayni sekilde Edge icin 5443 icin yapilandirmayi asagidaki gibi yapin.

[![Image](images/azure/application-gateway-17.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/application-gateway-17.png)

Backend target olarak Edge ve HTTP Settings olarak "BackendHttpSetting" secin.

[![Image](images/azure/application-gateway-18.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/application-gateway-18.png)

Tam olarak application-gateway ayarlari asagidaki gibi gozukecektir. Her sey yolundaysa "Next:Tags" tiklayin.

[![Image](images/azure/application-gateway-19.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/application-gateway-19.png)

Son olarak "Create" basarak kurulumu tamamlayin.

[![Image](images/azure/application-gateway-20.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/application-gateway-20.png)

### Step 4: Create Edge/Origin Scale Sets

We need to setup scale sets. Click Create a Resource in the portal which is on the upper left. Enter Application Gateway in the Search the Marketplace box at the top of the New pane that appears. Click Application Gateway when it appears in the search results.

[![Image](images/azure/virtual-machine-1.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/virtual-machine-1.png)

Select Classic in the Select a deployment model box in the "Virtual Machine Scale Set" pane that appears, then clicks Create.

[![Image](images/azure/virtual-machine-2.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/virtual-machine-2.png)

Ilk olarak Origin Scale Set'i olusturacagiz. Resource Groupu secin Virtual Machine scale set name Region ayarlarini asagidaki gibi yapin. Browse all public and private images 'e tiklayin.

[![Image](images/azure/virtual-machine-3.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/virtual-machine-3.png)

Acilan pencerede Ant Media Server diye aratip "Ant Media Server Enterprise 2.1.0" surumu secin.

[![Image](images/azure/virtual-machine-3_1.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/virtual-machine-3_1.png)

Disk secimini yaptiktan sonra "Next: Networking" 'i secin.

[![Image](images/azure/virtual-machine-4.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/virtual-machine-4.png)

Bu ekranda Virtual Network olarak onceden olusturmus oldugumuz antmedia-cluster-virtual-network 'u secin. Network interface olarak origin subnetini secin ve Use a load Balancer Yes tiklayin. Onceden olusturmus oldugumuz application gateway 'i secin ve Backend Pool olarak origin i secin ve "Next: Scaling" 'i tiklayin.

[![Image](images/azure/virtual-machine-5.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/virtual-machine-5.png)

Bu ekranda Custom'i secin ve Cpu threshold %60 'a getirin. Diger ayarlari kendinize gore yapabilirsiniz.

[![Image](images/azure/virtual-machine-6.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/virtual-machine-6.png)

Management ve Health sekmelerinde direk next diyerek devam edin ve Advanced kisminda Custom data alanina asagidaki satirlari ekleyin.

```
#!/bin/bash
cd /usr/local/antmedia/
./change_server_mode.sh cluster 10.0.2.4
```
10.0.2.4 IP adresi daha once kurmus oldugum MongoDB instanceinin private IP adresidir. Kendinize gore duzenleyiniz.

[![Image](images/azure/virtual-machine-7.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/virtual-machine-7.png)

Create butonuna tiklayarak Origin icin Scale Set'ini olusturun. 

[![Image](images/azure/virtual-machine-8.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/virtual-machine-8.png)

Tekrar yeni bir adet Scale Set olusturun yukaridaki adimlari tekrarlayin. Sonra Networking kismini da asagidaki gibi duzenleyin. Network Interface olarak Edge Subnetu olacak ve Application Gateway Backend pool icin de Edge secili olacaktir.

[![Image](images/azure/virtual-machine-9.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/virtual-machine-9.png)

Son olarak scale setleriniz asagidaki gibi gozukecektir.

[![Image](images/azure/virtual-machine-10.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/virtual-machine-10.png)

### Troubleshoot

Asagidaki sayfalar cevap veriyorsa edge/origin yonlendirmeleriniz dogru calisiyor demektir.

Edge icin login https://application-gateway-ip:5443
Origin icin login https://application-gateway-ip

[![Image](images/azure/antmedia-login.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/antmedia-login.png)

Cluster Menusune geldiginizde de asagidaki gibi join olmus nodelari gorebilirsiniz.

[![Image](images/azure/antmedia-cluster.png)](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/azure/antmedia-cluster.png)