# 일급 컬렉션(First-Class Collection)

- 일급 컬렉션은 컬렉션인, 예를 들어 List, Set, Map을 감싸 하나의 클래스로 표현한 객체이다. <br/>
- 컬렉션 자체를 감싸는 클래스를 만들어 관련된 로직을 응집시키고, 불필요한 외부 의존성을 줄이는 데 목적이 있으며 이를 통해 코드의 가독성과 유지보수성을 높일 수 있다.

<br/>

일급 컬렉션의 특징
---
> ### 1. 컬렉션 외에 다른 멤버 변수를 가지지 않는다. 
컬렉션 외에 추가적인 상태값을 가지지 않으며, 오직 컬렉션과 관련된 작업만 처리한다.
> ### 2. 비즈니스 로직을 캡슐화한다.
컬렉션과 관련된 로직은 일급 컬렉션 내부에서 처리되므로, 중복 코드를 제거하고 응집도를 높인다.
> ### 3. 불변성을 유지한다.
컬렉션을 수정할 수 없게 만들어 외부에서 컬렉션의 상태를 변경하지 못하도록 한다.
> ### 4. 컬렉션의 검증 책임을 가진다.
컬렉션에 요소를 추가하거나 변경할 때, 일급 컬렉션이 스스로 적절한지 검증한다.

<br/>

일급 컬렉션의 장점
---

> ### 응집도 향상
컬렉션과 관련된 모든 로직이 한곳에 모여 코드가 직관적이고 유지보수가 용이해진다.

> ### 불변성을 통한 안전성
외부에서 컬렉션의 상태를 임의로 변경할 수 없으므로, 사이드 이펙트를 방지할 수 있다.

> ### 도메인 로직 표현력 강화
단순한 자료구조 대신 비즈니스 의도를 담은 클래스로 대체되어 코드의 의도가 명확해진다.

<br/>

예제코드
---

### Java

```java
import java.util.Collections;
import java.util.List;

public class Tags {
    private final List<String> tags;

    public Tags(List<String> tags) {
        validate(tags);
        this.tags = Collections.unmodifiableList(tags);
    }

    // 검증 로직
    private void validate(List<String> tags) {
        if (tags == null || tags.isEmpty()) {
            throw new IllegalArgumentException("Tags cannot be null or empty.");
        }
    }

    // 태그 추가
    public Tags addTag(String tag) {
        if (tag == null || tag.isBlank()) {
            throw new IllegalArgumentException("Tag cannot be null or blank.");
        }
        List<String> newTags = new ArrayList<>(this.tags);
        newTags.add(tag);
        return new Tags(newTags);
    }

    // 태그 조회
    public List<String> getTags() {
        return tags;
    }
}
```

### Kotlin

```Kotlin
class Tags(private val tags: List<String>) {

    init {
        require(tags.isNotEmpty()) { "Tags cannot be empty." }
    }

    // 태그 추가
    fun addTag(tag: String): Tags {
        require(tag.isNotBlank()) { "Tag cannot be blank." }
        return Tags(tags + tag)
    }

    // 태그 조회
    fun getTags(): List<String> = tags
}
```
