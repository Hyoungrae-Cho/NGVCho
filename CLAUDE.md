# NGVCho


## 소프트웨어 개발 정보

이 소프트웨어 개발은 다음의 항목을 이용해서 개발한다.

- Python 3.14를 이용해서 개발한다.
- 코드 테스트는 unittest를 활용한다.
- 순환복잡도, 함수라인수 등 측정 지표는 오픈소스 도구를 이용한다.


## 프로젝트 개발 정책

### 개발 생명 주기
- 이 프로젝트는 **반드시** 분석, 설계, 구현, 테스트의 순서로 개발을 진행한다.
- 각 단계가 완료되었을 때, 지정된 템플릿을 이용한 산출물이 생성되어야 한다.


### 분석 지침

- 요구사항 분석 단계 수행은 requirements-analyst 서브에이전트가 담당한다.

### 아키텍처 설계 지침

- 아키텍처 설계 단계 수행은 architecture-designer 서브에이전트가 담당한다.

### 상세 설계 지침

- 상세 설계 단계 수행은 detailed-designer 서브에이전트가 담당한다.

### 구현 지침

- 구현 단계 수행은 Coding 서브에이전트가 담당한다.
- TDD 방식으로 진행하고, TDD 스킬을 사용해야한다.
- 다음의 품질 지표를 **반드시** 준수해야 한다.
  - 함수 라인수는 순수코드라인 50라인 이하여야 한다.
  - 함수 순환복잡도는 10 이하여야 한다.
  - 중복 코드는 7라인까지 허용한다.
  - 주석은 Doxygen 방식으로 작성하며, 20% 이상 작성해야 한다.
- 함수명, 변수명은 3글자 이상 사용하고, 낙타 표기법을 활용한다.


### 단위 테스트 지침

- 단위 테스트는 TDD로 대체한다.
- 단위 테스트는 branch coverage 100%를 달성해야한다.
- 테스트 성공률은 100%여야 한다.


### 통합 테스트 지침

- 통합 테스트는 integration-tester 가 수행한다.
- 테스트 성공률은 100%여야 한다.

### 시스템 테스트 지침

- 시스템 테스트는 sw-system-tester 가 수행한다.
- 테스트 성공률은 100%여야 한다.

### 서브에이전트·스킬 구성 원칙

- 각 단계의 **스킬**은 방법론(표준 근거, 원칙, 판정기준, 템플릿)만 정의하고, **서브에이전트**는 그 방법론을 이 저장소에 적용하는 실행(선행조건/템플릿 확인, 사용자 확인, 도구 실행, 산출물 작성)을 전담한다. 스킬 내용을 에이전트가 다시 설명하지 않는다.
- 단계별 서브에이전트·스킬·표준 매핑:

| 단계 | 서브에이전트 | 스킬 | 표준 |
|---|---|---|---|
| 분석 | requirements-analyst | requirements-engineering | ISO 26262, A-SPICE SYS.2/SWE.1 |
| 설계(아키텍처) | architecture-designer | architecture-design | ISO 26262-6 clause 7, A-SPICE SWE.2/SYS.3 |
| 설계(상세) | detailed-designer | detailed-design | ISO 26262-6 clause 8, A-SPICE SWE.3 |
| 구현 | Coding | implementation, tdd | ISO 26262-6 clause 8, A-SPICE SWE.3, 본 문서 구현 지침 |
| 단위 테스트(TDD로 대체) | Coding | tdd | A-SPICE SWE.4 |
| 통합 테스트 | integration-tester | integration-test | ISO 26262-6 clause 10, A-SPICE SWE.5 |
| 시스템 테스트 | sw-system-tester | sw-system-test | ISO 26262 Part 6, A-SPICE SWE.6, ISO 29119/ISTQB, ISO 25000 |

- 감사(횡단 역할): `aspice-cl2-auditor`(스킬: `aspice-auditor`)는 위 단계 산출물을 CL2 관점으로 점검하는 별도 역할이며 특정 생명주기 단계에 속하지 않는다.
- 동일한 표준(예: ISO 25000 품질특성 분류)이 여러 스킬에서 필요한 경우, 최초 정의한 스킬을 그대로 참조하고 다른 스킬에서 재정의하지 않는다.