---
title: Microsoft Azure Data Box 반송 | Microsoft Docs
description: Azure Data Box를 Microsoft에 배송하는 방법 알아보기
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 01/24/2019
ms.author: alkohli
ms.openlocfilehash: 158c2a8919bccea03f5c7b67aef23cd07022c259
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/25/2019
ms.locfileid: "54900513"
---
# <a name="tutorial-return-azure-data-box-and-verify-data-upload-to-azure"></a>자습서: Azure Data Box 반송 및 Azure에 대한 데이터 업로드 확인

이 자습서에서는 Azure Data Box를 반송하고 Azure에 업로드된 데이터를 확인하는 방법을 설명합니다.

이 자습서에서는 다음과 같은 주제에 대해 학습합니다.

> [!div class="checklist"]
> * 필수 조건
> * 배송 준비
> * Microsoft에 Data Box 배송
> * Azure에 대한 데이터 업로드 확인
> * Data Box에서 데이터 지우기

## <a name="prerequisites"></a>필수 조건

시작하기 전에 다음 사항을 확인합니다.

- [자습서: Azure Data Box에 데이터 복사 및 확인](data-box-deploy-copy-data.md)을 완료했는지 확인합니다. 
- 복사 작업이 완료되었습니다. 복사 작업이 진행 중이면 배송 준비를 실행할 수 없습니다.

## <a name="prepare-to-ship"></a>배송 준비

[!INCLUDE [data-box-prepare-to-ship](../../includes/data-box-prepare-to-ship.md)]

## <a name="ship-data-box-back"></a>Data Box 반송

1. 디바이스의 전원이 꺼져 있고 케이블이 분리되었는지 확인합니다. 디바이스와 함께 제공된 전원 코드를 디바이스 뒷면에 스풀링하고 안전하게 고정합니다.
2. 디바이스를 미국에서 배송하는 경우 배송 레이블이 E-ink 디스플레이에 표시되는지 확인하고 운송업체에 픽업을 예약합니다. 레이블이 손상되었거나 E-ink 디스플레이에 표시되지 않는 경우 Azure Portal에서 **개요 > 배송 레이블 다운로드**로 이동합니다. 배송 레이블을 다운로드하여 디바이스에 부착합니다.

    디바이스를 유럽에서 배송하는 경우 E-ink 디스플레이에 배송 레이블이 표시되지 않습니다. 대신 반송 레이블이 배송 레이블 아래 투명 파우치에 포함됩니다. 오래된 배송 레이블을 제거하여 배송 레이블이 명확히 보이도록 합니다.
    
3. 디바이스를 반송하는 경우 UPS로 픽업을 예약합니다. 픽업을 예약하려면 현지 UPS(국가 특정 무료 전화 번호)에 전화하거나 Data Box를 가장 가까운 반납 위치에 인계합니다.

4. 운송업체에서 Data Box를 픽업하고 나면 포털의 주문 상태가 **픽업됨**으로 업데이트됩니다. 추적 ID도 표시됩니다.

## <a name="verify-data-upload-to-azure"></a>Azure에 대한 데이터 업로드 확인

Microsoft에서 디바이스를 수신하고 검사하면 주문 상태가 **수신됨**으로 업데이트됩니다. 디바이스에 손상이나 변조 흔적이 있는지 물리적으로 확인됩니다.

확인이 완료되면 Data Box가 Azure 데이터 센터의 네트워크에 연결됩니다. 데이터 복사가 자동으로 시작됩니다. 데이터 크기에 따라 복사 작업을 완료하는 데 몇 시간에서 몇 일이 걸릴 수 있습니다. 포털에서 복사 작업 진행 상황을 모니터링할 수 있습니다.

복사가 완료되면 주문 상태는 **완료됨**으로 업데이트됩니다.

원본에서 데이터를 삭제하기 전에 데이터 저장소 계정에 있는지 확인합니다. Data Box에 데이터를 복사할 때, 데이터 형식에 따라 Azure Storage 계정에서 다음 경로 중 하나에 업로드됩니다.

- 블록 Blob 및 페이지 Blob의 경우: `https://<storage_account_name>.blob.core.windows.net/<containername>/files/a.txt`
- Azure Files의 경우: `https://<storage_account_name>.file.core.windows.net/<sharename>/files/a.txt`

또는 Azure Portal에서 Azure storage 계정으로 이동하여 찾아볼 수도 있습니다.

## <a name="erasure-of-data-from-data-box"></a>Data Box에서 데이터 지우기
 
Azure에 대한 업로드가 완료되면 Data Box 디스크의 데이터가 [NIST SP800-88 개정 1 지침](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi)에 따라 지워집니다. 

## <a name="next-steps"></a>다음 단계

이 자습서에서는 Azure Data Box 항목에 대해 다음과 같은 내용을 알아보았습니다.

> [!div class="checklist"]
> * 필수 조건
> * 배송 준비
> * Microsoft에 Data Box 배송
> * Azure에 대한 데이터 업로드 확인
> * Data Box에서 데이터 지우기

다음 문서로 이동하여 로컬 웹 UI를 통해 Data Box를 관리하는 방법을 알아봅니다.

> [!div class="nextstepaction"]
> [로컬 웹 UI를 사용하여 Azure Data Box 관리](./data-box-local-web-ui-admin.md)


