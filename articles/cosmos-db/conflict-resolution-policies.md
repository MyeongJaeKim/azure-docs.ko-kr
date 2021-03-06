---
title: Azure Cosmos DB에 여러 쓰기 지역이 있는 충돌 해결 유형 및 해결 정책
description: 이 문서에서는 Azure Cosmos DB의 충돌 범주 및 충돌 해결 정책에 대해 설명합니다.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 31e6fa596bb053ee8cd19f641349f02ee169b6eb
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55472340"
---
# <a name="conflict-types-and-resolution-policies"></a>충돌 유형 및 해결 정책

충돌 및 충돌 해결 정책은 Azure Cosmos DB 계정이 여러 쓰기 지역으로 구성된 경우 적용될 수 있습니다.

여러 쓰기 지역으로 구성된 Azure Cosmos DB 계정의 경우 작성자가 여러 지역에서 동일한 항목을 동시에 업데이트하는 경우 업데이트 충돌이 발생할 수 있습니다. 업데이트 충돌은 다음 세 가지 유형으로 분류됩니다.

* **삽입 충돌**: 이러한 충돌은 애플리케이션이 두 개 이상의 지역에서 동일한 고유 인덱스를 사용하여 두 개 이상의 항목을 동시에 삽입하는 경우 발생할 수 있습니다. 예를 들어 ID 속성을 사용하는 경우 이 충돌이 발생할 수 있습니다. 모든 쓰기는 처음에 각 로컬 지역에서 성공할 수 있습니다. 그러나 선택하는 충돌 해결 정책에 따라 원래 ID를 사용한 하나의 항목만 마지막으로 커밋됩니다.

* **대체 충돌**: 이러한 충돌은 애플리케이션이 두 개 이상의 지역에서 단일 항목을 동시에 업데이트하는 경우 발생할 수 있습니다.

* **삭제 충돌**: 이러한 충돌은 애플리케이션이 동시에 한 지역에서 항목을 삭제하고 다른 지역에서 업데이트하는 경우 발생할 수 있습니다.

## <a name="conflict-resolution-policies"></a>충돌 해결 정책

Azure Cosmos DB는 업데이트 충돌을 해결하기 위해 유연한 정책 기반 메커니즘을 제공합니다. Azure Cosmos DB 컨테이너의 두 가지 충돌 해결 정책 중에서 선택할 수 있습니다.

- **LWW(마지막으로 성공한 쓰기)**: 기본적으로 이 해결 정책은 시스템에서 정의된 타임스탬프 속성을 사용하며, 시간 동기화 클록 프로토콜을 기반으로 합니다. Azure Cosmos DB SQL API를 사용하는 경우 충돌 해결에 사용할 다른 사용자 지정 숫자 속성을 지정할 수 있습니다. 사용자 지정 숫자 속성은 충돌 해결 경로라고도 합니다. 

  두 개 이상 항목이 삽입 또는 대체 작업에서 충돌하는 경우 충돌 해결 경로에 대한 가장 높은 값이 포함된 항목이 선정됩니다. 여러 항목에 충돌 해결 경로에 대한 동일한 숫자 값이 있는 경우 시스템에서 선정 항목이 결정됩니다. 모든 지역은 단일 선정 항목으로 수렴되고 결국 동일한 버전의 커밋된 항목으로 끝나게 됩니다. 삭제 충돌이 포함되어 있으면 삭제된 버전이 항상 삽입 또는 대체 충돌에 비해 우선적으로 선정됩니다. 이 결과는 충돌 해결 경로 값에 관계없이 발생합니다.

  > [!NOTE]
  > 최종 작성자 인정(Last Write Wins)은 기본 충돌 해결 정책입니다. 이 정책은 SQL, Azure Cosmos DB Table, MongoDB, Cassandra 및 Gremlin API 계정에 사용할 수 있습니다.

  자세한 내용은 [LWW 충돌 해결 정책을 사용하는 예제](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)를 참조하세요.

- **사용자 지정**: 이 해결 정책은 충돌을 조정하기 위한 애플리케이션 정의 의미 체계용으로 설계되었습니다. Azure Cosmos DB 컨테이너에 이 정책을 설정한 경우 병합 저장 프로시저도 등록해야 합니다. 이 프로시저는 서버의 데이터베이스 트랜잭션 내에서 충돌을 감지하는 경우 자동으로 호출됩니다. 시스템은 커밋 프로토콜의 일부로 병합 프로시저의 실행에 대해 정확히 한 번의 보장을 제공합니다.  

  사용자 지정 해결 옵션을 사용하여 컨테이너를 구성하는 경우 두 가지 사항을 기억해야 합니다. 컨테이너에서 병합 프로시저를 등록하지 못하거나 병합 프로시저가 런타임 시 예외를 throw하는 경우 충돌이 충돌 피드에 기록됩니다. 그러면, 애플리케이션은 충돌 피드에서 충돌을 수동으로 해결해야 합니다. 자세한 내용은 [사용자 지정 해결 정책을 사용하는 방법 및 충돌 피드를 사용하는 방법의 예제](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)를 참조하세요.

  > [!NOTE]
  > 사용자 지정 충돌 해결 정책은 SQL API 계정에서만 사용할 수 있습니다.

## <a name="next-steps"></a>다음 단계

충돌 해결 정책을 구성하는 방법을 알아봅니다. 다음 문서를 참조하세요.

* [LWW 충돌 해결 정책 사용](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [사용자 지정 충돌 해결 정책 사용](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [충돌 피드 사용](how-to-manage-conflicts.md#read-from-conflict-feed)
