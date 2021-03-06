---
title: StorSimple 가상 배열에 업데이트 0.6 설치 | Microsoft Docs
description: StorSimple 가상 배열 웹 UI를 사용하여 Azure Portal 및 핫픽스 방법을 사용하는 업데이트를 적용하는 방법을 설명합니다.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/18/2017
ms.author: alkohli
ms.openlocfilehash: 89ca7a5fa13696856bed108e4cbf7462b536b4bb
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51246619"
---
# <a name="install-update-06-on-your-storsimple-virtual-array"></a>StorSimple 가상 배열에 업데이트 0.6 설치

## <a name="overview"></a>개요

이 문서에서는 로컬 웹 UI 및 Azure Portal을 사용하여 StorSimple 가상 배열에 업데이트 0.6을 설치하는 데 필요한 단계를 설명합니다. StorSimple 가상 배열을 최신 상태로 유지하려면 소프트웨어 업데이트 또는 핫픽스를 적용합니다.

업데이트를 적용하기 전에 호스트에서 볼륨 또는 공유를 오프라인으로 전환한 후 디바이스를 오프라인으로 전환하는 것이 좋습니다. 이렇게 하면 데이터 손상 가능성이 최소화됩니다. 볼륨 또는 공유가 오프라인 상태가 되면 디바이스의 수동 백업도 수행해야 합니다.

> [!IMPORTANT]
> - 업데이트 0.6은 디바이스에 있는 소프트웨어 버전 **10.0.10293.0**에 해당합니다. 이 업데이트의 새로운 기능에 대한 자세한 내용은 [업데이트 0.6에 대한 릴리스 정보](storsimple-virtual-array-update-06-release-notes.md)로 이동합니다.
>
> - 업데이트 0.2 이상을 실행 중인 경우 Azure Portal을 통해 업데이트를 설치하는 것이 좋습니다. 업데이트 0.1 또는 GA 소프트웨어 버전을 실행 중인 경우 로컬 웹 UI를 통해 핫픽스 메서드를 사용하여 업데이트 0.6을 설치해야 합니다.
>
> - 업데이트 또는 핫픽스를 설치하면 디바이스가 다시 시작됩니다. StorSimple 가상 배열이 단일 노드 디바이스인 경우 진행 중인 모든 IO가 중단되고 디바이스에 가동 중지 시간이 발생합니다.

## <a name="use-the-azure-portal"></a>Azure Portal 사용

업데이트 0.2 이상을 실행하는 경우 Azure Portal을 통해 업데이트를 설치하는 것이 좋습니다. 포털 절차를 사용하려면 사용자가 업데이트를 스캔, 다운로드한 다음 설치해야 합니다. 이 절차를 완료하는 데 약 7분이 걸립니다. 다음 단계를 수행하여 업데이트 또는 핫픽스를 설치합니다.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

설치가 완료된 후 StorSimple 디바이스 관리자 서비스로 이동합니다. **장치**를 선택한 후 방금 업데이트한 장치를 선택하고 클릭합니다. **설정 > 관리 > 장치 업데이트**로 이동합니다. 표시된 소프트웨어 버전은 **10.0.10293.0**이어야 합니다.

## <a name="use-the-local-web-ui"></a>로컬 웹 UI 사용

로컬 웹 UI를 사용하는 경우 다음 두 단계로 구성됩니다.

* 업데이트 또는 핫픽스 다운로드
* 업데이트 또는 핫픽스 설치

### <a name="download-the-update-or-the-hotfix"></a>업데이트 또는 핫픽스 다운로드

Microsoft 업데이트 카탈로그에서 소프트웨어 업데이트를 다운로드하려면 다음 단계를 수행합니다.

#### <a name="to-download-the-update-or-the-hotfix"></a>업데이트 또는 핫픽스를 다운로드하려면

