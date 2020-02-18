
# <a name="tutorial-filter-network-traffic-with-a-network-security-group-using-the-azure-portal"></a>教學課程：使用 Azure 入口網站透過網路安全性群組篩選網路流量

您可以透過網路安全性群組篩選輸入虛擬網路子網路和從中輸出的網路流量。 網路安全性群組包含可依 IP 位址、連接埠和通訊協定篩選網路流量的安全性規則。 安全性規則會套用至子網路中部署的資源。 在本教學課程中，您會了解如何：

> * 建立網路安全性群組和安全性規則
> * 建立虛擬網路，並將網路安全性群組與子網路產生關聯
> * 將虛擬機器 (VM) 部署至子網路中
> * 測試流量篩選


## <a name="sign-in-to-azure"></a>登入 Azure

在 https://portal.azure.com 登入 Azure 入口網站。

## <a name="create-application-security-groups"></a>建立應用程式安全性群組

應用程式安全性群組可讓您將具有類似功能 (例如 web 伺服器) 的伺服器群組在一起。

1. 從 Azure 入口網站功能表或 **[首頁]** 頁面，選取 [建立資源]  。 
2. 在 [搜尋 Marketplace]  方塊中，輸入「應用程式安全性群組」  。 當搜尋結果中出現 [應用程式安全性群組]  時，請加以選取，在 [所有項目]  下再次選取 [應用程式安全性群組]  ，然後選取 [建立]  。
3. 輸入或選取下列資訊，然後選取 [建立]  ︰

    | 設定        | 值                                                         |
    | ---            | ---                                                           |
    | 名稱           | myAsgWebServers                                               |
    | 訂用帳戶   | 選取您的訂用帳戶。                                     |
    | 資源群組 | 選取 [使用現有的]  ，然後選取 [myResourceGroup]  。 |
    | Location       | 美國東部                                                       |

4. 再次完成步驟 3，並指定下列值：

    | 設定        | 值                                                         |
    | ---            | ---                                                           |
    | 名稱           | myAsgDatasServers                                              |
    | 訂用帳戶   | 選取您的訂用帳戶。                                     |
    | 資源群組 | 選取 [使用現有的]  ，然後選取 [myResourceGroup]  。 |
    | Location       | 美國東部                                                       |

## <a name="create-a-network-security-group"></a>建立網路安全性群組

1. 從 Azure 入口網站功能表或 **[首頁]** 頁面，選取 [建立資源]  。 
2. 選擇 [網路]  ，然後選取 [網路安全性群組]  。
3. 輸入或選取下列資訊，然後選取 [建立]  ︰

    |設定|值|
    |---|---|
    |名稱|myNsg|
    |訂用帳戶| 選取您的訂用帳戶。|
    |資源群組 | 選取 [使用現有的]  ，然後選取 [myResourceGroup]  。|
    |Location|美國東部|

## <a name="associate-network-security-group-to-subnet"></a>將網路安全性群組關聯至子網路

1. 在入口網站頂端的 [搜尋資源、服務和文件]  方塊中，開始鍵入 myNsg  。 當搜尋結果中出現 **myNsg** 時，請加以選取。
2. 在 [設定]  底下選取 [子網路]  ，然後選取 [+ 關聯] 
3. 在 [建立子網路關聯]  底下，選取 [虛擬網路]  ，然後選取 [myVirtualNetwork]  。 選取 [子網路]  、選取 [myVirtualSubnet]  ，然後選取 [確定]  。

## <a name="create-security-rules"></a>建立安全性規則

1. 在 [設定]  下，選取 [輸入安全性規則]  ，然後選取 [+ 新增]  ，如下圖所示：

2. 建立安全性規則，允許連接埠 22 和 80 和 443 連至 **myAsgWebServers** 應用程式安全性群組。 在 [新增輸入安全性規則]  下，輸入或選取下列值、接受其餘預設值，然後選取 [新增]  ：

    | 設定                 | 值                                                                                                           |
    | ---------               | ---------                                                                                                       |
    | Destination             | 選取 [應用程式安全性群組]  ，然後針對 [應用程式安全性群組]  選取 [myAsgWebServers]  。  |
    | 目的地連接埠範圍 | 輸入 22, 80,443                                                                                                    |
    | 通訊協定                | 選取 TCP                                                                                                      |
    | 名稱                    | Allow-Web-All                                                                                                   |

一旦您完成步驟 1-2 後，請檢閱您所建立的規則。 您的清單看起來應如下圖中所示的清單︰

