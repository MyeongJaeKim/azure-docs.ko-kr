---
title: Azure Active Directory에서 엔터프라이즈 앱에 대한 사용자 프로비전 관리 | Microsoft Docs
description: Azure Active Directory를 사용하여 엔터프라이즈 앱에 대한 사용자 계정 프로비전을 관리하는 방법에 대해 알아봅니다.
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/13/2018
ms.author: barbkess
ms.reviewer: asmalser
ms.openlocfilehash: ca0be0d4e2b9f76a521d8fc04195311b3603e411
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180554"
---
# <a name="managing-user-account-provisioning-for-enterprise-apps-in-the-azure-portal"></a>Azure Portal에서 엔터프라이즈 앱에 대한 사용자 계정 프로비전 관리
이 문서에서는 [Azure Portal](https://portal.azure.com)을 사용하여 자동 사용자 계정 프로비전 및 지원되는 애플리케이션에 대한 프로비전 해제를 관리하는 방법을 설명합니다. 자동 사용자 계정 프로비전 및 작동 방식에 대한 자세한 내용은 [Azure Active Directory를 사용하여 SaaS 애플리케이션에 사용자 프로비전 및 프로비전 해제 자동화](user-provisioning.md)를 참조하세요.

## <a name="finding-your-apps-in-the-portal"></a>포털에서 앱 찾기
디렉터리에서 Single Sign-On에 대해 구성된 모든 애플리케이션은 [Azure Portal](https://portal.azure.com)에서 보고 관리할 수 있습니다. 애플리케이션은 포털의 **모든 서비스**&gt;**엔터프라이즈 애플리케이션** 섹션에서 찾을 수 있습니다. 엔터프라이즈 앱은 조직 내에서 배포 및 사용되는 앱입니다.

![Enterprise 애플리케이션 창](./media/configure-automatic-user-provisioning-portal/enterprise-apps-pane.png)

왼쪽의 **모든 애플리케이션** 링크를 선택하면 갤러리에서 추가된 앱을 포함하여 구성된 모든 앱의 목록이 표시됩니다. 앱을 선택하면 해당 앱의 리소스 창이 로드되며 여기에서 해당 앱에 대한 보고서를 볼 수 있고 다양한 설정을 관리할 수 있습니다.

사용자 계정 프로비전 설정은 왼쪽의 **프로비전** 을 선택하여 관리할 수 있습니다.

![애플리케이션 리소스 창](./media/configure-automatic-user-provisioning-portal/enterprise-apps-provisioning.png)

## <a name="provisioning-modes"></a>프로비전 모드
**프로비전** 창은 엔터프라이즈 애플리케이션을 위해 지원되는 프로비전 모드를 표시하는 **모드** 메뉴로 시작하고 해당 모드가 구성될 수 있도록 합니다. 사용 가능한 옵션은 다음과 같습니다.

* **자동** - Azure AD가 이 애플리케이션에 대해 사용자 계정의 자동 API 기반의 프로비전 및/또는 프로비전 해제를 지원하는 경우 이 옵션이 나타납니다. 이 모드를 선택하면 관리자가 애플리케이션의 사용자 관리 API에 연결하는 Azure AD를 구성하고 Azure AD와 앱 간 사용자 계정 데이터의 흐름 방식을 정의하는 계정 매핑 및 워크플로를 작성하고 Azure AD 프로비전 서비스를 관리할 수 있도록 안내하는 인터페이스가 표시됩니다.
* **수동** - Azure AD가 이 애플리케이션에 대한 사용자 계정의 자동 프로비전을 지원하지 않을 경우 이 옵션이 표시됩니다. 이 옵션을 사용하면 애플리케이션에 저장된 사용자 계정 레코드를 해당 애플리케이션에서 제공한 사용자 관리 및 프로비전 기능(SAML Just-In-Time 프로비전 포함 가능)에 따라 외부 프로세스를 사용하여 관리해야 합니다.

## <a name="configuring-automatic-user-account-provisioning"></a>자동 사용자 계정 프로비전 구성
**자동** 옵션을 선택하면 4개의 섹션으로 나뉜 화면이 표시됩니다.

### <a name="admin-credentials"></a>관리자 자격 증명
Azure AD가 애플리케이션의 사용자 관리 API에 연결하는 데 필요한 자격 증명을 이 섹션에 입력합니다. 필요한 입력은 애플리케이션에 따라 다릅니다. 특정 애플리케이션에 대한 자격 증명 형식 및 요구 사항에 대해 알아보려면 [특정 애플리케이션에 대한 구성 자습서](user-provisioning.md)를 참조하세요.

**연결 테스트** 단추를 선택하면 제공된 자격 증명을 사용하여 Azure AD에서 해당 앱의 프로비전 앱에 연결해 봄으로써 자격 증명을 테스트할 수 있습니다.

### <a name="mappings"></a>매핑
이 섹션에서는 사용자 계정이 프로비전되거나 업데이트될 때 Azure AD와 대상 애플리케이션 간에 흐르는 사용자 특성을 보고 편집할 수 있습니다.

Azure AD 사용자 개체와 각 SaaS 앱의 사용자 개체 간의 미리 구성된 매핑 세트가 있습니다. 일부 앱은 그룹이나 연락처 같은 다른 유형의 개체를 관리합니다. 표에서 매핑 중 하나를 선택하면 오른쪽에 표시되는 매핑 편집기에서 매핑을 보고 사용자 지정할 수 있습니다.

![애플리케이션 리소스 창](./media/configure-automatic-user-provisioning-portal/enterprise-apps-provisioning-mapping.png)

지원되는 사용자 지정은 다음과 같습니다.

* Azure AD 사용자 개체를 SaaS 앱의 사용자 개체로 매핑하는 것과 같이 특정 개체에 대한 매핑을 사용하거나 사용하지 않도록 설정합니다.
* Azure AD 사용자 개체에서 앱의 사용자 개체로 흐르는 특성을 편집합니다. 특성 매핑에 대한 자세한 내용은 [특성 매핑 유형 이해하기](customize-application-attributes.md#understanding-attribute-mapping-types)를 참조하세요.
* Azure AD는 대상 애플리케이션에서 수행하는 프로비전 작업을 필터링합니다. Azure AD가 개체를 완전히 동기화하도록 하지 않고 수행되는 작업을 제한할 수 있습니다. 예를 들어 **업데이트**만 선택하면 Azure AD가 애플리케이션에서 기존 사용자 계정을 업데이트만 하며 새 계정을 만들지는 않습니다. **만들기**만 선택하면 Azure가 새 사용자 계정을 만들기만 하고 기존 계정을 업데이트하지는 않습니다. 이 기능을 통해 관리자는 계정을 만들기 위해 여러 매핑을 만들고 워크플로를 업데이트할 수 있습니다.

### <a name="settings"></a>설정
이 섹션에서 관리자가 필요에 따라 프로비전 캐시를 지우고 서비스를 다시 시작할 뿐만 아니라 선택한 애플리케이션에 대한 Azure AD 프로비전 서비스를 시작하고 중지할 수 있습니다.

프로비전을 애플리케이션에 대해 처음으로 사용하는 경우 **프로비전 상태**를 **켜기**로 변경하여 서비스를 활성화합니다. 이렇게 변경하면 Azure AD 프로비전 서비스가 초기 동기화를 수행하며 여기서 **사용자 및 그룹** 섹션에 할당된 사용자를 읽고 대상 애플리케이션을 쿼리한 다음, Azure AD **매핑** 섹션에 정의된 프로비전 작업을 수행합니다. 이 과정에서 프로비전 서비스가 관리하는 사용자 계정에 대해 캐시된 데이터를 저장하므로 할당 범위에 들지 않는 대상 애플리케이션 내의 관리되지 않는 계정은 프로비전 해제 작업의 영향을 받지 않습니다. 초기 동기화 후 프로비전 서비스는 10분 간격으로 사용자 및 그룹 개체를 자동으로 동기화합니다.

**프로비전 상태**를 **끄기**로 변경하면 프로비전 서비스가 간단히 일시 중지됩니다. 이 상태에서는 Azure가 앱의 어떤 사용자 또는 그룹 개체도 만들거나 업데이트하거나 제거하지 않습니다. 상태를 켜기로 다시 변경하면 서비스가 중단된 위치를 선택합니다.

**Clear current state and restart synchronization** (현재 상태를 지우고 동기화 다시 시작) 확인란을 선택하고 저장하면 프로비전 서비스를 중지하고 Azure AD가 관리하는 계정에 대해 캐시된 데이터를 덤프하고 서비스를 다시 시작하고 초기 동기화를 다시 수행합니다. 이 옵션을 사용하면 관리자가 프로비전 배포 프로세스를 다시 시작할 수 있습니다.

### <a name="synchronization-details"></a>동기화 세부 정보
이 섹션에서는 프로비전 서비스의 애플리케이션에 대한 첫 번째 실행 및 마지막 실행과 관리 중인 사용자 및 그룹 개체의 수를 포함하여 프로비전 서비스의 작업에 대한 추가적인 세부 정보를 제공합니다.

Azure AD와 대상 애플리케이션 간에 만들어지고 업데이트 및 제거되는 모든 사용자 및 그룹의 로그를 제공하는 **프로비전 활동 보고서**와 읽기, 만들기, 업데이트 또는 제거에 실패한 사용자 및 그룹 개체에 대한 자세한 오류 메시지를 제공하는 **프로비전 오류 보고서**에 대한 링크가 제공됩니다. 



