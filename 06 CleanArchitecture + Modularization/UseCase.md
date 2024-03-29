# UseCase란 무엇인가?
- 서비스를 사용하고 있는 사용자가 해당 서비스를 통해 하고자 하는 것을 의미합니다.
- 블로그라는 서비스가 있다고 가정했을 경우, '검색', '댓글', '공유' 와 같은 것이 UseCase 라고 할 수 있습니다.

## UseCase를 사용하는 이유
1. 어떤 것을 하고자 하는 지 직관적으로 파악할 수 있다.
  - Screaming Architecture 라고 부른다. 어떠한 서비스를 제공하는지 보면 바로 알 수 있다.
  - ViewModel 에서 UseCase 를 파라미터로 전달 받아 사용하게 되니 파라미터만 봐도 어떤일을 하는 지 알 수 있어야 한다. (협업 구조에 유리하다)
2. 의존성을 줄일 수 있다.
  - UseCase를 사용하지 않는다면, Repository를 전달 받아서 사용하게 되는데, 전달받은 Repository가 수정이 된다면 해당 Repository 를 사용하는 많은 부분에서 수정이 이뤄저야할 가능성이 높다.
  - UseCase를 사용하게 되면, 영향이 있는 UseCase를 사용하는 부분에서만 수정을 하면 되기 때문에 의존성이 줄어든다.
  - 이렇게 되면, 클린 아키텍처 목적 중 요구사항 변경 시 변경의 최소화를 만족하기 위해서 필요하다.

## UseCase 사용법
1. Data 계층
- RepositoryImpl - Domain 계층의 Repository 구현부
  - DataSource - 실제 데이터 가져오기

2. Doman 계층
- Repository - interface
- UseCase - Repository를 사용

3. Presentation 계층
- ViewModel: UseCase를 사용

### 과정
- CRUD (data source) > Repository Impl > Repository Interface > Use Case > ViewModel
