---
title: "HDFS"
date: 2024-04-20T07:46:46+09:00
featuredImage: "/images/featured-image/hdfs-logo.jpeg"
tags: ["Apache Hadoop", "Big Data"]
categories: ["Computer", "Technology"]
draft: true
---

# Introduction

HDFS(하둡 분산형 파일 시스템)는 하둡 애플리케이션에서 사용하는 기본 스토리지 시스템입니다. 이 오픈 소스 프레임워크는 노드 사이에 데이터를 고속으로 전송하며 주로 빅데이터를 처리하고 저장해야 하는 기업이 사용하는 경우가 많습니다. HDFS는 빅데이터를 관리하고 빅데이터 분석을 지원하는 수단을 제공하기 때문에 많은 하둡 시스템의 핵심 구성 요소입니다.

전세계 많은 기업들이 사용하고 있는 HDFS란 정확히 무엇이며 왜 필요할까요? HDFS가 무엇이며 기업에 유용한 이유에 대해 자세히 살펴보겠습니다.

{{<admonition info>}}
POSIX는 Portable Operating System InterFace for Unix의 약자로 IEEE에서 지정한 운영체제간 호환성을 유지하기 위한 표준이다.

POSIX를 준수하는 운영체제는 POSIX를 준수하는 다른 운영체제와 호환되어야 한다.
{{</admonition>}}

# What is HDFS?

HDFS는 하둡 분산형 파일 시스템(Hadoop Distributed File System)을 뜻합니다. HDFS는 상용 하드웨어에서 실행되도록 고안된 분산형 파일 시스템으로 운영됩니다.

HDFS는 내결함성을 보유하여 저가형 상용 하드웨어에 배포하도록 설계되어 있습니다. HDFS는 애플리케이션 데이터에 고처리량 데이터 액세스를 제공하며 대규모 데이터 세트를 포함한 애플리케이션에 적합하고, Apache Hadoop에 보관된 파일 시스템 데이터에 대한 스트리밍 액세스를 지원합니다.

그렇다면 하둡이란 무엇일까요? 그리고 HDFS와 어떻게 다를까요? 하둡과 HDFS의 주요 차이점은 하둡은 데이터를 저장, 처리 및 분석할 수 있는 오픈 소스 프레임워크이고, HDFS는 데이터에 대한 액세스를 제공하는 하둡의 파일 시스템이라는 점입니다. 이는 본질적으로 HDFS는 하둡의 모듈이라는 의미입니다.

HDFS 아키텍쳐를 살펴 보겠습니다.

<img src="/images/hdfs/hdfs-image2.gif"/>

보시다시피 HDFS는 NameNode 및 DataNode에 중점을 둡니다.

NameNode는 GNU/Linux 운영 체제와 소프트웨어를 포함하는 하드웨어입니다. 하둡 분산형 파일 시스템은 마스터 서버 역할을 하며 파일 관리와 클라이언트의 파일 액세스 및 파일 이름 변경, 파일 열기 및 닫기와 같은 외부 파일 운영 프로세스를 제어할 수 있습니다.

- FSImage: 파일에 매핑된 블럭 등을 포함한 전체 네임스페이스 정보를 저장.

- EditLog: FSImage 정보에서 metadata 정보가 만약 변경된다면, 특정 파일에 변경 사항 Transaction 로그들 기록.

DataNode는 GNU/Linux 운영 체제와 DataNode 소프트웨어가 있는 하드웨어입니다. HDFS 클러스터에 있는 모든 노드는 DataNode에 위치합니다. 이러한 노드는 클라이언트의 요청에 따라 파일 시스템에 대한 작업을 수행할 수 있고 NameNode의 지시가 있을 때 파일을 생성, 복제, 차단할 수 있으므로 시스템의 데이터 스토리지를 제어하는 데 도움을 줍니다. DataNode는 주기적인 신호를 `Heartbeat` 3초마다 NameNode에 전송한다.

HDFS의 의미와 목적은 다음을 달성하는 것입니다.

- 대규모 데이터세트 관리(Manage large datasets): 데이터세트의 구성과 저장은 처리하기 어려울 수 있습니다. HDFS는 대규모 데이터세트를 처리해야 하는 애플리케이션을 관리하는 데 사용되는데 그러려면 HDFS는 클러스터 당 수백 개의 노드를 보유해야 합니다.

- 결함 감지(Detecting faults): HDFS에는 많은 상용 하드웨어가 있으므로 결함을 빠르고 효과적으로 스캔하고 감지할 수 있는 기술이 있어야 합니다. 구성 요소의 장애는 일반적인 문제이기 입니다.

- 하드웨어 효율성(Hardware efficiency): 대규모 데이터세트가 관련된 경우 네트워크 트래픽을 줄이고 처리 속도를 높일 수 있습니다.

# HDFS의 역사

하둡의 기원은 무엇입니까? HDFS는 Google 파일 시스템을 기초로 설계하였습니다. 원래는 Apache Nutch 웹 검색 엔진 프로젝트의 인프라로 구축했지만 결국은 하둡 에코시스템의 일원이 됐습니다.

