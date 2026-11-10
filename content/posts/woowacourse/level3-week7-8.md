---
title: "우테코 - 레벨3 프로젝트 7~8주차 회고(바로 읽히는 코드를 보고 싶었다)"
category: "wooteco"
slug: "mission-16"
num: 16
date: 2026-11-10
description: "레벨3 7~8주차. AI로 금방 짤 줄 알았는데 서로의 코드를 리뷰하며 코드 스타일과 개발 철학을 맞추는 데 시간을 거의 다 썼다."
tags: ["우테코", "레벨3", "회고", "비비디", "코드 리뷰"]
---

6주차 회고는 코드를 쓰기 전에 우리 서비스의 방향성과 해결하려는 문제를 디테일까지 맞춰 두자는 이야기로 끝냈다. 4주차부터 6주차까지 세 주를 기획에 썼고 코드는 7주차에 시작했다. 남은 시간이 2주밖에 없어서 개발은 급할 수밖에 없었다. 백엔드는 보예와 라티, 나 셋이 맡았다.

그래도 코드는 AI가 있으니 생각보다 금방 짤 거라고 생각했다. 결론부터 이야기하자면 금방 짜지 못했고 코드 스타일도 생각보다 많이 달랐다. 그래서 바쁜 와중에도 서로의 코드를 리뷰하며 코드 스타일과 개발 철학을 맞추려고 노력했고 남은 시간을 거의 다 거기에 썼다. 이번 글에는 8월 24일 개발을 시작한 날부터 9월 9일까지 있었던 일을 적었다.

### AI에게 하네스를 씌우고 싶었다

컨벤션은 개발을 시작하기 전에 내가 맡았는데 우리만의 규칙을 정해서 AI가 그 범위를 넘지 못하게 하네스를 씌우고 싶었다. 실제 회사도 그렇게 할 것 같았고 그러면 누가 AI에게 시켜도 비슷한 결과가 나올 거라고 생각했다.

셋이 레벨 1, 2 미션에서 짠 코드를 싹 긁어서 아홉 가지 카테고리로 나눴다.

- 아키텍처와 패키지
- 도메인과 검증
- 영속성
- REST API
- 예외와 에러 응답
- 트랜잭션과 동시성
- 테스트 범위와 대역
- 테스트 표현
- 커밋

카테고리마다 각자 왜 그렇게 짰는지 이야기했는데 다들 나름의 이유가 있어서 타협이 되지 않았다. 더 논의할 시간도 부족했는데 그때 라티가 죽어도 양보 못 하는 것만 정하면 되지 않겠냐고 했다.

나도 타협했다. 하네스를 많이 씌우지 않아도 AI가 코드를 잘 짜니까 코드 퀄리티가 크게 떨어지지는 않을 거라고 봤고 시간이 없어서 양보한 것도 있었다. 사실 하네스를 씌운다고 우리 코드의 결이 얼마나 잘 맞을지도 알 수 없어서 절대 안 되는 것만 정하고 일단 해 보자는 쪽으로 갔다.

그래도 예외를 처리하는 ProblemDetail은 꼭 필요할 것 같았다. 서버에서 나오는 모든 예외를 규격화해서 클라이언트가 처리할 수 있도록 스프링이 지원하는 오류 응답 표준인 ProblemDetail을 쓰기로 했다.

### 라티가 설명하는 도메인 설계를 왜 하는지 몰랐다

도메인 설계는 라티가 맡았다. 라티는 도메인끼리 어떤 관계인지, 어떻게 협력할지, 어떤 메시지를 주고받을지를 칠판 앞에서 열심히 설명했다. 그런데 클래스 다이어그램 같은 것도 아니어서 나는 저걸 보고 어떻게 개발하라는 건지 알 수 없었다.

대충 저렇게 나누면 좋겠다는 느낌은 알겠는데 딱 저것만 보고 이렇게 개발해야겠다는 생각은 들지 않았다. "그러면 이거 왜 하는 거지?"라는 생각이 컸던 것 같다.

나중에 코드를 짜면서 도메인 설계를 조금 다르게 보게 됐다. 그럼에도 라티가 설명하던 만큼 자세히 정하기보다는 도메인을 적당히 이야기하고 직접 코드를 짜면서 고민하고 이야기해서 맞춰 나가는 게 맞을 것 같다는 생각이 들었다.

