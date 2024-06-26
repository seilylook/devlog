---
title: "Kafka Spark"
date: 2024-04-11T15:58:05+09:00
featuredImage: "/images/featured-image/spark-kafka.webp"
tags: ["Spark", "Kafka", "Data Engineering"]
categories: ["Data Engineering"]
draft: true
---

# Kafka와 Spark의 차이점은 무엇일까요?

Apache Kafka는 스트림 처리 엔진이고 Apache Spark는 분산 데이터 처리 엔진입니다. 분석에서 조직은 두 가지 주요 방법, 즉 배치 처리와 스트림 처리를 사용하여 데이터를 처리합니다. 배치 처리에서는 단일 워크로드에서 매우 많은 양의 데이터를 처리합니다. 스트림 처리에서는 작은 단위의 데이터를 실시간 흐름으로 연속적으로 처리합니다. 원래 Spark는 배치 처리용으로 설계되었고 Kafka는 스트림 처리용으로 설계되었습니다. 이후 Spark는 Spark 스트리밍 모듈을 기본 분산 아키텍처에 추가 기능으로 추가했습니다. 하지만 Kafka는 대부분의 스트리밍 데이터 사용 사례에서 더 낮은 지연 시간과 더 높은 처리량을 제공합니다.

# Kafka와 Spark의 유사점은 무엇인가요?

Apache Kafka 및 Apache Spark는 모두 더 빠른 속도로 데이터를 처리하기 위해 Apache Software Foundation에서 설계되었습니다. 조직에는 다양한 데이터 소스의 실시간 정보를 수집, 저장 및 분석할 수 있는 최신 데이터 아키텍처가 필요합니다.

Kafka와 Spark는 고속 데이터 처리를 관리하기 위한 중복 특성을 가지고 있습니다.

## 빅 데이터 처리

Kafka는 여러 서버에 분산된 데이터 파이프라인을 제공하여 대량의 데이터를 실시간으로 수집하고 처리합니다. 다양한 소스 간에 효율적이고 지속적인 데이터 전달이 필요한 빅 데이터 사용 사례를 지원합니다.

마찬가지로 Spark를 사용하여 다양한 실시간 처리 및 분석 도구를 사용하여 대규모로 데이터를 처리할 수 있습니다. 예를 들어 Spark의 기계 학습 라이브러리인 MLlib를 통해 개발자는 저장된 빅 데이터 세트를 사용하여 비즈니스 인텔리전스 애플리케이션을 구축할 수 있습니다.

## 데이터 다양성

Kafka와 Spark는 모두 비정형, 반정형 및 정형 데이터를 수집합니다. Kafka 또는 Spark를 사용하여 엔터프라이즈 애플리케이션, 데이터베이스 또는 기타 스트리밍 소스에서 데이터 파이프라인을 생성할 수 있습니다. 두 데이터 처리 엔진 모두 분석에 일반적으로 사용되는 일반 텍스트, JSON, XML, SQL 및 기타 데이터 형식을 지원합니다.

또한 데이터를 데이터 웨어하우스와 같은 통합 스토리지로 이동하기 전에 데이터를 전환하기도 하지만, 이를 위해서는 추가 서비스나 API가 필요할 수 있습니다.

## 확장성

Kafka는 확장성이 뛰어난 데이터 스트리밍 엔진이며 수직 및 수평으로 모두 확장할 수 있습니다. 특정 Kafka 브로커를 호스팅하는 서버에 더 많은 컴퓨팅 리소스를 추가하여 증가하는 트래픽에 대응할 수 있습니다. 또는 로드 밸런싱을 개선하기 위해 서로 다른 서버에 여러 Kafka 브로커를 생성할 수 있습니다.

마찬가지로 클러스터에 노드를 더 추가하여 Spark의 처리 용량을 확장할 수도 있습니다. 예를 들어, 병렬 처리를 위해 다중 노드에 변경 불가능한 데이터의 논리적 파티션을 저장하는 Resilient Distributed Dataset(RDD)를 사용합니다. 따라서 Spark는 대용량 데이터를 처리하는 데 사용할 때도 최적의 성능을 유지합니다.

# 워크플로: Kafka vs. Spark

Apache Kafka 및 Apache Spark는 서로 다른 아키텍처로 구축됩니다. Kafka는 주제, 브로커, 클러스터 및 소프트웨어 ZooKeeper를 분산하여 실시간 데이터 스트림을 지원합니다. 한편 Spark는 데이터 처리 워크로드를 여러 작업자 노드로 나누고, 이는 기본 노드에 의해 조정됩니다.

## Kafka는 어떻게 작동하나요?

Kafka는 실시간 분산 처리 엔진을 사용하여 데이터 생산자와 소비자를 연결합니다. Kafka의 핵심 구성 요소는 다음과 같습니다.

소비자와 생산자 간의 거래를 용이하게 하는 브로커
서로 다른 서버에 있는 여러 브로커로 구성된 클러스터
생산자는 Kafka 클러스터에 정보를 게시하고, 소비자는 처리를 위해 정보를 검색합니다. 각 Kafka 브로커는 주제에 따라 메시지를 구성한 다음 브로커를 여러 파티션으로 나눕니다. 특정 주제에 공통 관심을 가진 여러 소비자가 관련 파티션을 구독하여 데이터 스트리밍을 시작할 수 있습니다.