인터넷 초창기에는 웹 사용자가 페이지에서 정보를 검색하는 방법으로 웹 크롤러가 등장했고 그 결과 Yahoo와 Google과 같은 다양한 검색 엔진이 만들어졌습니다.

또한 여러 컴퓨터에 동시에 데이터와 계산을 분산하려는 Nutch라는 검색 엔진도 개발되었습니다. Nutch에서 Yahoo로 이동하였고 Apache Spark 및 하둡이라는 개별 엔터티로 나뉘어졌습니다. 하둡이 일괄 처리를 처리하도록 설계되었다면, Spark는 실시간 데이터를 효율적으로 처리하도록 만들어졌습니다.

오늘날 하둡의 구조와 프레임워크는 소프트웨어 개발자와 기여자로 구성된 글로벌 커뮤니티인 Apache 소프트웨어 재단에서 관리합니다.

HDFS는 이 재단에서 탄생했으며 하드웨어 스토리지 솔루션을 더욱 효과적이고 효율적인 방법인 가상 파일 시스템으로 대체하도록 설계되었습니다. HDFS가 처음 등장했을 때 MapReduce는 HDFS를 사용할 수 있는 유일한 분산형 처리 엔진이었습니다. 최근에는 HBase와 Solr와 같은 대체 하둡 데이터 서비스 구성 요소도 HDFS를 활용하여 데이터를 저장할 수 있습니다.

# 빅데이터 세계에서 HDFS란 무엇입니까?

그러면 빅데이터란 무엇이며 어떻게 HDFS를 활용할까요? "빅데이터"란 용어는 저장, 처리 및 분석이 어려운 모든 데이터를 의미합니다. HDFS 빅데이터는 데이터를 HDFS 파일링 시스템으로 정리한 데이터입니다.

알려진대로 하둡은 병렬 처리 및 분산형 스토리지를 사용하여 작동하는 프레임워크이기 때문에 기존 방식으로는 저장할 수 없는 빅데이터를 정렬하고 저장하는 데 사용할 수 있습니다.

실제로 빅데이터를 처리하는 데 가장 일반적으로 사용되는 소프트웨어이며 데이터 저장을 위해 하둡과 긍정적인 관계를 맺고 있는 Netflix, Expedia 및 British Airways와 같은 회사에서 사용하고 있습니다. 현재 많은 기업이 데이터를 저장하는 방식으로 선택하고 있기 때문에 빅데이터에서 HDFS는 매우 중요합니다.

HDFS 서비스로 구성된 빅데이터의 5가지 핵심 요소는 다음과 같습니다.

- 속도(Velocity): 데이터가 생성, 대조 및 분석되는 속도입니다.

- 볼륨(Volume): 생성된 데이터의 양입니다.

- 다양성(Variety): 데이터 유형으로, 구조화, 비구조화 등이 될 수 있습니다.

- 진실성(Veracity): 데이터의 품질과 정확성입니다.

- 가치(Value): 데이터를 사용하여 비즈니스 프로세스에 대한 인사이트를 얻을 수 있는 방법입니다.

# 하둡 분산형 파일 시스템의 장점

HDFS는 하둡에 속한 오픈 소스 하위 프로젝트로서 빅데이터를 다룰 때 다섯 가지 중대한 장점을 제공합니다.

`내결함성` HDFS는 오류를 탐지하고 자동으로 신속히 복구하도록 설계되어 있으므로 지속성과 안정성을 보장합니다.

`속도` 클러스터 아키텍처이기 때문에 초당 2GB의 데이터를 유지관리할 수 있습니다.

`더 많은 유형의 데이터에 액세스`(특히, 스트리밍 데이터) 일괄 처리를 위해 대량의 데이터를 다루도록 설계되었기 때문에 높은 데이터 처리율을 달성하여 스트리밍 데이터 지원에 이상적입니다.

`호환성과 이동성` HDFS는 다양한 하드웨어 설정으로 이동이 가능하고 여러 기본 운영 체제와 호환되도록 설계되어 궁극적으로 사용자가 맞춤형 설정으로 HDFS를 사용할 수 있습니다. 이와 같은 장점은 특히 빅데이터를 다룰 때 중요한 의미가 있으며, HDFS가 데이터를 다루는 특별한 방식 때문에 가능한 일입니다.

아래 그래프는 로컬 파일 시스템과 HDFS의 차이점을 보여 줍니다.

<img src="/images/hdfs/hdfs-image3.png"/>

`확장성` 파일 시스템 크기에 맞춰 리소스 규모를 조정할 수 있습니다. HDFS에는 수직 및 수평 확장성 메커니즘이 포함되어 있습니다.

`데이터 지역성` 하둡 파일 시스템은 데이터가 계산 단위가 있는 위치로 이동하는 것과 달리 데이터는 데이터 노드에 상주합니다. 이를 통해 데이터와 컴퓨팅 프로세스 사이의 거리를 단축하여 네트워크 혼잡을 줄이고 시스템을 보다 효과적이고 효율적으로 만듭니다.