도메인은 카탈로그(준비 목록), 체크리스트, 일정 세 개로 나눴고 나는 그중 카탈로그와 체크리스트를 맡았다. 그때는 두 도메인의 책임과 역할을 이 정도까지만 이해했다.

카탈로그는 결혼 준비에 필요한 요소를 정리해서 사용자에게 보여 주는 도메인이었다. 웨딩홀 같은 카테고리 안에 웨딩홀 계약 같은 단계가 있고 단계 안에 계약서 확인이나 견적 비교 같은 항목이 있었다. 사용자 누구에게나 같은 목록을 보여 주는 것이 중요했다.

체크리스트는 사용자마다 하나씩 가지는 자기 할 일 목록이었다. 카탈로그의 항목을 들고 오거나 할 일을 직접 만들 수 있었고 할 일마다 시작 전, 진행 중, 완료 상태가 있었다.

일정은 체크리스트의 세부 일정 같은 느낌으로 할 일 하나에 날짜와 시간, 장소를 적은 일정이 붙는 식이었다. 왜 나눴는지 정확히 기억나지는 않는데 체크리스트와 생명주기가 다르고 변경의 이유가 다르다고 생각해서 결정했던 것 같다.

도메인과 테이블(ERD)도 따로 설계했다. 도메인 객체는 책임과 역할에 맞춰 만들고 테이블은 정규화를 기준으로 설계한 뒤 그 사이를 매퍼가 옮겨 담는 구조였다. 내가 맡은 카탈로그도 이렇게 나뉘었다.

```text
catalog
├── domain/Catalog, Category, Step, Item                                   (도메인 객체)
├── persistence/JpaCategoryEntity, JpaStepEntity, JpaCatalogItemEntity     (테이블)
└── repository/CatalogRepository, CatalogMapper                            (테이블을 도메인으로 옮김)
```

```java
// CatalogRepository: 테이블 세 개를 읽어 도메인 하나로 조립한다
public Catalog findCatalog() {
    return catalogMapper.toDomain(
            jpaCategoryRepository.findAll(),
            jpaStepRepository.findAll(),
            jpaCatalogItemRepository.findAll()
    );
}
```

도메인과 테이블이 분리되면서부터 이해가 어려웠고 처음에는 어떻게 해야 할지 감도 잡지 못했다. 체크리스트는 도메인만 먼저 만들어 PR을 올렸고 영속성은 다음 PR에서 따로 붙였다. 그래도 짜다 보니 도메인과 테이블이 같을 필요가 없다는 게 조금씩 보였고 중간쯤부터 감이 잡혔다.

### 바로 읽히는 코드를 보고 싶었다

내가 하고 싶었던 리뷰는 요구사항 문서를 보고 커밋과 PR만 읽어도 "아, 이렇게 구현했구나"를 알 수 있는 리뷰였다. 바로 읽히는 것이 제일 중요하다고 생각했다.

보예가 맡은 회원가입 코드가 있어야 우리 코드도 개발할 수 있어서 보예 코드부터 리뷰했는데 처음부터 바로 안 읽혔다. 올라온 회원가입 코드는 이랬다.

```java
// UserService: 처음 올라온 회원가입
@Transactional
public UserCreationResult createUser(String nickname, String rawPassword) {
    if (userRepository.existsByNickname(nickname)) {
        throw new DuplicateNicknameException();
    }
    String passwordHash = passwordHasher.hash(rawPassword);
    User user = userRepository.save(User.create(nickname, passwordHash));
    Checklist checklist = checklistService.createChecklistFor(user.id());
    return new UserCreationResult(user.id(), user.nickname(), checklist.id());
}
```

추상화 수준도 맞지 않았고 한 메서드가 닉네임 중복 확인과 비밀번호 해싱, 사용자 저장, 체크리스트 생성까지 다 하고 있었다. `User`에는 값을 꺼내 주는 메서드만 있어서 스스로 하는 일이 없었고 객체끼리 협력한다기보다 API 하나를 위에서 아래로 순서대로 처리하는 코드에 가까웠다. 내 눈에는 객체지향스럽지 않고 절차지향스러운 코드로 보였다.

회원가입 코드에서 먼저 걸린 건 체크리스트를 만드는 부분이었다. 사용자가 가입하면 빈 체크리스트도 같이 만들어야 했는데 사용자 도메인이 체크리스트 도메인을 직접 불러서 만들고 있었다. 보예는 둘 중 하나가 실패하면 둘 다 취소돼야 하고 회원가입은 사용자에서 시작하니까 사용자 쪽에서 체크리스트까지 만들면 된다고 생각했다.

