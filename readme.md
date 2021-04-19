# Cloud 스터디를 위한 개발환경 구성

 Cloud를 사용하고 익히기 위해 AWS, Azure, GCP 등 많은 Public Cloud가 있고 일부기능을 무료로 사용할 수 있습니다.
  
 그럼에도 Local 환경에 Kubernetes를 구성하는 이유는 몇가지를 들 수 있습니다.

    1. 좀더 운영환경을 이해하고 싶을 경우
    2. 마구 다루더라도 비용이 발생하지 않을 환경이 필요한 경우
    3. Private Cloud 구축이 필요한 경우

 1번의 경우 어차피 공부를 위해하는 것이기 때문에 제쳐 두더라도, 2번의 쉽게 "Amazon에서 비용이 얼마가 결제 되어 들어가 보니 뭔가 동작하고 있더라" 하는 것들의 두려움이 시작을 시킬 수 있습니다. 3번의 경우 좀더 전문적인 내용을 알아야 할 것입니다.

 내 생각에는 여러 이유를 제쳐 두고라도 관련된 업무를 하는 엔지니어나 개발자들은 한번 정도는 해볼만한 이 아닐까 생각합니다.

## 앞으로 설명되는 것들

설명되는 것들은 다음과 같습니다.

1. Virtual Box 및 Vagrant를 이용한 VM 생성
2. 생성된 3개의 VM을 Cluster로 연결
   - 개의 VM을 생성하여 Kubernetes Master, Node1, Node2를 구성합니다.
3. Jenkins의 설치
   - RBAC을 위한 Service Account를 설정
   - JENKINS_HOME을 위한 Persistence Volume의 사용ㅎ
   - Service Account를 이용하는 Jenkins를 CI/CD 구축
4. Springboot 기반 Restful API Sample 작성
   1. Docker Image 등록
   2. Docker Hub 등록
5. Jenkins
   1. Deploy.yaml 생성
   2. Service.yaml 생성
   3. 