`비용 효율` 처음에는 데이터를 생각할 때 값비싼 하드웨어와 대역폭 점유를 생각할 수 있습니다. 하드웨어 오류가 발생하면 수정하는 데 많은 비용이 발생할 수 있습니다. HDFS를 사용하면 데이터가 가상으로 저렴하게 저장되므로 파일 시스템 메타데이터 및 파일 시스템 네임스페이스 데이터 스토리지 비용을 크게 줄일 수 있습니다. 또한, HDFS는 오픈 소스이기 때문에 라이선스 비용에 대해 걱정할 필요가 없습니다.

`방대한 양의 데이터 저장` 데이터 스토리지는 HDFS가 개발된 이유이며 모든 종류와 크기의 데이터, 특히 데이터 저장에 어려움을 겪고 있는 기업의 방대한 데이터를 저장하는 데 유용합니다. 여기에는 정형 데이터와 비정형 데이터가 모두 포함됩니다.

`유연성` 다른 기존 스토리지 데이터베이스와 달리 수집된 데이터를 저장하기 전에 별도로 처리할 필요가 없습니다. 원하는 만큼 데이터를 저장할 수 있으며, 데이터로 무엇을 하고 싶은지, 나중에 어떻게 사용할지 정확히 결정할 수 있습니다. 여기에는 텍스트, 비디오 및 이미지와 같은 비정형 데이터도 포함됩니다.

# HDFS 사용 방법

그렇다면 HDFS를 어떻게 사용하시겠습니까? HDFS는 기본 NameNode 및 여러 다른 데이터 노드와 함께 작동하며 모두 상용 하드웨어 클러스터에서 작동합니다. 이 노드는 데이터 센터 내의 동일한 위치에 구성됩니다. 그런 다음 여러 DataNode에 블록으로 분산하여 저장합니다. 데이터 손실의 가능성을 줄이기 위해 블록은 노드 간에 복제되는 경우도 많습니다. 데이터 손실을 대비한 백업 시스템입니다.

NameNode를 살펴 보겠습니다. NameNode는 데이터의 내용, 데이터가 속한 블록, 블록 크기 및 데이터의 이동 위치를 알고 있는 클러스터 내의 노드입니다. 또한, NameNode는 파일에 대한 엑세스 권한 즉, 다양한 데이터 정보 전반에서 데이터를 쓰고, 읽고, 생성하고, 제거하고, 복제할 수 있는 권한을 제어하는 데에도 사용됩니다.

클러스터는 필요한 경우, 서버 용량에 따라 실시간으로 조정할 수 있어 데이터가 급증할 때 유용할 수 있습니다. 노드는 필요에 따라 추가 또는 제거할 수 있습니다.

다음으로, DataNode에 대해 살펴 보겠습니다. DataNode는 작업을 시작하고 완료해야 하는지 여부를 확인하기 위해 NameNode와 지속적으로 통신합니다. 이렇게 일관된 협업의 흐름은 NameNode가 각 DataNode의 상태를 정확하게 알고 있다는 것을 의미합니다.

NameNode는 DataNode가 올바르게 작동하지 않는 것을 확인하면 해당 작업을 동일한 데이터 블록의 다른 정상 작동 노드에 자동으로 재할당할 수 있습니다. 마찬가지로 DataNode 역시 서로 통신할 수 있으므로 표준 파일 작업 중에 협업할 수 있습니다. NameNode는 DataNode와 그 성능을 잘 파악하고 있기 때문에 시스템을 유지 관리하는 데 중요합니다.

데이터 블록은 여러 DataNode로 복제되고 NameNode에서 액세스합니다.

HDFS를 사용하려면 하둡 클러스터를 설치하고 설정해야 합니다. 이는 처음 사용하는 사용자에게 더 적합한 단일 노드 설정일 수도 있고, 대규모 분산 클러스터를 위한 클러스터 설정일 수도 있습니다. 설정한 후에 시스템을 작동하고 관리하려면 아래와 같은 HDFS 명령어를 숙지해야 합니다.

<img src="/images/hdfs/hdfs-image4.gif"/>

| 명령어       | 설명                                                 |
| ------------ | ---------------------------------------------------- |
| -rm          | 파일 및 디렉터리 제거                                |
| -ls          | 사용 권한 및 기타 세부 정보가 있는 파일 나열         |
| -mkdir       | HDFS에서 경로명의 디렉터리 작성                      |
| -cat         | 파일 내용 표시                                       |
| -rmdir       | 디렉터리 삭제                                        |
| -put         | 로컬 디스크에서 HDFS로 파일 또는 폴더 업로드         |
| -rmr         | 경로 또는 폴더 그리고 하위 폴더로 식별되는 파일 삭제 |
|              |
| -get         | HDFS에서 로컬 파일로 파일 또는 폴더 이동             |
|              |
| -count       | 파일 수, 디렉터리 수 및 파일 크기 계산               |
|              |
| -df          | 여유 공간 표시                                       |
|              |
| -getmerge    | HDFS에서 여러 파일 병합                              |
| -chmod       | 파일 사용 권한 변경                                  |
| -copyToLocal | 로컬 시스템으로 파일 복사                            |
| -Stat        | 파일 또는 디렉터리에 대한 통계 인쇄                  |
| -head        | 파일의 첫 번째 KB 표시                               |
| -usage       | 개별 명령에 대한 도움말 반환                         |
| -chown       | 파일의 새 소유자 및 그룹 할당                        |