나는 회원가입이 사용자 도메인(사용자 서비스)에서 시작한다고 해서 체크리스트를 만드는 일까지 거기에서 맡을 이유는 없다고 봤다. 나중에 체크리스트가 사용자의 존재를 확인하려고 사용자를 부르면 양방향 의존이 생길 것 같았다. 라티는 EDA(이벤트 기반 아키텍처)를 도입해 볼지 물었다.

하지만 EDA를 도입하기에는 러닝 커브가 있었다. 그렇다고 필요할 때마다 서로 불러 쓰면 양방향 의존이 생길 수밖에 없었다. 그래서 사용자와 체크리스트가 한 트랜잭션으로 묶이지 않는 걸 감수하더라도 트랜잭션을 풀고 클라이언트가 한 번 더 요청하는 방향으로 결정했다. 회원가입 API는 사용자만 만들고 빈 체크리스트는 별도 API로 만들기로 했다. 나중에 클라이언트는 준비 목록 항목을 담기 전에 체크리스트가 있는지 확인하고 없으면 만드는 요청을 먼저 보냈다.

```diff
  User user = userRepository.save(User.create(nickname, passwordHash));
- Checklist checklist = checklistService.createChecklistFor(user.id());
- return new UserCreationResult(user.id(), user.nickname(), checklist.id());
+ return new UserCreationResult(user.id(), user.nickname());
```

라티와 나는 코드 퀄리티를 맞추려고 PR마다 계속 피드백을 달았는데 회원가입 다음으로 올라온 로그인 PR에서도 같은 문제가 나왔다. 회원가입에서 사용자가 체크리스트를 직접 불렀던 것처럼 이번에는 인증이 사용자 도메인의 저장소와 도메인 객체를 가져다 쓰고 있었다. 패키지로 보면 이랬다.

```text
auth
└── service/AuthService
        │ import
        ▼
user
├── domain/User                 ◀ AuthService가 직접 사용
├── repository/UserRepository   ◀ AuthService가 직접 사용
└── service/PasswordHasher      ◀ AuthService가 직접 사용
```

```java
public class AuthService {

    private final UserRepository userRepository;
    private final PasswordHasher passwordHasher;

    public AuthResult login(String nickname, String rawPassword) {
        User user = userRepository.findByNickname(nickname)
                .orElseThrow(() -> new BusinessException(ClientError.AUTHENTICATION_FAILED, "로그인 인증에 실패했습니다."));
        if (!passwordHasher.matches(rawPassword, user.passwordHash())) {
            throw new BusinessException(ClientError.AUTHENTICATION_FAILED, "로그인 인증에 실패했습니다.");
        }
        return new AuthResult(user.id(), user.nickname());
    }
}
```

나는 어떤 기능이 필요하다는 건 의존을 더할 이유일 뿐이고 어느 쪽으로 의존할지를 정하는 근거는 아니라고 리뷰를 달았다. 이대로면 기능을 추가하는 순서대로 의존이 이어지고 나중에 사용자 쪽에서 인증이 필요해지면 순환 의존이 생길 것 같았다.

저장소를 직접 쓰는 것과 사용자 서비스를 거치는 것의 차이는 인증이 사용자를 어떻게 찾는지, 못 찾으면 무엇을 돌려주는지 같은 사용자 도메인의 안쪽까지 아느냐였다. 저장소를 직접 쓰면 사용자를 못 찾았을 때 어떻게 할지를 인증이 정해야 하고 사용자를 쓰는 다른 서비스도 저마다 같은 처리를 하게 된다. 그때 내가 걱정한 것도 이 부분이었다. 사용자를 쓰는 서비스마다 이렇게 하면 예외 코드가 바뀔 때 그걸 다 찾아서 고쳐야 했다.

라티도 인증과 사용자는 책임이 다르다고 봤다.

> Auth와 User의 변경 시점이 일치하는 지 궁금합니다!
> 지금 제 생각도 티뉴와 같습니다. 변경의 이유가 다르기 때문에 책임이 다르다고 생각이 드는데, 이 부분에 대해서 어떻게 생각하시나요

그래서 인증과 사용자의 책임을 나눴다. 비밀번호 검증과 인증 실패 정책은 인증이 맡고 사용자는 자기 정보를 조회하고 만들고 저장하는 일만 맡았다. 패키지도 이렇게 바뀌었다.

