---
title: Azure SQL Database 보안 개요 | Microsoft Docs
description: 클라우드와 온-프레미스 SQL Server 간 차이를 포함하여 Azure SQL Database 및 SQL Server 보안에 대해 알아보세요.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto, carlrab, emlisa
manager: craigg
ms.date: 01/29/2019
ms.openlocfilehash: 7eb3b115c1d16c2a5c380178d316a60b854e80df
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55462021"
---
# <a name="an-overview-of-azure-sql-database-security-capabilities"></a>Azure SQL Database 보안 기능의 개요

이 문서에서는 Azure SQL Database를 사용하여 애플리케이션의 데이터 계층에 보안을 설정하기 위한 기본 사항을 간략하게 설명합니다. 여기서 설명하는 보안 전략은 아래 그림에 나와 있는 계층형 심층 방어 방식을 따르며 외부에서 내부로 적용됩니다.

![sql-security-layer.png](media/sql-database-security-overview/sql-security-layer.png)

## <a name="network-security"></a>네트워크 보안

Microsoft Azure SQL Database는 클라우드 및 엔터프라이즈 애플리케이션용 관계형 데이터베이스 서비스를 제공합니다. 고객 데이터를 보호하기 위해 방화벽은 Azure Virtual Network 트래픽 출처 또는 IP 주소를 기준으로 액세스 권한이 명시적으로 부여될 때까지 네트워크에서 데이터베이스 서버에 액세스할 수 없도록 차단합니다.

### <a name="ip-firewall-rules"></a>IP 방화벽 규칙

IP 방화벽 규칙은 각 요청이 시작된 IP 주소를 기준으로 하여 데이터베이스 액세스 권한을 부여합니다. 자세한 내용은 [Azure SQL Database 및 SQL Data Warehouse 방화벽 규칙 개요](sql-database-firewall-configure.md)를 참조하세요.

### <a name="virtual-network-firewall-rules"></a>Virtual Network 방화벽 규칙

[Virtual Network 서비스 엔드포인트](../virtual-network/virtual-network-service-endpoints-overview.md)는 Azure 백본을 통해 Virtual Network 연결을 확장하며, 트래픽이 생성되는 Virtual Network 서브넷을 Azure SQL Database가 식별할 수 있도록 합니다. 트래픽이 Azure SQL Database로 전송되도록 하려면 SQL [서비스 태그](../virtual-network/security-overview.md)를 사용해 네트워크 보안 그룹을 통한 아웃바운드 트래픽을 허용합니다.

Azure SQL Database는 [Virtual Network 규칙](sql-database-vnet-service-endpoint-rule-overview.md)을 통해 Virtual Network 내의 선택한 서브넷에서 전송된 통신만 수락할 수 있습니다.

> [!NOTE]
> 방화벽 규칙을 사용한 액세스 제어는 **Azure SQL Database Managed Instance**에 적용되지 않습니다. 필요한 네트워킹 구성에 대한 자세한 내용은 [Managed Instance에 연결](sql-database-managed-instance-connect-app.md)을 참조하세요.

## <a name="access-management"></a>액세스 관리

> [!IMPORTANT]
> Azure 내에서 데이터베이스와 데이터베이스 서버를 관리하는 작업은 포털 사용자 계정의 역할 할당을 통해 제어됩니다. 이 아티클에 대한 자세한 내용은 [Azure Portal의 역할 기반 액세스 제어](../role-based-access-control/overview.md)를 참조하세요.

### <a name="authentication"></a>인증

인증은 사용자의 신원을 증명하는 과정입니다. Azure SQL Database는 두 가지 인증 유형을 지원합니다.

- **SQL 인증**:

    SQL Database 인증은 사용자 이름과 암호를 사용하여 [Azure SQL Database](sql-database-technical-overview.md)에 연결할 때 사용자가 수행하는 인증을 지칭합니다. 데이터베이스용 데이터베이스 서버를 만들 때 사용자 이름과 암호를 사용하는 "서버 관리자" 로그인을 지정해야 합니다. "서버 관리자"는 이러한 자격 증명을 사용하여 해당 데이터베이스 서버의 모든 데이터베이스에 데이터베이스 소유자로 인증할 수 있습니다. 그리고 나면 서버 관리자는 추가 SQL 로그인 및 사용자를 만들 수 있으며, 그러면 사용자가 사용자 이름과 암호를 사용하여 연결할 수 있습니다.

