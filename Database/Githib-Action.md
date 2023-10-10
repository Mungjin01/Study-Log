## Github action 이란?

- 깃허브가 공식적으로 제공하는 빌드, 테스트, 및 배포 파이프라인을 자동화하는 CI/CD 플랫폼

레포지토리에서 Pull Request나 push 같은 이벤트를 트리거로 깃헙 작업 워크플로우(Work Flow)를 구성할 수있다.

워크플로우는 .yml 또는 .yaml 파일에 의해 구성되며, 테스트,배포 등 기능에 따라 여러개의 워크 플로우로도 만들 수 있다.

생성된 워크 플로우는 *.github/workflows 디렉토리 이하*에 위치한다

깃허브 액션를 설명하기 전에 ci/cd에 대해서 먼저 살펴보기.

만약 서비스를 배포하고 운용하던 중에 코드를 변경할 일이 생기면 어떤 작업을 해야 할까? 우선은 코드 수정을 하고, 로컬 환경에서 테스트를 진행할 겁니다. 그리고 빌드도 잘되는지 확인해야 한다. 그런 다음에는 jar 파일을 생성해 복사하고, AWS에 접속해서 복사한 jar 파일을 업로드해 새 배포 버전을 제공해야 한다. 그리고 프로젝트 규모가 엄청나게 커지면 이 작업은 굉장히 힘들 것이다. 그럴 때 도입하는 것이 CI/CD이다.

이 방법을 도입하면 빌드부터 배포까지의 과정을 자동화할 수 있고, 또 잘 되는지 모니터링할 수 있습니다. 사실 CI는 지속적 통합, CD는 지속적 제공이라는 의미가 있다. 

### **1.1 지속적 통합, CI**

CI는 Continuous Integration을 줄인 표현으로 한글로 해석하면 지속적 통합이고, 풀어서 설명하면 개발자를 위해 빌드와 테스트를 자동화하는 과정이다. CI는 변경 사항을 자동으로 테스트해 애플리케이션에 문제가 없다는 것을 보장한다. 그리고 코드를 정기적으로 빌드하고, 테스트하므로 여러 명이 동시에 작업을 하는 경우 충돌을 방지하고 모니터링할 수 있다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/8d9585d3-9c0c-4a4e-8f60-e45445271bf3/51d1a07c-20a3-4da2-a2dc-aa05cb5b82a7/Untitled.png)

보통 코드 변경 사항이 코드 저장소에 업로드되면 CI를 시작하고, CI 도중 문제가 생기면 실패하므로 코드의 오류도 쉽게 파악할 수 있다.

### **1.2 지속적 제공과 지속적 배포, CD**

CD는 CI 작업을 끝낸 다음 실행하는 작업입니다. 배포 준비가 된 코드를 자동으로 서버에 배포하는 작업을 자동화하는 것으로, CI가 통과되면 개발자가 수작업으로 코드를 배포하지 않아도 자동으로 배포하니 매우 편리하다. 때문에 CD는 지속적 제공(continuous delivery)이라는 의미와 지속적배포(continuous deployment)라는 의미를 모두 가진다.

### **1.4 지속적 배포에서의 CD의미**

지속적 제공을 통해 성공적으로 병합한 코드 내역을 AWS와 같은 배포 환경으로 보내는 것을 의미한다. 지속적인 배포는 지속적 제공의 다음 단계까지 자동화한다. 즉, 개발자가 애플리케이션에 변경 사항을 커밋한 후 몇 분 이내에 애플리케이션을 자동으로 배포되어 적용된다. 

