---
title: Azure Stack SQL 리소스 공급자 1.1.30.0 릴리스 정보 | Microsoft Docs
description: 알려진된 문제를 포함 하 여 최신 Azure Stack SQL 리소스 공급자 업데이트에 포함 된 내용 및 다운로드 위치를 알아봅니다.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/09/2019
ms.author: jeffgilb
ms.reviewer: jiahan
ms.lastreviewed: 1/09/2019
ms.openlocfilehash: aaa4dc48f237a4647cfde330e40eda91eeb1388d
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55768735"
---
# <a name="sql-resource-provider-11300-release-notes"></a>SQL 리소스 공급자 1.1.30.0 릴리스 정보

*적용 대상: Azure Stack 통합 시스템 및 Azure Stack 개발 키트*

이러한 릴리스 향상 된 기능 및 SQL 리소스 공급자 버전 1.1.30.0의에서 알려진된 문제에 설명 합니다.

## <a name="build-reference"></a>빌드 참조
이진 SQL 리소스 공급자를 다운로드 하 고 임시 디렉터리에 콘텐츠를 추출할 자동 압축 풀기 프로그램을 실행 합니다. 리소스 공급자에는 빌드를 최소 해당 Azure Stack에 있습니다. 이 버전의 SQL 리소스 공급자를 설치 하는 데 필요한 최소 Azure Stack 릴리스 버전은 다음과 같습니다.

> |Azure Stack의 최소 버전|SQL 리소스 공급자 버전|
> |-----|-----|
> |버전 1808 (1.1808.0.97)|[1.1.30.0](https://aka.ms/azurestacksqlrp11300)|
> |     |     |

> [!IMPORTANT]
> Azure Stack 통합 시스템에 최소 지원 되는 Azure Stack 업데이트를 적용 하거나 SQL 리소스 공급자의 최신 버전을 배포 하기 전에 최신 Azure Stack 개발 키트 ASDK ()를 배포 합니다.

## <a name="new-features-and-fixes"></a>새 기능 및 수정
이 버전의 Azure Stack SQL 리소스 공급자는 다음과 같은 향상 된 기능 및 수정 합니다.

- **SQL 리소스 공급자 배포에 사용 하도록 설정 하는 원격 분석**합니다. SQL 리소스 공급자 배포에 대 한 원격 분석 수집을 설정 합니다. 리소스 공급자 배포를 포함 하는 수집 된 원격 분석, 시작 및 중지 시간, 상태, 종료 메시지 및 오류 세부 정보 (있는 경우)을 종료 합니다.

- **TLS 1.2 암호화 업데이트**합니다. 설정 된 TLS 1.2 전용 내부 Azure Stack 구성 요소를 사용 하 여 리소스 공급자 통신에 대 한 지원. 

### <a name="fixes"></a>수정 프로그램

- **SQL 리소스 공급자 Azure Stack PowerShell 호환성**합니다. SQL 리소스 공급자가 AzureRM 1.3.0으로 호환성을 제공 하 고 Azure Stack 2018-03-01-하이브리드 PowerShell 프로필을 사용 하 여 작업을 나중에 업데이트 되었습니다.

- **SQL 로그인 변경 암호 블레이드가**합니다. 문제를 해결 하 고 변경 암호 블레이드에서 암호를 변경할 수 없는 키를 누릅니다. 제거 링크에서 암호 변경 알림.

- **SQL server 설정 블레이드에서 업데이트를 호스팅**합니다. 문제를 해결 하 고 있는 설정 블레이드에서 올바르게 제목 없습니다 "Password"로 키를 누릅니다.

## <a name="known-issues"></a>알려진 문제 

- **SQL Sku은 포털에 표시 되도록 최대 1 시간까지 걸릴 수 있습니다**합니다. 까지 걸릴 수 있으므로 한 시간에 새로 만든된 Sku 새 SQL database를 만들 때 사용 하 여 볼 수 있도록 합니다. 

    **해결 방법**: 없음.

- **SQL 로그인을 다시**입니다. 새 SQL을 만들려고 시도 동일한 구독에서 기존 로그인으로 동일한 사용자 이름 사용 하 여 로그인 하면 동일한 로그인 및 기존 암호를 다시 사용 합니다. 

    **해결 방법**: 동일한 구독에서 새 로그인을 만들 때 다른 사용자를 사용 하거나 다른 구독에서 동일한 사용자 이름을 사용 하 여 로그인을 만듭니다.

- **공유 SQL 로그인 데이터 불일치가 발생할**합니다. SQL 로그인을 동일한 구독에서 여러 SQL database에 대 한 공유 인 경우 데이터 불일치가 발생할 수 있는 로그인 암호를 변경 합니다.

    **해결 방법**: 항상 동일한 구독에서 다른 데이터베이스에 대 한 다른 로그인을 사용 합니다.

- **TLS 1.2 지원 요구 사항**합니다. 배포 또는 TLS 1.2 해제 되어 없는 컴퓨터에서 SQL 리소스 공급자를 업데이트 하려고 하면 작업이 실패할 수 있습니다. 배포 또는 지원 되는 TLS 1.2가 반환 되는 확인 하려면 리소스 공급자를 업데이트 하는 데 사용 되는 컴퓨터에서 다음 PowerShell 명령을 실행 합니다.

  ```powershell
  [System.Net.ServicePointManager]::SecurityProtocol
  ```

  하는 경우 **Tls12** 은 명령의 출력에 포함 되지 않습니다, TLS 1.2가 사용 되지 않습니다 컴퓨터.

    **해결 방법**: TLS 1.2를 사용 하도록 설정 하 고 다음 리소스 공급자 배포를 시작 또는 동일한 PowerShell 세션에서 스크립트를 업데이트 하려면 다음 PowerShell 명령을 실행 합니다.

    ```powershell
    [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12
    ```
- **SQL 리소스 공급자를 SQL Server Always On 수신기를 추가 하지**합니다. SQL Server Always On 수신기의 수신기 IP 주소를 사용 하는 경우 VM의 SQL 리소스 공급자에 수신기의 호스트 이름을 확인할 수 없습니다.

    **해결 방법**: 수신기 IP 수신기 호스트 이름을 확인할 DNS 제대로 작동 하는지 확인 합니다.
    
### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Azure Stack을 운영 하는 클라우드 관리자에 대 한 알려진된 문제
설명서를 참조 합니다 [Azure Stack 릴리스](azure-stack-servicing-policy.md)합니다.

## <a name="next-steps"></a>다음 단계
[SQL 리소스 공급자에 자세히 알아보려면](azure-stack-sql-resource-provider.md)합니다.

[SQL 리소스 공급자 배포 준비](azure-stack-sql-resource-provider-deploy.md#prerequisites)합니다.

[SQL 리소스 공급자는 이전 버전에서 업그레이드](azure-stack-sql-resource-provider-update.md)합니다. 