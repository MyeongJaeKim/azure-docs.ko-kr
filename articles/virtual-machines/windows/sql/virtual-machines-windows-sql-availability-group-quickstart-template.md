---
title: Azure 빠른 시작 템플릿을 사용하여 SQL Server VM에서 WSFC, 수신기 만들기 및 Always On 가용성 그룹에 대해 ILB 구성
description: Azure 빠른 시작 템플릿을 사용하면 템플릿을 통해 클러스터를 만들고, SQL VM을 클러스터에 연결하고, 수신기를 만들고, ILB를 구성하여 Azure에서 SQL Server VM에 대한 가용성 그룹을 만드는 프로세스를 간소화할 수 있습니다.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/04/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 9be8717bc9b1d15a59486edf206dd0657a711c06
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54360558"
---
# <a name="create-wsfc-listener-and-configure-ilb-for-an-always-on-availability-group-on-a-sql-server-vm-with-azure-quickstart-template"></a>Azure 빠른 시작 템플릿을 사용하여 SQL Server VM에서 WSFC, 수신기 만들기 및 Always On 가용성 그룹에 대해 ILB 구성
이 문서에서는 Azure 빠른 시작 템플릿을 사용하여 Azure에서 SQL Server Virtual Machines에 대한 Always On 가용성 그룹 구성의 배포를 부분적으로 자동화하는 방법을 설명합니다. 이 프로세스에서 사용되는 두 개의 Azure 빠른 시작 템플릿이 있습니다. 

   | Template | 설명 |
   | --- | --- |
   | [101-sql-vm-ag-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup) | Windows 장애 조치(failover) 클러스터를 만들고 SQL Server VM을 연결합니다. |
   | [101-sql-vm-aglistener-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup) | 가용성 그룹 수신기를 만들고 내부 Load Balancer를 구성합니다. |
   | &nbsp; | &nbsp; |

가용성 그룹 만들기 및 내부 Load Balancer 만들기와 같은 가용성 그룹 구성의 기타 부분은 수동으로 수행해야 합니다. 이 문서에서는 자동 및 수동 단계의 시퀀스를 제공합니다.
 

