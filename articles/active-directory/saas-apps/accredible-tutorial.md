---
title: '자습서: Accredible과 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory와 Accredible 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7284dfb6-df62-41f1-a4a4-1b8322b7ef44
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: jeedes
ms.openlocfilehash: c5eaa0944210e701c306180709bf97abdbb3b9e3
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55151280"
---
# <a name="tutorial-azure-active-directory-integration-with-accredible"></a>자습서: Accredible과 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 Accredible을 통합하는 방법에 대해 알아봅니다.

Accredible을 Azure AD와 통합하면 다음과 같은 이점이 제공됩니다.

- Azure AD에서 Accredible에 대한 액세스 권한이 있는 사용자를 제어할 수 있습니다.
- 사용자가 해당 Azure AD 계정으로 Accredible에 자동으로 로그온(Single Sign-on)되도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory의 애플리케이션 액세스 및 Single Sign-On이란 무엇인가요?](../manage-apps/what-is-single-sign-on.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

Accredible과 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- Accredible Single Sign-On이 설정된 구독

> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.

이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [1개월 평가판을 얻을](https://azure.microsoft.com/pricing/free-trial/) 수 있습니다.

## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다.  이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 Accredible 추가
2. Azure AD Single Sign-on 구성 및 테스트

## <a name="adding-accredible-from-the-gallery"></a>갤러리에서 Accredible 추가
Accredible과 Azure AD 통합을 구성하려면 갤러리의 Accredible을 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 Accredible을 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다. 

    ![Azure Active Directory 단추][1]

2. **엔터프라이즈 애플리케이션**으로 이동합니다. 그런 후 **모든 애플리케이션**으로 이동합니다.

    ![엔터프라이즈 애플리케이션 블레이드][2]
    
3. 새 애플리케이션을 추가하려면 대화 상자 맨 위 있는 **새 애플리케이션** 단추를 클릭합니다.

    ![새 애플리케이션 단추][3]

4. 검색 상자에 **Accredible**을 입력하고 결과 패널에서 **Accredible**을 선택한 후 **추가** 단추를 클릭하여 애플리케이션을 추가합니다.

    ![결과 목록의 Accredible](./media/accredible-tutorial/tutorial_accredible_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

이 섹션에서는 “Britta Simon”이라는 테스트 사용자를 기반으로 Accredible에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD에서 Azure AD 사용자에 해당하는 Accredible 사용자가 누구인지 알고 있어야 합니다. 즉, Azure AD 사용자와 Accredible의 관련 사용자 간에 연결이 형성되어야 합니다.

Accredible에서 Azure AD의 **사용자 이름** 값을 **Username** 값으로 할당하여 링크 관계를 설정합니다.

Accredible에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Azure AD Single Sign-On 구성](#configure-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
3. **[ 테스트 사용자 만들기](#create-an-accredible-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 Accredible에 만듭니다.
4. **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[Single Sign-On 테스트](#test-single-sign-on)** - 구성이 작동하는지 여부를 확인합니다.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정하고 Accredible 애플리케이션에서 Single Sign-On을 구성합니다.

**Accredible에서 Azure AD Single Sign-on을 구성하려면 다음 단계를 수행합니다.**

1. Azure Portal의 **Accredible** 애플리케이션 통합 페이지에서 **Single Sign-On**을 클릭합니다.

    ![Single Sign-On 구성 링크][4]

2. **Single Sign-On** 대화 상자에서 **모드**를 **SAML 기반 로그온**으로 선택하여 Single Sign-On을 사용하도록 설정합니다.
 
    ![Single Sign-On 대화 상자](./media/accredible-tutorial/tutorial_accredible_samlbase.png)

3. **Accredible 도메인 및 URL** 섹션에서 다음 단계를 수행합니다.

    ![Accredible 도메인 및 URL Single Sign-On 정보](./media/accredible-tutorial/tutorial_accredible_url.png)

    a. **식별자** 텍스트 상자에 다음 URL을 입력합니다.
    | |
    |--|
    |  `https://api.accredible.com/sp/admin/accredible` |
    | `https://api.accredible.com/sp/user/accredible` |

    b. **회신 URL** 텍스트 상자에 다음 패턴으로 URL을 입력합니다.`https://api.accredible.com/v1/saml/admin/<Unique id>/consume`

    > [!NOTE] 
    > [회신 URL] 값은 실제 값이 아닙니다. 사용자 역할에 따라 해당하는 ID 값을 사용합니다. 각 고객에게는 ID에 따라 고유한 회신 URL이 있습니다. 이러한 값을 얻으려면 [Accredible 지원 팀](mailto:support@accredible.com) 에 문의하세요.
 
4. **SAML 서명 인증서** 섹션에서 **메타데이터 XML**을 클릭한 후 컴퓨터에 메타데이터 파일을 저장합니다.

    ![인증서 다운로드 링크](./media/accredible-tutorial/tutorial_accredible_certificate.png) 

5. **저장** 단추를 클릭합니다.

    ![Single Sign-On 구성 저장 단추](./media/accredible-tutorial/tutorial_general_400.png)

6. **Accredible** 쪽에서 Single Sign-On을 구성하려면 다운로드한 **메타데이터 XML**을 [Accredible 지원 팀](mailto:support@accredible.com)에 보내야 합니다. 이렇게 설정하면 SAML SSO 연결이 양쪽에서 제대로 설정됩니다.

> [!TIP]
> 이제 앱을 설정하는 동안 [Azure Portal ](https://portal.azure.com) 내에서 이러한 지침의 간결한 버전을 읽을 수 있습니다.  **Active Directory &gt; 엔터프라이즈 애플리케이션** 섹션에서 이 앱을 추가한 후에는 **Single Sign-On** 탭을 클릭하고 맨 아래에 있는 **구성** 섹션을 통해 포함된 설명서에 액세스하면 됩니다. 포함된 설명서 기능에 대한 자세한 내용은 [Azure AD 포함된 설명서]( https://go.microsoft.com/fwlink/?linkid=845985)에서 확인할 수 있습니다.

### <a name="create-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기

이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

   ![Azure AD 테스트 사용자 만들기][100]

**Azure AD에서 테스트 사용자를 만들려면 다음 단계를 수행하세요.**

1. Azure Portal의 왼쪽 창에서 **Azure Active Directory** 단추를 클릭합니다.

    ![Azure Active Directory 단추](./media/accredible-tutorial/create_aaduser_01.png)

2. 사용자 목록을 표시하려면 **사용자 및 그룹**으로 이동한 후 **모든 사용자**를 클릭합니다.

    !["사용자 및 그룹" 및 "모든 사용자" 링크](./media/accredible-tutorial/create_aaduser_02.png)

3. **사용자** 대화 상자를 열려면 **모든 사용자** 대화 상자 위쪽에서 **추가**를 클릭합니다.

    ![추가 단추](./media/accredible-tutorial/create_aaduser_03.png)

4. **사용자** 대화 상자에서 다음 단계를 수행합니다.

    ![사용자 대화 상자](./media/accredible-tutorial/create_aaduser_04.png)

    a. **이름** 상자에 **BrittaSimon**을 입력합니다.

    나. **사용자 이름** 상자에 사용자인 Britta Simon의 전자 메일 주소를 입력합니다.

    다. **암호 표시** 확인란을 선택한 다음 **암호** 상자에 표시된 값을 적어둡니다.

    d. **만들기**를 클릭합니다.
  
### <a name="create-an-accredible-test-user"></a>Accredible 테스트 사용자 만들기

이 섹션에서는 Accredible에서 Britta Simon이라는 사용자를 만듭니다. 사용자의 메일 ID를  [Accredible 지원 팀](mailto:support@accredible.com)에 보내야 하며, 그러면 지원 팀은 이메일을 확인하고 Accredible 플랫폼에서 사용자를 추가할 수 있도록 초대 메일을 보내줍니다.
 
### <a name="assign-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 Accredible에 대한 액세스 권한을 부여합니다.

![사용자 역할 할당][200] 

**Britta Simon을 Accredible에 할당하려면 다음 단계를 수행합니다.**

1. Azure Portal에서 애플리케이션 보기를 연 다음 디렉터리 보기로 이동하고 **엔터프라이즈 애플리케이션**으로 이동한 후 **모든 애플리케이션**을 클릭합니다.

    ![사용자 할당][201] 

2. 애플리케이션 목록에서 **Accredible**을 선택합니다.

    ![애플리케이션 목록의 Accredible 링크](./media/accredible-tutorial/tutorial_accredible_app.png)  

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 클릭합니다.

    !["사용자 및 그룹" 링크][202]

4. **추가** 단추를 클릭합니다. 그런 후 **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![할당 추가 창][203]

5. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택합니다.

6. **사용자 및 그룹** 대화 상자에서 **선택** 단추를 클릭합니다.

7. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.
    
### <a name="test-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 Accredible 타일을 클릭하면 Accredible 애플리케이션에 자동으로 로그온됩니다.
액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](../user-help/active-directory-saas-access-panel-introduction.md)를 참조하세요. 

## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](tutorial-list.md)
* [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/accredible-tutorial/tutorial_general_01.png
[2]: ./media/accredible-tutorial/tutorial_general_02.png
[3]: ./media/accredible-tutorial/tutorial_general_03.png
[4]: ./media/accredible-tutorial/tutorial_general_04.png

[100]: ./media/accredible-tutorial/tutorial_general_100.png

[200]: ./media/accredible-tutorial/tutorial_general_200.png
[201]: ./media/accredible-tutorial/tutorial_general_201.png
[202]: ./media/accredible-tutorial/tutorial_general_202.png
[203]: ./media/accredible-tutorial/tutorial_general_203.png

