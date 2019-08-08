---
title: "컴포넌트 아키텍쳐"
sidebar_label: "컴포넌트"
---

Home Assistant 는 **컴포넌트(구성요소)**로 확장될 수 있습니다. 각 컴포넌트는 Home Assistant 내의 특정 도메인을 담당합니다. 컴포넌트는 이벤트를 수신하거나 트리거하고, 서비스를 제공하고, 상태를 유지 관리 할 수 ​​있습니다. 컴포넌트는 파이썬(Python)으로 작성되며 Python 에서 제공하는 모든 유용한 점을 이용할 수 있습니다. Home Assistant 는 바로 사용가능한 다양한 [내장 컴포넌트](https://www.home-assistant.io/components/)를 제공합니다.

<img src='/img/en/architecture/component_interaction.png' alt='컴포넌트와 Home Assistant 코어간 상호 작용을 보여주는 다이어그램.' />

Home Assistant 에는 크게 다음의 두가지 컴포넌트로 나눌 수 있습니다: 이는 사물 인터넷 도메인과 상호작용하는 컴포넌트와 Home Assistant 에서 발생하는 이벤트에 응답하는 컴포넌트입니다. 각 유형에 대해 알아 보려면 계속 읽어주세요!

## 사물 인터넷 도메인과 상호작용하는 컴포넌트

이 컴포넌트는 특정 도메인 내의 장치를 추적하며 핵심 부분과 플랫폼 특화 로직으로 구성됩니다. 이러한 컴포넌트는 상태 시스템 및 이벤트 버스를 통해 정보를 제공합니다 컴포넌트는 서비스 레지스트리에 서비스를 등록하여 장치의 제어를 노출합니다.

예를 들어, 내장 된 [ `스위치` 컴포넌트](https://www.home-assistant.io/components/switch/)는 다양한 유형의 스위치와 상호 작용을 담당합니다. 플랫폼은 특정 종류나 브랜드의 기기를 지원합니다. 예를 들어 스위치는 WeMo 나 Orvibo 플랫폼을 사용할 수 있으며 조명 컴포넌트는 색조 또는 LIFX 플랫폼과 상호 작용할 수 있습니다.

새로운 플랫폼에 대한 지원을 추가하려면 [새로운 플랫폼 추가](creating_platform_index.md) 섹션을 참조해주세요.

## Home Assistant 내에서 발생하는 이벤트에 응답하는 컴포넌트

이 컴포넌트는 소소한 홈 자동화 로직을 제공하거나 집안에서 일반적인 작업을 수행하는 서비스를 포함합니다.

예를 들어, [`device_sun_light_trigger` 컴포넌트](https://www.home-assistant.io/components/device_sun_light_trigger/)는 기기의 상태와 태양 위치를 추적해서 날이 어두워지고 사람이 집안에 있을 때 조명이 켜지는지 확인합니다. 이 컴포넌트는 다음과 같은 로직을 사용합니다.

```text
'Paulus Nexus 5' 트래킹기기가 '집' 상태로 변경되는 이벤트:
  만약 태양이 일몰이고 조명이 켜지지 않았으면:
    조명을 켜시오
```

```text
모든 트래킹기기가 '집이 아님' 상태로 모두 변경되는 이벤트:
  만약 조명이 켜져 있다면:
    조명을 끄시오
```

```text
태양이 일몰이 되는 이벤트:
  만약 조명이 꺼져있고 모든 트래킹기기가 '집' 상태라면:
    조명을 켜시오
```

## 전체 그림

Home Assistant 의 다른 모든 부분들을 함께 배치하면, 이는 초기 형태의 홈 자동화 개요 스케치와 거의 일치합니다. 스마트 홈 AI 는 아직 구현되지 않았으므로, 이 그림에는 포함되어있지 않았습니다.

![몇몇 로드 된 컴포넌트와 플랫폼을 사용한 전체 Home Assistant 아키텍처의 개요](/img/en/architecture/ha_full_architecture.png)

컴포넌트의 플랫폼 로직은 서드파티 파이썬 라이브러리를 이용해서 기기와 통신합니다. 이를 통해 우리는 Python 커뮤니티의 끝내주는 라이브러리를 활용할 수 있습니다.