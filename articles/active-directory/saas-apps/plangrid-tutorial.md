---
title: '자습서: PlanGrid와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory 및 PlanGrid 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0ba72432-9b49-4358-b756-14c982422be8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2018
ms.author: jeedes
ms.openlocfilehash: 5ceb5f09b599b6c42dea3359b18c319e7b44fed7
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55165084"
---
# <a name="tutorial-azure-active-directory-integration-with-plangrid"></a>자습서: PlanGrid와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 PlanGrid를 통합하는 방법을 알아봅니다.

PlanGrid를 Azure AD와 통합하면 다음과 같은 이점이 제공됩니다.

- PlanGrid에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
- 사용자가 해당 Azure AD 계정으로 PlanGrid에 자동으로 로그온(Single Sign-on)되도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory의 애플리케이션 액세스 및 Single Sign-On이란 무엇인가요?](../manage-apps/what-is-single-sign-on.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

PlanGrid와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- PlanGrid Single Sign-on이 설정된 구독

> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.

이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [1개월 평가판을 얻을](https://azure.microsoft.com/pricing/free-trial/) 수 있습니다.

## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다.  이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 PlanGrid 추가
2. Azure AD Single Sign-on 구성 및 테스트

## <a name="adding-plangrid-from-the-gallery"></a>갤러리에서 PlanGrid 추가
PlanGrid의 Azure AD 통합을 구성하려면 갤러리의 PlanGrid를 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 PlanGrid를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다. 

    ![Azure Active Directory 단추][1]

2. **엔터프라이즈 애플리케이션**으로 이동합니다. 그런 후 **모든 애플리케이션**으로 이동합니다.

    ![엔터프라이즈 애플리케이션 블레이드][2]
    
3. 새 애플리케이션을 추가하려면 대화 상자 맨 위 있는 **새 애플리케이션** 단추를 클릭합니다.

    ![새 애플리케이션 단추][3]

4. 검색 상자에서 **PlanGrid**를 입력하고, 결과 패널에서 **PlanGrid**를 선택한 다음, **추가** 단추를 클릭하여 응용 프로그램을 추가합니다.

    ![결과 목록의 PlanGrid](./media/plangrid-tutorial/tutorial_plangrid_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

이 섹션에서는 "Britta Simon"이라는 테스트 사용자를 기반으로 PlanGrid에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD에서 Azure AD 사용자에 해당하는 PlanGrid 사용자가 누구인지 알고 있어야 합니다. 즉, Azure AD 사용자와 PlanGrid의 관련 사용자 간에 연결이 형성되어야 합니다.

PlanGrid에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Azure AD Single Sign-On 구성](#configure-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
3. **[PlanGrid 테스트 사용자 만들기](#create-a-plangrid-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 PlanGrid에 만듭니다.
4. **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[Single Sign-On 테스트](#test-single-sign-on)** - 구성이 작동하는지 여부를 확인합니다.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정하고 PlanGrid 응용 프로그램에서 Single Sign-On을 구성합니다.

**PlanGrid에서 Azure AD Single Sign-on을 구성하려면 다음 단계를 수행합니다.**

1. Azure Portal의 **PlanGrid** 응용 프로그램 통합 페이지에서 **Single Sign-On**을 클릭합니다.

    ![Single Sign-On 구성 링크][4]

2. **Single Sign-On** 대화 상자에서 **모드**를 **SAML 기반 로그온**으로 선택하여 Single Sign-On을 사용하도록 설정합니다.
 
    ![Single Sign-On 대화 상자](./media/plangrid-tutorial/tutorial_plangrid_samlbase.png)

3. **PlanGrid 도메인 및 URL** 섹션에서 **IDP 시작 모드**로 응용 프로그램을 구성하려는 경우 다음 단계를 수행합니다.

    ![PlanGrid 도메인 및 URL Single Sign-On 정보](./media/plangrid-tutorial/tutorial_plangrid_url1.png)

    **식별자(엔터티 ID)** 텍스트 상자에 URL을 입력합니다. `https://io.plangrid.com/sessions/saml/metadata`

4. **SP** 시작 모드에서 애플리케이션을 구성하려면 **고급 URL 설정 표시**를 확인하고 다음 단계를 수행합니다.

    ![PlanGrid 도메인 및 URL Single Sign-On 정보](./media/plangrid-tutorial/tutorial_plangrid_url2.png)

    **로그온 URL** 텍스트 상자에서 URL `https://app.plangrid.com/login`을 입력합니다.

5. **SAML 서명 인증서** 섹션에서 **메타데이터 XML**을 클릭한 후 컴퓨터에 메타데이터 파일을 저장합니다.

    ![인증서 다운로드 링크](./media/plangrid-tutorial/tutorial_plangrid_certificate.png) 

6. **저장** 단추를 클릭합니다.

    ![Single Sign-On 구성 저장 단추](./media/plangrid-tutorial/tutorial_general_400.png)
    
7. **PlanGrid** 쪽에서 Single Sign-On을 구성하려면 다운로드한 **메타데이터 XML**을 [PlanGrid 지원 팀](mailto:help@plangrid.com)에 보내야 합니다. 이렇게 설정하면 SAML SSO 연결이 양쪽에서 제대로 설정됩니다.

### <a name="create-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기

이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

   ![Azure AD 테스트 사용자 만들기][100]

**Azure AD에서 테스트 사용자를 만들려면 다음 단계를 수행하세요.**

1. Azure Portal의 왼쪽 창에서 **Azure Active Directory** 단추를 클릭합니다.

    ![Azure Active Directory 단추](./media/plangrid-tutorial/create_aaduser_01.png)

2. 사용자 목록을 표시하려면 **사용자 및 그룹**으로 이동한 후 **모든 사용자**를 클릭합니다.

    !["사용자 및 그룹" 및 "모든 사용자" 링크](./media/plangrid-tutorial/create_aaduser_02.png)

3. **사용자** 대화 상자를 열려면 **모든 사용자** 대화 상자 위쪽에서 **추가**를 클릭합니다.

    ![추가 단추](./media/plangrid-tutorial/create_aaduser_03.png)

4. **사용자** 대화 상자에서 다음 단계를 수행합니다.

    ![사용자 대화 상자](./media/plangrid-tutorial/create_aaduser_04.png)

    a. **이름** 상자에 **BrittaSimon**을 입력합니다.

    나. **사용자 이름** 상자에 사용자인 Britta Simon의 전자 메일 주소를 입력합니다.

    다. **암호 표시** 확인란을 선택한 다음 **암호** 상자에 표시된 값을 적어둡니다.

    d. **만들기**를 클릭합니다.
 
### <a name="create-a-plangrid-test-user"></a>PlanGrid 테스트 사용자 만들기

이 섹션에서는 PlanGrid에서 Britta Simon이라는 사용자를 만듭니다. PlanGrid 플랫폼에서 사용자를 추가하려면  [PlanGrid 지원 팀](mailto:help@plangrid.com) 에 문의하세요. Single Sign-On을 사용하려면 먼저 사용자를 만들고 활성화해야 합니다. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 PlanGrid에 대한 액세스 권한을 부여합니다.

![사용자 역할 할당][200] 

**Britta Simon을 PlanGrid에 할당하려면 다음 단계를 수행합니다.**

1. Azure Portal에서 애플리케이션 보기를 연 다음 디렉터리 보기로 이동하고 **엔터프라이즈 애플리케이션**으로 이동한 후 **모든 애플리케이션**을 클릭합니다.

    ![사용자 할당][201] 

2. 응용 프로그램 목록에서 **PlanGrid**를 선택합니다.

    ![응용 프로그램 목록의 PlanGrid 링크](./media/plangrid-tutorial/tutorial_plangrid_app.png)  

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 클릭합니다.

    !["사용자 및 그룹" 링크][202]

4. **추가** 단추를 클릭합니다. 그런 후 **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![할당 추가 창][203]

5. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택합니다.

6. **사용자 및 그룹** 대화 상자에서 **선택** 단추를 클릭합니다.

7. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.
    
### <a name="test-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 PlanGrid 타일을 클릭하면 PlanGrid 애플리케이션에 자동으로 로그온됩니다.
액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](../user-help/active-directory-saas-access-panel-introduction.md)를 참조하세요. 

## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](tutorial-list.md)
* [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/plangrid-tutorial/tutorial_general_01.png
[2]: ./media/plangrid-tutorial/tutorial_general_02.png
[3]: ./media/plangrid-tutorial/tutorial_general_03.png
[4]: ./media/plangrid-tutorial/tutorial_general_04.png

[100]: ./media/plangrid-tutorial/tutorial_general_100.png

[200]: ./media/plangrid-tutorial/tutorial_general_200.png
[201]: ./media/plangrid-tutorial/tutorial_general_201.png
[202]: ./media/plangrid-tutorial/tutorial_general_202.png
[203]: ./media/plangrid-tutorial/tutorial_general_203.png