# HDFS는 어떻게 작동할까요?

앞서 언급했듯이 HDFS는 NameNode와 DataNode를 사용합니다. HDFS를 사용하면 컴퓨팅 노드 간에 데이터를 빠르게 전송할 수 있습니다. HDFS는 받은 데이터의 정보를 블록으로 분류하고 클러스터의 여러 노드에 분산할 수 있습니다.

데이터는 블록으로 분류되어 데이터 노드에 분산 저장되며, 이들 블록은 노드 간에 복제될 수 있어 효율적으로 병렬 처리할 수 있습니다. 사용자는 다양한 명령을 사용하여 데이터에 액세스하고 이동하고 볼 수 있습니다. "-get" 및 "-put"과 같은 HDFS DFS 옵션을 사용하면 필요에 따라 데이터를 검색하고 이동할 수 있습니다.

뿐만 아니라, HDFS는 높은 경계 수준을 구현하여 결함을 빠르게 감지할 수 있습니다. 파일 시스템은 데이터 복제를 사용하여 모든 데이터를 여러 번 저장한 다음, 개별 노드에 할당하여 최소한 하나의 복사본이 다른 복사본과 서로 다른 랙에 저장되도록 합니다.

이는 DataNode가 더 이상 NameNode에 신호를 보내지 않으면 NameNode는 클러스터에서 DataNode를 삭제하고 DataNode 없이 작동함을 의미합니다. DataNode가 다시 정상으로 돌아오면 새 클러스터에 할당할 수 있습니다. 또한, 데이터 블록이 여러 DataNode에서 복제되므로 DataNode를 하나 삭제하더라도 어떤 종류의 파일 손상도 유발하지 않습니다.

# HDFS 구성 요소

하둡에는 반드시 알아야 할 세 가지 주요 구성 요소가 있는데 Hadoop HDFS, Hadoop MapReduce 및 Hadoop YARN입니다. 그러면 이 세 가지 구성 요소가 하둡에 어떤 기능을 하는지 알아 보겠습니다.

`Hadoop HDFS` - HDFS(Hadoop Distributed File System)은 하둡의 저장 단위입니다.

`Hadoop MapReduce` - Hadoop MapReduce은 하둡의 처리 단위입니다. 이 소프트웨어 프레임워크는 방대한 양의 데이터를 처리하는 애플리케이션을 작성하는 데 사용됩니다.

`Hadoop YARN` - Hadoop YARN은 하둡의 리소스 관리 구성 요소입니다. 배치, 스트림, 상호작용 및 그래프 처리를 위해 데이터를 처리 및 실행하며, 모든 데이터는 HDFS에 저장됩니다.

# HDFS 파일 시스템을 만드는 방법

HDFS 파일 시스템을 만드는 방법이 궁금하신가요? 아래 단계에 따라 시스템을 만들고 편집하거나 필요한 경우 제거할 수 있습니다.

## HDFS 목록 표시

HDFS 목록은 `/user/yourUserName`이어야 합니다. HDFS 홈 디렉터리의 내용을 보려면 아래 명령을 입력합니다.

```bash
hdfs dfs -ls
```

처음 시작했기 때문에 현재 단계에서는 아무것도 표시되지 않습니다. 비어있지 않은 디렉터리의 내용을 보려면 다음 명령을 입력하면 됩니다.

```bash
hdfs dfs -ls /user
```

이제 다른 모든 하둡 사용자의 홈 디렉터리 이름을 볼 수 있습니다.

## HDFS에서 디렉터리 생성

이제 testHDFS라는 테스트 디렉터리를 만들 수 있습니다. 사용자의 HDFS 내에 나타납니다. 아래 명령을 입력하면 됩니다.

```bash
hdfs dfs -mkdir testHDFS
```

이제 HDFS 목록이 표시될 때 입력한 명령을 사용하여 디렉터리가 있는지 확인해야 합니다. testHDFS 디렉터리가 나열되어야 합니다.

사용자 HDFS의 HDFS 전체 경로 이름을 사용하여 다시 확인합니다. 다음을 입력합니다.

```bash
hdfs dfs -ls /user/yourUserName
```

다음 단계로 넘어가기 전에 올바르게 작동하는지 다시 한 번 확인해야 합니다.

## 파일 복사

로컬 파일 시스템에서 HDFS로 파일을 복사하려면 복사할 파일을 만들면 됩니다. 그러려면 다음을 입력하십시오.

```bash
echo "HDFS test file" >> testFile
```

그러면 HDFS 테스트 파일 문자를 포함하는 testFile이라는 새 파일이 생성됩니다. 이를 확인하려면 다음을 입력합니다.

ls

파일이 만들어졌는지 확인하려면 다음을 입력합니다.

```bash
cat testFile
```

그런 다음 파일을 HDFS에 복사해야 합니다. Linux에서 HDFS로 파일을 복사하려면 다음을 사용해야 합니다.

```bash
hdfs dfs -copyFromLocal testFile
```