![Untitled](https://i.esdrop.com/d/f/AfOYjCl4ON/Bq0dQvZE76.png)

### **Github Action**

GitHub Actions란 GitHub에서 제공하는 CI/CD 플랫폼

- 소프트웨어 workflow를 자동화할 수 있도록 도와주는 도구

---

### **Github Action Core 개념**

![Untitled](https://i.esdrop.com/d/f/AfOYjCl4ON/qnHYwcOuHL.png)

GitHub Actions란 GitHub에서 제공하는 CI/CD 플랫폼입니다.

특정 리포지토리에 트리거 이벤트가 발생할 경우 GitHub Action Runner에서 워크플로우 파일에 작성해 둔 일련의 작업들을 실행하는 구조이다

워크플로우 실행환경을 가리키는 GitHub Action Runner의 경우 GitHub에서 자체 제공하는 가상머신인 GitHub-hosted Runner와 직접 사용자가 환경을 구성하는 Self-hosted Runner가 있다.

- 1) Workflow
    
    워크플로우 구문에 대해 간단하게 설명드리려 합니다. GitHub Actions로 어떤 작업들을 만들어낼 수 있을지 구상하기 위해서는 workflow 구문을 자유롭게 활용할 수 있어야 한다.
    
    **name**
    
    ```
    # Lint test 워크플로우 이름 예시
    name: 'lint-on-pr'
    ```
    
    - 작성하고 있는 workflow의 명칭을 지정할 수 있습니다. name 속성을 생략할 경우 리포지토리 “Actions” 탭에서 워크플로우 파일 상대경로로 표시되기 때문에 지정해서 사용하는 것이 좋다.
    - 여러 Job으로 구성되고, Event에 의해 트리거될 수 있는 자동화된 프로세스
    - 최상위 개념
    - Workflow 파일은 YAML으로 작성되고, Github Repository의 `.github/workflows` 폴더 아래에 저장됨
- 2) Event
    - Workflow를 Trigger(실행)하는 특정 활동이나 규칙
    - 예를 들어 다음과 같은 상황을 사용할 수 있음
        - 특정 브랜치로 Push하거나
        - 특정 브랜치로 Pull Request하거나
        - 특정 시간대에 반복(Cron)
        - Webhook을 사용해 외부 이벤트를 통해 실행
    
- 3) Job
    - Job은 여러 Step으로 구성되고, 가상 환경의 인스턴스에서 실행됨
    - 다른 Job에 의존 관계를 가질 수 있고, 독립적으로 병렬 실행도 가능함
    
    ```
    jobs:
      first-job:
      second-job:
        needs: first-job
      third-job:
        needs: [first-job, second-job] # 조건부 실행
    ```
    
    - jobs 구문은 기본적으로 워크플로우에서 병렬적으로 실행되는 작업 단위를 의미합니다. 각 job들을 직렬, 조건부 실행하기 위해선 `jobs.<job_id>.needs` 옵션을 활용하면 특정 job이 실행 완료되었을 경우 다음 job을 실행하는 등의 동작도 가능하다.
- 4) Step
    - Task들의 집합으로, 커맨드를 날리거나 action을 실행할 수 있음
    
    ```
    jobs:
      steps:
        - name: first-step
        - name: second-step
    ```
    
    - step은 job을 구성하는 하나의 작은 작업 단위를 의미합니다. steps는 jobs와 달리 순차적으로 실행되며 서로 의존적입니다. 우리는 이 step 단위로 Action들과 스크립트를 실행하며 원하는 작업들을 구성하게 된다.
    
- 5) Action
    - Workflow의 가장 작은 블럭(smallest portable building block)
    - Job을 만들기 위해 Step들을 연결할 수 있음
    - 재사용이 가능한 컴포넌트
    - 개인적으로 만든 Action을 사용할 수도 있고, Marketplace에 있는 공용 Action을 사용할 수도 있음
        
        
- 6) Runner
    - Gitbub Action Runner 어플리케이션이 설치된 머신으로, Workflow가 실행될 인스턴스
    - Github에서 호스팅해주는 Github-hosted runner와 직접 호스팅하는 Self-hosted runner로 나뉨
    - Github-hosted runner는 Azure의 Standard_DS2_v2로 vCPU 2, 메모리 7GB, 임시 스토리지 14GB

앞서 살펴본 것처럼 GitHub Actions은 GitHub 플랫폼 내부에서 모든 CI/CD를 해결할 수 있을 뿐 아니라 커뮤니티 생태계가 잘 형성되어 있는 큰 장점을 가지고 있는 서비스이다.

워크플로우 작성이 매우 간단하기 때문에 Runner 머신만 세팅되어 있다면 누구나 짧은 시간 안에 도입이 가능하다.