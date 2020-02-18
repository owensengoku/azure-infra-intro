
# <a name="quickstart-create-a-virtual-network-using-the-azure-portal"></a>快速入門：使用 Azure 入口網站建立虛擬網路

虛擬網路是私人網路在 Azure 中的基本建置組塊。 它可讓 Azure 資源 (例如虛擬機器 (VM)) 彼此及與網際網路安全地進行通訊。 在此快速入門中，您將了解如何使用 Azure 入口網站建立虛擬網路。 然後，您可以將兩部 VM 部署至虛擬網路中、在兩部 VM 之間安全地進行通訊，並從網際網路連線到 VM。


如果您沒有 Azure 訂用帳戶，請立即建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="sign-in-to-azure"></a>登入 Azure

登入 [Azure 入口網站](https://portal.azure.com)。

## <a name="create-a-virtual-network"></a>建立虛擬網路

1. 從 Azure 入口網站功能表選取 [建立資源]  。

2. 從 Azure Marketplace 選取 [網路]   > [虛擬網路]  。

3. 在 [建立虛擬網路]  中，輸入或選取這項資訊：

    | 設定 | 值 |
    | ------- | ----- |
    | 名稱 | 輸入 *myVirtualNetwork*。 |
    | 位址空間 | 輸入 *10.1.0.0/16*。 |
    | 訂用帳戶 | 選取您的訂用帳戶。|
    | 資源群組 | 選取 [新建]  ，輸入 *myResourceGroup*，然後選取 [確定]  。 |
    | 位置 | 選取 [美國東部]  。|
    | 子網路 - 名稱 | 輸入 *myVirtualSubnet*。 |
    | 子網路 - 位址範圍 | 輸入 *10.1.0.0/24*。 |

4. 將其他項目保留為預設值，然後選取 [建立]  。

## <a name="create-virtual-machines"></a>建立虛擬機器

在虛擬網路內建立兩個 VM：

### <a name="create-the-first-vm"></a>建立第一個 VM

1. 從 Azure 入口網站功能表選取 [建立資源]  。

2. 從 Azure Marketplace 選取 [計算]   > [Ubuntu Server 18.04 LTS]  。

3. 在 [建立虛擬機器 - 基本]  中，輸入或選取這項資訊：

    | 設定 | 值 |
    | ------- | ----- |
    | **專案詳細資料** | |
    | 訂用帳戶 | 選取您的訂用帳戶。 |
    | 資源群組 | 選取 **myResourceGroup**。 您已在上一節中建立此項目。 |
    | **執行個體詳細資料** |  |
    | 虛擬機器名稱 | 輸入 *myFront1*。 |
    | 區域 | 選取 [美國東部]  。 |
    | 可用性選項 | 保留預設值 [不需要基礎結構備援]  。 |
    | 映像 | 保留預設值 [Ubuntu Server 18.04 LTS]  。 |
    | 大小 | [標準 DS1 v2]  。 |
    | **系統管理員帳戶** |  |
    | 使用者名稱 | azureuser |
    | [SSH 公開金鑰]  | 貼上您的公開金鑰。 請移除公開金鑰中的任何前置或尾端的空白字元。|
    | **輸入連接埠規則** |  |
    | 公用輸入連接埠 | 保留預設值  。 |
 
4. 選取 [下一步：  磁碟]。

5. 在 [建立虛擬機器 - 磁碟]  ，保留預設值並選取 [下一步:  網路功能]。

6. 在 [建立虛擬機器 - 網路功能]  中，選取這項資訊：

    | 設定 | 值 |
    | ------- | ----- |
    | 虛擬網路 | 保留預設值 [myVirtualNetwork]  。 |
    | 子網路 | 保留預設值 [myVirtualSubnet (10.1.0.0/24)]  。 |
    | 公用 IP | 保留預設值 [(new) myFront1-ip]  。 |
    | 公用輸入連接埠 | 選取 [允許選取的連接埠]  。 |
    | 選取輸入連接埠 | 選取 [HTTP]  和 [SSH]  。

7. 選取 [下一步:  管理]。

8. 在 [建立儲存體帳戶]  中，選取這項資訊：

    | 設定 | 值 |
    | ------- | ----- |
    | 名稱 | 選擇 ($storageaccount 的值) 。您已在上一節中建立此項目。|

9. 選取 [確定] 

10. 選取 [檢閱 + 建立]  。 您會移至 [檢閱 + 建立]  頁面，其中 Azure 會驗證您的設定。

11. 當您看到 [驗證成功]  訊息時，請選取 [建立]  。

### <a name="create-the-second-vm"></a>建立第二個 VM

1. 完成上述的步驟 1 到 9。

    > [!NOTE]
    > 在步驟 2 中，針對 [虛擬機器名稱]  輸入 *myFront2*。
    >
    > 在步驟 7 中，針對 [診斷儲存體帳戶]  ，務必選取您在上一節中建立此項目  。

2. 選取 [檢閱 + 建立]  。 您會移至 [檢閱 + 建立]  頁面，且 Azure 會驗證您的設定。

3. 當您看到 [驗證成功]  訊息時，請選取 [建立]  。


## <a name="connect-to-a-vm-from-the-internet"></a>從網際網路連線至 VM

1. 在入口網站的搜尋列中，輸入 *myFront2*。

2. 在 [連線至虛擬機器]  頁面中，維持預設選項，以便使用 IP 位址透過連接埠 22 進行連線。 在**使用 VM 本機帳戶登入**中，會顯示連線命令。 選取按鈕以複製該命令。 下列範例說明 SSH 連線命令的內容：

    ```bash
    ssh azureuser@10.111.12.123
    ```

## <a name="communicate-between-vms"></a>虛擬機器之間的通訊

1. 在 *myFront2* 的 shell 中。

2. 輸入 `ping myFront1`。

    您會收到類似此訊息：

    ``bash
    PING myFront1.zw2tuckdrraepm5kktygatnkwg.bx.internal.cloudapp.net (10.1.1.4) 56(84) bytes of data.
    64 bytes from myfront1.internal.cloudapp.net (10.1.1.4): icmp_seq=1 ttl=64 time=0.570 ms
    64 bytes from myfront1.internal.cloudapp.net (10.1.1.4): icmp_seq=2 ttl=64 time=0.778 ms
    64 bytes from myfront1.internal.cloudapp.net (10.1.1.4): icmp_seq=3 ttl=64 time=1.21 ms
    64 bytes from myfront1.internal.cloudapp.net (10.1.1.4): icmp_seq=4 ttl=64 time=1.18 ms
    ....
    ```
    

## <a name="clean-up-resources"></a>清除資源

當您完成使用虛擬網路與 VM 時，可以刪除資源群組以及其包含的所有資源：

1. 在入口網站頂端的 [搜尋]  方塊中輸入 *myResourceGroup*，然後從搜尋結果中選取 [myResourceGroup]  。

2. 選取 [刪除資源群組]  。

3. 針對 [輸入資源群組名稱]  輸入 *myResourceGroup*，然後選取 [刪除]  。
e