---
title: PowerShell을 사용하여 Azure AD 앱 만들기 및 Azure Media Services API 액세스 | Microsoft Docs
description: PowerShell을 사용하여 Azure AD(Azure Active Directory) 앱을 만들고 Azure Media Services API에 액세스하도록 설정하는 방법을 알아봅니다.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/29/2018
ms.author: juliako
ms.openlocfilehash: bd7938fb600a1a0cd65b89fde6ca25135306b76d
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50232645"
---
# <a name="use-powershell-to-create-an-azure-ad-app-to-use-with-the-azure-media-services-api"></a>PowerShell을 사용하여 Azure Media Services API와 함께 사용할 Azure AD 앱 만들기

PowerShell 스크립트를 사용하여 Azure Media Services 리소스에 액세스하기 위한 Azure AD(Azure Active Directory) 애플리케이션 및 서비스 주체를 만드는 방법에 대해 알아봅니다.  

## <a name="prerequisites"></a>필수 조건

- Azure 계정. 계정이 없는 경우 [Azure 평가판](https://azure.microsoft.com/pricing/free-trial/)으로 시작하세요. 
- Media Services 계정. 자세한 내용은 [Azure Portal에서 Azure Media Services 계정 만들기](media-services-portal-create-account.md)를 참조하세요.
- Azure PowerShell 버전 0.8.8 이상. 자세한 내용은 [Azure PowerShell 사용 방법](https://docs.microsoft.com/powershell/azure/overview)을 참조하세요.
- Azure Resource Manager cmdlet.  

## <a name="create-an-azure-ad-app-by-using-powershell"></a>PowerShell을 사용하여 Azure AD 앱 만들기  

```powershell
Connect-AzureRmAccount
Import-Module AzureRM.Resources
Set-AzureRmContext -SubscriptionId $SubscriptionId
$ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password

Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 
$NewRole = $null
$Scope = "/subscriptions/your subscription id/resourceGroups/userresourcegroup/providers/microsoft.media/mediaservices/your media account"

$Retries = 0;While ($NewRole -eq $null -and $Retries -le 6)
{
    # Sleep here for a few seconds to allow the service principal application to become active (usually, it will take only a couple of seconds)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
}
```

자세한 내용은 다음 문서를 참조하세요.

- [Azure PowerShell을 사용하여 리소스에 액세스하는 서비스 주체 만들기](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)
- [Azure PowerShell을 사용하여 역할 기반 Access Control 관리](../../role-based-access-control/role-assignments-powershell.md)
- [인증서를 사용하여 디먼 앱을 수동으로 구성하는 방법](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a>다음 단계

[계정에 파일 업로드](media-services-portal-upload-files.md) 시작