"-cp" 명령은 HDFS 내에서 파일을 복사하는 데 사용되므로 "-copyFromLocal" 명령을 사용해야 합니다.

이제 파일이 올바르게 복사되었는지 확인하면 됩니다. 다음 명령을 입력하여 확인할 수 있습니다.

```bash
code>hdfs dfs -ls
hdfs dfs -cat testFile
```

## 파일 이동 및 복사

testfile을 복사할 때 기본 홈 디렉터리에 저장되었습니다. 이제 이미 만든 testHDFS 디렉터리로 이동하려면 다음 명령을 사용합니다.

```bash
hdfs dfs -mv testFile testHDFS
hdfs dfs -ls
hdfs dfs -ls testHDFS/
```

첫 번째 줄은 testFile을 HDFS 홈 디렉터리에서 사용자가 만든 테스트 디렉터리로 이동했습니다. 두 번째 줄에서는 테스트 파일이 더 이상 HDFS 홈 디렉터리에 없음을 보여 주고, 세 번째 줄에서는 이제 테스트 HDFS 디렉터리로 이동되었음을 확인합니다.

파일을 복사하려면 다음 명령을 입력합니다.

```bash
hdfs dfs -cp testHDFS/testFile testHDFS/testFile2
hdfs dfs -ls testHDFS/
```

## 디스크 사용량 확인

디스크 공간 확인은 HDFS를 사용할 때 유용합니다. 디스크 사용량 확인은 다음 명령을 입력하면 됩니다.

```bash
hdfs dfs -du
```

그러면 HDFS에서 사용 중인 공간을 확인할 수 있습니다. 또한, 다음 명령을 입력하면 클러스터 전반에서 HDFS가 사용할 수 있는 공간을 볼 수 있습니다.

## 파일/디렉터리 제거

HDFS에서 파일이나 디렉터리를 삭제해야 하는 경우가 할 발생할 수 있습니다. 다음 명령을 사용하여 삭제할 수 있습니다.

```bash
hdfs dfs -rm testHDFS/testFile
hdfs dfs -ls testHDFS/
```

생성한 testHDFS 디렉터리와 testFile2가 아직 남아 있는 것을 볼 수 있습니다. 다음을 입력하여 디렉터리를 제거합니다.

```bash
hdfs dfs -rmdir testhdfs
```

오류 메시지가 표시되겠지만 당황하지 마세요. "rmdir : testhdfs : 디렉터리가 비어 있지 않습니다" 같은 내용이 표시됩니다. 디렉터리를 삭제하려면 비어 있어야 하기 때문입니다. 표시된 메시지를 무시하고 "rm" 명령을 사용하여 포함된 모든 파일과 디렉터리를 제거할 수 있습니다. 다음을 입력합니다.

```bash
hdfs dfs -rm -r testHDFS
hdfs dfs -ls
```

# HDFS 설치 방법

하둡을 설치하려면 단일 노드와 다중 노드를 기억해야 합니다. 필요에 따라 단일 노드 클러스터나 다중 노드 클러스터를 사용할 수 있습니다.

단일 노드 클러스터는 하나의 DataNode가 실행 중이라는 의미이며 하나의 머신에 NameNode, DataNode, 리소스 관리자 및 노드 관리자가 포함됩니다.

일부 산업군에서는 이 정도 수준이면 충분합니다. 예를 들면 의료 분야에서는 연구를 수행하고 데이터를 순서대로 수집, 정렬 및 처리해야 할 때 단일 노드 클러스터를 사용할 수 있습니다. 단일 노드 클러스터는 수백 대의 컴퓨터에 분산된 데이터를 처리하는 것보다 더 작은 규모로 데이터를 쉽게 처리할 수 있습니다. 단일 노드 클러스터를 설치하려면 다음 단계를 따릅니다.

1. Java 8 패키지를 다운로드하여 홈 디렉터리에 저장합니다.

2. Java Tar 파일의 압축을 풉니다.

3. Hadoop 2.7.3 패키지를 다운로드합니다.

4. Hadoop tar 파일의 압축을 풉니다.

5. bash 파일(.bashrc)에 Hadoop 및 Java 경로를 추가합니다.

6. Hadoop 구성 파일을 편집합니다.

7. core-site.xml 열고 속성을 편집합니다.

8. HDFS-site.xml를 편집하고 속성을 편집합니다.

9. mapred-site.xml을 편집하고 속성을 편집합니다.

10. yarn-site.xml을 편집하고 속성을 편집합니다.

11. hadoop-env.sh를 편집하고 Java 경로를 추가합니다.

12. Hadoop 홈 디렉터리로 이동하여 NameNode를 포맷합니다.

13. hadoop-2.7.3/sbin 디렉터리로 이동하여 모든 데몬을 시작합니다.

14. 모든 하둡 서비스가 실행 중인지 확인합니다.

# HDFS 파일에 액세스하는 방법

우리는 데이터를 다루기 때문에 HDFS의 보안은 엄격할 수 밖에 없습니다. HDFS는 기술적으로 가상 스토리지이므로 클러스터 전반에 걸쳐 파일 시스템의 메타데이터만 볼 수 있고, 실제 특정 데이터는 볼 수 없습니다.