- **Azure Active Directory 인증**:

    Azure Active Directory 인증은 Azure AD(Azure Active Directory)의 ID를 사용하여 [Azure SQL Database](sql-database-technical-overview.md) 및 [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)에 연결하는 메커니즘입니다. 관리자는 Azure AD 인증을 통해 데이터베이스 사용자의 ID 및 권한과 기타 Microsoft 서비스를 중앙 위치 한 곳에서 관리할 수 있습니다. 그러면 저장해야 하는 암호를 최소화하고 중앙 집중식 암호 순환 정책을 사용할 수 있습니다.

     SQL Database를 사용한 Azure AD 인증을 사용하려면 서버 관리자 **Active Directory 관리자**를 만들어야 합니다. 자세한 내용은 [Azure Active Directory 인증을 사용하여 SQL Database에 연결](sql-database-aad-authentication.md)을 참조하세요. Azure AD 인증에서는 관리 계정과 페더레이션된 계정이 모두 지원됩니다. 페더레이션된 계정은 Azure AD와 페더레이션된 고객 도메인용 Windows 사용자 및 그룹을 지원합니다.

    사용 가능한 추가 Azure AD 인증 옵션으로는 [다단계 인증](../active-directory/authentication/concept-mfa-howitworks.md) 및 [조건부 액세스](sql-database-conditional-access.md)를 비롯한 [SQL Server Management Studio용 Active Directory 유니버설 인증](sql-database-ssms-mfa-authentication.md) 연결이 있습니다.

> [!IMPORTANT]
> Azure 내에서 데이터베이스와 서버를 관리하는 작업은 포털 사용자 계정의 역할 할당을 통해 제어됩니다. 이 아티클에 대한 자세한 내용은 [Azure Portal의 역할 기반 액세스 제어](../role-based-access-control/overview.md)를 참조하세요. 방화벽 규칙을 사용한 액세스 제어는 **Azure SQL Database Managed Instance**에 적용되지 않습니다. 필요한 네트워킹 구성에 대한 자세한 내용은 [Managed Instance에 연결](sql-database-managed-instance-connect-app.md)하는 방법에 대한 다음 문서를 참조하세요.

Azure SQL Database 내에서 사용자에게 할당되는 권한을 지칭하는 권한 부여는 사용자가 수행할 수 있는 작업을 결정합니다. 데이터베이스 수준 권한을 정의하는 [데이터베이스 역할](/sql/relational-databases/security/authentication-access/database-level-roles)에 사용자 계정을 추가하거나, 사용자에게 특정 [개체 수준 권한](/sql/relational-databases/security/permissions-database-engine)을 부여하는 방식으로 권한을 제어합니다. 자세한 내용은 [로그인 및 사용자](sql-database-manage-logins.md)를 참조하세요.

사용자가 직무를 수행하는 데 필요한 최소 권한이 있는 역할에 사용자를 추가하는 것이 모범 사례입니다. 서버 관리자 계정은 db_owner 역할의 구성원입니다. 이 역할은 광범위한 권한을 포함하므로 사용자에게 부여할 때는 주의해야 합니다. Azure SQL Database와 함께 애플리케이션을 사용할 때는 권한이 제한된 [애플리케이션 역할](/sql/relational-databases/security/authentication-access/application-roles)을 사용합니다. 그러면 데이터베이스에 연결하는 애플리케이션에 필요한 최소 권한만 제공됩니다.

### <a name="row-level-security"></a>행 수준 보안

행 수준 보안을 통해 고객은 쿼리를 실행하는 사용자의 특성(예: 그룹 멤버 자격 또는 실행 컨텍스트)을 기반으로 하여 데이터베이스 테이블의 행에 대한 액세스를 제어할 수 있습니다. 자세한 내용은 [행 수준 보안](/sql/relational-databases/security/row-level-security)을 참조하세요.

