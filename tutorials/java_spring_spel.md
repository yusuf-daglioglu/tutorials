# JAVA SPRING SPEL

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 spEL (⟷ Spring Expression Language)

Bean property atamalarına örnekler verelim:

örnek:

```xml
<property name="message" value="Available thread number is #{100+1}"/>
```

yada

```java
@Value("Available thread number is #{100+1}")
private String message;
```

Diğer örnekler:

```java
@Value("#{1 == 1}") // true
private boolean equal;

@Value("#{1 eq 1}") // true
private boolean equalAlphabetic;

@Value("#{400 > 300 || 150 < 100}") // true
private boolean or;

@Value("#{not true}") // false
private boolean notAlphabetic;

@Value("#{'valid alphabetic string' matches '[a-zA-Z\\s]+' }") // true
private boolean validAlphabeticStringResult;
```

`string`'deki kod'u çalıştırmak için önce parse edilmesi lazım. bu parse işlemini `spEL` üstleniyor.

```java
import org.springframework.expression.Expression;
import org.springframework.expression.ExpressionParser;
import org.springframework.expression.spel.standard.SpelExpressionParser;

ExpressionParser parser = new SpelExpressionParser()

Expression expression = expressionParser.parseExpression("'Any string'.replace(\" \", \"\").length()");

Integer result = (Integer) expression.getValue();
```

`parseExpression` metodunun aldığı `string`'in içindeki syntax kendine has bir dildir.