Kafka는 소비자가 데이터를 읽은 후에도 데이터 사본을 보관합니다. 이를 통해 Kafka는 생산자와 소비자에게 탄력적이고 내결함성이 있는 데이터 흐름 및 메시징 기능을 제공할 수 있습니다. 또한 ZooKeeper는 모든 Kafka 브로커의 상태를 지속적으로 모니터링합니다. 항상 다른 브로커를 관리하는 리더 브로커가 있는지 확인합니다.

## Spark는 어떻게 작동하나요?

Spark Core는 기본 Spark 기능을 포함하는 주요 구성 요소입니다. 이 기능에는 분산 데이터 처리, 메모리 관리, 작업 스케줄링 및 디스패치, 스토리지 시스템과의 상호 작용이 포함됩니다.

Spark는 데이터 전환 및 일괄 처리 워크플로를 지원하는 여러 순차 계층이 있는 분산형 기본-보조 아키텍처를 사용합니다. 프라이머리 노드는 데이터 처리 작업을 예약하고 워커 노드에 할당하는 중앙 코디네이터입니다.

데이터 사이언티스트가 데이터 처리 요청을 제출하면 다음 단계가 수행됩니다.

프라이머리 노드는 변경할 수 없는 데이터 복사본을 여러 개 생성합니다.
그래프 스케줄러를 사용하여 요청을 일련의 처리 작업으로 나눕니다.
작업을 Spark Core로 전달하고, Spark Core는 작업을 예약하고 특정 워커 노드에 할당합니다.
워커 노드가 작업을 완료하면 클러스터 관리자를 통해 결과를 프라이머리 노드에 반환합니다.

# 주요 차이점: Kafka vs Spark

Apache Kafka와 Apache Spark는 모두 조직에 빠른 데이터 처리 기능을 제공합니다. 하지만 아키텍처 설정이 다르기 때문에 빅 데이터 처리 사용 사례에서 작동하는 방식이 영향을 받습니다.

## ETL

추출, 전환, 적재(ETL)는 다양한 소스의 데이터를 대형 중앙 집중식 리포지토리에 결합하는 과정입니다. 다양한 데이터를 표준 형식으로 전환하려면 데이터 전환 기능이 필요합니다.

Spark에는 다양한 전환 및 로드 기능이 내장되어 있습니다. 사용자는 클러스터에서 데이터를 검색하고 적절한 데이터베이스에서 전환 및 저장할 수 있습니다.

반면, Kafka는 기본적으로 ETL을 지원하지 않습니다. 대신 사용자는 API를 사용하여 데이터 스트림에서 ETL 함수를 수행해야 합니다. 예:

개발자는 Kafka Connect API를 사용하여 두 시스템 간에 추출(E) 및 적재(L) 작업을 수행할 수 있습니다.
Kafka Streams API는 개발자가 이벤트 메시지를 다른 형식으로 조작하는 데 사용할 수 있는 데이터 전환(T) 기능을 제공합니다.

## 대기 시간

Spark는 실시간 처리 및 데이터 분석을 지원할 수 없었던 Apache Hadoop을 대체하기 위해 개발되었습니다. Spark는 하드 디스크 대신 RAM에 데이터를 저장하기 때문에 거의 실시간 읽기/쓰기 작업을 제공합니다.

하지만 Kafka는 지연 시간이 매우 짧은 이벤트 스트리밍 기능으로 Spark를 능가합니다. 개발자는 Kafka를 사용하여 실시간 데이터 변경에 대응하는 이벤트 기반 애플리케이션을 구축할 수 있습니다. 예를 들어, 디지털 음악 제공업체인 The Orchard는 Kafka를 사용하여 사일로화된 애플리케이션 데이터를 직원 및 고객과 거의 실시간으로 공유합니다.

## 프로그래밍 언어

개발자는 Spark를 사용하여 데이터 처리 플랫폼에서 다양한 언어로 애플리케이션을 구축하고 배포할 수 있습니다. 이러한 언어에는 Java, Python, Scala 및 R이 포함되며, Spark는 개발자가 그래프 처리 및 기계 학습 모델을 구현하는 데 사용할 수 있는 사용자 친화적인 API와 데이터 처리 프레임워크도 제공합니다.

반대로 Kafka는 데이터 전환 사용 사례에 대한 언어 지원을 제공하지 않습니다. 따라서 개발자는 추가 라이브러리 없이 플랫폼에서 기계 학습 시스템을 구축할 수 없습니다.

## 가용성

Kafka와 Spark는 모두 가용성과 내결함성이 높은 데이터 처리 플랫폼입니다.

Spark는 여러 노드에 워크로드의 영구 사본을 유지합니다. 노드 중 하나에 장애가 발생하면 시스템은 나머지 활성 노드의 결과를 다시 계산할 수 있습니다.

