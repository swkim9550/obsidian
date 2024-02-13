1. RESTful API 설계 원칙 중 하나는 API가 self-descriptive해야 한다는 것입니다. 이는 API가 사용하는 방법을 명확하게 문서화하고, 자원과 메서드, 상태 코드 등이 의도하는 바를 명확히 전달해야 한다는 의미입니다. 예를 들어, HTTP 메서드 (GET, POST, PUT, DELETE 등)는 자신의 역할을 명확히 나타내며, API 경로와 매개변수도 쉽게 이해되어야 합니다.
    
2. **코드의 Self-Descriptive성**: 좋은 코드는 주석이 거의 없어도 그 자체로 무엇을 하는지 이해할 수 있어야 합니다. 변수명, 함수명, 클래스명 등이 명확하고 직관적이어야 하며, 코드의 구조가 그 기능을 자연스럽게 설명할 수 있어야 합니다.
    

이러한 개념은 개발자에게 명확하고 유지보수하기 쉬운 코드 및 시스템을 작성하는 데 중요합니다. 따라서 면접에서 이 용어에 대한 질문을 받았다면, 이러한 관점에서 답변하는 것이 좋을 것입니다.




Self-descriptive Messages 원칙은 메시지 자체가 충분한 정보를 포함하여, 그 메시지를 이해하고 해석할 수 있어야 한다는 것을 의미합니다. 즉, 메시지는 자신이 어떻게 처리되어야 하는지에 대한 충분한 정보(메타데이터, 미디어 타입, 링크 등)를 포함해야 합니다. 이 원칙을 만족하는 API는 클라이언트가 별도의 문서 없이도 요청을 이해하고 응답을 해석할 수 있도록 합니다.

  
Self-descriptive Messages 원칙의 핵심은 메시지가 스스로를 충분히 설명해야 한다는 것입니다. 이는 메시지가 포함하는 데이터 구조와 의미, 그리고 데이터를 어떻게 처리해야 할지에 대한 정보가 명확해야 함을 의미합니다. 하지만, 이것이 반드시 JSON 응답의 모든 필드에 대한 설명을 포함해야 한다는 것을 의미하지는 않습니다.

JSON 응답 본문에 있는 모든 필드에 대한 설명이 포함되지 않았더라도, 응답이 Self-descriptive 원칙을 만족할 수 있는 조건은 다음과 같습니다:

1. **명확한 필드명:** 필드명이 자명하거나 일반적으로 알려져 있어, 클라이언트 개발자가 필드의 의미를 쉽게 이해할 수 있어야 합니다.
    
2. **표준화된 데이터 포맷:** 예를 들어, 날짜와 시간 필드는 ISO 8601 같은 국제 표준을 따르는 경우, 별도의 설명 없이도 그 형식과 의미를 이해할 수 있습니다.
    
3. **Contextual Information:** 메시지가 링크나 다른 메타데이터를 포함하여, 클라이언트가 메시지의 맥락을 이해할 수 있게 도와줍니다.
    
4. **문서화 및 미디어 타입의 사용:** 때로는 자기 서술적 메시지를 위해, 각 필드에 대한 상세한 설명을 포함한 문서나 미디어 타입 정의를 제공할 수 있습니다. 예를 들어, `application/vnd.mycompany.myapp+json` 같은 커스텀 미디어 타입은 특정 JSON 구조를 나타내며, 이 구조에 대한 문서가 존재한다는 것을 의미할 수 있습니다.
    
5. **Hypermedia Controls:** HATEOAS 원칙과 관련하여, 응답이 다음 가능한 액션에 대한 링크를 포함함으로써, 클라이언트가 API를 탐색하고 상호작용하는 방법을 이해할 수 있게 합니다.
    

따라서, JSON 응답이 모든 필드에 대한 상세한 설명을 포함하지 않더라도, 위와 같은 방법으로 충분한 컨텍스트와 정보를 제공한다면 Self-descriptive Messages 원칙을 만족한다고 볼 수 있습니다. 그러나 이 원칙을 완전히 충족시키기 위해서는 종종 문서화와 충분한 메타데이터가 필수적입니다.


https://www.blog.ecsimsw.com/entry/REST-API-Self-descriptive%EC%99%80-HATEOS-%EB%8C%80%EB%B6%80%EB%B6%84-%EB%AA%BB-%EC%A7%80%ED%82%A4%EA%B3%A0-%EC%9E%88%EB%8A%94-%EC%A0%9C%EC%95%BD%EC%A1%B0%EA%B1%B4


```json
GET /users/123 HTTP/1.1
Host: api.example.com
Accept: application/json
```

```json
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 123

{
    "userId": 123,
    "name": "John Doe",
    "email": "john.doe@example.com"
}
```