![安全性規則](./media/tutorial-filter-network-traffic/security-rules.png)

### <a name="create-the-first-vm"></a>建立Data VM

1. 從 Azure 入口網站功能表或 **[首頁]** 頁面，選取 [建立資源]  。 
2. 選取 [計算]  ，然後選取 [Ubuntu Server 18.04 LTS]  。
3. 輸入或選取下列資訊，然後接受其餘設定的預設值：

    | 設定 | 值 |
    | ------- | ----- |
    | **專案詳細資料** | |
    | 訂用帳戶 | 選取您的訂用帳戶。 |
    | 資源群組 | 選取 **myResourceGroup**。 您已在上一節中建立此項目。 |
    | **執行個體詳細資料** |  |
    | 虛擬機器名稱 | 輸入 *myDataVM*。 |
    | 區域 | 選取 [美國東部]  。 |
    | 可用性選項 | 保留預設值 [不需要基礎結構備援]  。 |
    | **系統管理員帳戶** |  |
    | 使用者名稱 | azureuser |
    | [SSH 公開金鑰]  | 貼上您的公開金鑰。 請移除公開金鑰中的任何前置或尾端的空白字元。|
    | **輸入連接埠規則** |  |
    | 公用輸入連接埠 | 保留預設值  。 |
   

4. 選取 VM 的大小 [標準 DS1 v2]
5. 在 [網路]  下，選取下列值，然後接受其餘預設值：

    |設定|值|
    |---|---|
    |虛擬網路 |選取 **myVirtualNetwork**。|
    |NIC 網路安全性群組 |選取 [無]  。|
  

6. 選取左下角的 [檢閱 + 建立]  ，然後選取 [建立]  以開始 VM 部署。



## <a name="associate-network-interfaces-to-an-asg"></a>將網路介面關聯至 ASG

當入口網站建立 VM 時，會建立每個 VM 的網路介面，並將網路介面連結至 VM。 請將每個 VM 的網路介面，新增至您先前建立的其中一個應用程式安全性群組：

1. 在入口網站頂端的「搜尋資源、服務和文件」  方塊中，開始輸入 myFrontVM  。 當搜尋結果中出現 **myFrontVM** VM 時，請加以選取。
2. 在 [設定]  底下，選取 [網路]  。  選取 [設定應用程式安全性群組]  ，針對 [應用程式安全性群組]  選取 myAsgWebServers  ，然後選取 [儲存]  ，如下圖所示：

3. 再次完成步驟 1 和 2、搜尋 **myDataVM** VM，然後選取 **myAsgDatasServers** ASG。

## <a name="test-traffic-filters"></a>測試流量篩選


在您建立 *myDataVM* 之後，請進行以下安裝。

1. 在入口網站的搜尋列中，輸入 *myDataVM*。

2. 在 [連線至虛擬機器]  頁面中，維持預設選項，以便使用 IP 位址透過連接埠 22 進行連線。 在**使用 VM 本機帳戶登入**中，會顯示連線命令。 選取按鈕以複製該命令。 下列範例說明 SSH 連線命令的內容：

    ```bash
    ssh azureuser@10.111.12.123
    ```

若要查看作用中的 VM，請安裝 NGINX 網頁伺服器。 從 SSH 工作階段更新套件來源，然後安裝最新的 NGINX 套件。

```bash
sudo apt-get -y update
sudo apt-get -y install nginx
```

完成時，輸入 `exit` 來離開 SSH 工作階段。

## <a name="clean-up-resources"></a>清除資源

當不再需要資源群組時，請將資源群組及其包含的所有資源刪除：

1. 在入口網站頂端的 [搜尋]  方塊中，輸入 myResourceGroup  。 當您在搜尋結果中看到 myResourceGroup  時，請加以選取。
2. 選取 [刪除資源群組]  。
3. 針對 [輸入資源群組名稱:]  輸入 myResourceGroup  ，然後選取 [刪除]  。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已建立網路安全性群組，並將其與虛擬網路子網路產生關聯。 若要深入了解網路安全性群組，請參閱[網路安全性群組概觀](security-overview.md)和[管理網路安全性群組](manage-network-security-group.md)。

Azure 依預設會路由傳送子網路之間的流量。 您可以改採其他方式，例如，透過作為防火牆的 VM 路由傳送子網路之間的流量。 若想了解如何建立路由表，請移至下一個教學課程。

> [!div class="nextstepaction"]
> [建立路由表](./tutorial-create-route-table-portal.md)
