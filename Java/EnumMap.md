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
```
