# 목차

[📌중요한 점](#📌중요한-점)

[📗배운 점 ](#📗배운-점)

[🤔궁금한 점](#🤔궁금한-점)

---

<br>

# 📌중요한 점

# 브라우저 렌더링 과정

1. HTML, CSS, JavaScript, 이미지, 폰트 파일 등 렌더링에 필요한 리소스를 요청하고 서버로부터 응답 받는다.
2. 브라우저의 렌더링 엔진은 서버에서 받은 HTML, CSS를 파싱해 DOM과 CSSOM을 생성하고, 이를 결합하여 렌더 트리를 생성한다.
3. 브라우저의 자바스크립트 엔진은 서버에서 받은 자바스크립트를 파싱하여 추상적 구문 트리(AST, Abstract Syntax Tree)를 생성하고 바이트코드로 변환하여 실행한다.

   이때, 자바스크립트는 DOM API를 통해 DOM, CSSOM을 변경할 수 있다.

   변경된 DOM, CSSOM은 다시 렌더 트리로 결합된다.

4. 렌더 트리를 기반으로 HTML 요소의 레이아웃(위치/크기)를 계산하고 브라우저 화면에 요소를 페인팅한다.

<br>

# 요청과 응답

- 브라우저의 핵심 기능은 필요한 리소스(HTML, CSS, JS, 이미지, 파일 등)를 서버에 요청하고 서버로부터 응답 받아 브라우저에 시각적으로 렌더링하는 것이다.
- 즉, 렌더링에 필요한 리소스는 모두 서버에 존재하므로 필요한 리소스를 서버에 요청하고 서버가 응답한 리소스를 파싱하여 렌더링하는 것이다.

예를 들어,

1. 브라우저의 주소창에 URL을 입력하면 호스트 이름이 DNS를 통해 IP 주소로 변환되고 이 IP 주소를 갖는 서버에게 요청을 전송한다. 요청에는 명확히 리소스를 요청하는 내용이 없지만 일반적으로 서버는 루트 요청에 대해 암묵적으로 index.html을 응답하도록 기본 설정되어 있다.

2. 서버는 루트 요청에 의해 서버의 루트 폴더에 존재하는 정적 파일 index.html을 클라이언트(브라우저)로 응답한다.

3. 만약 index.html이 아닌 다른 정적 파일을 서버에 요청하려면 요청할 정적 파일의 경로와 파일 이름을 URL의 호스트의 패스에 기술하여 서버에 요청한다.

<br>

# HTTP 1.1과 HTTP2.0

## HTTP 1.1

- 기본적으로 커넥션 당 하나의 요청과 응답만 처리하며 응답도 마찬가지다.
- 즉, CSS 파일을 로드하는 link 태그, 이미지 파일을 로드하는 img 태그, 자바스크립트를 로드하는 script 태그 등에 의한 리소스 요청과 응답이 개별적으로 이루어진다.
- 동시 전송이 불가능한 구조이므로 요청할 리소스의 개수에 비례하여 응답 시간도 증가하는 단점이 있다.

## HTTP 2.0

- 커넥션당 여러 개의 요청과 응답, 즉 다중 요청/응답이 가능하다.
- 리소스의 동시 전송이 가능하므로 HTTP/1.1에 비해 페이지 로드 속도가 약 50% 정도 빠르다고 알려져 있다.

<br>

# HTML 파싱과 DOM 생성

순수한 텍스트인 HTML 문서를 브라우저에 시각적인 픽셀로 렌더링하려면 HTML 문서를 브라우저가 이해할 수 있는 자료구조(객체)로 변환하여 메모리에 저장해야 한다.

브라우저의 렌더링 엔진은 응답받은 HTML 문서를 파싱하여 브라우저가 이해할 수 있는 자료구조인 **DOM**을 생성한다.

1.  서버에 존재하던 HTML 파일이 브라우저의 요청에 의해 응답된다. 이 때 서버는 브라우저가 요청한 HTML 파일을 읽어들여 메모리에 저장한 다음 메모리에 저장된 바이트(2진수)를 인터넷을 경유하여 응답한다.

2.  브라우저는 서버가 응답한 HTML 문서를 바이트(2진수) 형태로 응답받는다. 응답된 바이트 형태의 HTML 문서는 meta 태그의 charset 어트리뷰트에 의해 지정된 인코딩 방식을 기준(UTF-8)으로 문자열로 변환되는데 브라우저는 이를 확인하고 문자열로 변환한다.

3.  문자열로 변환된 HTML 문서를 읽어 들여 문법적 의미를 갖는 코드의 최소 단위인 토큰들로 분해한다.

4.  각 토큰들을 객체로 변환하여 노드를 생성한다. 토큰의 내용에 따라 문서 노드, 요소 노드, 어트리뷰트 노드, 텍스트 노드가 생성된다. 노드는 이후 DOM을 구성하는 기본 요소가 된다.

5.  HTML 문서는 HTML 요소들의 집합으로 이루어지며 HTML요소는 중첩 관계를 갖는다. HTML 요소 간의 부자관계를 반영하여 모든 노드들을 트리 자료구조로 구성한다. 이를 DOM이라고 한다.

`즉, DOM은 HTML문서를 파싱한 결과물이다.`

<br>

# 자바스크립트의 파싱과 실행

HTML 문서를 파싱한 결과물로서 생성된 DOM은 HTML 문서의 구조와 정보뿐만 아니라 HTML 요소와 스타일 등을 변경할 수 있는 프로그래밍 인터페이스로서 DOM API를 제공한다.
즉, 자바스크립트 코드에서 DOM API를 사용하면 이미 생성된 DOM을 동적으로 조작할 수 있다.

렌더링 엔진은 HTML을 순차적으로 파싱하여 DOM을 생성하다 script 태그나 자바스크립트 코드를 만나면 DOM 생성을 일시 중단하고 자바스크립트 엔진에게 제어권을 넘긴다. 이후 자바스크립트 파싱이 종료되면 다시 HTML을 파싱한다.

자바스크립트의 파싱과 실행은 CPU가 이해할 수 있는 저수준 언어로 변환하여 자바스크립트 엔진이 처리한다. (V8, SpiderMonKey, JavaScriptCore 등등..)

제어권을 받은 자바스크립트 엔진은 자바스크립트 코드를 파싱하기 시작한다.
자바스크립트 엔진은 자바스크립트를 해석하여 AST(추상적 구문 트리)를 생성한다. 그리고 AST를 기반으로 인터프리터가 실행할 수 있는 중간 코드인 바이트코드를 생성하여 실행한다.

<br>

# script 태그의 async/defer 어트리뷰트

자바스크립트 파싱에 의한 DOM 생성이 중단되는 문제를 해결하기 위한 방법으로 `async/defer `속성을 사용하는 방법이 있다.

**async/defer 속성은 HTML 파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 동시에 진행되지만 자바스크립트의 실행 시점에 차이가 있다.**

### async

- 자바스크립트 파싱과 실행은 자바스크립트 파일의 로드가 완료된 직후 진행되며, 이때 HTML 파싱이 중단된다.
- 순서와 상관없이 로드가 완료된 자바스크립트 먼저 실행되므로 순서 보장이 필요할 때는 해당 속성을 지정하지 않는다.

### defer

- HTML 파싱이 완료되는 시점에 자바스크립트 파싱 및 실행을 수행한다.