```text
auth
├── password/PasswordHasher     (user에서 옮겨 옴)
└── service/AuthService
        │ import
        ▼
user
├── service/UserService, UserAuthenticationInfo   ◀ AuthService는 여기만 사용
├── exception/UserNotFoundException
├── domain/User                 (AuthService가 직접 쓰지 않음)
└── repository/UserRepository   (AuthService가 직접 쓰지 않음)
```

```java
public class AuthService {

    private final UserService userService;
    private final PasswordHasher passwordHasher;

    public AuthResult login(String nickname, String rawPassword) {
        UserAuthenticationInfo user;
        try {
            user = userService.findAuthenticationInfo(nickname);
        } catch (UserNotFoundException ignored) {
            throw new BusinessException(ClientError.AUTHENTICATION_FAILED, "로그인 인증에 실패했습니다.");
        }
        if (!passwordHasher.matches(rawPassword, user.passwordHash())) {
            throw new BusinessException(ClientError.AUTHENTICATION_FAILED, "로그인 인증에 실패했습니다.");
        }
        return new AuthResult(user.userId(), user.nickname());
    }
}
```

이제 인증은 사용자 저장소를 직접 쓰지 않았다. 인증이 사용자를 부르는 한 방향이 됐고 사용자는 인증을 몰라도 됐다.

상태를 바꾸는 코드도 마찬가지였다. 비밀번호를 바꿀 때 도메인이 아니라 JPA 엔티티를 바로 가져다 값을 바꾸고 있어서 도메인 객체가 있는데도 정작 상태를 바꿀 때는 쓰지 않았다. 나는 라티 코드처럼 도메인이 상태를 바꾸는 쪽으로 맞추자고 했고 라티는 영속성 계층과 관계없이 도메인만으로 동작하는 코드가 이상적이라고 했다. 결국 상태를 바꾸는 일은 도메인으로 옮겼다.

```java
// 전: 저장소가 JPA 엔티티를 바로 바꾼다
public void updatePasswordHash(Long userId, String passwordHash) {
    JpaUserEntity entity = getJpaUserEntity(userId);
    entity.changePasswordHash(passwordHash);
    jpaUserRepository.save(entity);
}
```

```java
// 후: 도메인이 값을 바꾼 새 객체를 돌려주고 서비스는 저장만 한다

// User
public User changePasswordHash(String passwordHash) {
    return new User(id, nickname, passwordHash);
}

// UserService
User user = findCurrentUser(currentUserId);
User changedUser = user.changePasswordHash(passwordHash);
userRepository.save(changedUser);
```

이런 피드백은 생각보다 금방 해결되지 않았다. 리뷰가 몰린 PR은 대부분 올리고 머지하기까지 반나절이 넘게 걸렸다.

| PR | 사람이 단 코멘트 | 올리고 머지하기까지 |
| --- | --- | --- |
| 회원가입 | 52개 | 약 17시간 |
| 로그인과 로그아웃 | 27개 | 약 16시간 30분 |
| 닉네임 변경 | 16개 | 약 18시간 |
| 비밀번호 변경 | 18개 | 약 17시간 |
| 할 일에 일정 추가 | 22개 | 약 4시간 |
| 준비 목록 항목을 체크리스트에 담기 | 14개 | 약 17시간 30분 |

9월 초까지 라티가 단 리뷰 코멘트는 86개, 내가 단 것은 78개, 보예가 단 것은 68개였다. 7~8주차에는 리뷰에 정말 시간을 많이 썼다.

코드에도 리뷰에도 AI를 썼는데 생산성은 생각보다 막 늘지 않았던 것 같다. 서로 생각이 너무 달랐다.

AI를 쓰더라도 객체지향이나 의존 방향을 사람이 고민하지 않으면 그냥 절차지향스럽게 필요한 걸 가져다 쓰는 식으로 개발하게 된다고 느꼈다. 그렇게 가면 금방 꼬이고 읽기 힘든 코드가 될 것 같았다.

### 술술 읽히는 코드와 다 뜯어봐야 하는 코드

보예가 올린 내 정보 조회 PR을 리뷰하다가 현재 사용자를 찾는 코드를 봤다.

