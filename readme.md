# Kubernetes - elementary school

 이 실습은 Jenkins를 통해 Kubernetes에 Application을 배포하기 위한 실습을 위해 만들어졌습니다.
 모든 것을 설명할 능력이 안되지만 필요로 하시는 분들이 있을 것이라 생각되어 Quick Starter를 위한 Guide를 하고자 합니다.

---

 우리는 Cloud를 사용하고 익히기 위해 AWS, Azure, GCP 등 많은 Public Cloud가 있고 일부기능을 무료로 사용할 수 있습니다.
  
 그럼에도 Local 환경에 Kubernetes를 구성하는 이유는 몇가지를 들 수 있습니다.

    1. 좀더 운영환경을 이해하고 싶을 경우
    2. 마구 다루더라도 비용이 발생하지 않을 환경이 필요한 경우
    3. Private Cloud 구축이 필요한 경우

 1번의 경우 어차피 공부를 위해하는 것이기 때문에 제쳐 두더라도, 2번의 쉽게 "Amazon에서 비용이 얼마가 결제 되어 들어가 보니 뭔가 동작하고 있더라" 하는 것들의 두려움이 시작을 시킬 수 있습니다. 3번의 경우 좀더 전문적인 내용이 필요하다 생각합니다.

 만일 내 생각에는 여러 이유를 제쳐 두고라도 관련된 업무를 하는 엔지니어나 개발자들은 한번 정도는 해볼만한 이 아닐까 생각합니다.
 그냥 내꺼 하나 있었으면 좋겠네.. 로 시작해도 좋을 것 같습니다.

## 앞으로 설명되는 것들

앞으로 설명되는 것들은 다음과 같습니다.

- Chapter 01. Virtual Box 및 Vagrant를 이용한 VM 및 Kubernetes Cluster로 연결
- Chapter 02. Jenkins의 설치
- Chapter 03. Springboot 기반 Restful API Sample 배포
- Chapter 04. API G/W
- Chapter 05. Jenkins 심화 (SonarQube, Slack 연동)