1. Internet Explorer를 시작하고 [http://catalog.update.microsoft.com](https://catalog.update.microsoft.com)으로 이동합니다.

2. 이 컴퓨터에서 Microsoft 업데이트 카탈로그를 처음 사용하는 경우 Microsoft 업데이트 카탈로그 추가 기능을 설치하라는 메시지가 나타나면 **설치**를 클릭합니다.

3. Microsoft 업데이트 카탈로그의 검색 상자에 다운로드하려는 핫픽스의 KB(기술 자료) 번호를 입력합니다. 업데이트 0.6의 경우 **4023268**을 입력하고 **검색**을 클릭합니다.
   
    핫픽스 목록(예: **StorSimple 가상 배열 업데이트 0.6**)이 나타납니다.
   
    ![카탈로그 검색](./media/storsimple-virtual-array-install-update-06/download1.png)

4. **다운로드**를 클릭합니다.

5. 다운로드할 다섯 개 파일이 표시되어야 합니다. 이러한 각 파일을 폴더에 다운로드합니다. 디바이스에서 연결할 수 있는 네트워크 공유에 폴더도 복사할 수 있습니다.

6. 파일이 있는 폴더를 엽니다.
    ![패키지의 파일](./media/storsimple-virtual-array-install-update-06/update06folder.png)

    다음이 표시됩니다.
    -  Microsoft 업데이트 독립 실행형 패키지 파일 `WindowsTH-KB3011067-x64`. 이 파일은 디바이스 소프트웨어를 업데이트하는 데 사용됩니다.
    - Geneva 모니터링 에이전트 패키지 파일 `GenevaMonitoringAgentPackageInstaller`. 이 파일은 MDS(모니터링 및 진단 서비스) 에이전트를 업데이트하는 데 사용됩니다. cab 파일을 두 번 클릭합니다. _.msi_가 표시됩니다. 파일을 선택하고 마우스 오른쪽 단추로 클릭한 다음 파일 **압축을 풉니다**. _.msi_ 파일을 사용하여 에이전트를 업데이트합니다.

        ![MDS 에이전트 업데이트 파일 압축 풀기](./media/storsimple-virtual-array-install-update-06/extract-geneva-monitoring-agent-installer.png)

        > [!IMPORTANT]
        > StorSimple 업데이트 0.5(0.0.10293.0)를 실행중인 경우 MDS 에이전트를 업데이트할 필요가 없습니다.

    - 중요 Windows 보안 업데이트 `windows8.1-kb4012213-x64`, `windows8.1-kb3205400-x64` 및 `windows8.1-kb4019213-x64`이 포함된 세 개의 파일


### <a name="install-the-update-or-the-hotfix"></a>업데이트 또는 핫픽스 설치

업데이트 또는 핫픽스를 설치하기 전에, 업데이트 또는 핫픽스를 호스트의 로컬에 다운로드하거나 네트워크 공유를 통해 액세스할 수 있는지 확인합니다.

GA 또는 업데이트 0.1 소프트웨어 버전을 실행하는 디바이스에 업데이트를 설치하려면 이 방법을 사용합니다. 이 절차를 완료하는 데 약 12분이 소요됩니다. 다음 단계를 수행하여 업데이트 또는 핫픽스를 설치합니다.

#### <a name="to-install-the-update-or-the-hotfix"></a>업데이트 또는 핫픽스를 설치하려면

1. 로컬 웹 UI에서 **유지 관리** > **소프트웨어 업데이트**로 이동합니다. 실행 중인 소프트웨어 버전을 기록해 둡니다. **10.0.10290.0**을 실행 중인 경우 6단계에서 MDS 에이전트를 업데이트할 필요가 없습니다.
   
    ![디바이스 업데이트](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. **업데이트 파일 경로**에 업데이트 또는 핫픽스의 파일 이름을 입력합니다. 네트워크 공유에 있는 경우 업데이트 또는 핫픽스 설치 파일로 이동할 수 있습니다. **적용**을 클릭합니다.
   
    ![디바이스 업데이트](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. 경고가 표시됩니다. 가상 배열이 단일 노드 디바이스인 경우 업데이트가 적용된 후 디바이스를 다시 시작하고 가동 중지 시간이 발생합니다. 확인 아이콘을 클릭합니다.
   
   ![디바이스 업데이트](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. 업데이트가 시작됩니다. 디바이스가 성공적으로 업데이트된 후 다시 시작됩니다. 이 시간 동안 로컬 UI에 액세스할 수 없습니다.
   
    ![디바이스 업데이트](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. 다시 시작이 완료된 후 **로그인** 페이지가 열립니다. 로컬 웹 UI에서 디바이스 소프트웨어가 업데이트되었는지 확인하려면 **유지 관리** > **소프트웨어 업데이트**로 이동합니다. 표시된 소프트웨어 버전은 업데이트 0.6의 경우 **10.0.0.0.0.10293**이어야 합니다.
   
   > [!NOTE]
   > 로컬 웹 UI 및 Azure Portal에서 약간 다른 방법으로 소프트웨어 버전을 보고합니다. 예를 들어 같은 버전에 대해 로컬 웹 UI는 **10.0.0.0.0.10293**, Azure Portal은 **10.0.10293.0**을 보고합니다.
   
    ![디바이스 업데이트](./media/storsimple-virtual-array-install-update-06/update6m.png)

6. 이 업데이트를 적용하기 전에 StorSimple 가상 배열 업데이트 0.5(**10.0.10290.0**)를 실행하고 있다면 이 단계를 건너뜁니다. 업데이트를 시작하기 전에 1단계에서 소프트웨어 버전을 기록했습니다. 업데이트 0.5가 실행 중이면 MDS 에이전트는 이미 최신 상태입니다.

    업데이트 0.5 이전의 소프트웨어 버전을 실행 중이면 다음 단계는 MDS 에이전트를 업데이트하는 것입니다. **소프트웨어 업데이트** 페이지에는 **업데이트 파일 경로**로 이동한 후 `GenevaMonitoringAgentPackageInstaller.msi` 파일을 찾습니다. 2-4단계를 반복합니다. 가상 배열이 다시 시작되면 로컬 웹 UI에 로그인합니다.

7. 2-4단계를 반복하여 `windows8.1-kb4012213-x64`, `windows8.1-kb3205400-x64` 및 `windows8.1-kb4019213-x64` 파일을 통해 Windows 보안 수정을 설치합니다. 각각 설치 후 가상 배열이 다시 시작되고 로컬 웹 UI에 로그인해야 합니다.

## <a name="next-steps"></a>다음 단계

[StorSimple 가상 배열 관리](storsimple-ova-web-ui-admin.md)에 대해 자세히 알아봅니다.