```java
// UserService: 현재 사용자 조회
private User findCurrentUser(Long currentUserId) {
    try {
        return userRepository.findById(currentUserId);
    } catch (BusinessException exception) {
        if (exception.clientError() != ClientError.USER_NOT_FOUND) {
            throw exception;
        }
        throw authenticationRequired(currentUserId);
    }
}
```

처음 봤을 때는 왜 이렇게 안 읽히지 싶었다. 사용자가 없을 때 어떤 예외가 나가는지 알려면 저장소가 어떤 예외를 던지는지 열어 보고 서비스가 그 예외를 다시 잡아 바꾸는 것까지 따라가야 했다. 서비스에 `try-catch`까지 들어가 있어서 너무 뚱뚱하고 읽기 어렵게 느껴졌다.

좀 이따 라티가 짠 일정 완료 코드를 읽었을 때는 달랐다.

```java
// AppointmentService: 일정 완료
public AppointmentCompletionResult changeStatus(AppointmentCompletionCommand command) {
    Appointment appointment = appointmentRepository.findById(command.appointmentId());
    checklistService.validateItemOwnership(appointment.checklistItemId(), command.userId());

    Appointment changed = appointment.changeCompletion(command.isDone());
    Appointment saved = appointmentRepository.save(changed);

    boolean checklistItemDone = checklistService.changeItemStatusByAppointment(
            command.userId(),
            saved.checklistItemId(),
            saved.isDone()
    );

    return AppointmentCompletionResult.from(saved, checklistItemDone);
}
```

아, 이렇게 구현했구나 하고 바로 이해했다. 메서드 이름만 따라가도 일정을 찾아 주인을 확인한 뒤 완료로 바꾸고 할 일 상태까지 맞춘다는 게 보였다. 완료로 바꾸는 일은 일정 객체가 맡고 서비스는 객체들이 협력하는 순서만 보여 주고 있었다. 그전까지 클래스를 나누고 객체끼리 협력하게 하는 건 이론으로 배운 것에 가까웠는데 이 코드를 보고 실제 애플리케이션에서도 쓸 수 있겠다는 생각이 들었다.

두 코드의 차이는 한 동작을 이해하려고 따라가야 하는 곳의 수에 가까웠다. 일정 완료 코드는 `validateItemOwnership`, `changeCompletion`처럼 메서드 이름이 하는 일을 말해 줘서 그 안을 열어 보지 않아도 흐름이 읽혔다. 현재 사용자 조회 코드는 메서드 이름만으로는 사용자가 없을 때 무슨 일이 일어나는지 알 수 없어서 저장소가 던지는 예외와 서비스가 그 예외를 바꾸는 조건, 바뀐 예외가 어떤 응답으로 나가는지까지 하나씩 열어 봐야 했다. 그래서 보예 PR에는 예외는 저장소가 맡고 서비스는 흐름만 보이게 하자고 리뷰를 달았다.

### 할 일과 일정은 나누되 패키지는 합쳤다

할 일과 일정은 처음에 다른 도메인으로 나눴지만 개발하면서 서로를 부르는 일이 계속 생겼다. 일정을 만들거나 바꿀 때는 그 할 일이 사용자 것인지 확인해야 했고 할 일을 완료하면 그 할 일에 붙은 일정도 모두 완료해야 했다.

```text
appointment.service.AppointmentService ──▶ checklist.service.ChecklistService
checklist.service.ChecklistService     ──▶ appointment.service.ChecklistAppointmentService
```

일정 코드를 건드리면서 이 의존을 처음 만났을 때는 헷갈렸다. 생명주기가 다르면 다른 도메인으로 나누는 게 맞을 텐데 한 API에서 두 도메인의 작업이 이어지니 의존이 양쪽으로 생겼다. 원래 정답이 없는 건지 아니면 내가 모르는 건지도 알 수 없었다. 협력할 때마다 이렇게 해야 하나 싶어서 둘의 협력만 맡는 서비스를 따로 두기도 했지만 서로를 부르는 구조는 그대로였다.

그러다 패키지 사이의 순환 참조를 테스트로 막는 ArchUnit 규칙을 넣으려고 하자 일정과 체크리스트가 걸렸다.

```java
static final ArchRule feature_패키지_사이에는_순환참조가_없다 = slices()
        .matching("com.bibbidi.wedding.(*)..")
        .should().beFreeOfCycles();
```

