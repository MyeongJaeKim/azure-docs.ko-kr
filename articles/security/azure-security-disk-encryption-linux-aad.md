---
title: Linux IaaS VM용 Azure AD 앱을 사용한 Azure Disk Encryption(이전 릴리스)
description: 이 문서에서는 Linux IaaS VM용 Microsoft Azure Disk Encryption을 사용하도록 설정하는 방법에 대한 지침을 제공합니다.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 12/17/2018
ms.custom: seodec18
ms.openlocfilehash: ebc948e6f875fa39b09766342b7034d7b467fdeb
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53549465"
---
# <a name="enable-azure-disk-encryption-for-linux-iaas-vms-previous-release"></a>Linux IaaS VM용 Azure Disk Encryption 사용(이전 릴리스)

**Azure Disk Encryption의 새 릴리스는 Azure AD 애플리케이션 매개 변수 제공에 대한 요구 사항을 제거하여 VM 디스크 암호화를 사용하도록 설정합니다. 새 릴리스를 사용하면 암호화 단계를 사용하는 동안 더 이상 Azure AD 자격 증명을 제공할 필요가 없습니다. 모든 새 VM은 새 릴리스를 사용하는 Azure AD 애플리케이션 매개 변수를 사용하지 않고 암호화되어야 합니다. 새 릴리스를 사용한 VM 디스크 암호화 사용에 관한 지침을 보려면 [Linux VM용 Azure Disk Encryption](azure-security-disk-encryption-linux.md)을 참조하세요. Azure AD 애플리케이션 매개 변수를 사용하여 이미 암호화된 VM도 여전히 지원되며 AAD 구문을 사용하여 계속 유지 관리되어야 합니다.**