HDFS 파일에 액세스하기 위해면 HDFS에서 로컬 파일 시스템으로 "JAR" 파일을 다운로드할 수 있습니다. 또한, 웹 사용자 인터페이스를 사용하여 HDFS에 액세스할 수도 있습니다. 간단히 브라우저를 열고 검색 창에 "localhost:50070"을 입력하기만 하면 됩니다. 그러면 HDFS의 웹 사용자 인터페이스가 표시되고 우측의 유틸리티 탭으로 이동할 수 있습니다. 여기서 "파일 시스템 찾아보기"를 클릭하면 HDFS에 있는 전체 파일 목록이 표시됩니다.

# HDFS DFS 예제

가장 일반적인 하둡 명령 예제를 살펴보겠습니다.

## 예제 A

디렉터리를 삭제하려면 다음을 입력해야 합니다(참고 : 파일이 비어있는 경우에만 삭제 가능).

```bash
$Hadoop fs -rmdir /directory-name

or

$hdfs dfs -rmdir /directory-name
```

## 예제 B

HDFS에 여러 파일이 있는 경우, "-getmerge" 명령을 사용할 수 있습니다. 이 명령은 여러 파일을 단일 파일로 병합하여 로컬 파일 시스템에 다운로드할 수 있게 합니다. 다음 명령으로 수행할 수 있습니다.

```bash
$ Hadoop fs -getmerge [-nl] /source /local-destination

or

$ hdfs dfs -getmerge [-nl] /source /local-destination
```

## 예제 C

HDFS에서 로컬로 파일을 업로드하려는 경우 "-put" 명령을 사용할 수 있습니다. 복사할 위치와 HDFS에 복사할 파일을 지정합니다. 다음 명령을 사용하십시오.

```bash
$ Hadoop fs -put /local-file-path /hdfs-file-path

or

$ hdfs dfs -put /local-file-path /hdfs-file-path
```

## 예제 D

count 명령은 HDFS에서 디렉터리, 파일 및 파일 크기의 수를 추적하는 데 사용됩니다. 다음 명령을 사용하십시오.

```bash
$ Hadoop fs -count /hdfs-file-path

or

$ hdfs dfs -count /hdfs-file-path
```

## 예제 E

"chown" 명령을 사용하여 파일의 소유자와 그룹을 변경할 수 있습니다. 변경하려면 다음을 사용하십시오.

```bash
$ Hadoop fs -chown [-R] [owner][:[group]] hdfs-file-path

or

$ hdfs dfs -chown [-R] [owner][:[group]] hdfs-file-path
```

# HDFS 스토리지란 무엇입니까?

앞에서 언급했듯이 HDFS 데이터는 블록에 저장됩니다. 이 블록은 파일 시스템이 저장할 수 있는 가장 작은 데이터 단위입니다. 파일은 블록으로 처리되고 분류되어 클러스터 전체에 배포되고 만일을 대비해 안전하게 복제됩니다. 일반적으로 각 블록은 3번 복제될 수 있습니다. 다음 다이어그램은 빅데이터와 HDFS로 빅데이터를 저장하는 방법을 보여 줍니다.

<img src="/images/hdfs/hdfs-image5.png"/>

첫 번째는 DataNode에서 찾을 수 있고, 두 번째는 클러스터 내의 별도 DataNode에 저장되며, 세 번째는 다른 클러스터의 DataNode에 저장됩니다. 이는 3중 보호 보안 단계와 같습니다. 따라서 최악의 상황이 발생하고 하나의 복제본이 실패하더라도 추가 백업 데이터가 남아 있습니다.

NameNode는 블록 수와 본제본 저장 위치와 같은 중요한 정보를 가지고 있습니다. 이에 반해, DataNode는 실제 데이터를 저장하고 명령에 따라 블록을 생성, 제거 및 복제할 수 있습니다. 사용할 수 있는 명령은 다음과 같습니다.

```bash
In hdfs-site.xml
dfs.NameNode.name.dirfile:/Hadoop/hdfs/NameNode
```

```bash
dfs.DataNode.data.dir
file:/Hadoop/hdfs/DataNode
Dfs.DataNode.data.dir
```

이 명령은 DataNode가 블록을 저장하는 위치를 결정합니다.

# HDFS는 데이터를 어떻게 보관합니까?

HDFS 파일 시스템은 마스터 서비스(NameNode, 보조 NameNode 및 DataNode) 세트로 구성되어 있습니다. NameNode와 보조 NameNode가 HDFS 메타데이터를 관리합니다. DataNode는 기본 HDFS 데이터를 호스팅합니다.

NameNode는 HDFS에 속한 입력된 파일의 내용을 포함한 DataNode가 무엇인지 추적합니다. HDFS는 파일을 여러 블록으로 나누고, 각 블록을 DataNode에 하나씩 보관합니다. 여러 DataNode가 클러스터에 연결됩니다. 그런 다음 NameNode가 이러한 데이터 블록의 복제본을 클러스터 전체에 배포합니다. 또한 사용자나 애플리케이션에 원하는 정보의 위치가 어디 있는지 안내해 주기도 합니다.

