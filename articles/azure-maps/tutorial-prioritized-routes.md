---
title: Azure Maps를 사용하여 여러 경로 찾기 | Microsoft Docs
description: Azure Maps를 사용하여 여러 여행 모드에 대한 경로 찾기
author: walsehgal
ms.author: v-musehg
ms.date: 11/14/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 5458c7e74728952df89380a3649c6ed60eb6ea9a
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55749766"
---
# <a name="find-routes-for-different-modes-of-travel-using-azure-maps"></a>Azure Maps를 사용하여 여러 여행 모드에 대한 경로 찾기

이 자습서에서는 Azure Maps 계정과 경로 서비스를 사용하여 여행 모드의 우선 순위에 따라 관심 지점까지의 경로를 찾는 방법을 보여 줍니다. 지도에는 서로 다른 두 개의 경로가 표시됩니다. 하나는 승용차용이고, 다른 하나는 높이, 무게 또는 위험 화물로 인해 경로가 제한되는 화물차용입니다. 이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * 지도 컨트롤 API를 사용하여 새 웹 페이지 만들기
> * 지도에서 교통 흐름 시각화
> * 여행 모드를 선언하는 경로 쿼리 만들기
> * 지도에 여러 경로 표시

## <a name="prerequisites"></a>필수 조건

