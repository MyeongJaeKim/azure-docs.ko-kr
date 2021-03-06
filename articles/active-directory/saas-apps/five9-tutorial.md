---
title: '자습서: Five9 Plus Adapter(CTI, Contact Center Agents)와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory와 Five9 Plus Adapter(CTI, Contact Center Agents) 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: c4eeb3da3a3ac8cc6464e29881843a5aefd452d3
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55177596"
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a>자습서: Five9 Plus Adapter(CTI, Contact Center Agents)와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 Five9 Plus Adapter(CTI, Contact Center Agents)를 통합하는 방법에 대해 알아봅니다.

Five9 Plus Adapter(CTI, Contact Center Agents)를 Azure AD와 통합하면 다음과 같은 이점이 있습니다.

- Five9 Plus Adapter(CTI, Contact Center Agents)에 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
- 사용자가 자신의 Azure AD 계정으로 Five9 Plus Adapter(CTI, Contact Center Agents)에 자동으로 로그온(Single Sign-On) 되도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory의 애플리케이션 액세스 및 Single Sign-On이란 무엇인가요?](../manage-apps/what-is-single-sign-on.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

Five9 Plus Adapter(CTI, Contact Center Agents)와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- Five9 Plus Adapter(CTI, Contact Center Agents) Single Sign-On이 설정된 구독

> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.

이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [평가판 제품](https://azure.microsoft.com/pricing/free-trial/)에서 1개월 평가판을 얻을 수 있습니다.

## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다.  이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 Five9 Plus Adapter(CTI, Contact Center Agents) 추가
1. Azure AD Single Sign-on 구성 및 테스트

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-the-gallery"></a>갤러리에서 Five9 Plus Adapter(CTI, Contact Center Agents) 추가
Five9 Plus Adapter(CTI, Contact Center Agents)가 Azure AD에 통합되도록 구성하려면 갤러리에서 Five9 Plus Adapter(CTI, Contact Center Agents)를 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 Five9 Plus Adapter(CTI, Contact Center Agents)를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다. 

    ![Active Directory][1]

1. **엔터프라이즈 응용 프로그램**으로 이동합니다. 그런 후 **모든 애플리케이션**으로 이동합니다.

    ![애플리케이션][2]
    
1. 새 애플리케이션을 추가하려면 대화 상자 맨 위 있는 **새 애플리케이션** 단추를 클릭합니다.

    ![애플리케이션][3]

1. 검색 상자에 **Five9 Plus Adapter(CTI, Contact Center Agents)** 를 입력합니다.

    ![Azure AD 테스트 사용자 만들기](./media/five9-tutorial/tutorial_five9_search.png)

1. 결과 패널에서 **Five9 Plus Adapter(CTI, Contact Center Agents)** 를 선택한 다음 **추가** 단추를 클릭하여 애플리케이션을 추가합니다.

    ![Azure AD 테스트 사용자 만들기](./media/five9-tutorial/tutorial_five9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD Single Sign-on 구성 및 테스트
이 섹션에서는 “Britta Simon”이라는 테스트 사용자를 기반으로 Five9 Plus Adapter(CTI, Contact Center Agents)에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD의 사용자에 해당하는 Five9 Plus Adapter(CTI, Contact Center Agents)의 사용자가 누구인지 Azure AD에서 알고 있어야 합니다. 즉, Azure AD 사용자와 Five9 Plus Adapter(CTI, Contact Center Agents)의 관련 사용자 간에 링크 관계가 설정되어야 합니다.

Five9 Plus Adapter(CTI, Contact Center Agents)에서 Azure AD의 **사용자 이름** 값을 **Username** 값으로 할당하여 링크 관계를 설정합니다.

Five9 Plus Adapter(CTI, Contact Center Agents)에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
1. **[Azure AD 테스트 사용자 만들기](#creating-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On 테스트하는 데 사용합니다.
1. **[Five9 Plus Adapter(CTI, Contact Center Agents) 테스트 사용자 만들기](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)** - Britta Simon의 Azure AD 표현과 연결된 사용자를 Five9 Plus Adapter(CTI, Contact Center Agents)에 만듭니다.
1. **[Azure AD 테스트 사용자 할당](#assigning-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
1. **[Single Sign-On 테스트](#testing-single-sign-on)** - 구성이 작동하는지 확인합니다.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정하고 Five9 Plus Adapter(CTI, Contact Center Agents) 애플리케이션에서 Single Sign-On을 구성합니다.

**Five9 Plus Adapter(CTI, Contact Center Agents)에서 Azure AD Single Sign-On을 구성하려면 다음 단계를 수행합니다.**

1. Azure Portal의 **Five9 Plus Adapter(CTI, Contact Center Agents)** 애플리케이션 통합 페이지에서 **Single Sign-On**을 클릭합니다.

    ![Configure Single Sign-On][4]

1. **Single Sign-On** 대화 상자에서 **모드**를 **SAML 기반 로그온**으로 선택하여 Single Sign-On을 사용하도록 설정합니다.
 
    ![Configure Single Sign-On](./media/five9-tutorial/tutorial_five9_samlbase.png)

1. **Five9 Plus Adapter(CTI, Contact Center Agents) 도메인 및 URL** 섹션에서 다음 단계를 수행합니다.

    ![Configure Single Sign-On](./media/five9-tutorial/tutorial_five9_url.png)
    
    a. **식별자** 텍스트 상자에서 다음 패턴을 사용하여 URL을 입력합니다.

    |    Environment      |       URL      |
    | :-- | :-- |
    | “Five9 Plus Adapter for Microsoft Dynamics CRM”의 경우 | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | “Five9 Plus Adapter for Zendesk”의 경우 | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | “Five9 Plus Adapter for Agent Desktop Toolkit”의 경우 | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    b. **회신 URL** 텍스트 상자에 다음 패턴으로 URL을 입력합니다.

    |      Environment     |      URL      |
    | :--                  | :--           |
    | “Five9 Plus Adapter for Microsoft Dynamics CRM”의 경우 | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | “Five9 Plus Adapter for Zendesk”의 경우 | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | “Five9 Plus Adapter for Agent Desktop Toolkit”의 경우 | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

1. **SAML 서명 인증서** 섹션에서 **인증서(Base64)** 를 클릭한 후 컴퓨터에 인증서 파일을 저장합니다.

    ![Configure Single Sign-On](./media/five9-tutorial/tutorial_five9_certificate.png) 

1. **저장** 단추를 클릭합니다.

    ![Configure Single Sign-On](./media/five9-tutorial/tutorial_general_400.png)

1. **Five9 Plus Adapter(CTI, Contact Center Agents) 구성** 섹션에서 **Five9 Plus Adapter(CTI, Contact Center Agents) 구성**을 클릭하고 **로그온 구성** 창을 엽니다. **빠른 참조 섹션**에서 **로그아웃 URL, SAML 엔터티 ID 및 SAML Single Sign-On 서비스 URL**을 복사합니다.

    ![Configure Single Sign-On](./media/five9-tutorial/tutorial_five9_configure.png) 

1. **Five9 Plus Adapter(CTI, Contact Center Agents)** 에서 Single Sign-On을 구성하려면 다운로드한 **인증서(Base64), 로그아웃 URL, SAML 엔터티 ID 및 SAML Single Sign-On 서비스 URL**을 [Five9 Plus Adapter(CTI, Contact Center Agents) 지원 팀](https://www.five9.com/about/contact)에 보내야 합니다. 또한 SSO를 더 구성하려면 어댑터에 따라 다음 단계를 수행합니다.

    a. “Five9 Plus Adapter for Agent Desktop Toolkit” 관리자 가이드: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)
    
    b. “Five9 Plus Adapter for Microsoft Dynamics CRM” 관리자 가이드: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)
    
    다. “Five9 Plus Adapter for Zendesk” 관리자 가이드: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)


> [!TIP]
> 이제 앱을 설정하는 동안 [Azure Portal ](https://portal.azure.com) 내에서 이러한 지침의 간결한 버전을 읽을 수 있습니다.  **Active Directory &gt; 엔터프라이즈 애플리케이션** 섹션에서 이 앱을 추가한 후에는 **Single Sign-On** 탭을 클릭하고 맨 아래에 있는 **구성** 섹션을 통해 포함된 설명서에 액세스하면 됩니다. 포함된 설명서 기능에 대한 자세한 내용은 [Azure AD 포함된 설명서]( https://go.microsoft.com/fwlink/?linkid=845985)에서 확인할 수 있습니다.
> 

### <a name="creating-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기
이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

![Azure AD 사용자 만들기][100]

**Azure AD에서 테스트 사용자를 만들려면 다음 단계를 수행하세요.**

1. **Azure Portal**의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure AD 테스트 사용자 만들기](./media/five9-tutorial/create_aaduser_01.png) 

1. 사용자 목록을 표시하려면 **사용자 및 그룹**으로 이동한 후 **모든 사용자**를 클릭합니다.
    
    ![Azure AD 테스트 사용자 만들기](./media/five9-tutorial/create_aaduser_02.png) 

1. **사용자** 대화 상자를 열려면 대화 상자 위쪽에서 **추가**를 클릭합니다.
 
    ![Azure AD 테스트 사용자 만들기](./media/five9-tutorial/create_aaduser_03.png) 

1. **사용자** 대화 상자 페이지에서 다음 단계를 수행합니다.
 
    ![Azure AD 테스트 사용자 만들기](./media/five9-tutorial/create_aaduser_04.png) 

    a. **이름** 텍스트 상자에 **BrittaSimon**을 입력합니다.

    b. **사용자 이름** 텍스트 상자에 BrittaSimon의 **전자 메일 주소**를 입력합니다.

    다. **암호 표시**를 선택하고 **암호** 값을 적어둡니다.

    d. **만들기**를 클릭합니다.
 
### <a name="creating-a-five9-plus-adapter-cti-contact-center-agents-test-user"></a>Five9 Plus Adapter(CTI, Contact Center Agents) 테스트 사용자 만들기

이 섹션에서는 Five9 Plus Adapter(CTI, Contact Center Agents)에서 Britta Simon이라는 사용자를 만듭니다. Five9 Plus Adapter(CTI, Contact Center Agents) 플랫폼에서 사용자를 추가하려면  [Five9 Plus Adapter(CTI, Contact Center Agents) 지원 팀](https://www.five9.com/about/contact) 에 문의하세요. Single Sign-On을 사용하려면 먼저 사용자를 만들고 활성화해야 합니다.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Britta Simon이 Azure Single Sign-On을 사용할 수 있도록 Five9 Plus Adapter(CTI, Contact Center Agents)에 대한 액세스 권한을 부여합니다.

![사용자 할당][200] 

**Britta Simon을 Five9 Plus Adapter(CTI, Contact Center Agents)에 할당하려면 다음 단계를 수행합니다.**

1. Azure Portal에서 애플리케이션 보기를 연 다음 디렉터리 보기로 이동하고 **엔터프라이즈 애플리케이션**으로 이동한 후 **모든 애플리케이션**을 클릭합니다.

    ![사용자 할당][201] 

1. 애플리케이션 목록에서 **Five9 Plus Adapter(CTI, Contact Center Agents)** 를 선택합니다.

    ![Configure Single Sign-On](./media/five9-tutorial/tutorial_five9_app.png) 

1. 왼쪽 메뉴에서 **사용자 및 그룹**을 클릭합니다.

    ![사용자 할당][202] 

1. **추가** 단추를 클릭합니다. 그런 후 **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![사용자 할당][203]

1. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택합니다.

1. **사용자 및 그룹** 대화 상자에서 **선택** 단추를 클릭합니다.

1. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.
    
### <a name="testing-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 Five9 Plus Adapter(CTI, Contact Center Agents) 타일을 클릭하면 Five9 Plus Adapter(CTI, Contact Center Agents) 애플리케이션에 자동으로 로그온됩니다.
액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](../user-help/active-directory-saas-access-panel-introduction.md)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](tutorial-list.md)
* [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/five9-tutorial/tutorial_general_01.png
[2]: ./media/five9-tutorial/tutorial_general_02.png
[3]: ./media/five9-tutorial/tutorial_general_03.png
[4]: ./media/five9-tutorial/tutorial_general_04.png

[100]: ./media/five9-tutorial/tutorial_general_100.png

[200]: ./media/five9-tutorial/tutorial_general_200.png
[201]: ./media/five9-tutorial/tutorial_general_201.png
[202]: ./media/five9-tutorial/tutorial_general_202.png
[203]: ./media/five9-tutorial/tutorial_general_203.png

