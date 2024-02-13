단일 책임 원칙(Single Responsibility Principle, SRP)은 클래스나 모듈이 하나의 책임만을 가져야 한다는 원칙입니다. 이것은 클래스나 모듈이 한 가지 일을 잘하도록 유지하고, 코드의 응집성을 높이며 유지보수를 쉽게 만듭니다.


```java
// 잘못된 예시: 단일 책임 원칙을 위반
class BlogPost {
    private String title;
    private String content;

    public BlogPost(String title, String content) {
        this.title = title;
        this.content = content;
    }

    public void saveToDatabase() {
        // 데이터베이스에 게시물을 저장하는 로직
    }

    public void print() {
        // 게시물을 화면에 출력하는 로직
    }
}

```

```java
// 개선된 예시: 단일 책임 원칙 준수
class BlogPost {
    private String title;
    private String content;

    public BlogPost(String title, String content) {
        this.title = title;
        this.content = content;
    }

    // 게시물을 데이터베이스에 저장하는 책임을 가진 클래스
    public class BlogPostSaver {
        public void saveToDatabase(BlogPost blogPost) {
            // 데이터베이스에 게시물을 저장하는 로직
        }
    }

    // 게시물을 화면에 출력하는 책임을 가진 클래스
    public class BlogPostPrinter {
        public void print(BlogPost blogPost) {
            // 게시물을 화면에 출력하는 로직
        }
    }
}

```