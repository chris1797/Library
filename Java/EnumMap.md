# EnumMap 구현

```java
public interface CommonCode {

    @Getter
    enum StatusCode implements EnumModel {

        ACTIVE("활성"),
        STOP("중지"),
        EXPIRED("만료"),
        DELETED("삭제");

        private final String label;

        StatusCode(String value) { this.label = value; }
        @Override public String getKey() { return this.name(); }
        @Override public String getValue() { return this.label; }
    }
}



public interface EnumModel {
    String getKey();
    String getValue();
}

```
<br/>

Reflections 로 @Enum이 붙은 클래스들을 찾아 작업
```java
Reflections reflections = new Reflections(enumPath);
Set<Class<?>> set = reflections.getTypesAnnotatedWith(Enum.class);
StringBuilder stringBuilder = new StringBuilder();
File filePath = "~~"

for (Class<?> clazz : set) {
    for (Class<?> innerClazz : clazz.getDeclaredClasses()) {
        stringBuilder.append(~~~);
    }
}

```
