---
title: 포함 파일
description: 포함 파일
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: cynthn;davberg
ms.custom: include file
ms.openlocfilehash: 29f1ad29bf305150592fd1e634e006f83b296bbc
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/22/2018
ms.locfileid: "53784418"
---
ACU(Azure 계산 단위) 개념은 Azure SKU에서 계산(CPU) 성능을 비교하는 방법을 제공합니다. 어떤 SKU가 성능 요구 사항을 충족할 가능성이 높은지 쉽게 식별하도록 합니다.  ACU는 현재 100인 작은(Standard_A1) VM에서 표준화되고 다른 SKU는 모두 SKU가 표준 벤치 마크를 얼마나 빨리 실행할 수 있는지를 나타냅니다. 

> [!IMPORTANT]
> ACU는 단지 지침일 뿐입니다.  워크로드에 대한 결과가 달라질 수 있습니다. 
> 
> 

<br>

| SKU 제품군 | ACU \ vCPU | vCPU: 코어 |
| --- | --- |---|
| [A0](../articles/virtual-machines/windows/sizes-general.md) |50 | 1:1 |
| [A1 - A4](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A5 - A7](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A1_v2 - A8_v2](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A2m_v2 - A8m_v2](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A8 - A11](../articles/virtual-machines/windows/sizes-hpc.md) |225* | 1:1 |
| [D1 - D14](../articles/virtual-machines/windows/sizes-general.md) |160 | 1:1 |
| [D1_v2 - D15_v2](../articles/virtual-machines/windows/sizes-general.md) |210 - 250* | 1:1 |
| [DS1 - DS14](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160 | 1:1 |
| [DS1_v2 - DS15_v2](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |210-250* | 1:1 |
| [D_v3](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |160-190* | 2:1\*\*\* |
| [Ds_v3](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |160-190* | 2:1\*\*\* |
| [E_v3](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160-190* | 2:1\*\*\*|
| [Es_v3](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160-190* | 2:1\*\*\* |
| [F2s_v2 - F72s_v2](../articles/virtual-machines/windows/sizes-compute.md) |195-210* | 2:1\*\*\* |
| [F1 - F16](../articles/virtual-machines/windows/sizes-compute.md) |210-250* | 1:1 |
| [F1s - F16s](../articles/virtual-machines/windows/sizes-compute.md) |210-250* | 1:1 |
| [G1 - G5](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |180 - 240* | 1:1 |
| [GS1 - GS5](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |180 - 240* | 1:1 |
| [H](../articles/virtual-machines/windows/sizes-hpc.md) |290 - 300* | 1:1 |
| [L4s - L32s](../articles/virtual-machines/windows/sizes-storage.md) |180 - 240* | 1:1 |
| [L8s_v2 - L80s_v2](../articles/virtual-machines/windows/sizes-storage.md) |150 - 175** | 2:1 |
| [M](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) | 160-180 | 2:1\*\*\* |

*ACU는 Intel® Turbo 기술을 사용하여 CPU 빈도를 늘리고 성능을 개선합니다.  성능 증가량은 VM 크기, 워크로드 및 동일한 호스트에서 실행 중인 다른 워크로드에 따라 달라질 수 있습니다.

**ACU는 AMD® Boost 기술을 사용하여 CPU 빈도를 늘리고 성능을 개선합니다.  성능 증가량은 VM 크기, 워크로드 및 동일한 호스트에서 실행 중인 다른 워크로드에 따라 달라질 수 있습니다.

***하이퍼 스레드 및 중첩된 가상화 실행 기능
