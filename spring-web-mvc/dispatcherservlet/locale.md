---
description: 이 페이지에서는 Locale Resolver에 대해 설명합니다.
---

# Locale

Locale Resolver는 어플리케이션에 다국어 처리가 필요한 경우 사용할 수 있습니다.&#x20;

Spring Web MVC 에서는 3가지의 기본 Resolver 를 제공하고 있습니다.&#x20;

1. Header Resolver Header Resolver는 클라이언트로 부터 전송된 accept-language 를 사용하여 Locale 를 설정합니다. 일반적으로 accept-language 는 클라이언트의 운영체제 Locale과 같습니다.&#x20;
2. Cookie Resolver Cookie Resolver는 쿠키를 통해 Locale를 설정하거나, 설정된 Locale 를 가져올 수 있다.&#x20;
3. Session Resolver Session Resolver는 세션을 통해 Locale를 설정하거나, 설정된 Locale 를 가져올 수 있다.&#x20;

Spring Web MVC에서는 LocaleContextResolver를 Spring Bean으로 등록하여 CustomLocaleResolver를 사용할 수 있습니다.&#x20;

다음은 URL Parameter로 Locale를 설정하는 코드입니다.

```java
@Component
public class CustomLocaleResolver extends AbstractLocaleContextResolver {
    @Override
    public LocaleContext resolveLocaleContext(HttpServletRequest request) {
        return new LocaleContext() {
            @Override
            public Locale getLocale() {
                String lang = request.getParameter("lang");
                return Locale.forLanguageTag(lang != null ? lang : "en");
            }
        };
    }
    // .. more code
}
```
