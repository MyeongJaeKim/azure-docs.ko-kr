---
title: 예측 점수
titleSuffix: Language Understanding - Azure Cognitive Services
description: 예측 점수는 예측 결과에 대한 LUIS의 신뢰도를 나타냅니다.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 9e301edfc183c627307e6e00ced2077ceaa15f3c
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55222000"
---
# <a name="prediction-scores-indicate-prediction-accuracy-for-intent-and-entities"></a>예측 점수는 의도 및 엔터티에 대한 예측 정확도를 나타냅니다.
예측 점수는 예측 결과에 대한 LUIS의 신뢰도를 나타냅니다. 

예측 점수는 0(영)과 1(일) 사이입니다. 신뢰도가 높은 LUIS 점수의 예는 0.99입니다. 신뢰도 점수의 예는 0.01입니다. 

|점수 값|신뢰도|
|--|--|
|1|확실한 일치|
|0.99|높은 신뢰도|
|0.01|낮은 신뢰도|
|0|확실한 불일치|

발언의 신뢰도 점수가 낮으면 LUIS는 [LUIS](luis-reference-regions.md) 웹 사이트 **의도** 페이지에서 빨간색 윤곽선이 있는 식별된 **레이블이 지정된 의도**를 사용하여 해당 발언을 강조 표시합니다. 

![점수 불일치](./media/luis-concept-score/score-discrepancy.png)

## <a name="top-scoring-intent"></a>상위 점수 의도
모든 발화 예측은 상위 점수 의도를 반환합니다. 이는 예측 점수의 수치 비교입니다. 상위 두 개의 점수는 서로 차이가 매우 작을 수 있습니다. LUIS는 점수 반환 외에는 이 근접성을 표시하지 않습니다.  

상위 점수의 근접성이 우려된다면 모든 의도의 점수를 반환해야 합니다. 단어 선택 및 정렬을 통해 차이를 나타내는 두 개의 의도에 발화를 추가하거나, 챗봇과 같은 LUIS 호출 애플리케이션이 두 개의 상위 의도를 처리하는 방법을 프로그래밍 방식으로 선택하도록 할 수 있습니다. 

점수가 너무 가깝게 매겨진 두 가지 의도는 비결정적 학습으로 인해 반전될 수 있습니다. 최고 점수가 두 번째로 높은 점수가 될 수 있고 두 번째로 높은 점수가 최고 점수가 될 수 있습니다. 이를 방지하려면 두 가지 의도를 구분하는 상황 및 단어를 선택하여 해당 발언의 최고 두 가지 의도 각각에 대해 예제 발언을 추가합니다. 두 가지 의도에는 동일한 수의 예제 발언이 있어야 합니다. 학습으로 인한 반전을 방지할 수 있는 일반적인 분리 기준은 15%의 점수 차이입니다.

## <a name="return-prediction-score-for-all-intents"></a>모든 의도의 예측 점수 반환
테스트 또는 엔드포인트 결과에는 모든 의도가 포함될 수 있습니다. 이 구성은 [엔드포인트](https://aka.ms/v1-endpoint-api-docs)에서 `verbose=true` 쿼리 문자열 이름/값 쌍을 사용하여 설정됩니다. 

## <a name="review-intents-with-similar-scores"></a>유사한 점수를 가진 의도 검토
올바른 의도가 식별되는지, 다음 식별된 의도의 점수가 여러 발화에 일관되게 상당히 더 낮은지 확인하려면 모든 의도의 점수를 검토하는 것이 좋습니다. 

여러 의도의 예측 점수가 가까운 경우에는 발화 컨텍스트에 따라 LUIS가 의도 간에 전환할 수 있습니다. 이를 수정하려면 다양한 컨텍스트 차이를 사용하여 각 의도에 발화를 계속 추가합니다.   

## <a name="e-exponent-notation"></a>E(지수) 표기법

예측 점수는 `9.910309E-07`과 같이 0~1 범위 위에 ‘나타나는’ 지수 표기법을 사용합니다. 이 점수는 매우 **작은** 수를 나타냅니다.

|E 표기법 점수 |실제 점수|
|--|--|
|9.910309E-07|.0000009910309|

## <a name="differences-with-predictions"></a>예측에 대한 차이
다른 앱에서 동일한 모델을 학습시키는데 점수가 이처럼 동일하지 않으면 이는 학습에 무작위 요소가 있기 때문입니다. 두 번째로, 둘 이상 의도의 발화가 겹치는 것은 동일 발화의 상위 의도가 학습에 따라 변경될 수 있음을 의미합니다.

챗봇에 의도의 신뢰도를 나타내는 특정 LUIS 점수가 필요한 경우에는 상위 두 의도 간 점수 차이를 사용해야 합니다. 이렇게 하면 학습에서 변형 유연성이 제공됩니다. 

## <a name="punctuation"></a>문장 부호
문장 부호는 LUIS에서 별도 토큰입니다. 끝에 마침표가 포함된 발언과 포함되지 않은 발언은 별도의 두 발언이며 두 가지 다른 예측이 이루어질 수 있습니다. 모델이 [예제 발언](luis-concept-utterance.md)(문장 부호를 포함하거나 포함하지 않음) 또는 [패턴](luis-concept-patterns.md)(`I am applying for the {Job} position[.]` 특수 구문을 사용하여 문장 부호를 무시하는 것이 더 쉬움)에서 문장 부호를 처리하는지 확인합니다.

## <a name="next-steps"></a>다음 단계

LUIS 앱에 엔터티를 추가하는 방법에 대한 자세한 내용은 [엔터티 추가](luis-how-to-add-entities.md)를 참조하세요.
