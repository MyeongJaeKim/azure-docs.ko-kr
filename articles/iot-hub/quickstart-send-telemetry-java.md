---
title: Azure IoT Hub에 원격 분석 데이터 보내기 빠른 시작(Java) | Microsoft Docs
description: 이 빠른 시작에서는 두 개의 Java 애플리케이션 샘플을 실행하여 시뮬레이션된 원격 분석을 IoT Hub에 보내고, 클라우드에서 처리할 원격 분석을 IoT Hub에서 읽습니다.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/22/2018
ms.author: dobett
ms.openlocfilehash: 2527f6df8294c81816ade8708c6240714bcabc3d
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/12/2019
ms.locfileid: "54244854"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-it-with-a-back-end-application-java"></a>빠른 시작: 디바이스에서 IoT Hub로 원격 분석을 보내고 백 엔드 애플리케이션(Java)으로 읽습니다.

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub는 저장 또는 처리를 위해 IoT 디바이스에서 클라우드로 다량의 원격 분석 데이터를 수집할 수 있게 해주는 Azure 서비스입니다. 이 빠른 시작에서는 시뮬레이션된 장치 애플리케이션에서 IoT Hub를 통해 백 엔드 애플리케이션으로 원격 분석을 처리를 위해 보냅니다.

여기서는 미리 작성된 두 개의 Java 애플리케이션을 사용합니다. 하나는 원격 분석을 보내고, 다른 하나는 허브에서 원격 분석을 읽습니다. 이 두 애플리케이션을 실행하기 전에 IoT 허브를 만들고 허브에 장치를 등록합니다.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure 구독이 아직 없는 경우 시작하기 전에 [무료 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 을 만듭니다.

## <a name="prerequisites"></a>필수 조건

이 빠른 시작에서 실행하는 두 개의 샘플 애플리케이션은 Java를 사용하여 작성되었습니다. 개발 컴퓨터에 Java SE 8 이상이 필요합니다.

[Oracle](https://aka.ms/azure-jdks)에서 여러 플랫폼용 Java를 다운로드할 수 있습니다.

다음 명령을 사용하여 개발 컴퓨터에서 Java의 현재 버전을 확인할 수 있습니다.

```cmd/sh
java --version
```

샘플을 빌드하려면 Maven 3을 설치해야 합니다. [Apache Maven](https://maven.apache.org/download.cgi)에서 여러 플랫폼용 Maven을 다운로드할 수 있습니다.

다음 명령을 사용하여 개발 컴퓨터에서 Maven의 현재 버전을 확인할 수 있습니다.

```cmd/sh
mvn --version
```

https://github.com/Azure-Samples/azure-iot-samples-java/archive/master.zip에서 Java 프로젝트 샘플을 다운로드하고 ZIP 보관 파일을 추출합니다.

## <a name="create-an-iot-hub"></a>IoT Hub 만들기

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>디바이스 등록

연결을 위해 디바이스를 IoT Hub에 등록해야 합니다. 이 빠른 시작에서는 Azure Cloud Shell을 사용하여 시뮬레이션된 디바이스를 등록합니다.

1. Azure Cloud Shell에서 다음 명령을 실행하여 IoT Hub CLI 확장을 추가하고 디바이스 ID를 만듭니다. 

   **YourIoTHubName**: 이 자리 표시자를 IoT 허브용으로 선택한 이름으로 바꿉니다.

   **MyJavaDevice**: 등록된 디바이스에 지정된 이름입니다. 표시된 것처럼 MyJavaDevice를 사용합니다. 다른 장치 이름을 선택하는 경우 이 문서 전체에서 해당 이름을 사용해야 하고, 샘플 애플리케이션에서 장치 이름을 업데이트한 후 실행해야 합니다.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyJavaDevice
    ```

2. Azure Cloud Shell에서 다음 명령을 실행하여 방금 등록한 디바이스의 _디바이스 연결 문자열_을 가져옵니다.  **YourIoTHubName**: 이 자리 표시자를 IoT 허브용으로 선택한 이름으로 바꿉니다.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyJavaDevice --output table
    ```

    다음과 같은 디바이스 연결 문자열을 기록해 둡니다.

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    이 값은 빠른 시작의 뒷부분에서 사용합니다.

3. 또한 백 엔드 애플리케이션이 IoT 허브에 연결하고 메시지를 검색할 수 있도록 하려면 IoT 허브에서 _Event Hubs 호환 엔드포인트_, _Event Hubs 호환 경로_ 및 _iothubowner 기본 키_가 필요합니다. 다음 명령은 IoT 허브에 대해 이 값을 검색합니다.

     **YourIoTHubName**: 이 자리 표시자를 IoT 허브용으로 선택한 이름으로 바꿉니다.

    ```azurecli-interactive
    az iot hub show --query properties.eventHubEndpoints.events.endpoint --name YourIoTHubName

    az iot hub show --query properties.eventHubEndpoints.events.path --name YourIoTHubName

    az iot hub policy show --name iothubowner --query primaryKey --hub-name YourIoTHubName
    ```

    빠른 시작의 뒷부분에서 사용하기 위해 이 세 개의 값을 적어 둡니다.

## <a name="send-simulated-telemetry"></a>시뮬레이션된 원격 분석 전송

시뮬레이션된 장치 애플리케이션은 IoT 허브의 장치 관련 엔드포인트에 연결하고 시뮬레이션된 온도 및 습도 원격 분석을 전송합니다.

1. 로컬 터미널 창에서 Java 프로젝트 샘플의 루트 폴더로 이동합니다. 그런 다음, **iot-hub\Quickstarts\simulated-device** 폴더로 이동합니다.

2. 원하는 텍스트 편집기에서 **src/main/java/com/microsoft/docs/iothub/samples/SimulatedDevice.java** 파일을 엽니다.

    `connString` 변수의 값을 이전에 적어둔 디바이스 연결 문자열로 바꿉니다. 그런 다음 변경 내용을 **SimulatedDevice.java** 파일에 저장합니다.

3. 로컬 터미널 창에서 다음 명령을 실행하여 필요한 라이브러리를 설치하고 시뮬레이션된 장치 애플리케이션을 빌드합니다.

    ```cmd/sh
    mvn clean package
    ```

4. 로컬 터미널 창에서 다음 명령을 실행하여 시뮬레이션된 장치 애플리케이션을 실행합니다.

    ```cmd/sh
    java -jar target/simulated-device-1.0.0-with-deps.jar
    ```

    다음 스크린샷에서는 시뮬레이션된 장치 애플리케이션에서 IoT 허브에 원격 분석을 보낼 때의 출력을 보여 줍니다.

    ![시뮬레이션된 디바이스 실행](media/quickstart-send-telemetry-java/SimulatedDevice.png)

## <a name="read-the-telemetry-from-your-hub"></a>허브에서 원격 분석 읽기

백 엔드 애플리케이션은 IoT Hub의 서비스 측 **이벤트** 엔드포인트에 연결합니다. 애플리케이션은 시뮬레이션된 장치에서 전송하는 장치-클라우드 메시지를 받습니다. IoT Hub 백 엔드 애플리케이션은 일반적으로 클라우드에서 실행되며 장치-클라우드 메시지를 수신하고 처리합니다.

1. 다른 로컬 터미널 창에서 Java 프로젝트 샘플의 루트 폴더로 이동합니다. 그런 다음, **iot-hub\Quickstarts\read-d2c-messages** 폴더로 이동합니다.

2. 원하는 텍스트 편집기에서 **src/main/java/com/microsoft/docs/iothub/samples/ReadDeviceToCloudMessages.java** 파일을 엽니다. 다음 변수를 업데이트하고 파일에 변경 내용을 저장합니다.

    | 변수 | 값 |
    | -------- | ----------- |
    | `eventHubsCompatibleEndpoint` | 변수 값을 이전에 적어둔 Event Hubs 호환 엔드포인트로 바꿉니다. |
    | `eventHubsCompatiblePath`     | 변수 값을 이전에 적어둔 Event Hubs 호환 경로로 바꿉니다. |
    | `iotHubSasKey`                | 변수 값을 이전에 적어둔 iothubowner 기본 키로 바꿉니다. |


3. 로컬 터미널 창에서 다음 명령을 실행하여 필요한 라이브러리를 설치하고 백 엔드 애플리케이션을 빌드합니다.

    ```cmd/sh
    mvn clean package
    ```

4. 로컬 터미널 창에서 다음 명령을 실행하여 백 엔드 애플리케이션을 실행합니다.

    ```cmd/sh
    java -jar target/read-d2c-messages-1.0.0-with-deps.jar
    ```

    다음 스크린샷에서는 시뮬레이션된 장치에서 허브에 보낸 원격 분석을 백 엔드 애플리케이션에서 받을 때의 출력을 보여 줍니다.

    ![백 엔드 애플리케이션 실행](media/quickstart-send-telemetry-java/ReadDeviceToCloud.png)

## <a name="clean-up-resources"></a>리소스 정리

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서는 IoT 허브를 설정하고, 장치를 등록하고, Java 애플리케이션을 사용하여 허브에 시뮬레이션된 원격 분석 데이터를 보내고, 간단한 백 엔드 애플리케이션을 사용하여 허브에서 원격 분석 데이터를 읽습니다.

백 엔드 애플리케이션에서 시뮬레이션된 장치를 제어하는 방법을 알아보려면 다음 빠른 시작을 계속합니다.

> [!div class="nextstepaction"]
> [빠른 시작: IoT 허브에 연결된 디바이스 제어](quickstart-control-device-java.md)