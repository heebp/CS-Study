# View Resolver란?

**뷰 리졸버(View Resolver)**는 Spring MVC 웹 애플리케이션의 중요한 컴포넌트 중 하나로, **MVC 패턴에서 컨트롤러가 처리한 결과를 어떤 뷰로 응답할지 결정**하는 역할을 합니다.

![다운로드 (8)](https://github.com/user-attachments/assets/70659a34-4db5-4cb4-a7b8-40b107f3840f)

---

## View Resolver의 역할

### 1. **뷰 이름 매핑**
컨트롤러는 특정 뷰를 식별하기 위해 **문자열 형태의 뷰 이름**을 반환합니다.  
뷰 리졸버는 이 이름을 실제 뷰 템플릿 파일로 매핑합니다.  
예를 들어, 컨트롤러가 "home"이라는 뷰 이름을 반환하면, 뷰 리졸버는 이를 `home.jsp` 또는 `home.html`로 변환합니다.

### 2. **다양한 뷰 기술 지원**
Spring MVC는 다양한 뷰 기술을 지원합니다.  
이를 위해 여러 **뷰 리졸버**를 제공합니다.  
- JSP, Thymeleaf, FreeMarker 등 다양한 템플릿 엔진을 사용할 수 있습니다.

---

## 주요 View Resolver

| **View Resolver**              | **설명**                                                                                     |
|---------------------------------|---------------------------------------------------------------------------------------------|
| **InternalResourceViewResolver** | JSP와 같은 리소스를 처리하는 가장 일반적인 뷰 리졸버. 접두사와 접미사를 설정하여 경로를 결정.           |
| **UrlBasedViewResolver**         | URL 패턴을 기반으로 뷰를 결정하는 리졸버.                                                    |
| **ResourceBundleViewResolver**   | 뷰 설정 정보를 **프로퍼티 파일**에 저장하고 이를 기반으로 뷰를 결정.                               |
| **XmlViewResolver**              | 뷰 설정 정보를 **XML 파일**에 저장하고 이를 기반으로 뷰를 결정.                                  |
| **BeanNameViewResolver**         | 뷰의 **빈 이름**을 직접 사용하여 뷰를 결정.                                                 |
| **ContentNegotiatingViewResolver** | 클라이언트의 요청 내용(예: Accept 헤더)을 기반으로 적절한 뷰를 선택.                              |

---

## 예시: `UrlBasedViewResolver`

다음은 **UrlBasedViewResolver**를 설정하는 예제 코드입니다:

```java
@Bean
public UrlBasedViewResolver urlBasedViewResolver() {
    UrlBasedViewResolver urlBasedViewResolver = new UrlBasedViewResolver();
    urlBasedViewResolver.setOrder(1);
    urlBasedViewResolver.setViewClass(JstlView.class);
    urlBasedViewResolver.setPrefix("/WEB-INF/jsp/sharon/pj/");
    urlBasedViewResolver.setSuffix(".jsp");
    return urlBasedViewResolver;
}
```
### 주요 설정 설명

1. **`setOrder(1)`**
   - 여러 뷰 리졸버가 설정된 경우 이 순서를 사용하여 어떤 리졸버가 먼저 시도되는지 결정한다. 값이 작을수록 먼저 시도된다.

2. **`setViewClass(JstlView.class)`**
   - 사용할 **뷰 클래스**를 설정합니다. 여기서는 `JstlView` 클래스가 사용되며, JSTL 태그를 지원하는 JSP를 렌더링하는 데 사용됩니다.

3. **`setPrefix("/WEB-INF/jsp/sharon/pj/")`**
   - 반환된 뷰 이름 앞에 추가되는 **접두사**입니다. 예를들어 컨트롤러에서 "home"이라는 뷰 이름을 반환하면, 실제 JSP 파일 경로는 `/WEB-INF/jsp/sharon/pj/home.jsp`가 됩니다.

4. **`setSuffix(".jsp")`**
   - 반환된 뷰 이름 뒤에 추가되는 **접미사**입니다. 접두사와 함께 사용되어 실제 JSP 파일 경로를 결정합니다.