# HDFS(Hadoop Distributed File System)은 무엇을 처리하도록 설계되었나요?

간단히 말해 "하둡 분산 파일 시스템은 무엇을 처리하도록 설계되었습니까?"라고 묻는다면 빅데이터라고 하겠습니다. 따라서 비즈니스와 고객의 데이터를 관리하고 저장하는 데 어려움을 겪는 대기업에 매우 유용할 수 있습니다.

하둡을 사용하면 업무, 과학, 소셜 미디어, 광고, 기계 등 다양한 데이터를 저장하고 통합할 수 있습니다. 또한 이 데이터를 활용하여 비즈니스 성과 및 분석에 대한 귀중한 인사이트를 얻을 수 있다는 의미이기도 합니다.

HDFS는 데이터를 저장하기 위해 설계되었기 때문에 과학자나 이 데이터를 분석하려는 의료 분야 종사자가 일반적으로 사용하는 원시 데이터도 처리할 수 있습니다. 이를 데이터 레이크라고 합니다. 데이터 레이크는 사용자들이 제약 없이 더 어려운 질문을 해결할 수 있게 해줍니다.

또한, 하둡은 주로 방대한 양의 데이터를 다양한 방식으로 처리하도록 설계되었기 때문에 분석 알고리즘을 실행하는 데에도 사용할 수 있습니다. 이를 통해 기업은 데이터를 보다 효율적으로 처리하고 분석하여 새로운 트렌드와 이상 징후를 발견할 수 있습니다. 특정 데이터세트는 데이터 웨어하우스에서 제거되고 하둡으로 이동되기도 합니다. 그러면 모든 것을 쉽게 접근할 수 있는 단일 위치에 보관할 수 있습니다.

업무용 데이터의 경우, 하둡은 수백만 건의 업무를 처리할 수 있는 기능을 갖추고 있습니다. 하둡의 스토리지와 처리 능력을 활용하여 고객 데이터를 저장하고 분석하는 데 사용할 수 있습니다. 또한 데이터를 심층적으로 분석하여 비즈니스 목표에 도움이 되는 새로운 트렌드와 패턴을 발견할 수 있습니다. 하둡은 지속적으로 새로운 데이터로 업데이트되고 있으며, 새 데이터와 이전 데이터를 비교하면 변경된 내용과 그 이유를 확인할 수 있습니다.

# HDFS 고려 사항

HDFS는 기본적으로 3회 복제로 구성되어 있습니다. 다시 말해 데이터 세트에 사본이 2개 더 있다는 의미입니다. 이렇게 하면 처리 중에 지역화된 데이터의 생성율이 개선되는 것은 사실이지만, 스토리지 비용 오버헤드가 발생하는 것도 맞습니다.

HDFS는 로컬로 연결된 스토리지를 사용해 구성하면 가장 효과가 좋습니다. 이렇게 하면 파일 시스템의 최상의 성능을 보장할 수 있기 때문입니다.
HDFS 용량을 늘리려면 스토리지 매체만이 아니라 새 서버(컴퓨팅, 메모리, 디스크)를 추가해야 합니다.

# HDFS 대 클라우드 오브젝트 스토리지

앞서 언급한 것과 같이 HDFS 용량은 컴퓨팅 리소스와 밀접하게 결합되어 있습니다. 스토리지 용량을 늘리면 CPU 리소스가 필요하지 않더라도 함께 증가하게 됩니다. HDFS에 데이터 노드를 더 많이 추가하면 재조정 작업을 수행하여 기존 데이터를 새로 추가된 서버에 배포해야 합니다.

이 작업을 하는 데 다소 시간이 걸릴 수 있습니다. 하둡 클러스터를 온프레미스 환경에서 확장하기란 비용과 공간 면에서도 어려운 일일 수 있습니다. HDFS는 로컬로 연결된 스토리지를 사용하기 때문에 YARN이 처리할 데이터를 보관한 서버에서 처리 작업을 제공할 수 있는 것으로 가정하면 IO 성능 면에서 유익합니다.

활용도가 높은 환경인 경우, 대부분의 데이터 읽기/쓰기 작업을 로컬이 아니라 네트워크에서 수행할 가능성이 큽니다. 클라우드 오브젝트 스토리지에는 Azure Data Lake Storage, AWS S3 또는 Google Cloud Storage와 같은 기술이 포함됩니다. 이 경우 스토리지에 액세스하는 컴퓨팅 리소스와 별개이므로 클라우드에는 훨씬 많은 양의 데이터를 저장할 수 있습니다.

페타바이트급의 데이터를 보관할 곳을 찾는 고객이라면 클라우드 오브젝트 스토리지를 이용하면 간편합니다. 다만 클라우드 스토리지에 대한 읽기 및 쓰기 작업은 모두 네트워크 상에서 수행됩니다. 따라서 자기 데이터에 액세스하는 애플리케이션이 캐싱을 활용하거나(가능한 경우) 로직을 포함하여 IO 작업을 최소한으로 줄이는 것이 중요합니다.

# Appendix

## Data Type