![azure-database-rls.png](media/sql-database-security-overview/azure-database-rls.png)

  이 인증 방법은 사용자 이름과 암호를 사용합니다. 

Azure SQL Database의 권한 개요는 [로그인 및 사용자](sql-database-manage-logins.md#permissions)를 참조하세요.

## <a name="threat-protection"></a>위협 보호

SQL Database는 감사 및 위협 검색 기능을 제공하여 고객 데이터를 보호합니다.

### <a name="sql-auditing-in-log-analytics-and-event-hubs"></a>Log Analytics 및 Event Hubs의 SQL 감사

SQL Database 감사는 고객이 소유한 Azure Storage 계정의 감사 로그에 데이터베이스 이벤트를 기록하여 보안 표준 준수 상태를 유지할 수 있도록 지원하며 데이터베이스 활동을 추적합니다. 사용자는 감사를 통해 진행 중인 데이터베이스 활동을 모니터링하고 이전 활동을 분석 및 조사하여 잠재적 위협이나 악용 의심 사례 및 보안 위반을 식별할 수 있습니다. 자세한 내용은 [SQL Database 감사 시작](sql-database-auditing.md)을 참조하세요.  

### <a name="sql-threat-detection"></a>SQL 위협 검색

위협 검색을 수행하면 감사 로그를 분석해 유해한 데이터베이스 액세스/악용 시도와 비정상적인 동작을 파악함으로써 감사 효율성을 개선할 수 있습니다. SQL 삽입 공격, 데이터 침투 가능성, 무차별 암호 대입 공격(brute force attack) 등의 의심스러운 활동이나 비정상적인 액세스 패턴이 있으면 경고가 생성됩니다. [Azure Security Center](https://azure.microsoft.com/services/security-center/)에서 위협 검색 경고를 확인할 수 있습니다. 여기서는 의심스러운 활동의 세부 정보가 제공되며, 위협 완화를 위한 조치와 함께 추가 조사를 위한 권장 사항이 제공됩니다. 위협 감지 비용은 $15/서버/월입니다. 최초 60일 동안은 무료로 사용 가능합니다. 자세한 내용은 [SQL Database 위협 감지 시작](sql-database-threat-detection.md)을 참조하세요.

![azure-database-td.jpg](media/sql-database-security-overview/azure-database-td.jpg)

## <a name="information-protection-and-encryption"></a>정보 보호 및 암호화

### <a name="transport-layer-security-tls-encryption-in-transit"></a>TLS(전송 계층 보안)(전송 중 암호화)

SQL Database는 [전송 계층 보안](https://support.microsoft.com/en-us/help/3135244/tls-1-2-support-for-microsoft-sql-server)을 사용하여 이동 중인 데이터를 암호화해 고객 데이터를 보호합니다.

> [!IMPORTANT]
> Azure SQL Database는 항상 모든 연결에 암호화(SSL/TLS)를 적용하여 데이터베이스와 클라이언트 간에 “전송”되는 모든 데이터를 암호화합니다. 이 동작은 연결 문자열의 **암호화** 또는 **TrustServerCertificate** 설정에 관계없이 발생합니다.
>
> 애플리케이션의 연결 문자열에서, 암호화된 연결을 사용하고 서비스 인증서를 신뢰하지 _않도록_ 지정해야 합니다(ADO.NET 드라이버의 경우 **Encrypt=True** 및 **TrustServerCertificate=False**). 이렇게 하면 애플리케이션이 서버를 확인하고 암호화를 적용하도록 강제하여 중간자 공격으로부터 애플리케이션을 보호할 수 있습니다. Azure Portal에서 연결 문자열을 얻는 경우 올바른 설정이 사용됩니다.
>
> TLS 및 연결에 대한 정보는 [TLS 고려 사항](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity)을 참조하세요.

### <a name="transparent-data-encryption-encryption-at-rest"></a>투명한 데이터 암호화(미사용 데이터 암호화)

[Azure SQL Database용 TDE(투명한 데이터 암호화)](transparent-data-encryption-azure-sql.md)는 원시 파일이나 백업에 무단/오프라인으로 액세스할 수 없도록 미사용 데이터를 보호하기 위해 보안 계층을 추가합니다. 무단/오프라인 액세스의 일반적인 시나리오에는 데이터 센터 도난, 안전하지 않은 하드웨어 또는 미디어(예: 디스크 드라이브 및 백업 테이프) 폐기 등이 포함됩니다. TDE는 AES 알고리즘을 사용하여 전체 데이터베이스를 암호화합니다. 따라서 애플리케이션 개발자가 기존 애플리케이션을 변경할 필요가 없습니다.

Azure에서는 새로 만드는 모든 SQL Database가 기본적으로 암호화되며, 기본 제공 서버 인증서를 통해 데이터베이스 암호화 키가 보호됩니다.  서비스에서 인증서 유지 관리 및 순환을 관리하므로 사용자 입력은 필요하지 않습니다. 암호화 키를 직접 제어하려는 고객은 [Azure Key Vault](../key-vault/key-vault-secure-your-key-vault.md)에서 키를 관리할 수 있습니다.

### <a name="key-management-with-azure-key-vault"></a>Azure Key Vault으로 키 관리

고객은 TDE( [투명한 데이터 암호화](/sql/relational-databases/security/encryption/transparent-data-encryption))용으로 지원되는 BYOK([Bring Your Own Key](transparent-data-encryption-byok-azure-sql.md))를 활용해 Azure의 클라우드 기반 외부 키 관리 시스템인  [Azure Key Vault](../key-vault/key-vault-secure-your-key-vault.md)를 사용하여 키 관리 및 순환을 직접 제어할 수 있습니다. 데이터베이스의 키 자격 증명 모음 액세스 권한이 철회되면 데이터베이스를 암호 해독하여 메모리로 읽어들일 수 없습니다. 중앙 키 관리 플랫폼을 제공하며 철저하게 모니터링되는 HSM(하드웨어 보안 모듈)을 활용하는 Azure Key Vault를 사용하면 키와 데이터 관리 작업을 분리하여 보안 규정 준수 요구 사항을 충족할 수 있습니다.

### <a name="always-encrypted-encryption-in-use"></a>Always Encrypted(사용 중인 데이터 암호화)

![azure-database-ae.png](media/sql-database-security-overview/azure-database-ae.png)

[Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine)는 신용 카드 번호, 주민 등록 번호 또는 _확인이 필요_한 데이터와 같이 특정 데이터베이스 열에 저장된 중요한 데이터를 액세스할 수 없도록 보호하는 기능입니다. 예를 들어 이 기능을 통해 데이터베이스에 액세스하여 관리 작업을 수행할 권한은 부여되었지만 업무상 암호화된 열의 특정 데이터에는 액세스할 필요가 없는 데이터베이스 관리자 또는 기타 권한 있는 사용자로부터 데이터를 보호할 수 있습니다. 데이터는 항상 암호화되므로 암호화 키 액세스 권한이 있는 클라이언트 애플리케이션에서 처리해야 하는 경우에만 암호화된 데이터의 암호가 해독됩니다.  암호화 키는 SQL에 표시되지 않으며 [Windows 인증서 저장소](sql-database-always-encrypted.md) 또는 [Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md)에 저장할 수 있습니다.

### <a name="masking"></a>마스킹

![azure-database-ddm.png](media/sql-database-security-overview/azure-database-ddm.png)

#### <a name="dynamic-data-masking"></a>동적 데이터 마스킹

SQL Database 동적 데이터 마스킹에서는 권한이 없는 사용자에 대해 중요한 데이터를 마스킹해 표시함으로써 데이터 노출을 제한합니다. 동적 데이터 마스킹은 Azure SQL Database에서 잠재적으로 중요한 데이터를 자동으로 검색하고 애플리케이션 계층에 미치는 영향을 최소화하면서 이러한 필드를 마스킹할 수 있는 실행 가능한 권장 사항을 제공합니다. 이 기능은 지정된 데이터베이스 필드를 통해 쿼리의 결과 집합에 있는 중요한 데이터를 혼란스럽게 만들면서 작동하지만 데이터베이스의 데이터를 변경하지는 않습니다. 자세한 내용은 [SQL 데이터베이스 동적 데이터 마스킹](sql-database-dynamic-data-masking-get-started.md) 시작을 참조하세요.

#### <a name="static-data-masking"></a>정적 데이터 마스킹

[정적 데이터 마스킹](/sql/relational-databases/security/static-data-masking)은 [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) 18.0 미리 보기 5 이상에서 사용 가능한 클라이언트 쪽 도구입니다.  사용자는 정적 데이터 마스킹을 통해 선택한 열의 데이터가 영구 마스킹된 데이터베이스의 복사본을 만들 수 있습니다. 사용 가능한 마스킹 기능에는 NULL 마스킹, 단일 값 마스킹, 순서 섞기/그룹 순서 섞기 마스킹, 문자열 복합 마스킹 등이 있습니다. 조직에 데이터의 마스킹된 복사본이 있으면 해당 복사본을 공유해 프로덕션 환경과 테스트 환경을 분리할 수 있습니다. 그러면 기타 모든 데이터베이스 특성이 충분히 보호된 상태로 중요한 데이터가 충분히 보호됩니다. 타사가 데이터베이스에 액세스해야 한다면 데이터베이스를 마스킹하는 것이 좋습니다.

## <a name="security-management"></a>보안 관리

### <a name="sql-vulnerability-assessment"></a>SQL 취약성 평가

[SQL 취약성 평가](sql-vulnerability-assessment.md)는 전반적인 데이터베이스 보안을 미리 개선하기 위해 잠재적인 데이터베이스 취약성을 검색, 추적 및 수정할 수 있는 손쉽게 구성 가능한 서비스입니다. VA(취약성 평가)는 고급 SQL 보안 기능용 통합 패키지인 SQL ADS(Advanced Data Security) 제품에 포함되어 있습니다. 중앙 SQL ADS 포털을 통해 취약성 평가에 액세스하고 취약성 평가 서비스를 관리할 수 있습니다.

### <a name="data-discovery--classification"></a>데이터 검색 및 분류

데이터 검색 및 분류(현재 미리 보기)는 데이터베이스에 있는 중요한 데이터를 검색, 분류, 레이블 지정 및 보호하기 위한 Azure SQL Database에 내장된 고급 기능을 제공합니다. 업무/금융, 의료, 개인 데이터 등의 매우 중요한 데이터를 검색하고 분류하는 작업은 조직 정보 보호 상태를 유지하는 데 중추적인 역할을 할 수 있습니다. 그것은 다음에 대한 인프라 역할을 할 수 있습니다.

- 중요한 데이터에 대한 비정상적인 엑세스 모니터링(감사) 및 경고하는 것과 같은 다양한 보안 시나리오.
- 매우 중요한 데이터가 들어 있는 데이터베이스에 대한 액세스 제어 및 보안 강화.
- 데이터 프라이버시 표준 및 규정 준수 요구 사항을 충족하도록 지원.

자세한 내용은 [SQL DB Data 검색 및 분류 시작](sql-database-data-discovery-and-classification.md)을 참조하세요.

### <a name="compliance"></a>규정 준수

Azure SQL Database는 위의 기능 및 애플리케이션이 다양한 보안 요구 사항을 충족하는 데 도움이 될 수 있는 기능을 포함할 뿐 아니라, 정기 감사도 받고 있으며 다수의 규정 준수 표준 충족 인증도 취득했습니다. 자세한 내용은 [Microsoft Azure 보안 센터](https://azure.microsoft.com/support/trust-center/)를 참조하세요. 여기서 최신 [SQL Database 규정 준수 인증서](https://www.microsoft.com/trustcenter/compliance/complianceofferings) 목록을 찾을 수 있습니다.

## <a name="next-steps"></a>다음 단계

- SQL Database에서 액세스 제어 기능을 사용하는 방법에 대한 설명은 [액세스 제어](sql-database-control-access.md)를 참조하세요.
- 데이터베이스 감사 내용은 [SQL Database 감사](sql-database-auditing.md)를 참조하세요.
- 위협 요소 탐지 내용은 [SQL Database 위협 요소 탐지](sql-database-threat-detection.md)를 참조하세요.