계속 진행하기 전에 첫 번째 자습서의 단계에 따라 [Azure Maps 계정 만들기](./tutorial-search-location.md#createaccount), [계정에 대한 구독 키 가져오기](./tutorial-search-location.md#getkey)를 수행합니다.

## <a name="create-a-new-map"></a>새 지도 만들기

다음 단계에서는 지도 컨트롤 API가 포함된 정적 HTML 페이지를 만드는 방법을 보여줍니다.

1. 로컬 컴퓨터에서 새 파일을 만들고 이름을 **MapTruckRoute.html**로 지정합니다.
2. 다음 HTML 구성 요소를 파일에 추가합니다.

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title>Multiple routes by mode of travel</title>
        
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>

        <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
        <script src="https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=1"></script>

        <script>
            var map, datasource, client;

            function GetMap() {
                //Add Map Control JavaScript code here.
            }
        </script>
        <style>
            html,
            body {
                width: 100%;
                height: 100%;
                padding: 0;
                margin: 0;
            }

            #myMap {
                position: relative;
                width: 100%;
                height: 100%;
            }
        </style>
    </head>
    <body onload="GetMap()">
        <div id="myMap"></div>
    </body>
    </html>
    ```
    
    HTML 헤더는 Azure 맵 컨트롤 라이브러리에서 호스팅하는 CSS 및 JavaScript 리소스 파일을 포함합니다. 페이지의 본문의 `onload` 이벤트는 페이지 본문이 로드될 때 `GetMap` 함수를 호출합니다. 이 함수에는 Azure Maps API에 액세스하는 인라인 JavaScript 코드가 포함됩니다.

3. 다음 JavaScript 코드를 `GetMap` 함수에 추가합니다. **\<Your Azure Maps Key\>** 문자열을 Maps 계정에서 복사한 기본 키로 바꿉니다.

    ```JavaScript
    //Add your Azure Maps subscription key to the map SDK. 
    atlas.setSubscriptionKey('<Your Azure Maps Key>');

    //Initialize a map instance.
    map = new atlas.Map('myMap');
    ```

    **atlas Map**은 시각적 및 대화형 웹 맵에 대한 컨트롤을 제공하고 Azure 맵 컨트롤 API의 구성 요소입니다.

4. 파일을 저장하고 브라우저에서 엽니다. 이 시점에서 더 자세히 개발할 수 있는 기본 지도가 있습니다.

   ![기본 지도 보기](./media/tutorial-prioritized-routes/basic-map.png)

## <a name="visualize-traffic-flow"></a>교통 흐름 시각화

1. 지도에 교통 흐름 표시를 추가합니다. `map.events.add`는 맵이 완전히 로드된 후 맵에 추가된 모든 맵 함수가 로드되도록 합니다.

    ```JavaScript
    map.events.add("load", function() {
        // Add Traffic Flow to the Map
        map.setTraffic({
            flow: "relative"
        });
    });
    ```
    
     로드 이벤트는 맵에 추가되고, 맵 리소스가 완전히 로드될 때 실행됩니다. 맵 로드 이벤트 처리기에서 맵의 트래픽 흐름 설정이 자유 흐름에 대한 상대 속도인 `relative`로 설정됩니다. 또한 도로의 `absolute` 속도 또는 자유 흐름과 다른 상대 속도를 표시하는 `relative-delay`로 설정할 수도 있습니다.

2. **MapTruckRoute.html** 파일을 저장하고, 브라우저에서 페이지를 새로 고칩니다. 현재 교통 데이터가 있는 로스앤젤레스의 거리가 표시됩니다.

   ![교통 지도 보기](./media/tutorial-prioritized-routes/traffic-map.png)

<a id="queryroutes"></a>

## <a name="define-how-the-route-will-be-rendered"></a>경로가 렌더링되는 방식 정의

이 자습서에서는 두 개의 경로를 계산하여 지도에 렌더링합니다. 한 경로는 승용차가 접근할 수 있는 도로를 사용하고 다른 경로는 트럭이 접근할 수 있는 도로를 사용합니다. 경로가 렌더링되면 경로의 시작과 끝부분에 대한 기호 아이콘을 표시하고 각 경로를 서로 다른 색상의 선으로 표시할 것입니다.

1. GetMap 함수에서 맵을 초기화한 후, 다음 JavaScript 코드를 추가합니다.

    ```JavaScript
    //Wait until the map resources have fully loaded.
    map.events.add('load', function () {

        //Create a data source and add it to the map.
        datasource = new atlas.source.DataSource();
        map.sources.add(datasource);

        //Add a layer for rendering the route lines and have it render under the map labels.
        map.layers.add(new atlas.layer.LineLayer(datasource, null, {
            strokeColor: ['get', 'strokeColor'],
            strokeWidth: ['get', 'strokeWidth'],
            lineJoin: 'round',
            lineCap: 'round',
            filter: ['==', '$type', 'LineString']
        }), 'labels');

        //Add a layer for rendering point data.
        map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
            iconOptions: {
                image: ['get', 'icon'],
                allowOverlap: true
            },
            textOptions: {
                textField: ['get', 'title'],
                offset: [0, 1.2]
            },
            filter: ['==', '$type', 'Point']
        }));
    });
    ```
    
    로드 이벤트는 맵에 추가되고, 맵 리소스가 완전히 로드될 때 실행됩니다. 맵 로드 이벤트 처리기에서, 경로 선과 시작 및 끝 지점을 저장하는 데이터 원본이 만들어집니다. 선 레이어를 만들어 데이터 원본에 연결하여 경로 선이 렌더링되는 방식을 정의합니다. 식은 경로 선 기능의 속성에서 선 너비 및 색상을 검색하는 데 사용됩니다. 이 레이어만 GeoJSON LineString 데이터를 렌더링하도록 보장하기 위한 필터가 추가됩니다. 맵에 레이어를 추가하면 값이 `'labels'`인 두 번째 매개 변수가 전달됩니다. 이 매개 변수는 맵 레이블 아래의 이 레이어를 렌더링하도록 지정합니다. 이렇게 하면 경로 선이 도로 레이블을 가리지 않습니다. 기호 레이어가 생성되어 데이터 원본에 연결됩니다. 이 레이어는 시작 및 끝 지점이 렌더링되는 방식을 지정합니다. 이 예에서는 지점 개체의 속성에서 아이콘 이미지 및 텍스트 레이블 정보를 검색하는 식이 추가되었습니다. 
    
2. 이 자습서에서는 출발 지점을 시애틀 소재의 Fabrikam이라는 가상 회사로, 도착 지점을 Microsoft 본사로 설정합니다. 맵 로드 이벤트 처리기에서 다음 코드를 추가합니다.

    ```JavaScript
    //Create the GeoJSON objects which represent the start and end point of the route.
    var startPoint = new atlas.data.Feature(new atlas.data.Point([-122.356099, 47.580045]), {
        title: 'Fabrikam, Inc.',
        icon: 'pin-round-blue'
    });

    var endPoint = new atlas.data.Feature(new atlas.data.Point([-122.201164, 47.616940]), {
        title: 'Microsoft - Lincoln Square',
        icon: 'pin-blue'
    });
    ```
    
    이 코드는 경로의 출발점과 도착점을 나타낼 두 개의 [GeoJSON 개체](https://en.wikipedia.org/wiki/GeoJSON)를 만듭니다. `title` 및 `icon` 속성이 각 지점에 추가됩니다.

3. 다음으로, JavaScript 코드를 추가하여 맵에 출발점과 종료점에 대한 핀을 추가합니다.

    ```JavaScript
    //Add the data to the data source.
    datasource.add([startPoint, endPoint]);

    //Fit the map window to the bounding box defined by the start and end positions.
    map.setCamera({
        bounds: atlas.data.BoundingBox.fromData([startPoint, endPoint]),
        padding: 100
    });
    ```
    
    시작 지점과 끝 지점이 데이터 원본에 추가됩니다. `atlas.data.BoundingBox.fromData` 함수를 사용하여 시작 및 끝 지점의 경계 상자가 계산됩니다. 이 경계 상자는 `map.setCamera` 함수를 사용하여 시작 및 끝 지점에 맵 카메라 보기를 설정하는 데 사용됩니다. 기호 아이콘의 픽셀 크기를 보정하기 위해 안쪽 여백이 추가됩니다.

4. 파일을 저장하고, 브라우저를 새로 고쳐 지도에 표시된 핀을 확인합니다. 이제 지도의 중심에 시애틀이 표시되며, 출발점을 표시하는 둥근 파란색 핀과 도착점을 표시하는 파란색 핀을 볼 수 있습니다.

   ![출발 지점과 도착 지점이 포함된 지도 보기](./media/tutorial-prioritized-routes/pins-map.png)

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>여행 모드에 따라 우선 순위가 지정된 경로 렌더링

이 섹션에서는 Maps 경로 서비스 API를 사용하여 운송 모드에 따라 지정된 출발 지점과 도착 지점 간의 여러 경로를 찾는 방법을 보여 줍니다. 경로 서비스는 현재 교통 상황을 고려하여 두 위치 간의 *최소 시간*, *최단 거리*, *최적* 또는 *모험* 경로를 계획할 수 있는 API를 제공합니다. 또한 사용자는 Azure의 광범위한 교통 기록 데이터베이스를 사용해 어떤 날짜 및 시간에 대한 경로 기간을 예측하여 미래의 경로를 계획할 수 있습니다. 자세한 내용은 [경로 방향 가져오기](https://docs.microsoft.com/rest/api/maps/route/getroutedirections)를 참조하세요. 다음 코드 블록은 모두 맵 로드 후에 지도가 완전히 로드되도록 **map load eventListener 내**에 추가해야 합니다.

1. 맵 로드 이벤트 처리기에서 다음 Javascript 코드를 추가하여 서비스 클라이언트를 인스턴스화합니다.

    ```JavaScript
    //If the service client hasn't been created, create an instance of it.
    if (!client) {
        client = new atlas.service.Client(atlas.getSubscriptionKey());
    }
    ```
    
2. 다음 블록의 코드를 추가하여 경로 쿼리 문자열을 생성합니다.

    ```JavaScript
    //Create the route request with the query being the start and end point in the format 'startLongitude,startLatitude:endLongitude,endLatitude'.
    var routeQuery = startPoint.geometry.coordinates[1] +
        ',' +
        startPoint.geometry.coordinates[0] +
        ':' +
        endPoint.geometry.coordinates[1] +
        ',' +
        endPoint.geometry.coordinates[0];
    ```

3. USHazmatClass2 클래스 화물을 운반하는 트럭의 경로를 요청하는 다음 JavaScript 코드를 추가하고 결과를 표시합니다.

    ```JavaScript
    //Execute the truck route query then add the route to the map.
    client.route.getRouteDirections(routeQuery, {
        travelMode: 'truck',
        vehicleWidth: 2,
        vehicleHeight: 2,
        vehicleLength: 5,
        vehicleLoadType: 'USHazmatClass2'
    }).then(function (response) {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson.GeoJsonRouteDirectionsResponse(response);

        //Get the route line and add some style properties to it.
        var routeLine = geoJsonResponse.getGeoJsonRoutes().features[0];
        routeLine.properties.strokeColor = '#2272B9';
        routeLine.properties.strokeWidth = 9;

        //Add the route line to the data source. We want this to render below the car route which will likely be added to the data source faster, so insert it at index 0.
        datasource.add(routeLine, 0);
    });
    ```
    위의 코드 조각은 [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest) 메서드를 통해 Azure Maps 라우팅 서비스를 쿼리하고, [getGeoJsonRouteDirectionsResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest)를 사용하여 응답을 GeoJSON 형식으로 구문 분석합니다. 그 후 반환되는 경로의 좌표 배열을 만들어서 데이터 원본에 추가합니다. 하지만 데이터 원본의 다른 선보다 먼저 렌더링되도록 인덱스 0을 추가합니다. 이는 트럭 경로 계산이 종종 승용차 경로 계산보다 느리기 때문이며, 트럭 경로 선이 승용차 경로 후에 데이터 원본에 추가되면 그 위에 렌더링됩니다. 두 속성(파란색의 멋진 음영인 스트로크 색 및 9픽셀의 스트로크 너비)이 트럭 경로 선에 추가됩니다. 

4. 다음 JavaScript 코드를 추가하여 승용차 경로를 요청하고 결과를 표시합니다.

    ```JavaScript
    //Execute the car route query then add the route to the map.
    client.route.getRouteDirections(routeQuery).then(function (response) {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson.GeoJsonRouteDirectionsResponse(response);

        //Get the route line and add some style properties to it.
        var routeLine = geoJsonResponse.getGeoJsonRoutes().features[0];
        routeLine.properties.strokeColor = '#B76DAB';
        routeLine.properties.strokeWidth = 5;

        //Add the route line to the data source.
        datasource.add(routeLine);
    });
    ```
    이 코드 조각은 자동차에 대해 동일한 트럭 경로 쿼리를 사용합니다. [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest) 메서드를 통해 Azure Maps 라우팅 서비스를 쿼리하고, [getGeoJsonRouteDirectionsResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest)를 사용하여 응답을 GeoJSON 형식으로 구문 분석합니다. 그런 다음, 반환되는 경로의 좌표 배열을 만들어서 데이터 원본에 추가합니다. 두 속성(보라색 음영인 스트로크 색 및 5픽셀의 스트로크 너비)이 승용차 경로 선에 추가됩니다. 

5. **MapTruckRoute.html** 파일을 저장하고, 브라우저를 새로 고쳐 결과를 확인합니다. Maps API와 성공적으로 연결되면 다음과 비슷한 지도가 표시됩니다.

    ![Azure Route Service를 사용하여 우선 순위가 지정된 경로](./media/tutorial-prioritized-routes/prioritized-routes.png)

    화물차 경로는 두꺼운 파란색이지만, 승용차 경로는 얇은 자주색입니다. 승용차 경로는 I-90 도로를 통해 워싱턴 호수를 가로질러 주거 지역 아래의 터널을 통과하므로 위험한 폐기 화물을 제한합니다. USHazmatClass2 화물 유형을 지정한 화물차 경로는 다른 고속 도로를 사용하도록 올바르게 지시됩니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 다음 방법에 대해 알아보았습니다.

> [!div class="checklist"]
> * 지도 컨트롤 API를 사용하여 새 웹 페이지 만들기
> * 지도에서 교통 흐름 시각화
> * 여행 모드를 선언하는 경로 쿼리 만들기
> * 지도에 여러 경로 표시

이 자습서에서 사용할 코드 샘플에 액세스하려면 다음을 참조하세요.

> [Azure Maps를 사용하여 여러 경로 찾기](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/truckRoute.html)

[여기서 실제 샘플 살펴보기](https://azuremapscodesamples.azurewebsites.net/?sample=Multiple%20routes%20by%20mode%20of%20travel)

다음 자습서에서는 Azure Maps를 사용하여 간단한 저장소 로케이터를 만드는 과정을 보여 줍니다.

> [!div class="nextstepaction"]
> [Azure Maps를 사용하여 저장소 로케이터 만들기](./tutorial-create-store-locator.md)
