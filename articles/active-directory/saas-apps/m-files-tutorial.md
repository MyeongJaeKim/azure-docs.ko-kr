---
title: '자습서: M-Files와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory와 M-Files 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 4536fd49-3a65-4cff-9620-860904f726d0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 41c31a987e766551682b2c90c00473ea44215dd5
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55150176"
---
# <a name="tutorial-azure-active-directory-integration-with-m-files"></a>자습서: M-Files와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 M-Files를 통합하는 방법에 대해 알아봅니다.

M-Files를 Azure AD와 통합하면 다음과 같은 이점이 제공됩니다.

- M-Files에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
- 사용자가 해당 Azure AD 계정으로 M-Files에 자동으로 로그온(Single Sign-On)되도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory의 애플리케이션 액세스 및 Single Sign-On이란 무엇인가요?](../manage-apps/what-is-single-sign-on.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

M-Files와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- M-Files Single Sign-On이 설정된 구독

> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.

이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [여기](https://azure.microsoft.com/pricing/free-trial/)에서 1개월 평가판을 얻을 수 있습니다.

## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다.  이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 M-Files 추가
1. Azure AD Single Sign-on 구성 및 테스트

## <a name="adding-m-files-from-the-gallery"></a>갤러리에서 M-Files 추가
M-Files의 Azure AD 통합을 구성하려면 갤러리의 M-Files를 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 M-Files를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다. 

    ![Active Directory][1]

1. **엔터프라이즈 응용 프로그램**으로 이동합니다. 그런 후 **모든 애플리케이션**으로 이동합니다.

    ![애플리케이션][2]
    
1. 새 애플리케이션을 추가하려면 대화 상자 맨 위 있는 **새 애플리케이션** 단추를 클릭합니다.

    ![애플리케이션][3]

1. 검색 상자에 **M-Files**를 입력합니다.

    ![Azure AD 테스트 사용자 만들기](./media/m-files-tutorial/tutorial_m-files_search.png)

1. 결과 패널에서 **M-Files**를 선택하고 **추가** 단추를 클릭하여 애플리케이션을 추가합니다.

    ![Azure AD 테스트 사용자 만들기](./media/m-files-tutorial/tutorial_m-files_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD Single Sign-on 구성 및 테스트
이 섹션에서는 "Britta Simon"이라는 테스트 사용자를 기반으로 M-Files에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD에서 Azure AD 사용자에 해당하는 M-Files 사용자가 누구인지 알고 있어야 합니다. 즉, Azure AD 사용자와 M-Files의 관련 사용자 간에 연결이 형성되어야 합니다.

M-Files에서 Azure AD의 **사용자 이름** 값을 **Username** 값으로 할당하여 링크 관계를 설정합니다.

M-Files에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
1. **[Azure AD 테스트 사용자 만들기](#creating-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On 테스트하는 데 사용합니다.
1. **[M-Files 테스트 사용자 만들기](#creating-a-m-files-test-user)** - Britta Simon의 Azure AD 표현과 연결되는 대응 사용자를 M-Files에 만듭니다.
1. **[Azure AD 테스트 사용자 할당](#assigning-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
1. **[Single Sign-On 테스트](#testing-single-sign-on)** - 구성이 작동하는지 확인합니다.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정하고 M-Files 애플리케이션에서 Single Sign-On을 구성합니다.

**M-Files에서 Azure AD Single Sign-on을 구성하려면 다음 단계를 수행합니다.**

1. Azure Portal의 **M-Files** 애플리케이션 통합 페이지에서 **Single Sign-On**을 클릭합니다.

    ![Configure Single Sign-On][4]

1. **Single Sign-On** 대화 상자에서 **모드**를 **SAML 기반 로그온**으로 선택하여 Single Sign-On을 사용하도록 설정합니다.
 
    ![Configure Single Sign-On](./media/m-files-tutorial/tutorial_m-files_samlbase.png)

1. **M-Files 도메인 및 URL** 섹션에서 다음 단계를 수행합니다.

    ![Configure Single Sign-On](./media/m-files-tutorial/tutorial_m-files_url.png)

    a. **로그온 URL** 텍스트 상자에서 다음 패턴으로 URL을 입력합니다. `https://<tenantname>.cloudvault.m-files.com/authentication/MFiles.AuthenticationProviders.Core/sso`

    b. **식별자** 텍스트 상자에서 `https://<tenantname>.cloudvault.m-files.com` 패턴을 사용하여 URL을 입력합니다.

    > [!NOTE] 
    > 이러한 값은 실제 값이 아닙니다. 실제 로그온 URL 및 식별자로 값을 업데이트합니다. 이러한 값을 얻으려면 [M-Files 클라이언트 지원 팀](mailto:support@m-files.com)에 문의하세요. 
 
1. **SAML 서명 인증서** 섹션에서 **메타데이터 XML**을 클릭한 후 컴퓨터에 메타데이터 파일을 저장합니다.

    ![Configure Single Sign-On](./media/m-files-tutorial/tutorial_m-files_certificate.png) 

1. **저장** 단추를 클릭합니다.

    ![Configure Single Sign-On](./media/m-files-tutorial/tutorial_general_400.png)

1. 애플리케이션에 대해 SSO를 구성하려면 [M-Files 지원 팀](mailto:support@m-files.com)에 문의하고 다운로드한 메타데이터를 제공합니다.
   
    >[!NOTE]
    >M-Files 데스크톱 애플리케이션에 대한 SSO를 구성하려면 다음 단계를 따릅니다. M-Files 웹 버전에 대해서만 SSO를 구성하려는 경우 추가 단계가 필요하지 않습니다.  

1. 다음 단계에 따라 Azure AD에서 SSO를 사용하도록 M-Files 데스크톱 애플리케이션을 구성합니다. M-Files를 다운로드하려면 [M-Files 다운로드](https://www.m-files.com/en/download-latest-version) 페이지로 이동합니다.

1. **M-Files 데스크톱 설정** 창을 엽니다. 그런 다음 **추가**를 클릭합니다.
   
    ![Configure Single Sign-On](./media/m-files-tutorial/tutorial_m_files_10.png)

1. **문서 자격 증명 모음 연결 속성** 창에서 다음 단계를 수행합니다.
   
    ![Configure Single Sign-On](./media/m-files-tutorial/tutorial_m_files_11.png)  

    서버 섹션 아래에서 다음과 같이 값을 입력합니다.  

    a. **이름**에 `<tenant-name>.cloudvault.m-files.com`을 입력합니다. 
 
    b. **포트 번호**에 **4466**을 입력합니다. 

    다. **프로토콜**에서 **HTTPS**를 선택합니다. 

    d. **인증** 필드에서 **특정 Windows 사용자**를 선택합니다. 그러면 서명 페이지가 표시됩니다. Azure AD 자격 증명을 삽입합니다. 

    e. **서버의 자격 증명 모음**에서 서버에 있는 해당 자격 증명 모음을 선택합니다.
 
    f. **확인**을 클릭합니다.

> [!TIP]
> 이제 앱을 설정하는 동안 [Azure Portal ](https://portal.azure.com) 내에서 이러한 지침의 간결한 버전을 읽을 수 있습니다.  **Active Directory &gt; 엔터프라이즈 애플리케이션** 섹션에서 이 앱을 추가한 후에는 **Single Sign-On** 탭을 클릭하고 맨 아래에 있는 **구성** 섹션을 통해 포함된 설명서에 액세스하면 됩니다. 포함된 설명서 기능에 대한 자세한 내용은 [Azure AD 포함된 설명서]( https://go.microsoft.com/fwlink/?linkid=845985)에서 확인할 수 있습니다.
> 

### <a name="creating-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기
이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

![Azure AD 사용자 만들기][100]

**Azure AD에서 테스트 사용자를 만들려면 다음 단계를 수행하세요.**

1. **Azure Portal**의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure AD 테스트 사용자 만들기](./media/m-files-tutorial/create_aaduser_01.png) 

1. 사용자 목록을 표시하려면 **사용자 및 그룹**으로 이동한 후 **모든 사용자**를 클릭합니다.
    
    ![Azure AD 테스트 사용자 만들기](./media/m-files-tutorial/create_aaduser_02.png) 

1. **사용자** 대화 상자를 열려면 대화 상자 위쪽에서 **추가**를 클릭합니다.
 
    ![Azure AD 테스트 사용자 만들기](./media/m-files-tutorial/create_aaduser_03.png) 

1. **사용자** 대화 상자 페이지에서 다음 단계를 수행합니다.
 
    ![Azure AD 테스트 사용자 만들기](./media/m-files-tutorial/create_aaduser_04.png) 

    a. **이름** 텍스트 상자에 **BrittaSimon**을 입력합니다.

    b. **사용자 이름** 텍스트 상자에 BrittaSimon의 **전자 메일 주소**를 입력합니다.

    다. **암호 표시**를 선택하고 **암호** 값을 적어둡니다.

    d. **만들기**를 클릭합니다.
 
### <a name="creating-a-m-files-test-user"></a>M-Files 테스트 사용자 만들기

이 섹션의 목표는 M-Files에서 Britta Simon이라는 사용자를 생성하는 것입니다. M-Files에 사용자를 추가하려면 [M-Files 지원 팀](mailto:support@m-files.com)에 문의하세요.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 M-Files에 대한 액세스 권한을 부여합니다.

![사용자 할당][200] 

**Britta Simon을 M-Files에 할당하려면 다음 단계를 수행합니다.**

1. Azure Portal에서 애플리케이션 보기를 연 다음 디렉터리 보기로 이동하고 **엔터프라이즈 애플리케이션**으로 이동한 후 **모든 애플리케이션**을 클릭합니다.

    ![사용자 할당][201] 

1. 애플리케이션 목록에서 **M-Files**를 선택합니다.

    ![Configure Single Sign-On](./media/m-files-tutorial/tutorial_m-files_app.png) 

1. 왼쪽 메뉴에서 **사용자 및 그룹**을 클릭합니다.

    ![사용자 할당][202] 

1. **추가** 단추를 클릭합니다. 그런 후 **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![사용자 할당][203]

1. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택합니다.

1. **사용자 및 그룹** 대화 상자에서 **선택** 단추를 클릭합니다.

1. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.
    
### <a name="testing-single-sign-on"></a>Single Sign-On 테스트

이 섹션은 액세스 패널을 사용하여 Azure AD SSO 구성을 테스트하기 위한 것입니다.

액세스 패널에서 M-Files 타일을 클릭하면 M-Files 애플리케이션에 자동으로 로그온됩니다.

## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](tutorial-list.md)
* [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/m-files-tutorial/tutorial_general_01.png
[2]: ./media/m-files-tutorial/tutorial_general_02.png
[3]: ./media/m-files-tutorial/tutorial_general_03.png
[4]: ./media/m-files-tutorial/tutorial_general_04.png

[100]: ./media/m-files-tutorial/tutorial_general_100.png

[200]: ./media/m-files-tutorial/tutorial_general_200.png
[201]: ./media/m-files-tutorial/tutorial_general_201.png
[202]: ./media/m-files-tutorial/tutorial_general_202.png
[203]: ./media/m-files-tutorial/tutorial_general_203.png