이 문제로 라티와 오래 이야기했다. 라티는 둘의 생명주기가 다르다고 봤다. 나는 그래도 일정이 체크리스트에 종속된 것 아닌지, 생명주기가 달라도 이렇게 협력해야 하면 어떻게 해야 하는지 계속 물었는데 꼬여 있는 걸 어떻게 풀지 감도 오지 않았다.

이야기를 많이 한 끝에 할 일과 일정의 생명주기와 변경의 이유가 다른 건 맞지만 둘이 강결합된 도메인인 것도 맞다는 데 의견이 모였다. 그래서 도메인은 나눠 둔 채 패키지만 붙이기로 했고 라티가 일정을 체크리스트 패키지 안으로 옮겼다.

```text
checklist
├── domain/ChecklistItem, Appointment   (도메인 클래스는 따로)
└── service/ChecklistService, AppointmentService, ChecklistAppointmentService
```

서비스끼리 부르는 구조는 그대로였지만 같은 패키지 안의 일이 되면서 순환 참조 규칙에서도 빠졌다. 처음에 생명주기와 변경의 이유가 달라서 나눴던 두 도메인을 이번에는 강결합이라서 한 패키지로 붙였다. 논리로만 보면 나누는 게 맞아 보였는데 실제로 해 보니 강결합된 도메인에는 어쩔 수 없는 타협도 있어야 했다. 그때도 도메인을 어떻게 나눠야 하는지 다 알지는 못했지만 나누는 기준만큼이나 그 타협도 필요하다는 건 이때 배웠다.

### 준비 목록을 9단계로 줄이고 첫 배포를 했다

개발을 시작한 주 금요일에 밍글링 데이가 있었다. 팀마다 고른 기술과 그 이유, 아직 풀지 못한 고민을 들고 와서 코치들에게 피드백을 받는 자리였다. 그때까지 우리는 배포를 한 번도 하지 못했는데 코치들은 빨리 배포하라고 피드백을 줬다. 그래서 주말까지 준비 목록의 단계를 정리해서 배포하기로 했다.

6주차에 정리한 준비 목록은 카테고리마다 백본(카테고리 안에서 준비 순서를 이루는 큰 묶음) 수가 달랐다. 웨딩홀은 13개, 스드메(스튜디오·드레스·메이크업)는 15개, 초대는 10개였는데 화면에 그대로 보여 주기에는 단계가 너무 많았다. 백본이 9개를 넘는 카테고리는 비슷한 것끼리 묶어 9단계로 맞췄고 가족은 7개라 그대로 뒀다.

![웨딩홀 백본 13개를 9단계로 묶은 모습](/images/wooteco/level3-week7-8/dev-backbone-9steps-wedding-hall.png)

![초대 백본 10개를 9단계로 묶은 모습](/images/wooteco/level3-week7-8/dev-backbone-9steps-invite.png)

주말까지 카테고리 5개, 단계 43개, 항목 169개로 정리를 마쳤고 월요일에 로그인 없이 준비 목록을 볼 수 있는 화면을 www.bibbidi.kr에 올렸다. 개발을 시작하고 일주일 만에 한 첫 배포였다.

### 마치며

AI가 있으니 코드는 금방 짤 거라고 생각하고 시작했지만 생산성은 생각만큼 늘지 않았고 서로 읽히는 코드로 맞추는 일은 사람이 리뷰하고 이야기하면서 해야 했다. 그 과정에서 바로 읽히는 코드가 어떤 모양인지는 조금 보였다. 객체가 자기 일을 맡고 서비스는 협력하는 순서만 보여 주면 메서드 이름만 따라가도 흐름이 읽혔다.

도메인을 어떻게 나눠야 하는지는 아직 잘 모르겠다. 생명주기와 변경의 이유로 나누는 기준은 알게 됐지만 할 일과 일정처럼 강결합된 도메인에서는 타협해야 할 때도 있었다. 어디까지 나누고 어디서 타협할지는 더 짜 보면서 알아가야 할 것 같다.

### 참고

- 레벨3 6주차 회고: https://johnprk.github.io/woowacourse/level3-week6/
- 회원가입 PR: https://github.com/woowacourse-teams/2026-bibbidi/pull/10
- 로그인 PR: https://github.com/woowacourse-teams/2026-bibbidi/pull/16
- 내 정보 조회 PR: https://github.com/woowacourse-teams/2026-bibbidi/pull/88
- 일정과 체크리스트 패키지 통합 PR: https://github.com/woowacourse-teams/2026-bibbidi/pull/94
- 비비디: https://www.bibbidi.kr