수많은 디스크 암호화 시나리오를 사용할 수 있으며 단계는 시나리오에 따라 다를 수 있습니다. 다음 섹션에서는 Linux IaaS VM에 대한 시나리오를 자세히 설명합니다. 디스크 암호화를 사용하려면 먼저 [Azure Disk Encryption 필수 구성 요소](azure-security-disk-encryption-prerequisites-aad.md)를 완료해야 하며 [Linux IaaS VM 추가 필수 구성 요소](azure-security-disk-encryption-prerequisites-aad.md#bkmk_LinuxPrereq) 섹션을 검토해야 합니다.

디스크가 암호화되기 전에 먼저 [스냅숏](../virtual-machines/windows/snapshot-copy-managed-disk.md) 및/또는 백업을 수행하세요. 백업은 암호화 도중에 예기치 않은 오류가 발생할 경우 복구 옵션을 사용할 수 있습니다. 암호화가 수행되기 전에 관리 디스크가 있는 VM은 백업해야 합니다. 백업이 완료되면 Set-AzureRmVMDiskEncryptionExtension cmdlet을 사용하여 -skipVmBackup 매개 변수를 지정함으로써 관리 디스크를 암호화할 수 있습니다. 암호화된 VM을 백업하고 복원하는 방법에 대한 자세한 내용은 [Azure Backup](../backup/backup-azure-vms-encryption.md) 문서를 참조하세요. 

>[!WARNING]
 > - 이전에 [Azure AD 앱에서 Azure Disk Encryption](azure-security-disk-encryption-prerequisites-aad.md)를 사용하여 이 VM을 암호화한 경우에는 VM을 암호화하는 데 이 옵션을 계속 사용해야 합니다. 이는 지원되는 시나리오가 아니므로 이 암호화된 VM에서는 [Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md)을 사용할 수 없습니다. 즉, 이 암호화된 VM을 위해 AAD 애플리케이션에서 전환하는 기능은 아직 지원되지 않습니다.
 > - 암호화 비밀이 지역 경계를 넘지 않도록 하려면 Azure Disk Encryption에서 Key Vault와 VM을 동일한 지역에 공동 배치해야 합니다. 암호화할 VM과 동일한 지역에 있는 Key Vault를 만들고 사용합니다.
 > - Linux OS 볼륨을 암호화하는 경우 몇 시간이 걸릴 수 있습니다. 일반적으로 데이터 볼륨보다 Linux OS 볼륨을 암호화하는 데 시간이 더 오래 걸립니다. 
 > - Linux VM에서 암호화 사용 안 함은 데이터 볼륨에 대해서만 지원됩니다. OS 볼륨이 암호화된 경우 이 설정은 데이터 또는 OS 볼륨에서 지원되지 않습니다.  


## <a name="bkmk_NewLinux"></a> 디스크 암호화를 사용하도록 설정된 새 Linux IaaS VM 배포 

1. [Resource Manager 템플릿](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)을 사용하여 암호화된 Linux IaaS VM을 새로 만듭니다. 이 템플릿은 관리 디스크를 사용하여 200GB RAID-0 배열과 전체 디스크 암호화가 있는 새 RedHat Linux 7.2 VM을 만듭니다. [FAQ](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) 문서에서 일부 Linux 배포는 데이터 디스크 암호화만 지원한다는 것을 알 수 있습니다. 그러나 이 템플릿은 여러 메서드를 사용하여 템플릿 배포 및 암호화 상태 확인에 익숙해질 수 있는 기회를 제공합니다. 
 
1. Azure Resource Manager 템플릿에서 **Azure에 배포**를 클릭합니다.

2. 구독, 리소스 그룹, 리소스 그룹 위치, 매개 변수, 약관 및 규약을 선택합니다. **만들기**를 클릭하여 기존 또는 실행 중인 IaaS VM에서 암호화를 사용하도록 설정합니다.

3. 템플릿이 배포되면 다음과 같은 기본 설정 방법을 사용하여 VM 암호화 상태를 확인합니다.
     - Azure CLI에서 [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show) 명령을 사용하여 확인합니다. 

         ```azurecli-interactive 
         az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
         ```

     - Azure PowerShell에서 [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) cmdlet을 사용하여 확인합니다. 

         ```azurepowershell-interactive
         Get-AzureRmVmDiskEncryptionStatus -ResourceGroupName 'MySecureRg' -VMName 'MySecureVM'
         ```

     - 포털에서 VM을 선택한 다음, **설정** 제목 아래에서 **디스크**를 클릭하여 암호화 상태를 확인합니다. **암호화**에 있는 차트에서 사용하도록 설정되어 있는 것을 볼 수 있습니다. 

| 매개 변수 | 설명 |
| --- | --- |
| AAD 클라이언트 ID | Key Vault에 비밀을 쓸 수 있는 권한이 있는 Azure AD 애플리케이션의 클라이언트 ID |
| AAD 클라이언트 비밀 | Key Vault에 비밀을 쓸 수 있는 권한이 있는 Azure AD 애플리케이션의 클라이언트 ID |
| Key Vault 이름 | 키가 배치되어야 하는 키 자격 증명 모음의 이름 |
| Key Vault 리소스 그룹 | Key Vault의 리소스 그룹 |


## <a name="bkmk_RunningLinux"> </a> 기존 또는 실행 중인 IaaS Linux VM에서 암호화 사용
이 시나리오에서는 Resource Manager 템플릿, PowerShell cmdlet 또는 CLI 명령을 사용하여 암호화를 사용하도록 설정할 수 있습니다. 

>[!IMPORTANT]
 >Azure Disk Encryption을 사용하도록 설정하려면 먼저 관리 디스크 기반 VM 인스턴스에 대한 스냅숏 및/또는 백업을 외부에서 수행해야 합니다. 포털에서 관리 디스크에 대한 스냅숏을 수행하거나 [Azure Backup](../backup/backup-azure-vms-encryption.md)을 사용할 수 있습니다. Backup은 암호화 중에 예기치 않은 오류가 발생할 경우 복구 옵션으로 사용할 수 있습니다. 백업을 만들면 Set-AzureRmVMDiskEncryptionExtension cmdlet에 -skipVmBackup 매개 변수를 지정하여 관리 디스크를 암호화하는 데 사용할 수 있습니다. 백업이 만들어지고 이 매개 변수가 지정되지 않으면 관리 디스크 기반 VM에 대한 Set-AzureRmVMDiskEncryptionExtension 명령은 실패하게 됩니다. 
>
>암호화하거나 암호화를 사용하지 않도록 설정하면 VM이 다시 부팅될 수 있습니다. 
>

### <a name="bkmk_RunningLinuxCLI"> </a> Azure CLI를 사용하여 기존 또는 실행 중인 Linux VM에서 암호화 사용 
[Azure CLI 2.0](/cli/azure) 명령줄 도구를 설치하고 사용하여 암호화된 VHD에서 디스크 암호화를 사용하도록 설정할 수 있습니다. [Azure Cloud Shell](../cloud-shell/overview.md)과 함께 브라우저에서 사용하거나 로컬 컴퓨터에 설치하여 PowerShell 세션에서 사용할 수 있습니다. Azure에서 기존 또는 실행 중인 IaaS Linux VM에 암호화를 사용하도록 설정하려면 다음 CLI 명령을 사용합니다.

Azure에서 [az vm encryption enable](/cli/azure/vm/encryption#az-vm-encryption-enable) 명령을 사용하여 실행 중인 IaaS 가상 머신에서 암호화를 사용하도록 설정합니다.

-  **클라이언트 비밀을 사용하여 실행 중인 VM 암호화:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **클라이언트 비밀을 래핑하는 KEK를 사용하여 실행 중인 VM 암호화:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

    >[!NOTE]
    > disk-encryption-keyvault 매개 변수의 값 구문은 전체 식별자 문자열, 즉 /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]입니다.</br> </br>
key-encryption-key 매개변수의 값 구문은 KEK의 전체 URI, 즉 https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id]입니다. 