일반적으로 데이터 포맷은 크게 3 종류로 나뉜다. 이에 따라 Database의 종류가 나뉘고 저장 방식도 차이가 생긴다. 가령, 여러분들이 기존에 많이 접해본 RDBMS Mysql은 Structured Data 범주인 CSV 파일을 손쉽게 저장 및 관리할 수 있다. 하지만 Apache Log 과 같이 Json 데이터 포맷은 저장하기 쉽지 않으므로, 다른 Database 가령 Nosql을 사용해서 저장 및 관리해야한다. 이처럼 어떤 데이터 포맷들이 있는지 살펴보자.

### Structured Data

국문으로 `정형 데이터`라 하며, 미리 정해놓은 형식과 구조에 따라 저장되도록 구성하여 고정된 스키마, i.e., fixed schema 저장되는 데이터 입니다. 즉, 지정된 행열에 데이터가 구별되어 있으며, 이를 위에서 설명한 것처럼 RDBMS의 테이블 형태로 저장한다. 이처럼 정해진 구조 바탕으로 데이터를 용이하게 검색 및 수정 및 삭제 (CRUD) 등의 연산을 수행할 수 있다.

### Semi-Structured Data

국문으로 반정형 데이터라 하는데, 사실 국문이 어색하여 필자는 영문 그대로 SEMI-STRUCTURED DATA라고 부른다. 위에서 설명한 정형 데이터와 같이 TABLE의 행과 열로 구조화되어 있지는 않으나 Schema 및 Metadata 특성을 가지고 있다. 즉 정형 데이터와 달리 데어티 내용안에 구조에 대한 설명이 함께 존재한다고 봐도 무방한다. 예시로 `XML, HTML, JSON` 형태가 있다.

### Unstructured Data

`비정형 데이터`라 칭하며, 위 2가지 포맷과 달리 특별히 정해진 구조가 없이 저장된 데이터이다. social network test, image, video file, word, etc 문서와 같은 멀티미디어 데이터가 대표적인 예다. 약 20년전부터 SNS 이용률이 크게 높아지면서 실시간으로 많은 양의 비정형 데이터가 생산되고 있어 여러분들은 해당 데이터들을 쉽게 접할 수 있다.

## SPOF(Single Point of Failuture)?

단일 고장점이라고도 부르며 어떤 시스템에서 하나의 구성 요소가 동작하지 않으면 시스템 전체가 중단되는 상황. 장애가 발생한 구성 요소에 때한 여분 혹은 대체 가능한 요소가 구비되어 있지 않아서, 장애 복구가 완료될 때까지 운영이 불가능한 상황 직면.

EX) Without the namenode, the filesystem cannot be used.

- If the machine running the namenode were crashed, all the files on the filesystem would be lost since there would be no way of knowing how to reconstruct the files from the blocks on the datanodes.

## H.A

고가용성(High Availability)이란 서버와 네트워크, 프로그램 등의 정보 시스템이 상당히 오랜 기간 동안 지속적으로 정상 운영이 가능한 성질을 말하며, 가용성이 높다 뜻은 절대 고장 나지 않음을 의미. 흔히 가용한 시간의 비율을 99%, 99.9% 등과 같은 % 표현.

### Secondary Namenode

Its main role is to periodically merge the namespace image with the edit log to prevent the edit log from becoming too large.

The secondary namenode usually runs on a seperate physical machine because if requires plenty of CPU and as much memory as the namenode to perfome the merge.

### HDFS H.A

The namenode is a single point of failure(SPOF)

On large clusters with many files and blocks, the time it takes for a namenode to start from cold can be 30 minutes or more.

In Hadoop 2.x, there is a pair of namenodes in an active-standby configuration.

- In the event of the failutre of the active namenode, the standby takes over its duties to continue servicing client requests without a significant interruption(in a few tens of seconds).

### Data Integrity(데이터 무결성)

Every I/O operation on the disk or network carries with it a small chance of introducing errors into the data that it is reading or writing.

The usual way of detecting corrupted data is by computing a checksum for the data when it first enters the system(e.g, network interference, line noise, distortion)

- This does not offer any way to fix the data - it is merely error Detection

- A commonly used error-detecting code is CRC-32

HDFS transparently checksums all dat written to it and by default verifies chekcksums when reading data.

Datanodes are responsible for verifying the data they receive before storing the data and its checksum(If it detects an error, the client receives a ChecksumException)

When clients read data from data-nodes, they verify checksums as well, comparing them with the ones stored at the datanode.

### Fault Tolerance

Fault tolerance in Hadoop HDFS refers to the work intensity of the system under adverse conditions and how the system handles the situation

HDFS is extremely fault-tolerant.

Over hadoop 2.x, it handled failures through the process of replica creation. HDFS creates copies of data blocks & stores them on multiple machines(data nodes).

If any machine fails, the data block can be accessed from another computer that contains a copy of the same data. Therefore, there is no data loss because of replicas stored on different machines.

> This policy is called `Replica Placement`.

### Replica Placement

HDFS's policy is to put

1. one replica in the local rack

2. on a node in a remote rack

3. on a different node in the same remote rack

As the chance of rack failutre is far less than the node failure, this does not impact data reliability and availability guarantees.

<img src="/images/hdfs/hdfs-data-replication.png"/>
