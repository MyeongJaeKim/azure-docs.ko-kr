---
title: Azure Data Factory를 사용하여 Azure SQL Data Warehouse에서/로 데이터 복사 | Microsoft Docs
description: Data Factory를 사용하여 지원되는 원본 저장소에서 Azure SQL Data Warehouse로 또는 SQL Data Warehouse에서 지원되는 싱크 저장소로 데이터를 복사하는 방법을 알아봅니다.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: jingwang
ms.openlocfilehash: c429945d4832710125a419b4e9a9b9165869ca97
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54888688"
---
# <a name="copy-data-to-or-from-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Azure Data Factory를 사용하여 Azure SQL Data Warehouse 간 데이터 복사 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you're using:"]
> * [Version1 ](v1/data-factory-azure-sql-data-warehouse-connector.md)
> * [현재 버전](connector-azure-sql-data-warehouse.md)

이 문서에서는 Azure Data Factory의 복사 작업을 사용하여 Azure SQL Data Warehouse에서 데이터를 복사하는 방법을 설명합니다. 이 문서는 복사 작업에 대한 일반적인 개요를 제공하는 [복사 작업 개요](copy-activity-overview.md) 문서를 기반으로 합니다.

## <a name="supported-capabilities"></a>지원되는 기능

Azure SQL Data Warehouse에서 데이터를 지원되는 모든 싱크 데이터 저장소로 복사할 수 있습니다. 또한 지원되는 모든 원본 데이터 저장소에서 Azure SQL Data Warehouse로 데이터를 복사할 수 있습니다. 복사 작업에서 원본 또는 싱크로 지원되는 데이터 저장소 목록은 [지원되는 데이터 저장소 및 형식](copy-activity-overview.md#supported-data-stores-and-formats) 표를 참조하세요.

특히 이 Azure SQL Data Warehouse 커넥터는 다음 기능을 지원합니다.

- 서비스 주체 또는 Azure 리소스에 대한 관리 ID를 통해 SQL 인증 및 Azure AD(Azure Active Directory) 애플리케이션 토큰 인증을 사용하여 데이터를 복사합니다.
- 원본으로 SQL 쿼리 또는 저장 프로시저를 사용하여 데이터를 검색합니다.
- 싱크로 PolyBase 또는 대량 삽입을 사용하여 데이터를 로드합니다. 더 나은 복사 성능을 얻으려면 PolyBase를 사용하는 것이 좋습니다.

> [!IMPORTANT]
> PolyBase는 SQL 인증만 지원하고 Azure AD 인증은 지원하지 않습니다.

> [!IMPORTANT]
> Azure Data Factory Integration Runtime을 사용하여 데이터를 복사하는 경우, Azure 서비스가 서버에 액세스할 수 있도록 [Azure SQL Server 방화벽](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure)을 구성합니다.
> 자체 호스팅 통합 런타임을 사용하여 데이터를 복사하는 경우, 적절한 IP 범위를 허용하도록 Azure SQL Server 방화벽을 구성합니다. 이 범위에는 Azure SQL Database에 연결하는 데 사용되는 머신 IP가 포함됩니다.

## <a name="get-started"></a>시작하기

> [!TIP]
> 최상의 성능을 얻으려면 PolyBase를 사용하여 Azure SQL Data Warehouse에 데이터를 로드하세요. 자세한 내용은 [PolyBase를 사용하여 Azure SQL Data Warehouse에 데이터 로드](#use-polybase-to-load-data-into-azure-sql-data-warehouse) 섹션을 참조하세요. 사용 사례가 있는 연습을 보려면 [Azure Data Factory를 통해 Azure SQL Data Warehouse에 15분 이내 1TB 로드](load-azure-sql-data-warehouse.md)를 참조하세요.

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

다음 섹션에서는 Azure SQL Data Warehouse 커넥터와 관련된 Data Factory 엔터티를 정의하는 속성에 대해 자세히 설명합니다.

## <a name="linked-service-properties"></a>연결된 서비스 속성

Azure SQL Data Warehouse 연결된 서비스에 대해 지원되는 속성은 다음과 같습니다.

| 속성 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | type 속성은 **AzureSqlDW**로 설정해야 합니다. | 예 |
| connectionString | Azure SQL Data Warehouse 인스턴스에 연결하는 데 필요한 정보를 **connectionString** 속성에 대해 지정합니다. 이 필드를 **SecureString**으로 표시하여 Data Factory에 안전하게 저장하거나, [Azure Key Vault에 저장된 비밀을 참조](store-credentials-in-key-vault.md)합니다. | 예 |
| servicePrincipalId | 애플리케이션의 클라이언트 ID를 지정합니다. | 서비스 주체와 함께 Azure AD 인증을 사용하는 경우 예 |
| servicePrincipalKey | 애플리케이션의 키를 지정합니다. 이 필드를 SecureString으로 표시하여 Data Factory에 안전하게 저장하거나 [Azure Key Vault에 저장되는 비밀을 참조](store-credentials-in-key-vault.md)합니다. | 서비스 주체와 함께 Azure AD 인증을 사용하는 경우 예 |
| tenant | 애플리케이션이 있는 테넌트 정보(도메인 이름 또는 테넌트 ID)를 지정합니다. Azure Portal의 오른쪽 위 모서리에 마우스를 이동하여 검색할 수 있습니다. | 서비스 주체와 함께 Azure AD 인증을 사용하는 경우 예 |
| connectVia | 데이터 저장소에 연결하는 데 사용할 [통합 런타임](concepts-integration-runtime.md)입니다. Azure Integration Runtime 또는 자체 호스팅 통합 런타임을 사용할 수 있습니다(데이터 저장소가 개인 네트워크에 있는 경우). 지정하지 않으면 기본 Azure Integration Runtime을 사용합니다. | 아니오 |

다른 인증 형식의 경우, 각각의 필수 조건 및 JSON 샘플에 대한 다음 섹션을 참조하세요.

- [SQL 인증](#sql-authentication)
- Azure AD 애플리케이션 토큰 인증: [서비스 주체](#service-principal-authentication)
- Azure AD 애플리케이션 토큰 인증: [Azure 리소스에 대한 관리 ID](#managed-identity)

>[!TIP]
>"UserErrorFailedToConnectToSqlServer" 오류 코드 및 "데이터베이스에 대한 세션 제한이 XXX이고 이에 도달했습니다."와 같은 메시지가 있는 오류가 발생하면 연결 문자열에 `Pooling=false`를 추가하고 다시 시도하세요.

### <a name="sql-authentication"></a>SQL 인증

#### <a name="linked-service-example-that-uses-sql-authentication"></a>SQL 인증을 사용하는 연결된 서비스 예제

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>서비스 주체 인증

서비스 주체 기반의 Azure AD 애플리케이션 토큰 인증을 사용하려면 다음 단계를 따르세요.

1. Azure Portal에서 **[Azure Active Directory 애플리케이션을 만듭니다](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)**. 애플리케이션 이름 및 연결된 서비스를 정의하는 다음 값을 적어 둡니다.

    - 애플리케이션 UI
    - 애플리케이션 키
    - 테넌트 ID

1. Azure Portal에서 Azure SQL Server에 대한 **[Azure Active Directory 관리자를 프로비전](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**(아직 하지 않은 경우)합니다. Azure AD 관리자는 Azure AD 사용자 또는 Azure AD 그룹일 수 있습니다. MSI가 있는 그룹에 관리자 역할을 부여하는 경우, 3단계와 4단계를 건너뜁니다. 관리자는 데이터베이스에 대한 모든 권한을 갖습니다.

1. 서비스 주체에 대한 **[포함된 데이터베이스 사용자를 만듭니다](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**. 최소한 ALTER ANY USER 권한이 있는 Azure AD ID를 사용하여 SSMS 등의 도구를 통해 데이터를 복사하려는 데이터 웨어하우스에 연결합니다. 다음 T-SQL을 실행합니다.
    
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

1. 일반적으로 SQL 사용자나 기타 사용자에 대해 수행하듯이 **서비스 주체에 필요한 권한을 부여**합니다. 다음 코드를 실행합니다.

    ```sql
    EXEC sp_addrolemember [role name], [your application name];
    ```

1. Azure Data Factory에서 **Azure SQL Data Warehouse 연결된 서비스를 구성**합니다.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>서비스 주체 인증을 사용하는 연결된 서비스 예제

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
            },
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a>Azure 리소스 인증용 관리 ID

데이터 팩터리는 특정 팩터리를 나타내는 [Azure 리소스에 대한 관리 ID](data-factory-service-identity.md)와 연결할 수 있습니다. Azure SQL Data Warehouse 인증에 이 서비스 ID를 사용할 수 있습니다. 지정된 팩터리는 이 ID를 사용하여 데이터 웨어하우스의 데이터에 액세스하고 복사할 수 있습니다.

> [!IMPORTANT]
> PolyBase는 현재 MSI 인증에 대해 지원되지 않습니다.

MSI 기반 Azure AD 애플리케이션 토큰 인증을 사용하려면 다음 단계를 따르세요.

1. **Azure AD에서 그룹을 만듭니다.** 팩터리 MSI를 그룹의 구성원으로 지정합니다.

    1. Azure Portal에서 데이터 팩터리 서비스 ID를 찾습니다. 데이터 팩터리의 **속성**으로 이동합니다. 서비스 ID의 ID를 복사합니다.

    1. [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) 모듈을 설치합니다. `Connect-AzureAD` 명령을 사용하여 로그인합니다. 다음 명령을 실행하여 그룹을 만들고 데이터 팩터리 MSI를 구성원으로 추가합니다.
    ```powershell
    $Group = New-AzureADGroup -DisplayName "<your group name>" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId "<your data factory service identity ID>"
    ```

1. Azure Portal에서 Azure SQL Server에 대한 **[Azure Active Directory 관리자를 프로비전](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**(아직 하지 않은 경우)합니다.

1. Azure AD 그룹에 대한 **[포함된 데이터베이스 사용자를 만듭니다](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**. 최소한 ALTER ANY USER 권한이 있는 Azure AD ID를 사용하여 SSMS 등의 도구를 통해 데이터를 복사하려는 데이터 웨어하우스에 연결합니다. 다음 T-SQL을 실행합니다. 
    
    ```sql
    CREATE USER [your Azure AD group name] FROM EXTERNAL PROVIDER;
    ```

1. 일반적으로 SQL 사용자 및 기타 사용자에 대해 수행하듯이 **Azure AD 그룹에 필요한 권한을 부여**합니다. 예를 들어, 다음 코드를 실행합니다.

    ```sql
    EXEC sp_addrolemember [role name], [your Azure AD group name];
    ```

1. Azure Data Factory에서 **Azure SQL Data Warehouse 연결된 서비스를 구성**합니다.

#### <a name="linked-service-example-that-uses-msi-authentication"></a>MSI 인증을 사용하는 연결된 서비스 예제

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>데이터 세트 속성

데이터 세트 정의에 사용할 수 있는 섹션 및 속성의 전체 목록은 [데이터 세트](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services) 문서를 참조하세요. 이 섹션에서는 Azure SQL Data Warehouse 데이터 세트에서 지원하는 속성 목록을 제공합니다.

Azure SQL Data Warehouse에서 데이터를 복사하려면 데이터 세트의 **type** 속성을 **AzureSqlDWTable**로 설정합니다. 다음과 같은 속성이 지원됩니다.

| 속성 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | 데이터 세트의 **type** 속성을 **AzureSqlDWTable**로 설정해야 합니다. | 예 |
| tableName | 연결된 서비스가 참조하는 Azure SQL Data Warehouse 인스턴스의 테이블 또는 뷰 이름입니다. | 원본에는 아니요이고 싱크에는 예입니다 |

#### <a name="dataset-properties-example"></a>데이터 세트 속성 예제

```json
{
    "name": "AzureSQLDWDataset",
    "properties":
    {
        "type": "AzureSqlDWTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Data Warehouse linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>복사 활동 속성

작업 정의에 사용할 수 있는 섹션 및 속성의 전체 목록은 [파이프라인](concepts-pipelines-activities.md) 문서를 참조하세요. 이 섹션에서는 Azure SQL Data Warehouse 원본 및 싱크에서 지원하는 속성 목록을 제공합니다.

### <a name="azure-sql-data-warehouse-as-the-source"></a>Azure SQL Data Warehouse가 원본인 경우

Azure SQL Data Warehouse에서/로 데이터를 복사하려면 복사 작업 원본의 **type** 속성을 **SqlDWSource**로 설정합니다. 복사 작업 **source** 섹션에서 지원되는 속성은 다음과 같습니다.

| 속성 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | 복사 작업 원본의 **type** 속성을 **SqlDWSource**로 설정해야 합니다. | 예 |
| SqlReaderQuery | 사용자 지정 SQL 쿼리를 사용하여 데이터를 읽습니다. 예: `select * from MyTable`. | 아니오 |
| sqlReaderStoredProcedureName | 원본 테이블에서 데이터를 읽는 저장 프로시저의 이름입니다. 마지막 SQL 문은 저장 프로시저의 SELECT 문이어야 합니다. | 아니오 |
| storedProcedureParameters | 저장 프로시저에 대한 매개 변수입니다.<br/>허용되는 값은 이름 또는 값 쌍입니다. 매개 변수의 이름 및 대소문자와, 저장 프로시저 매개변수의 이름 및 대소문자와 일치해야 합니다. | 아니오 |

### <a name="points-to-note"></a>주의할 사항

- **sqlReaderQuery**가 **SqlSource**에 대해 지정된 경우, 복사 작업은 Azure SQL Data Warehouse 원본에 대해 이 쿼리를 실행하여 데이터를 가져옵니다. 또는 저장 프로시저를 지정할 수 있습니다. 저장 프로시저가 매개 변수를 사용하는 경우, **sqlReaderStoredProcedureName** 및 **storedProcedureParameters**를 지정합니다.
- **sqlReaderQuery** 또는 **sqlReaderStoredProcedureName** 중 하나를 지정하지 않는 경우, 데이터 세트 JSON의 **structure** 섹션에 정의된 열이 쿼리()를 생성하는 데 사용됩니다. `select column1, column2 from mytable`은 Azure SQL Data Warehouse에 대해 실행됩니다. 데이터 세트 정의에 **structure**가 없는 경우, 테이블에서 모든 열이 선택됩니다.

#### <a name="sql-query-example"></a>SQL 쿼리 예제

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL DW input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlDWSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="stored-procedure-example"></a>저장 프로시저 예제

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL DW input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlDWSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="stored-procedure-definition"></a>저장 프로시저 정의

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
    select *
    from dbo.UnitTestSrcTable
    where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="azure-sql-data-warehouse-as-sink"></a> Azure SQL Data Warehouse가 싱크인 경우

Azure SQL Data Warehouse에 데이터를 복사하려면 복사 작업의 싱크 형식을 **SqlDWSink**로 설정합니다. 복사 작업 **sink** 섹션에서 지원되는 속성은 다음과 같습니다.

| 속성 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | 복사 작업 싱크의 **type** 속성은 **SqlDWSink**로 설정해야 합니다. | 예 |
| allowPolyBase | BULKINSERT 메커니즘 대신 PolyBase(해당하는 경우)를 사용할지 여부를 나타냅니다. <br/><br/> PolyBase를 사용하여 SQL Data Warehouse에 데이터를 로드하는 것이 좋습니다. 제약 조건 및 세부 정보는 [PolyBase를 사용하여 Azure SQL Data Warehouse에 데이터 로드](#use-polybase-to-load-data-into-azure-sql-data-warehouse) 섹션을 참조하세요.<br/><br/>허용되는 값은 **True** 및 **False**(기본값)입니다.  | 아니오 |
| polyBaseSettings | **allowPolybase** 속성이 **true**로 설정된 경우 지정될 수 있는 속성의 그룹입니다. | 아니오 |
| rejectValue | 쿼리가 실패하기 전에 거부될 수 있는 행의 수 또는 백분율을 지정합니다.<br/><br/>[CREATE EXTERNAL TABLE(Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx)의 인수 섹션에서 PolyBase의 거부 옵션에 대해 자세히 알아봅니다. <br/><br/>허용되는 값은 0(기본값), 1, 2 등입니다. |아니오 |
| rejectType | **rejectValue** 옵션이 리터럴 값인지 또는 백분율인지를 지정합니다.<br/><br/>허용되는 값은 **Value**(기본값) 및 **Percentage**입니다. | 아니오 |
| rejectSampleValue | PolyBase가 거부된 행의 백분율을 다시 계산하기 전에 검색할 행 수를 결정합니다.<br/><br/>허용되는 값은 1, 2 등입니다. | **rejectType**이 **percentage**인 경우 예 |
| useTypeDefault | PolyBase가 텍스트 파일에서 데이터를 검색할 경우 구분된 텍스트 파일에서 누락된 값을 처리하는 방법을 지정합니다.<br/><br/>[외부 파일 서식 만들기(Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx)를 사용하여 파이프라인을 만드는 데 사용할 수 있는 샘플 JSON 정의를 제공합니다.<br/><br/>허용되는 값은 **True** 및 **False**(기본값)입니다. | 아니오 |
| writeBatchSize | 버퍼 크기가 **writeBatchSize**에 도달하면 SQL 테이블에 데이터를 삽입합니다. PolyBase가 사용되지 않는 경우에만 적용됩니다.<br/><br/>허용되는 값은 **정수**(행 수)입니다. | 아니요. 기본값은 10000입니다. |
| writeBatchTimeout | 시간 초과되기 전에 배치 삽입 작업을 완료하기 위한 대기 시간입니다. PolyBase가 사용되지 않는 경우에만 적용됩니다.<br/><br/>허용되는 값은 **시간 범위**입니다. 예제: “00:30:00”(30분) | 아니요 |
| preCopyScript | 각 실행 시 Azure SQL Data Warehouse에 데이터를 쓰기 전에 실행할 복사 작업에 대한 SQL 쿼리를 지정합니다. 이 속성을 사용하여 미리 로드된 데이터를 정리합니다. | 아니오 | (#repeatability-during-copy). | 쿼리 문입니다. | 아니오 |

#### <a name="sql-data-warehouse-sink-example"></a>SQL Data Warehouse 싱크 예제

```json
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true,
    "polyBaseSettings":
    {
        "rejectType": "percentage",
        "rejectValue": 10.0,
        "rejectSampleValue": 100,
        "useTypeDefault": true
    }
}
```

다음 섹션에서 PolyBase를 사용하여 SQL Data Warehouse를 효율적으로 로드하는 방법을 자세히 알아봅니다.

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>PolyBase를 사용하여 Azure SQL Data Warehouse에 데이터 로드

[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)를 사용하면 높은 처리량으로 대량 데이터를 Azure SQL Data Warehouse에 효율적으로 로드할 수 있습니다. 기본 BULKINSERT 메커니즘 대신 PolyBase를 사용하면 처리량이 훨씬 증가합니다. 자세한 비교는 [성능 참조](copy-activity-performance.md#performance-reference)를 참조하세요. 사용 사례가 있는 연습을 보려면 [Azure SQL Data Warehouse에 1TB 로드](https://docs.microsoft.com/azure/data-factory/v1/data-factory-load-sql-data-warehouse)를 참조하세요.

* 원본 데이터가 Azure Blob Storage 또는 Azure Data Lake Store에 있고 형식이 PolyBase와 호환되는 경우, PolyBase를 사용하여 Azure SQL Data Warehouse에 직접 복사합니다. 자세한 내용은 **[PolyBase를 사용하여 직접 복사](#direct-copy-by-using-polybase)** 를 참조하세요.
* 원본 데이터 저장소와 형식이 PolyBase에서 원래 지원되지 않는 경우, 대신 **[PolyBase를 사용한 준비된 복사](#staged-copy-by-using-polybase)** 기능을 사용합니다. 준비된 복사 기능을 사용할 경우, 처리량도 향상됩니다. 이 기능은 데이터를 PolyBase 호환 형식으로 자동으로 변환합니다. 또한 Azure Blob Storage에 데이터를 저장합니다. 그런 다음, SQL Data Warehouse에 데이터를 로드합니다.

> [!IMPORTANT]
> PolyBase는 현재 MSI 기반 Azure AD 애플리케이션 토큰 인증에 대해 지원되지 않습니다.

### <a name="direct-copy-by-using-polybase"></a>PolyBase를 사용한 직접 복사

SQL Data Warehouse PolyBase는 Azure Blob 및 Azure Data Lake Store를 직접 지원합니다. 서비스 주체를 원본으로 사용하며 특정 파일 형식 요구 사항이 있습니다. 원본 데이터가 이 섹션에 설명된 조건을 충족하는 경우, PolyBase를 사용하여 원본 데이터 저장소에서 Azure SQL Data Warehouse로 직접 복사합니다. 조건을 충족하지 않는 경우, [PolyBase를 사용한 준비된 복사](#staged-copy-by-using-polybase)를 사용합니다.

> [!TIP]
> Data Lake Store에서 SQL Data Warehouse로 데이터를 효율적으로 복사하려면 [Azure Data Factory makes it even easier and convenient to uncover insights from data when using Data Lake Store with SQL Data Warehouse](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/)(SQL Data Warehouse와 함께 Data Lake Store를 사용할 경우, Azure Data Factory에서 데이터의 인사이트를 쉽고 편리하게 얻을 수있음)에서 자세한 내용을 참조하세요.

조건을 충족하지 않는 경우, Azure Data Factory는 설정을 확인한 후 데이터 이동을 위해 BULKINSERT 메커니즘으로 자동으로 대체됩니다.

1. **원본에 연결된 서비스** 유형은 계정 키 인증을 사용하는 Azure Blob 스토리지(**AzureBLobStorage**/**AzureStorage**) 또는 서비스 주체 인증을 사용하는 Azure Data Lake Storage Gen1(**AzureDataLakeStore**)입니다.
2. **입력 데이터 세트** 유형은 **AzureBlob** 또는 **AzureDataLakeStoreFile**입니다. `type` 속성 아래의 형식 유형은 다음 구성을 사용하는 **OrcFormat**, **ParquetFormat** 또는 **TextFormat**입니다.

   1. `fileName`은 와일드 카드 필터를 포함하지 않습니다.
   2. `rowDelimiter`는 **\n**이어야 합니다.
   3. `nullValue`는 **빈 문자열**("")로 설정되거나 기본값으로 남아 있고, `treatEmptyAsNull`은 기본값으로 남아 있거나 true로 설정됩니다.
   4. `encodingName`이 기본값인 **utf-8**로 설정되어 있습니다.
   5. `escapeChar`, `quoteChar` 및 `skipLineCount`는 지정되지 않습니다. PolyBase 지원은 ADF에서 `firstRowAsHeader`로 구성될 수 있는 머리글 행을 건너뜁니다.
   6. `compression`은 **no compression**, **GZip** 또는 **Deflate**일 수 있습니다.

    ```json
    "typeProperties": {
        "folderPath": "<blobpath>",
        "format": {
            "type": "TextFormat",
            "columnDelimiter": "<any delimiter>",
            "rowDelimiter": "\n",
            "nullValue": "",
            "encodingName": "utf-8",
            "firstRowAsHeader": <any>
        },
        "compression": {
            "type": "GZip",
            "level": "Optimal"
        }
    },
    ```

```json
"activities":[
    {
        "name": "CopyFromAzureBlobToSQLDataWarehouseViaPolyBase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "BlobDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "SqlDWSink",
                "allowPolyBase": true
            }
        }
    }
]
```

### <a name="staged-copy-by-using-polybase"></a>PolyBase를 사용한 준비된 복사

원본 데이터가 이전 섹션의 조건을 충족하지 않는 경우, 중간 준비 Azure Blob Storage 인스턴스를 통해 데이터 복사를 사용하도록 설정합니다. Azure Premium Storage일 수 없습니다. 이 경우, Azure Data Factory는 PolyBase의 데이터 형식 요구 사항을 충족하도록 데이터에 대해 자동으로 변환을 실행합니다. 그런 다음 PolyBase를 사용하여 데이터를 SQL Data Warehouse에 로드합니다. 마지막으로, Blob Storage에서 임시 데이터를 정리합니다. 준비 Azure Blob Storage 인스턴스를 통해 데이터를 복사하는 방법에 대한 자세한 내용은 [준비된 복사](copy-activity-performance.md#staged-copy)를 참조하세요.

이 기능을 사용하려면 중간 Blob Storage가 있는 Azure Storage 계정을 참조하는 [Azure Storage 연결된 서비스](connector-azure-blob-storage.md#linked-service-properties)를 만듭니다. 그런 다음, 복사 작업의 `enableStaging` 및 `stagingSettings` 속성을 아래 코드에 표시된 대로 지정합니다.

```json
"activities":[
    {
        "name": "CopyFromSQLServerToSQLDataWarehouseViaPolyBase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "SQLServerDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlDWSink",
                "allowPolyBase": true
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingBlob",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
]
```

## <a name="best-practices-for-using-polybase"></a>PolyBase를 사용하는 모범 사례

다음 섹션에서는 [Azure SQL Data Warehouse에 대한 모범 사례](../sql-data-warehouse/sql-data-warehouse-best-practices.md)에 설명된 모범 사례 이외의 추가 모범 사례를 제공합니다.

### <a name="required-database-permission"></a>필수 데이터베이스 권한

PolyBase를 사용하려면 SQL Data Warehouse에 데이터를 로드하는 사용자에게 대상 데이터베이스에 대한 [“CONTROL” 권한](https://msdn.microsoft.com/library/ms191291.aspx)이 있어야 합니다. 한 가지 방법은 해당 사용자를 **db_owner** 역할의 구성원으로 추가하는 것입니다. [SQL Data Warehouse 개요](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization)에서 작업 방법을 알아보세요.

### <a name="row-size-and-data-type-limits"></a>행 크기 및 데이터 형식 한도

PolyBase는 1MB보다 작은 행으로 제한됩니다. VARCHR(MAX), NVARCHAR(MAX) 또는 VARBINARY(MAX)에 로드할 수 없습니다. 자세한 내용은 [SQL Data Warehouse 서비스 용량 한도](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads)를 참조하세요.

원본 데이터에 1MB보다 큰 행이 있는 경우, 원본 테이블을 여러 개의 작은 테이블로 수직 분할하는 것이 좋습니다. 각 행의 최대 크기가 한도를 초과하지 않는지 확인합니다. 한도보다 작은 테이블은 PolyBase를 사용하여 로드하고 Azure SQL Data Warehouse에 병합할 수 있습니다.

### <a name="sql-data-warehouse-resource-class"></a>SQL Data Warehouse 리소스 클래스

가능한 최고 수준까지 처리량을 높이려면 PolyBase를 통해 SQL Data Warehouse에 데이터를 로드하는 사용자에게 더 큰 리소스 클래스를 할당합니다.

### <a name="tablename-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse의 **tableName**

다음 표에서는 JSON 데이터 세트의 **tableName** 속성을 지정하는 방법에 대한 예를 제공합니다. 스키마 및 테이블 이름의 여러 조합을 보여 줍니다.

| DB 스키마 | 테이블 이름 | **tableName** JSON 속성 |
| --- | --- | --- |
| dbo | MyTable | MyTable 또는 dbo.MyTable 또는 [dbo].[MyTable] |
| dbo1 | MyTable | dbo1.MyTable 또는 [dbo1].[MyTable] |
| dbo | My.Table | [My.Table] 또는 [dbo].[My.Table] |
| dbo1 | My.Table | [dbo1].[My.Table] |

다음 오류가 표시되는 경우, **tableName** 속성에 대해 지정한 값이 문제일 수 있습니다. **tableName** JSON 속성의 값을 지정하는 올바른 방법은 앞의 표를 참조하세요.

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a>기본값이 있는 열

현재, Data Factory의 PolyBase 기능은 대상 테이블과 동일한 열 수만 허용합니다. 예를 들어, 네 개의 열이 있고 그 중 한 열이 기본값으로 정의된 테이블이 있다고 가정합니다. 이 경우, 입력 데이터에 네 개의 열이 있어야 합니다. 3열 입력 데이터 세트를 제공하면 다음 메시지와 비슷한 오류가 발생합니다.

```
All columns of the table must be specified in the INSERT BULK statement.
```

NULL 값은 특별한 형태의 기본값입니다. 열이 Null을 허용하는 경우, 해당 열에 대한 Blob의 입력 데이터가 비어 있을 수 있습니다. 그러나 입력 데이터 세트에서는 누락할 수 없습니다. PolyBase는 Azure SQL Data Warehouse에서 누락된 값에 대해 NULL을 삽입합니다.

## <a name="data-type-mapping-for-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse에 대한 데이터 형식 매핑

Azure SQL Data Warehouse에서/로 데이터를 복사하는 경우, Azure SQL Data Warehouse 데이터 형식에서 Azure Data Factory 중간 데이터 형식으로 다음 매핑이 사용됩니다. 복사 작업에서 원본 스키마 및 데이터 형식을 싱크에 매핑하는 방법에 대한 자세한 내용은 [스키마 및 데이터 형식 매핑](copy-activity-schema-and-type-mapping.md)을 참조하세요.

| Azure SQL Data Warehouse 데이터 형식 | Data Factory 중간 데이터 형식 |
|:--- |:--- |
| bigint | Int64 |
| binary | Byte[] |
| bit | BOOLEAN |
| char | String, Char[] |
| date | Datetime |
| DateTime | Datetime |
| datetime2 | Datetime |
| Datetimeoffset | DateTimeOffset |
| 10진수 | 10진수 |
| FILESTREAM 특성(varbinary(max)) | Byte[] |
| Float | Double |
| 이미지 | Byte[] |
| int | Int32 |
| money | 10진수 |
| nchar | String, Char[] |
| ntext | String, Char[] |
| numeric | 10진수 |
| nvarchar | String, Char[] |
| real | 단일 |
| rowversion | Byte[] |
| smalldatetime | Datetime |
| smallint | Int16 |
| smallmoney | 10진수 |
| sql_variant | Object |
| text | String, Char[] |
| 실시간 | timespan |
| timestamp | Byte[] |
| tinyint | Byte |
| uniqueidentifier | Guid |
| varbinary | Byte[] |
| varchar | String, Char[] |
| xml | xml |

## <a name="next-steps"></a>다음 단계
Azure Data Factory의 복사 작업에서 원본 및 싱크로 지원되는 데이터 저장소 목록은 [지원되는 데이터 저장소 및 형식](copy-activity-overview.md##supported-data-stores-and-formats)을 참조하세요.