- **디스크가 암호화되어 있는지 확인:** IaaS VM의 암호화 상태를 확인하려면 [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show) 명령을 사용합니다. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
     ```

- **암호화 사용 안 함:** 암호화를 사용하지 않도록 설정하려면 [az vm encryption disable](/cli/azure/vm/encryption#az-vm-encryption-disable) 명령을 사용합니다. 암호화 사용 안 함은 Linux VM용 데이터 볼륨에서만 허용됩니다.

     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type DATA
     ```

### <a name="bkmk_RunningLinuxPSH"> </a> PowerShell을 사용하여 기존 또는 실행 중인 Linux VM에서 암호화 사용
Azure에서 [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) cmdlet을 사용하여 실행 중인 IaaS 가상 머신에서 암호화를 사용하도록 설정합니다. 

-  **클라이언트 비밀을 사용하여 실행 중인 VM 암호화:** 아래 스크립트는 변수를 초기화하고 Set-AzureRmVMDiskEncryptionExtension cmdlet을 실행합니다. 리소스 그룹, VM, 키 자격 증명 모음, AAD 응용 프로그램 및 클라이언트 비밀은 필수 구성 요소로 이미 만들어져 있어야 합니다. MySecureRg, MySecureVM, MySecureVault, My-AAD-client-ID 및 My-AAD-client-secret를 사용자 고유의 값으로 바꿉니다. OS 디스크가 아닌 데이터 디스크를 암호화하는 경우 -VolumeType 매개 변수를 추가해야 할 수도 있습니다. 

     ```azurepowershell-interactive
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $aadClientID = 'My-AAD-client-ID';
      $aadClientSecret = 'My-AAD-client-secret';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```
