![](http://i.imgur.com/p0mw85L.png)

> `OpenStack`是一個開放原始碼計畫，**目標是要用來打造出一套開源免費的雲端服務平臺**，提供任何人或任何企業自行建立自己的雲端服務環境，工研院雲端中心主任闕志克甚至形容**`OpenStack`就是一套`雲端作業系統`(`Cloud OS`)**。從2010年7月19日，`美國太空總署``NASA`和美國雲端服務業者`Rackspace OpenStack`推出之後，才釋出第一個版本Austin版本。隔天微軟宣布，Windows Server 2008 R2 Hyper-V支援`OpenStack`，也貢獻了`OpenStack`支援`Hyper-V`的程式碼，連微軟也不敢小覷`OpenStack`的影響。

> `OpenStack`起初是由雲端服務廠商`RackSpace`和`美國太空總署``NASA`，在2010年7月時共同成立，將原本在各自內部發展的專案原始碼整合在一起，並在同年10月發行了`OpenStack`的第一版`Austin`。隨後交由`RackSpace`負責管理。然而，為了能夠讓`OpenStack`成功發展，負責管理的`RackSpace`知道必須使其成為一個獨立運作的計畫，便在2011年10月時宣布，將在2012年成立`OpenStack`基金會。

> 就像是在實體的電腦上，作業系統可以用來控制實體電腦上的硬體機制，讓應用程式和底層實體硬體隔離。**作業系統提供了一個可以跨硬體的共通執行環境，讓應用程式不用受制於不同廠牌、規格的硬體功能。**同樣的邏輯，雲端作業系統作為雲端應用程式和實體資料中心的中間層，讓雲端應用不用受制於實體資料中心內各種硬體設備的侷限，同樣可以提供一個共通的雲端執行環境。

> `OpenStack`不是唯一一套可以實現這個構想的技術，已有不少成熟的商用軟體，如`VMware`和微軟的虛擬化平臺，同樣也能實現這樣的目標。但是，`OpenStack`不一樣，它是一套免費開源的雲端作業系統。

> 就像早在`Linux`出現之前，已有不少商用作業系統。但是免費開源`Linux`卻掀起了新的開放革命。隨著電腦硬體成本越來越低，`Linux`讓電腦的進入門檻更低了，甚至軟體成本幾乎可以忽略，也不用受限於特定軟硬體廠商的綁定。任何人只要買了一臺實體電腦，就能用免費的`Linux`打造出一個低成本的桌面執行環境，甚至是應用系統的執行環境。`Linux`提供了商用軟體產品以外的另一個選擇，打破了IT廠商壟斷OS平臺的局面。

> 同樣道理， `OpenStack`對雲端平臺的價值，如同是`Linux`對個人電腦的價值一樣。`OpenStack`可以讓企業打造出一套免費的雲端平臺，來實現上雲端的目標，不論是公有雲、私有雲，都可以使用`OpenStack`來建置。

> `OpenStack`是一套使用`Python`程式語言撰寫的軟體，也是個開放原始碼計畫，以`Apache`許可證授權。`OpenStack`內部包括了`運算模組`、`網通模組`和`儲存模組`，再搭配一個可以集中管理上述三大類模組的`儀表板模組`，最後組合而成一套`OpenStack`共享服務，並且可以提供虛擬機器的方式，對外提供運算資源以便彈性擴充或調度。換句話說，**`OpenStack`也是一套可以用來打造IaaS服務的開源軟體**。對應用程式而言，只要透過`API`就可以和`OpenStack`溝通，例如用`API`來調度虛擬機器的部署等，`OpenStack`再負責和不同廠牌的硬體設備，或是軟體系統溝通。

1. 運算套件`Nova`：**提供部署與管理虛擬機器的功能**
2. 物件儲存套件`Swift`：**是個分散式儲存平臺，可存放非結構化的資料**
3. 區塊儲存套件`Cinder`：提供區塊儲存容量，具有快照功能
4. 網通套件`Quantum`：透過API來管理的網路架構系統，支援多家網通廠商技術
5. 身分識別套件`Keystone`：提供了多種驗證方式，能查看哪位使用者可存取哪些服務
6. 映象檔管理套件`Glance`：提供映象檔尋找、註冊以及服務交付等功能
7. 儀表板套件`Horizon`：圖形化的網頁介面，讓IT人員管理雲端服務的硬體資源
8. `Orchestration`層整合套件`Heat`（開發中）：整合與管理運行在運算套件`Nova`上的應用程式
9. 資料監控套件`Ceilometer`（開發中）：計算雲端服務所使用的資料量，作為日後收費參考依


套件名稱	        | 套件功能               | Amazon AWS相似的服務
--------------- | --------------------- | ------------- 
運算套件`Nova`	    | 部署與管理虛擬機器的功能                                          | `EC2`
物件儲存套件`Swift` | 可擴展的分布式儲存平臺，以防止單點故障的情況產生，可存放非結構化的資料 | `S3`
區塊儲存套件`Cinder` |	整合了運算套件，可讓IT人員查看儲存設備的容量使用狀態，具有快照功能	| `EBS`
網通套件`Quantum` | 可擴展、隨插即用，透過API來管理的網路架構系統，以確保IT人員在部署雲端服務時，網路服務不會出現瓶頸，或是成為無法部署的因素之一 | `VPC`
身分識別套件`Keystone` | 具有中央目錄，能查看哪位使用者可存取哪些服務，並且，提供了多種驗證方式	| `None`
映象檔管理套件`Glance` | 硬碟或伺服器的映象檔尋找、註冊以及服務交付等功能	 | `VM Import/Export`
儀表板套件`Horizon`	  | 圖形化的網頁介面，讓IT人員可以綜觀雲端服務目前的規模與狀態，並能夠統一存取、部署與管理所有雲端服務所使用到的資源。| `Console`
 
#### :books: 參考網站：
- [挑戰雙V雲端版圖 OpenStack逐漸成氣候](http://www.ithome.com.tw/news/90145)
- [伺服器整合Ceph儲存方案紛紛出籠](http://www.ithome.com.tw/tech/99512)
- [](http://www.ithome.com.tw/node/81091)

---

`Red Hat Enterprise Linux (RHEL) 7`

`/etc/environment`
```
LANG=en_US.utf-8
LC_ALL=en_US.utf-8
```

```
shell> sudo yum install -y https://www.rdoproject.org/repos/rdo-release.rpm
shell> sudo yum update -y
shell> sudo yum install -y openstack-packstack
shell> packstack --allinone
```

#### :books: 參考網站：
- [rdoproject](https://www.rdoproject.org/)
- [quickstart](https://www.rdoproject.org/install/quickstart/)

---