한편, Kafka는 데이터 파티션을 여러 서버에 지속적으로 복제합니다. Kafka 파티션이 오프라인 상태가 되면 자동으로 소비자 요청을 백업에 전달합니다.

## 여러 데이터 원본

Kafka는 여러 데이터 소스의 메시지를 동시에 스트리밍합니다. 예를 들어 다양한 웹 서버, 애플리케이션, 마이크로서비스, 기타 엔터프라이즈 시스템의 데이터를 특정 Kafka 주제에 실시간으로 보낼 수 있습니다.

반면 Spark는 언제든지 단일 데이터 소스에 연결됩니다. 하지만 Spark Structured Streaming 라이브러리를 사용하면 Spark가 여러 소스의 데이터 스트림을 마이크로 배치로 처리할 수 있습니다.

# 주요 차이점: Kafka vs Spark Structured Streaming

Spark 스트리밍을 통해 Apache Spark는 수신 스트림에 대해 마이크로 일괄 처리 방식을 채택할 수 있습니다. 이후 DataFrame과 Dataset API를 사용하여 스트림 처리 성능을 개선하는 Spark Structured Streaming을 통해 이 기능이 향상되었습니다. 이러한 접근 방식을 통해 Spark는 Apache Kafka와 같이 연속적인 데이터 흐름을 처리할 수 있지만, 두 플랫폼 간에는 몇 가지 차이점이 있습니다.

## 처리 모델

Kafka는 다양한 애플리케이션 또는 마이크로서비스를 연결하여 지속적인 처리를 가능하게 하는 분산 스트리밍 플랫폼입니다. 클라이언트 애플리케이션이 소스로부터 정보를 지속적으로 실시간으로 수신하도록 하는 것이 목표입니다.

Kafka와 달리 Spark Structured Streaming은 Spark 아키텍처에 추가 이벤트 스트리밍 지원을 제공하는 확장 프로그램입니다. 이를 사용하여 실시간 데이터 흐름을 캡처하고, 데이터를 작은 배치로 전환하고, Spark의 데이터 분석 라이브러리 및 병렬 처리 엔진으로 배치를 처리할 수 있습니다. 그럼에도 불구하고 Spark 스트리밍은 Kafka의 실시간 데이터 모으기 속도에 필적할 수 없습니다.

## 데이터 스토리지

Kafka는 생산자가 보내는 메시지를 주제라는 로그 파일에 저장합니다. 로그 파일의 경우, 정전 시에도 저장된 데이터가 영향을 받지 않도록 영구 스토리지가 필요합니다. 일반적으로 로그 파일은 다른 물리적 서버에 백업으로 복제됩니다.

한편, Spark Structured Streaming은 데이터 스트림을 RAM에 저장하고 처리하지만 데이터가 RAM 용량을 초과할 경우 디스크를 보조 스토리지로 사용할 수 있습니다. Spark Structured Streaming은 Apache Hadoop Distributed File System(HDFS)과 원활하게 통합되지만, Amazon Simple Storage Service(Amazon S3)를 비롯한 다른 클라우드 스토리지와도 호환됩니다.

## API

Kafka를 사용하면 개발자가 Kafka 데이터 스트림을 게시, 구독 및 설정한 다음 다양한 API로 처리할 수 있습니다. 이러한 API는 Java, Python, Go, Swift, 및 .NET을 비롯한 광범위한 프로그래밍 언어를 지원합니다.

한편, Spark Structured Streaming의 API는 다양한 소스에서 수집한 실시간 입력 데이터에 대한 데이터 변환에 중점을 둡니다. Kafka와 달리 Spark Structured Streaming API는 제한된 언어로만 제공됩니다. 개발자는 Java, Python 및 Scala와 함께 Spark Structured Streaming을 사용하여 애플리케이션을 구축할 수 있습니다.

# 사용 시기: Kafka vs. Spark

Kafka와 Spark는 서로 다른 용도로 사용되는 두 가지 데이터 처리 플랫폼입니다.

Kafka를 사용하면 확장 가능한 분산형 메시지 브로커 아키텍처를 통해 여러 클라이언트 앱이 실시간 정보를 게시하고 구독할 수 있습니다. 반면 Spark를 사용하면 애플리케이션에서 대량의 데이터를 일괄 처리할 수 있습니다.

따라서 Kafka는 클라우드의 다양한 애플리케이션 또는 서비스 간에 안정적이고 지연 시간이 짧고 처리량이 높은 메시징을 보장하는 데 더 좋은 옵션입니다. 한편, Spark를 통해 조직은 대규모 데이터 분석 및 기계 학습 워크로드를 실행할 수 있습니다.

Kafka와 Spark는 서로 다른 사용 사례에도 불구하고 상호 배타적이지 않습니다. 두 데이터 처리 아키텍처를 결합하여 내결함성이 있는 실시간 일괄 처리 시스템을 구성할 수 있습니다. 이 설정에서 Kafka는 여러 소스로부터 연속 데이터를 수집한 후 Spark의 중앙 코디네이터에 전달합니다. 그런 다음 Spark는 일괄 처리가 필요한 데이터를 각 작업자 노드에 할당합니다.