- **클라이언트 비밀을 래핑하는 KEK를 사용하여 실행 중인 VM 암호화:** Azure Disk Encryption을 사용하면 키 자격 증명 모음에서 기존 키를 지정하여 암호화를 사용하도록 설정하는 동안 생성된 디스크 암호화 비밀을 래핑할 수 있습니다. 키 암호화 키가 지정되면 Azure Disk Encryption에서 해당 키를 사용하여 Key Vault에 쓰기 전에 암호화 비밀을 래핑합니다. OS 디스크가 아닌 데이터 디스크를 암호화하는 경우 -VolumeType 매개 변수를 추가해야 할 수도 있습니다. 

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;

     ```

 >[!NOTE]
 > disk-encryption-keyvault 매개 변수의 값 구문은 전체 식별자 문자열, 즉 /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]입니다.</br> </br>
key-encryption-key 매개변수의 값 구문은 KEK의 전체 URI, 즉 https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id]입니다. 
    
- **디스크가 암호화되어 있는지 확인:** IaaS VM의 암호화 상태를 확인하려면 [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) cmdlet을 사용합니다. 
    
     ```azurepowershell-interactive 
     Get-AzureRmVmDiskEncryptionStatus -ResourceGroupName MySecureRg -VMName MySecureVM
     ```
    
- **디스크 암호화 사용 안 함:** 암호화를 사용하지 않도록 설정하려면 [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption) cmdlet을 사용합니다. 암호화 사용 안 함은 Linux VM용 데이터 볼륨에서만 허용됩니다.
     
     ```azurepowershell-interactive 
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM'
     ```


### <a name="bkmk_RunningLinux"> </a> 템플릿을 사용하여 기존 또는 실행 중인 IaaS Linux VM에서 암호화 사용

[Resource Manager 템플릿](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm)을 사용하여 Azure에서 기존 또는 실행 중인 IaaS Linux VM에 디스크 암호화를 사용하도록 설정할 수 있습니다.

1. Azure 빠른 시작 템플릿에서 **Azure에 배포**를 클릭합니다.

2. 구독, 리소스 그룹, 리소스 그룹 위치, 매개 변수, 약관 및 규약을 선택합니다. **만들기**를 클릭하여 기존 또는 실행 중인 IaaS VM에서 암호화를 사용하도록 설정합니다.

다음 표에 Azure AD 클라이언트 ID를 사용하는 기존 또는 실행 중인 VM에 대한 Resource Manager 템플릿 매개 변수 목록이 나와 있습니다.

| 매개 변수 | 설명 |
| --- | --- |
| AADClientID | Key Vault에 비밀을 쓸 수 있는 권한이 있는 Azure AD 애플리케이션의 클라이언트 ID |
| AADClientSecret | Key Vault에 비밀을 쓸 수 있는 권한이 있는 Azure AD 애플리케이션의 클라이언트 ID |
| keyVaultName | 키가 업로드되어야 하는 Key Vault의 이름. `az keyvault show --name "MySecureVault" --query resourceGroup` Azure CLI cmdlet을 사용하여 가져올 수 있습니다. |
|  keyEncryptionKeyURL | 생성된 키를 암호화하는 데 사용되는 키 암호화 키의 URL. UseExistingKek 드롭다운 목록에서 **nokek**를 선택하면 이 매개 변수가 선택 사항입니다. UseExistingKek 드롭다운 목록에서 **kek**를 선택하면 _keyEncryptionKeyURL_ 값을 반드시 입력해야 합니다. |
| volumeType | 암호화 작업을 수행할 볼륨의 유형. 지원되는 유효한 값은 _OS_ 또는 _All_입니다. 위의 필수 구성 요소 섹션에서 지원되는 Linux 배포판과 해당 OS 버전을 확인하세요. |
| sequenceVersion | BitLocker 작업의 시퀀스 버전. 동일한 VM에서 디스크 암호화 작업이 수행될 때마다 이 버전 번호가 증가합니다. |
| vmName | 암호화 작업을 수행할 VM의 이름. |
| 암호 | 데이터 암호화 키로 강력한 암호를 입력합니다. |



## <a name="bkmk_EFA"> </a> Linux IaaS VM의 데이터 디스크에 EncryptFormatAll 기능 사용
**EncryptFormatAll** 매개 변수는 Linux 데이터 디스크가 암호화되는 시간을 줄여줍니다. 특정 조건을 충족하는 파티션은 현재 파일 시스템으로 포맷됩니다. 그런 다음, 명령 실행 이전의 위치에 다시 탑재됩니다. 조건을 충족하는 데이터 디스크를 제외하려면 명령을 실행하기 전에 분리할 수 있습니다.

 이 명령이 실행되면 이전에 탑재된 모든 드라이브를 포맷합니다. 그런 다음, 암호화 계층이 이제 비어 있는 드라이브를 기반으로 하여 시작됩니다. 이 옵션을 선택하면 VM에 연결된 사용 후 삭제 리소스 디스크도 암호화됩니다. 사용 후 삭제 드라이브가 다시 설정되면 다음 기회에 Azure Disk Encryption 솔루션에서 VM용으로 다시 포맷되고 다시 암호화됩니다.

>[!WARNING]
> VM의 데이터 볼륨에 필요한 데이터가 있는 경우 EncryptFormatAll을 사용하면 안됩니다. 디스크는 분리하여 암호화에서 제외할 수 있습니다. 프로덕션 VM에서 EncryptFormatAll을 시도하기 전에 먼저 테스트 VM에서 시도하여 이 기능 매개 변수와 그 의미를 이해해야 합니다. EncryptFormatAll 옵션은 데이터 디스크를 포맷하여 해당 데이터 디스크의 모든 데이터가 손실됩니다. 계속하기 전에 제외하려는 디스크가 제대로 분리되었는지 확인합니다. </br></br>
 >암호화 설정을 업데이트하는 동안 이 매개 변수를 설정하면 실제로 암호화되기 전에 다시 부팅될 수 있습니다. 이 경우 fstab 파일에서 포맷하지 않으려는 디스크를 제거하는 것도 좋습니다. 마찬가지로 암호화 작업을 시작하기 전에 암호화 형식으로 포맷하려는 파티션을 fstab 파일에 추가해야 합니다. 

### <a name="bkmk_EFACriteria"> </a> EncryptFormatAll 조건
매개 변수에서 아래 조건을 **모두** 충족하는 경우 모든 파티션으로 이동하여 암호화합니다. 
- 루트/OS/부팅 파티션이 아닙니다.
- 아직 암호화되지 않았습니다.
- BEK 볼륨이 아닙니다.
- RAID 볼륨이 아닙니다.
- LVM 볼륨이 아닙니다.
- 탑재되어 있습니다.

RAID 볼륨 또는 LVM 볼륨이 아닌, RAID 볼륨 또는 LVM 볼륨을 구성하는 디스크를 암호화합니다.

### <a name="bkmk_EFATemplate"> </a> 템플릿에서 EncryptFormatAll 매개변수 사용
EncryptFormatAll 옵션을 사용하려면 Linux VM을 암호화하고 AzureDiskEncryption 리소스에 대한 **EncryptionOperation** 필드를 변경하는 기존 Azure Resource Manager 템플릿을 모두 사용합니다.

1. 예를 들어 [Resource Manager 템플릿을 사용하여 실행 중인 Linux IaaS VM을 암호화](https://github.com/vermashi/azure-quickstart-templates/tree/encrypt-format-running-linux-vm/201-encrypt-running-linux-vm)합니다. 
2. Azure 빠른 시작 템플릿에서 **Azure에 배포**를 클릭합니다.
3. **EncryptionOperation**을 **EnableEncryption**에서 **EnableEncryptionFormatAl**로 변경합니다.
4. 구독, 리소스 그룹, 리소스 그룹 위치, 기타 매개 변수, 약관 및 규약을 선택합니다. **만들기**를 클릭하여 기존 또는 실행 중인 IaaS VM에서 암호화를 사용하도록 설정합니다.


### <a name="bkmk_EFAPSH"> </a> PowerShell cmdlet에서 EncryptFormatAll 매개변수 사용
[Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) cmdlet을 [EncryptFormatAll 매개 변수](https://www.powershellgallery.com/packages/AzureRM/5.0.0)와 함께 사용합니다. 

**클라이언트 비밀 및 EncryptFormatAll을 사용하여 실행 중인 VM 암호화:** 예를 들어, 아래 스크립트는 변수를 초기화하고 EncryptFormatAll 매개 변수를 사용하여 Set-AzureRmVMDiskEncryptionExtension cmdlet을 실행합니다. 리소스 그룹, VM, 키 자격 증명 모음, AAD 응용 프로그램 및 클라이언트 비밀은 필수 구성 요소로 이미 만들어져 있어야 합니다. MySecureRg, MySecureVM, MySecureVault, My-AAD-client-ID 및 My-AAD-client-secret를 사용자 고유의 값으로 바꿉니다.
  
   ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = 'MySecureVM';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
      
     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -EncryptFormatAll
   ```


