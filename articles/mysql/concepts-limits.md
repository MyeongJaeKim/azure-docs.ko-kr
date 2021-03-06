---
title: Azure Database for MySQL의 제한 사항
description: 이 문서에서는 Azure Database for MySQL에 대한 연결 수 및 저장소 엔진 옵션과 같은 제한 사항을 설명합니다.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/6/2018
ms.openlocfilehash: 19fc20f21a57c2325254581c642b75c92c221fd9
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53536093"
---
# <a name="limitations-in-azure-database-for-mysql"></a>Azure Database for MySQL의 제한 사항
다음 섹션에서는 데이터베이스 서비스의 용량, 저장소 엔진 지원, 권한 지원, 데이터 조작 명령문 지원 및 기능 제한 사항에 대해 설명합니다. 또한 MySQL 데이터베이스 엔진에 적용할 수 있는 [일반적인 제한 사항](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html)도 참조하세요.

## <a name="maximum-connections"></a>최대 연결 수
가격 책정 계층 및 vCores당 최대 연결 수는 다음과 같습니다. 

|**가격 책정 계층**|**vCore**| **최대 연결**|
|---|---|---|
|Basic| 1| 50|
|Basic| 2| 100|
|범용| 2| 300|
|범용| 4| 625|
|범용| 8| 1250|
|범용| 16| 2500|
|범용| 32| 5,000|
|범용| 64| 10000|
|메모리 최적화| 2| 600|
|메모리 최적화| 4| 1250|
|메모리 최적화| 8| 2500|
|메모리 최적화| 16| 5,000|
|메모리 최적화| 32| 10000|

연결 한도를 초과하면 다음과 같은 오류가 발생할 수 있습니다.
> 오류 1040(08004): 연결이 너무 많음

## <a name="storage-engine-support"></a>저장소 엔진 지원

### <a name="supported"></a>지원됨
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [MEMORY](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>지원되지 않음
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [BLACKHOLE](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARCHIVE](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [FEDERATED](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>권한 지원

### <a name="unsupported"></a>지원되지 않음
- DBA 역할: 서버 매개 변수 및 설정이 많으면 실수로 서버 성능이 저하되거나 DBMS의 ACID 속성이 무효화될 수 있습니다. 따라서 제품 수준에서 서비스 무결성 및 SLA를 유지하기 위해 이 서비스에서는 DBA 역할이 노출되지 않습니다. 사용자는 새 데이터베이스 인스턴스를 만들 때 생성되는 기본 사용자 계정을 통해 관리되는 데이터베이스 인스턴스에서 대부분의 DDL 및 DML 문을 수행할 수 있습니다. 
- SUPER 권한: 마찬가지로 [SUPER 권한](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super)도 제한됩니다.

## <a name="data-manipulation-statement-support"></a>데이터 조작 명령문 지원

### <a name="supported"></a>지원됨
- `LOAD DATA INFILE`은 지원되지만 `[LOCAL]` 매개 변수를 지정하고 UNC 경로(SMB를 통해 탑재된 Azure 저장소)로 전달해야 합니다.

### <a name="unsupported"></a>지원되지 않음
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>기능 제한 사항

### <a name="scale-operations"></a>크기 조정 작업
- 기본 가격 책정 계층 간의 동적 크기 조정은 현재 지원되지 않습니다.
- 서버 저장소 크기를 줄이는 것은 지원되지 않습니다.

### <a name="server-version-upgrades"></a>서버 버전 업그레이드
- 주 데이터베이스 엔진 버전 간에 자동화된 마이그레이션은 현재 지원되지 않습니다. 다음의 주 버전으로 업그레이드하려는 경우 새 엔진 버전을 사용하여 만든 서버에 주 버전을 [덤프 및 복원](./concepts-migrate-dump-restore.md)합니다.

### <a name="point-in-time-restore"></a>특정 시점 복원
- PITR 기능을 사용하면 새 서버가 기반으로 하는 서버와 동일한 구성으로 새 서버가 만들어집니다.
- 삭제된 서버 복원은 지원되지 않습니다.

### <a name="vnet-service-endpoints"></a>VNet 서비스 엔드포인트
- VNet 서비스 엔드포인트는 범용 및 메모리 최적화 서버에 대해서만 지원됩니다.

## <a name="current-known-issues"></a>현재 알려진 문제
- 연결이 설정된 후에 MySQL 서버 인스턴스에서 잘못된 서버 버전을 표시합니다. 올바른 서버 인스턴스 엔진 버전을 설치하려면 `select version();` 명령을 사용합니다.

## <a name="next-steps"></a>다음 단계
- [각 서비스 계층에서 사용할 수 있는 기능](concepts-pricing-tiers.md)
- [지원되는 MySQL 데이터베이스 버전](concepts-supported-versions.md)
