RESTful API의 자격을 갖추려면 단순히 HTTP 메서드와 엔드포인트의 설계를 넘어 특정 아키텍처 제약 조건과 원칙을 준수해야 합니다. 이러한 원칙은 API가 확장 가능하고 유연하며 유지 관리가 용이하도록 보장합니다. 이러한 원칙을 기술적인 세부 사항과 비유를 통해 살펴봄으로써 개념을 보다 쉽게 이해할 수 있도록 하겠습니다.

### 1. 클라이언트-서버 아키텍처

**기술적 세부 사항**: RESTful API는 클라이언트-서버 아키텍처를 따라야 하며, 클라이언트(사용자 인터페이스) 문제와 서버(데이터 저장소) 문제를 분리해야 합니다. 이러한 분리는 양쪽이 독립적으로 발전할 수 있도록 하여 확장성과 유연성을 향상시킵니다.

**비유**: 식당을 생각해 보세요. 식사 공간(클라이언트)은 주방(서버)과 분리되어 있습니다. 고객(사용자)은 메뉴 및 웨이터(클라이언트 인터페이스)와 상호 작용하며 주문을 하면 주방(서버)이 이를 처리합니다. 이러한 분리를 통해 주방은 식사 경험을 방해하지 않고도 프로세스나 메뉴 제공을 업데이트할 수 있습니다.

### 2. Statelessness

**기술적 세부 사항**: 클라이언트가 서버로 보내는 각 요청에는 서버가 요청을 처리하는 데 필요한 모든 정보(인증 및 권한 부여 정보 포함)가 포함되어야 합니다. 클라이언트는 자체 상태를 유지할 수 있지만 서버는 요청 사이에 클라이언트 상태를 저장하지 않습니다.

**비유**: 매일 방문하는 카페에서 커피를 구매한다고 상상해 보세요. 단골이라고 해도 주문할 때마다 주문 내용을 완전히 명시해야 합니다(예: "라지 카푸치노 한 잔 주세요"). 바리스타는 어제의 주문을 기억하지 못하며, 각 주문은 독립적으로 처리됩니다.

### 3. Cacheability

**기술적 세부사항**: 클라이언트가 오래되거나 부적절한 데이터를 재사용하지 못하도록 서버의 응답은 캐시 가능 또는 캐시 불가능으로 명시적으로 레이블이 지정되어야 합니다. 이 메커니즘은 서버의 부하를 줄이고 응답 시간을 단축하여 효율성과 사용자 경험을 향상시킵니다.

**예시**: 슈퍼마켓에서는 제품에 '유통기한' 라벨을 사용합니다. 고객은 이러한 라벨을 통해 제품이 여전히 소비해도 좋은지 여부를 알 수 있습니다. 이와 유사하게 캐시 가능한 응답은 매번 스토어(서버)로 돌아가지 않고도 식료품 저장실에 보관하고 사용할 수 있는 품목과 같습니다.

### 4. 통일된 인터페이스

**기술적 세부사항**: REST API는 각 부분이 독립적으로 발전할 수 있도록 아키텍처를 단순화 및 분리하여 통일된 인터페이스를 제공해야 합니다. 여기에는 표준화된 HTTP 메서드(GET, POST, PUT, PATCH, DELETE), 리소스 기반 URL, 데이터 표현을 위한 미디어 유형, 상태 비저장 통신의 사용이 포함됩니다.

**비유**: 플러그 소켓과 가전제품의 개념을 생각해 보세요. 기기(램프, 토스터기, TV)에 관계없이 모두 동일한 유형의 플러그를 사용하여 전기 시스템(API)과 인터페이스합니다. 이러한 표준화를 통해 소켓이나 플러그를 변경할 필요 없이 다양한 가전제품을 사용할 수 있습니다.

### 5. 레이어드 시스템

**기술적 세부 사항**: RESTful API는 계층화된 서버(보안 계층, 로드 밸런서 등)로 구성될 수 있습니다. 각 계층은 특정 문제를 해결하며, 클라이언트는 상호 작용하는 바로 앞의 계층을 넘어서는 어떤 것도 인지할 필요가 없습니다.

**비유**: 우편 시스템을 통해 편지를 보내려면 여러 계층(지역 우체국, 분류 센터, 도착 우체국)을 거쳐야 하지만 발신자는 최초 우체국과만 상호 작용하고 수신자는 최종 배달만 확인할 수 있습니다. 복잡성과 관련된 계층이 양쪽 끝에서 추상화됩니다.

### 6. 코드 온디맨드(선택 사항)

**기술적 세부 사항**: REST는 클라이언트 측에서 애플리케이션 로직의 일부를 제공하는 애플릿 또는 스크립트 형태의 코드를 다운로드하여 실행할 수 있도록 합니다. 이는 REST의 유일한 선택적 제약 조건이며 클라이언트 기능을 확장할 수 있습니다.

**아날로그**: 이것은 미니 툴킷이 포함된 플랫팩 가구를 구입하는 것과 같습니다. 기본 구조(가구)는 있지만 툴킷(주문형 코드)을 통해 집에서 더 쉽게 사용자 지정하거나 조립할 수 있습니다.

### 이해도 테스트

API를 "RESTful"하게 만드는 요소의 본질을 잘 이해했는지 확인하려면 다음 질문을 고려해 보세요:

1. 상태 비저장 원칙이 클라이언트가 서버와 상호 작용하는 방식에 어떤 영향을 미치는가?
2. RESTful API에 통일된 인터페이스가 중요한 이유는 무엇인가요?
3. 계층화된 시스템의 개념을 설명하는 실제 사례를 생각해 볼 수 있나요?

이러한 원칙을 이해하는 것은 RESTful API를 효과적으로 설계, 구현 및 상호 작용하는 데 매우 중요합니다.