### <a name="bkmk_EFALVM"> </a> LVM(논리 볼륨 관리자)에서 EncryptFormatAll 매개 변수 사용 
LVM-on-crypt 설정을 사용하는 것이 좋습니다. 다음의 모든 예제에서 device-path와 mountpoint를 사용 사례에 맞는 것으로 바꿉니다. 이 설정은 다음과 같이 수행할 수 있습니다.

- VM을 구성할 데이터 디스크를 추가합니다.
- 이러한 디스크를 포맷하고, 탑재하고, fstab 파일에 추가합니다.

    1. 새로 추가된 디스크를 포맷합니다. 여기서는 Azure에서 생성한 바로 가기 링크를 사용합니다. 바로 가기 링크를 사용하면 디바이스 이름 변경과 관련된 문제를 방지할 수 있습니다. 자세한 내용은 [디바이스 이름 문제 해결](../virtual-machines/linux/troubleshoot-device-names-problems.md) 문서를 참조하세요.
    
         `mkfs -t ext4 /dev/disk/azure/scsi1/lun0`
    
    2. 디스크를 탑재합니다.
         
         `mount /dev/disk/azure/scsi1/lun0 /mnt/mountpoint`
    
    3. fstab에 추가 합니다.
         
        `echo "/dev/disk/azure/scsi1/lun0 /mnt/mountpoint ext4 defaults,nofail 1 2" >> /etc/fstab`
    
    4. Set-AzureRmVMDiskEncryptionExtension PowerShell cmdlet을 -EncryptFormatAll과 함께 실행하여 이러한 디스크를 암호화합니다.
         ```azurepowershell-interactive
         Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName "MySecureGroup" -VMName "MySecureVM" -DiskEncryptionKeyVaultUrl "https://mykeyvault.vault.azure.net/" -EncryptFormatAll
         ```
    5. 이러한 새 디스크를 기반으로 하여 LVM을 설정합니다. VM 부팅이 완료되면 암호화된 드라이브는 잠금 해제됩니다. 따라서 LVM 탑재도 나중으로 지연되어야 합니다.




