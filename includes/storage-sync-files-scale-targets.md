---
title: 포함 파일
description: 포함 파일
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 07/18/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: fea31bf1c5869164dde1eb7014b2aacc8fcc5e9c
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55757545"
---
| 리소스 | 대상 | 하드 한도 |
|----------|--------------|------------|
| 지역당 스토리지 동기화 서비스 수 | 15개 저장소 동기화 서비스 | 예 |
| 저장소 동기화 서비스당 동기화 그룹 수 | 100개 동기화 그룹 | 예 |
| 저장소 동기화 서비스당 등록된 서버 | 서버 99대 | 예 |
| 동기화 그룹당 클라우드 엔드포인트 수 | 1개 클라우드 엔드포인트 | 예 |
| 동기화 그룹당 서버 엔드포인트 수 | 50개 서버 엔드포인트 | 아니요 |
| 서버당 서버 엔드포인트 수 | 30개 서버 엔드포인트 | 예 |
| 엔드포인트 크기 | 4TiB | 아니요 |
| 동기화 그룹당 파일 시스템 개체(디렉터리 및 파일) 수 | 2500만 개 개체 | 아니요 |
| 디렉터리에 있는 파일 시스템 개체(디렉터리 및 파일)의 최대 수 | 200,000만 개 개체 | 예 |
| 최대 개체(디렉터리 및 파일) 보안 설명자 크기 | 4KiB | 예 |
| 파일 크기 | 100GiB | 아니요 |
| 계층화할 파일에 대한 최소 파일 크기 | 64KiB | 예 |
| 동시 동기화 세션 | V4 에이전트: 제한은 사용할 수 있는 시스템 리소스에 따라 달라집니다. <BR> V3 에이전트: 프로세서당 2개의 활성 동기화 세션 또는 서버당 최대 8개의 활성 동기화 세션 | 예
