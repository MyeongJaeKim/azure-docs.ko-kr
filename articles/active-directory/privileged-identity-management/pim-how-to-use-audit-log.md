---
title: PIM에서 Azure AD 디렉터리 역할에 대한 감사 기록 보기 | Microsoft Docs
description: Azure AD PIM(Privileged Identity Management)에서 Azure AD 디렉터리 역할에 대한 감사 기록을 보는 방법을 알아봅니다.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 02/14/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: a4dbce678d5d3eac106363e958636810ea9040bb
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180214"
---
# <a name="view-audit-history-for-azure-ad-directory-roles-in-pim"></a>PIM에서 Azure AD 디렉터리 역할에 대한 감사 기록 보기
Azure Privileged Identity Management(PIM) 감사 기록을 사용하여 권한 있는 모든 역할에 대한 지정된 기간의 모든 사용자 할당 및 활성화를 볼 수 있습니다. 테넌트에서 관리자, 최종 사용자 및 동기화 작업을 비롯한 활동의 전체 감사 기록을 보려는 경우 [Azure Active Directory 액세스 및 사용 보고서](../reports-monitoring/overview-reports.md)

## <a name="navigate-to-audit-history"></a>감사 기록으로 이동
[Azure Portal](https://portal.azure.com) 대시보드에서 **Azure AD Privileged Identity Management** 앱을 선택합니다. 여기에서 PIM 대시보드에 **권한 있는 역할 관리** > **감사 기록**을 클릭하여 감사 기록에 액세스합니다.

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
[작업]별로 데이터를 정렬하고 "활성화 승인됨"을 찾습니다.


## <a name="audit-history-graph"></a>감사 기록 그래프
감사 기록을 사용하면 총 활성화 수, 일일 최대 활성화, 일일 평균 활성화를 선 그래프로 볼 수 있습니다.  감사 기록에 역할이 둘 이상인 경우 역할별로 데이터를 필터링할 수도 있습니다.

**시간**, **작업** 및 **역할** 단추를 사용하여 기록을 정렬할 수 있습니다.

## <a name="audit-history-list"></a>감사 기록 목록
감사 기록 목록의 열은 다음과 같습니다.

* **요청자** - 역할 활성화 또는 변경을 요청한 사용자입니다.  값이 "Azure 시스템"인 경우 Azure 감사 기록에서 자세한 내용을 확인합니다.
* **사용자** - 활성화되어 있거나 역할에 할당된 사용자입니다.
* **역할** - 사용자가 할당하거나 활성화한 역할입니다.
* **작업** - 요청자가 수행하는 작업입니다. 여기에는 할당, 할당 해제, 활성화 또는 비활성화가 포함될 수 있습니다.
* **Time** - 작업이 발생한 시간.
* **Reasoning** - 활성화 중에 reason(이유) 필드에 입력한 텍스트가 있는 경우 여기에 표시됩니다.
* **만료** - 역할의 활성화에만 관련이 있습니다.

## <a name="filter-audit-history"></a>감사 기록 필터링
**필터** 단추를 클릭하여 감사 기록에 표시되는 정보를 필터링할 수 있습니다.  **Update chart parameters blade** (차트 매개 변수 업데이트 블레이드)가 표시됩니다.

필터를 설정한 후 **업데이트** 를 클릭하여 기록의 데이터를 필터링합니다.  데이터가 바로 표시되지 않으면 페이지를 새로 고칩니다.

### <a name="change-the-date-range"></a>날짜 범위 변경
**오늘**, **지난 주**, **지난 달**, **사용자 지정** 단추를 사용하여 감사 기록의 시간 범위를 변경합니다.

**사용자 지정** 단추를 선택하면 기록에 대해 날짜 범위를 지정하도록 **시작** 날짜 필드와 **끝** 날짜 필드가 제공됩니다.  MM/DD/YYYY 형식으로 날짜를 입력하거나 **달력** 아이콘을 클릭하여 달력에서 날짜를 선택할 수 있습니다.

### <a name="change-the-roles-included-in-the-history"></a>기록에 포함된 역할 변경
기록에 포함시키거나 제외한 각 역할의 옆에 있는 **역할** 확인란을 선택 또는 선택 해제합니다.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>다음 단계

- [PIM에서 Azure 리소스 역할에 대한 작업 및 감사 기록 보기](azure-pim-resource-rbac.md)