## <a name="bkmk_VHDpre"> </a> 고객 암호화 VHD 및 암호화 키로 만든 새 IaaS VM
이 시나리오에서는 Resource Manager 템플릿, PowerShell cmdlet 또는 CLI 명령을 사용하여 암호화를 사용하도록 설정할 수 있습니다. 다음 섹션에서는 Resource Manager 템플릿과 CLI 명령에 대해 자세히 설명합니다. 

부록의 지침을 사용하여 Azure에서 사용할 수 있는 미리 암호화된 이미지를 준비합니다. 이미지를 만든 후 다음 섹션의 단계를 사용하여 암호화된 Azure VM을 만들 수 있습니다.

* [사전에 암호화된 Windows VHD 준비](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [사전에 암호화된 Linux VHD 준비](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Azure Disk Encryption을 사용하기 전에 외부에 관리 디스크 기반 VM 인스턴스에 대해 스냅숏을 만들고 백업해야 합니다. 포털에서 관리 디스크에 대한 스냅숏을 수행하거나 [Azure Backup](../backup/backup-azure-vms-encryption.md)을 사용할 수 있습니다. Backup은 암호화 중에 예기치 않은 오류가 발생할 경우 복구 옵션으로 사용할 수 있습니다. 백업을 만들면 Set-AzureRmVMDiskEncryptionExtension cmdlet에 -skipVmBackup 매개 변수를 지정하여 관리 디스크를 암호화하는 데 사용할 수 있습니다. 백업이 만들어지고 이 매개 변수가 지정되지 않으면 관리 디스크 기반 VM에 대한 Set-AzureRmVMDiskEncryptionExtension 명령은 실패하게 됩니다. 
>
>암호화하거나 암호화를 사용하지 않도록 설정하면 VM이 다시 부팅될 수 있습니다. 



### <a name="bkmk_VHDprePSH"> </a> Azure PowerShell을 사용하여 미리 암호화된 VHD가 있는 IaaS VM 암호화 
[Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk#examples) PowerShell cmdlet을 사용하여 암호화된 VHD에서 디스크 암호화를 사용하도록 설정할 수 있습니다. 아래 예제에서는 몇 가지 공통 매개 변수를 제공합니다. 

```powershell
$VirtualMachine = New-AzureRmVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzureRmVM -VM $VirtualMachine -ResourceGroupName "MySecureRG"
```

### <a name="bkmk_VHDpreRM"> </a> Resource Manager 템플릿을 사용하여 암호화된 VHD가 있는 IaaS VM 암호화 
[Resource Manager 템플릿](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm)을 사용하여 암호화된 VHD에서 디스크 암호화를 사용하도록 설정할 수 있습니다.

1. Azure 빠른 시작 템플릿에서 **Azure에 배포**를 클릭합니다.

2. 구독, 리소스 그룹, 리소스 그룹 위치, 매개 변수, 약관 및 규약을 선택합니다. **만들기**를 클릭하여 새 IaaS VM에서 암호화를 사용하도록 설정합니다.

다음 표에 암호화된 VHD에 대한 Resource Manager 템플릿 매개 변수 목록이 나와 있습니다.

| 매개 변수 | 설명 |
| --- | --- |
| newStorageAccountName | 암호화된 OS VHD를 저장할 저장소 계정의 이름. 이 저장소 계정은 VM과 동일한 리소스 그룹 및 동일한 위치에 이미 만들어져 있어야 합니다. |
| osVhdUri | 저장소 계정에서 OS VHD의 URI |
| osType | OS 제품 유형(Windows/Linux) |
| virtualNetworkName | VM NIC가 속해야 하는 VNet의 이름 이 이름은 VM과 동일한 리소스 그룹 및 동일한 위치에 이미 만들어져 있어야 합니다. |
| subnetName | VM NIC가 속해야 하는 VNet의 서브넷 이름 |
| vmSize | VM의 크기. 현재 표준 A, D 및 G 시리즈만 지원됩니다. |
| keyVaultResourceID | Azure Resource Manager에서 Key Vault 리소스를 식별하는 ResourceID. PowerShell cmdlet `(Get-AzureRmKeyVault -VaultName &lt;MyKeyVaultName&gt; -ResourceGroupName &lt;MyResourceGroupName&gt;).ResourceId` 또는 Azure CLI 명령 `az keyvault show --name "MySecureVault" --query id`를 사용하여 가져올 수 있습니다.|
| keyVaultSecretUrl | Key Vault에 설정된 디스크 암호화 키의 URL |
| keyVaultKekUrl | 생성된 디스크 암호화 키를 암호화하기 위한 주요 암호화 키의 URL |
| vmName | IaaS VM의 이름 |

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>새로 추가된 데이터 디스크에서 암호화 사용
[az vm disk attach](../virtual-machines/linux/add-disk.md)를 사용하거나 [Azure Portal을 통해](../virtual-machines/linux/attach-disk-portal.md) 새 데이터 디스크를 추가할 수 있습니다. 암호화하려면 먼저 새로 연결된 데이터 디스크를 탑재해야 합니다. 암호화가 진행되는 동안 드라이브를 사용할 수 없으므로 데이터 드라이브의 암호화를 요청해야 합니다. 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Azure CLI를 사용하여 새로 추가된 디스크에서 암호화 사용
 VM이 이전에 "All"로 암호화된 경우에는 --volume-type 매개 변수가 All로 유지되어야 합니다. All은 OS 디스크 및 데이터 디스크 모두를 포함합니다. VM이 이전에 "OS" 볼륨 유형으로 암호화된 경우에는 OS와 새 데이터 디스크가 모두 포함되도록 --volume-type 매개 변수를 All로 변경해야 합니다. VM이 "Data" 볼륨 유형으로만 암호화된 경우에는 아래 설명처럼 "Data"로 유지하면 됩니다. VM에 새 데이터 디스크를 추가하고 연결하는 것으로는 암호화 준비에 충분하지 않습니다. 암호화를 사용하도록 설정하기 전에 새로 연결된 디스크를 포맷하고 VM 내에서 올바르게 마운트해야 합니다. Linux에서는 디스크를 [영구 블록 디바이스 이름](https://docs.microsoft.com/azure/virtual-machines/linux/troubleshoot-device-names-problems)으로 /etc/fstab에 마운트해야 합니다.  

Powershell 구문과 달리 CLI에서는 사용자가 암호화를 사용하도록 설정할 때 고유 시퀀스 버전을 제공하지 않아도 됩니다. CLI는 고유 시퀀스 버전 값을 자동으로 생성하여 사용합니다.

-  **클라이언트 비밀을 사용하여 실행 중인 VM 암호화:** 

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI/my Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault "MySecureVault" --volume-type "Data"
     ```

- **클라이언트 비밀을 래핑하는 KEK를 사용하여 실행 중인 VM 암호화:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --aad-client-id "<my spn created with CLI which is the Azure AD ClientID>"  --aad-client-secret "My-AAD-client-secret" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "Data"
     ```

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Azure PowerShell을 사용하여 새로 추가된 디스크에서 암호화 사용
 Powershell을 사용하여 Linux용 새 디스크를 암호화하는 경우 새 순서 버전을 지정해야 합니다. 순서 버전은 고유해야 합니다. 아래 스크립트는 시퀀스 버전에 대한 GUID를 생성합니다. 
 

-  **클라이언트 비밀을 사용하여 실행 중인 VM 암호화:** 아래 스크립트는 변수를 초기화하고 Set-AzureRmVMDiskEncryptionExtension cmdlet을 실행합니다. 리소스 그룹, VM, 키 자격 증명 모음, AAD 응용 프로그램 및 클라이언트 비밀은 필수 구성 요소로 이미 만들어져 있어야 합니다. MySecureRg, MySecureVM, MySecureVault, My-AAD-client-ID 및 My-AAD-client-secret를 사용자 고유의 값으로 바꿉니다. -VolumeType 매개 변수는 OS 디스크가 아닌 데이터 디스크로 설정됩니다. VM이 이전에 "OS" 또는 "All" 볼륨 유형으로 암호화된 경우에는 OS와 새 데이터 디스크가 모두 포함되도록 -VolumeType 매개 변수를 All로 변경해야 합니다.

     ```azurepowershell-interactive
      $sequenceVersion = [Guid]::NewGuid();
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $aadClientID = 'My-AAD-client-ID';
      $aadClientSecret = 'My-AAD-client-secret';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data' –SequenceVersion $sequenceVersion;
    ```
- **클라이언트 비밀을 래핑하는 KEK를 사용하여 실행 중인 VM 암호화:** Azure Disk Encryption을 사용하면 키 자격 증명 모음에서 기존 키를 지정하여 암호화를 사용하도록 설정하는 동안 생성된 디스크 암호화 비밀을 래핑할 수 있습니다. 키 암호화 키가 지정되면 Azure Disk Encryption에서 해당 키를 사용하여 Key Vault에 쓰기 전에 암호화 비밀을 래핑합니다. -VolumeType 매개 변수는 OS 디스크가 아닌 데이터 디스크로 설정됩니다. VM이 이전에 "OS" 또는 "All" 볼륨 유형으로 암호화된 경우에는 OS와 새 데이터 디스크가 모두 포함되도록 -VolumeType 매개 변수를 All로 변경해야 합니다.

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $aadClientID = 'My-AAD-client-ID';
     $aadClientSecret = 'My-AAD-client-secret';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId -VolumeType 'data';
     ```


>[!NOTE]
> disk-encryption-keyvault 매개 변수의 값 구문은 전체 식별자 문자열, 즉 /subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]입니다. </br> </br>
key-encryption-key 매개변수의 값 구문은 KEK의 전체 URI, 즉 https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id]입니다. 

## <a name="disable-encryption-for-linux-vms"></a>Linux VM에 대한 암호화 사용 안 함
Azure PowerShell, Azure CLI 또는 Resource Manager 템플릿을 사용하여 암호화를 사용하지 않도록 설정할 수 있습니다. 

>[!IMPORTANT]
>Linux VM에서 Azure Disk Encryption을 통한 암호화 사용 안 함은 데이터 볼륨에 대해서만 지원됩니다. OS 볼륨이 암호화된 경우 이 설정은 데이터 또는 OS 볼륨에서 지원되지 않습니다.  

- **Azure PowerShell을 사용하여 디스크 암호화 사용 안 함:** 암호화를 사용하지 않도록 설정하려면 [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption) cmdlet을 사용합니다. 
     ```azurepowershell-interactive
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM' [--volume-type {ALL, DATA, OS}]
     ```

- **Azure CLI를 사용하여 암호화 사용 안 함:** 암호화를 사용하지 않도록 설정하려면 [az vm encryption disable](/cli/azure/vm/encryption#az-vm-encryption-disable) 명령을 사용합니다. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type [ALL, DATA, OS]
     ```
- **Resource Manager 템플릿으로 암호화를 사용하지 않도록 설정:** [실행 중인 Linux VM에서 암호화 사용 안 함](https://aka.ms/decrypt-linuxvm) 템플릿을 사용하여 암호화를 사용하지 않도록 설정할 수 있습니다.
     1. **Deploy to Azure**를 클릭합니다.
     2. 구독, 리소스 그룹, 위치, VM, 약관 및 규약을 선택합니다.
     3.  **구매**를 클릭하여 실행 중인 Windows VM에서 디스크 암호화를 사용하지 않도록 설정합니다. 


## <a name="next-steps"></a>다음 단계
> [!div class="nextstepaction"]
> [Windows용 Azure Disk Encryption 사용](azure-security-disk-encryption-windows-aad.md)