## <a name="prerequisites"></a>필수 조건 
빠른 시작 템플릿을 사용하여 Always On 가용성 그룹의 설정을 자동화하려면 다음과 같은 필수 조건이 이미 있어야 합니다. 
- [Azure 구독](https://azure.microsoft.com/free/).
- [도메인 컨트롤러](https://docs.microsoft.com/azure/architecture/reference-architectures/identity/adds-forest)를 포함하는 리소스 그룹 
- [SQL VM 리소스 공급자에 등록](#register-existing-sql-vm-with-new-resource-provider)되었으며, 동일한 가용성 집합 또는 가용성 영역에서 [SQL Server 2016 이상의 Enterprise Edition을 실행하는 Azure의 도메인 가입 VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) 하나 이상  

## <a name="register-existing-sql-vm-with-new-resource-provider"></a>새 리소스 공급자에 기존 SQL VM 등록
이러한 가용성 그룹 Azure 빠른 시작 템플릿은 SQL VM 리소스 공급자(Microsoft.SqlVirtualMachine)를 사용하므로 기존 SQL Server VM이 SQL VM 리소스 공급자에 등록되어야 합니다. 2018년 12월 이후에 SQL Server VM을 만든 경우 이 날짜 이후에 만든 SQL Server VM은 모두 자동으로 등록되므로 이 단계를 건너뜁니다. 이 섹션에서는 Azure Portal을 사용하여 공급자에 등록하는 단계를 제공하지만 [PowerShell](virtual-machines-windows-sql-ahb.md#powershell)을 사용할 수도 있습니다. 

  >[!IMPORTANT]
  > SQL Server VM 리소스를 삭제하면 이미지의 하드 코드된 라이선스 설정으로 돌아갑니다. 

1. Azure Portal을 열고 **모든 서비스**로 이동합니다. 
1. **구독**으로 이동하고 관심 있는 구독을 선택합니다.  
1. **구독** 블레이드에서 **리소스 공급자**로 이동합니다. 
1. 필터에 `sql`을 입력하여 SQL 관련 리소스 공급자를 표시합니다. 
1. 원하는 작업에 따라 **Microsoft.SqlVirtualMachine** 공급자에 대해 ‘등록’, ‘다시 등록’ 또는 ‘등록 해제’를 선택합니다. 

  ![공급자 수정](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

## <a name="step-1---create-the-wsfc-and-join-sql-server-vms-to-the-cluster-using-quickstart-template"></a>1단계 - 빠른 시작 템플릿을 사용하여 WSFC를 만들고 SQL Server VM을 클러스터에 연결 
SQL Server VM이 SQL VM 새 리소스 공급자에 등록되고 나면 SQL Server VM을 *SqlVirtualMachineGroup*에 연결할 수 있습니다. 이 리소스는 버전, 에디션, 정규화된 도메인 이름, 클러스터를 관리할 AD 계정, 클라우드 감시로서의 스토리지 계정 등 Windows 장애 조치(failover) 클러스터의 메타데이터를 정의합니다. SQL Server VM을 *SqlVirtualMachineGroup*에 추가하면 Windows 장애 조치(failover) 클러스터 서비스가 부트스트랩되고 SQL Server VM이 클러스터에 연결됩니다. 이 단계는 **101-sql-vm-ag-setup** 빠른 시작 템플릿을 통해 자동화되며 다음 단계로 구현될 수 있습니다.

1. [**101-sql-vm-ag-setup**](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup) 빠른 시작 템플릿으로 이동한 다음, **Azure에 배포**를 선택하여 Azure Portal 내에서 빠른 시작 템플릿을 시작합니다.
1. 필수 필드에 정보를 입력하여 Windows 장애 조치(failover) 클러스터 메타데이터를 구성합니다. 선택적 필드는 비워 둘 수 있습니다.

    다음 표에는 템플릿에 필요한 값이 나와 있습니다. 

   | **필드** | 값 |
   | --- | --- |
   | **구독** |  SQL Server VM이 있는 구독입니다. |
   |**리소스 그룹** | SQL Server VM이 있는 리소스 그룹입니다. | 
   |**장애 조치(failover) 클러스터 이름** | 새 Windows 장애 조치(failover) 클러스터에 사용할 이름입니다. |
   | **기존 VM 목록** | 가용성 그룹에 참여하고 새 클러스터의 일부로 포함하려는 SQL Server VM입니다. 이러한 값을 쉼표와 공백으로 구분합니다(예: SQLVM1, SQLVM2). |
   | **SQL Server 버전**: | 드롭다운에서 SQL Server VM의 SQL Server 버전을 선택합니다. 현재 SQL 2016 및 SQL 2017 이미지만 지원됩니다. |
   | **기존 정규화된 도메인 이름** | SQL Server VM이 있는 도메인의 기존 FQDN입니다. |
   | **기존 도메인 계정** | SQL Server에 대한 sysadmin 액세스 권한이 있는 기존 도메인 계정입니다. | 
   | **도메인 계정 암호** | 이전에 언급한 도메인 계정의 암호입니다. | 
   | **기존 SQL 서비스 계정** | SQL Server 서비스를 제어하는 데 사용되는 도메인 사용자 계정입니다. 이 정보는 [**SQL Server 구성 관리자**](https://docs.microsoft.com/sql/relational-databases/sql-server-configuration-manager?view=sql-server-2017)를 사용하여 확인할 수 있습니다. |
   | **SQL 서비스 암호** | SQL Server 서비스를 제어하는 도메인 사용자 계정에서 사용하는 암호입니다. |
   | &nbsp; | &nbsp; |

1. 사용 약관에 동의하는 경우 **위에 명시된 사용 약관에 동의함** 옆에 있는 확인란을 선택하고 **구매**를 선택하여 빠른 시작 템플릿 배포를 완료합니다. 
1. 배포를 모니터링하려면 상단 탐색 배너의 **알림** 벨 아이콘에서 배포를 선택하거나 Azure Portal의 **리소스 그룹**으로 이동하여 **설정** 필드에서 **배포**를 선택하고 ‘Microsoft.Template’ 배포를 선택합니다. 

## <a name="step-2---manually-create-the-availability-group"></a>2단계 - 수동으로 가용성 그룹 만들기 
[PowerShell](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell?view=sql-server-2017), [SQL Server Management Studio](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio?view=sql-server-2017) 또는 [Transact-SQL](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/create-an-availability-group-transact-sql?view=sql-server-2017)을 사용하여 일반적인 방법으로 가용성 그룹을 수동으로 만듭니다. 

  >[!IMPORTANT]
  > 지금 수신기를 만들지는 **마세요**. 이 작업은 4단계의 **101-sql-vm-aglistener-setup** 빠른 시작 템플릿을 통해 자동으로 수행됩니다. 

## <a name="step-3---manually-create-the-internal-load-balancer-ilb"></a>3단계 - 수동으로 ILB(내부 Load Balancer) 만들기
Always On AG(가용성 그룹) 수신기에는 ILB(내부 Azure Load Balancer)가 필요합니다. ILB는 AG 수신기에 대해 “부동” IP 주소를 제공하므로 더 빠른 장애 조치(failover) 및 재연결이 가능합니다. 가용성 그룹의 SQL Server VM이 동일한 가용성 집합의 일부인 경우 기본 Load Balancer를 사용할 수 있습니다. 그렇지 않으면 표준 Load Balancer를 사용해야 합니다.  **ILB는 SQL Server VM 인스턴스와 동일한 VNet에 있어야 합니다.** ILB만 만들면 되고, 나머지 구성(예: 백 엔드 풀, 상태 프로브 및 부하 분산 규칙)은 4단계의 **101-sql-vm-aglistener-setup** 빠른 시작 템플릿을 통해 처리됩니다. 

1. Azure 포털에서 SQL Server 가상 머신을 포함하는 리소스 그룹을 엽니다. 
2. 리소스 그룹에서 **추가**를 클릭합니다.
3. **부하 분산 장치**를 검색한 후 검색 결과에서 **Microsoft**에서 게시하는 **부하 분산 장치**를 선택합니다.
4. **부하 분산 장치** 블레이드에서 **만들기**를 클릭합니다.
5. **부하 분산 장치 만들기** 대화 상자에서 다음과 같이 부하 분산 장치를 구성합니다.

   | 설정 | 값 |
   | --- | --- |
   | **Name** |부하 분산 장치를 나타내는 텍스트 이름입니다. 예를 들어 **sqlLB**입니다. |
   | **형식** |**내부**: 대부분의 구현에서는 동일한 가상 네트워크 내에 있는 애플리케이션이 가용성 그룹에 연결할 수 있도록 하는 내부 부하 분산 장치를 사용합니다.  </br> **외부**: 애플리케이션이 공용 인터넷 연결을 통해 가용성 그룹에 연결할 수 있도록 합니다. |
   | **가상 네트워크** |SQL Server 인스턴스가 있는 가상 네트워크를 선택합니다. |
   | **서브넷** |SQL Server 인스턴스가 있는 서브넷을 선택합니다. |
   | **IP 주소 할당** |**정적** |
   | **개인 IP 주소** |서브넷에서 사용 가능한 IP 주소를 지정합니다. 클러스터에서 수신기를 만들 때 이 IP 주소를 사용합니다.|
   | **구독** |구독이 여러 개인 경우 이 필드가 나타날 수 있습니다. 이 리소스와 연결할 구독을 선택합니다. 일반적으로 가용성 그룹에 대한 모든 리소스와 동일한 구독입니다. |
   | **리소스 그룹** |SQL Server 인스턴스가 있는 리소스 그룹을 선택합니다. |
   | **위치**: |SQL Server 인스턴스가 있는 Azure 위치를 선택합니다. |
   | &nbsp; | &nbsp; |

6. **만들기**를 선택합니다. 


  >[!NOTE]
  > 각 SQL Server VM에 대한 공용 IP 리소스에 표준 Load Balancer와 호환되는 표준 SKU가 있어야 합니다. VM 공용 IP 리소스의 SKU를 확인하려면 **리소스 그룹**으로 이동하여 원하는 SQL Server VM에 대한 **공용 IP 주소** 리소스를 선택하고 **개요** 창의 **SKU** 아래에서 값을 찾습니다. 

## <a name="step-4---create-the-ag-listener-and-configure-the-ilb-with-the-quickstart-template"></a>4단계 - AG 수신기를 만들고 빠른 시작 템플릿을 사용하여 ILB 구성

가용성 그룹 수신기를 만들고 **101-sql-vm-aglistener-setup** 빠른 시작 템플릿을 사용하여 ILB(내부 부하 분산 장치)를 자동으로 구성합니다. 이 템플릿은 Microsoft.SqlVirtualMachine/Sql Virtual Machine Groups/Availability Group Listener 리소스를 프로비전합니다. SQL VM 리소스 공급자를 통해 **101-sql-vm-aglistener-setup** 빠른 시작 템플릿은 다음 작업을 수행합니다.

 - 클러스터 및 ILB에 대한 네트워크 설정을 구성합니다. 
 - ILB 백 엔드 풀, 상태 프로브 및 부하 분산 규칙을 구성합니다.
 - 지정된 IP 주소 및 이름을 사용하여 AG 수신기를 만듭니다.

ILB를 구성하고 AG 수신기를 만들려면 다음을 수행합니다.
1. [**101-sql-vm-aglistener-setup**](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup) 빠른 시작 템플릿으로 이동한 다음, **Azure에 배포**를 선택하여 Azure Portal 내에서 빠른 시작 템플릿을 시작합니다.
1. 필수 필드에 정보를 입력하여 ILB를 구성하고 가용성 그룹 수신기를 만듭니다. 선택적 필드는 비워 둘 수 있습니다. 

    다음 표에는 템플릿에 필요한 값이 나와 있습니다. 

   | **필드** | 값 |
   | --- | --- |
   |**리소스 그룹** | SQL Server VM 및 가용성 그룹이 있는 리소스 그룹입니다. | 
   |**기존 장애 조치(failover) 클러스터 이름** | SQL Server VM이 연결된 클러스터의 이름입니다. |
   | **기존 SQL 가용성 그룹**| SQL Server VM이 속한 가용성 그룹의 이름입니다. |
   | **기존 VM 목록** | 이전에 언급한 가용성 그룹에 속한 SQL Server VM의 이름입니다. 이름은 쉼표와 공백으로 구분되어야 합니다(예: SQLVM1, SQLVM2). |
   | **기존 정규화된 도메인 이름** | SQL Server VM이 있는 도메인의 기존 FQDN입니다. |
   | **수신기** | 수신기에 할당할 DNS 이름입니다. 기본적으로 이 템플릿은 ‘aglistener’라는 이름을 지정하지만 변경할 수 있습니다. |
   | **수신기 포트** | 수신기에서 사용할 포트입니다. 일반적으로 이 포트는 기본 포트인 1433이어야 하므로, 해당 포트 번호가 이 템플릿에서 지정됩니다. 그러나 기본 포트가 변경된 경우에는 수신기 포트에서 해당 값을 대신 사용해야 합니다. | 
   | **기존 VNet** | SQL Server VM 및 ILB가 있는 VNet의 이름입니다. |
   | **기존 서브넷** | SQL Server VM의 내부 서브넷 ‘이름’(예: default)입니다. 이 값은 **리소스 그룹**으로 이동하여 **VNet**을 선택하고 **설정** 창 아래에서 **서브넷**을 선택한 다음, **이름** 아래의 값을 복사하여 확인할 수 있습니다. |
   | **기존 내부 Load Balancer** | 3단계에서 만든 ILB의 이름입니다. |
   | **프로브 포트** | ILB에서 사용하려는 프로브 포트입니다. 템플릿은 기본적으로 59999를 사용하지만 이 값을 변경할 수 있습니다. |
   | &nbsp; | &nbsp; |

1. 사용 약관에 동의하는 경우 **위에 명시된 사용 약관에 동의함** 옆에 있는 확인란을 선택하고 **구매**를 선택하여 빠른 시작 템플릿 배포를 완료합니다. 
1. 배포를 모니터링하려면 상단 탐색 배너의 **알림** 벨 아이콘에서 배포를 선택하거나 Azure Portal의 **리소스 그룹**으로 이동하여 **설정** 필드에서 **배포**를 선택하고 ‘Microsoft.Template’ 배포를 선택합니다. 

  >[!NOTE]
  >배포에 실패하는 경우 **101-sql-vm-aglistener-setup** 빠른 시작 템플릿을 다시 배포하기 전에 PowerShell을 사용하여 [새로 만든 수신기를 수동으로 제거](#remove-availability-group-listener)해야 합니다. 

## <a name="remove-availability-group-listener"></a>가용성 그룹 수신기 제거
템플릿을 통해 구성된 가용성 그룹 수신기를 나중에 제거해야 하는 경우 SQL VM 리소스 공급자를 이용해야 합니다. 수신기가 SQL VM 리소스 공급자를 통해 등록되었으므로 SQL Server Management Studio를 통해 수신기를 삭제하는 것만으로는 충분하지 않습니다. 실제로 PowerShell을 사용하여 SQL VM 리소스 공급자를 통해 삭제해야 합니다. 이렇게 하면 SQL VM 리소스 공급자에서 AG 수신기 메타데이터가 제거되고 가용성 그룹에서 수신기가 물리적으로 삭제됩니다. 

다음 코드 조각은 SQL 리소스 공급자 및 가용성 그룹에서 SQL 가용성 그룹 수신기를 삭제합니다. 

```PowerShell
# Remove the AG listener
# example: Remove-AzureRmResource -ResourceId '/subscriptions/a1a11a11-1a1a-aa11-aa11-1aa1a11aa11a/resourceGroups/SQLAG-RG/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/Cluster/availabilitygrouplisteners/aglistener' -Force
Remove-AzureRmResource -ResourceId '/subscriptions/<SubscriptionID>/resourceGroups/<resource-group-name>/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/<cluster-name>/availabilitygrouplisteners/<listener-name>' -Force
```
 
## <a name="known-issues-and-errors"></a>알려진 문제 및 오류
이 섹션에서는 몇 가지 알려진 문제와 가능한 해결 방법을 설명합니다. 

### <a name="availability-group-listener-for-availability-group-ag-name-already-exists"></a>가용성 그룹 ‘\<AG-Name>’의 가용성 그룹 수신기가 이미 있음
AG 수신기 Azure 빠른 시작 템플릿에 사용된 선택한 가용성 그룹에서 수신기가 가용성 그룹 내에 물리적으로 또는 SQL VM 리소스 공급자 내에 메타데이터로 포함되어 있습니다.  **101-sql-vm-aglistener-setup** 빠른 시작 템플릿을 다시 배포하기 전에 [PowerShell](#remove-availability-group-listener)을 사용하여 수신기를 제거합니다. 

### <a name="connection-only-works-from-primary-replica"></a>연결이 주 복제본에서만 작동함
이 동작은 ILB 구성을 일관성 없는 상태로 두는, 실패한 **101-sql-vm-aglistener-setup** 템플릿 배포에서 발생할 가능성이 큽니다. 백 엔드 풀에 가용성 집합이 나열되는지, 상태 프로브 및 부하 분산 규칙에 대한 규칙이 있는지 확인합니다. 누락된 항목이 있으면 ILB 구성이 일관성 없는 상태입니다. 

이 동작을 해결하려면 [PowerShell](#remove-availability-group-listener)을 사용하여 수신기를 제거하고 Azure Portal을 통해 내부 Load Balancer를 삭제한 다음, [3단계](#step-3---manually-create-the-internal-load-balanced-ilb)에서 다시 시작합니다. 

### <a name="badrequest---only-sql-virtual-machine-list-can-be-updated"></a>BadRequest - SQL 가상 머신 목록만 업데이트할 수 있습니다.
이 오류는 수신기가 SSMS(SQL Server Management Studio)를 통해 삭제되었지만 SQL VM 리소스 공급자에서 삭제되지 않은 경우 **101-sql-vm-aglistener-setup** 템플릿을 배포할 때 발생할 수 있습니다. SSMS를 통해 수신기를 삭제해도 수신기의 메타데이터는 SQL VM 리소스 공급자에서 제거되지 않습니다. [PowerShell](#remove-availability-group-listener)을 사용하여 리소스 공급자에서 수신기를 삭제해야 합니다. 


## <a name="next-steps"></a>다음 단계

자세한 내용은 다음 문서를 참조하세요. 

* [SQL Server VM 개요](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server VM FAQ](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server VM 가격 안내](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server VM 릴리스 정보](virtual-machines-windows-sql-server-iaas-release-notes.md)
* [SQL Server VM에 대한 라이선스 모델 전환](virtual-machines-windows-sql-ahb.md)



