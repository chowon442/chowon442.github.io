---
title: Writing a New Post
author: chowon
date: 2024-06-04
---

## 게시물 작성 방법

### 글 작성 위치 및 조건
Jekyll 블로그는 `_posts` 디렉토리 안에 작성된 파일들을 모두 게시물로 인식한다. \\
단, 파일명을 `YYYY-MM-DD-PAGENAME`과 같은 형식으로 생성해야 한다. ex) `2024-06-04-mypage.md`

### 머리말 - 파일 상단에 위치한 페이지 정보
머리말에는 해당 게시글의 제목, 작성자, 작성 날짜, 게시물에 포함될 카테고리와 태그 등을 기재하게 된다.

```markdown
---
title: Writing a New Post
author: chowon442
date: 2024-06-04
---
```
<u>속성별로 알아보기</u>
- `title` :: 게시물의 제목을 의미한다.
- `author` :: 작성자의 정보를 입력한다.
- `date` :: 게시물이 작성된 일짜와 시간
    - `YYYY-MM-DD`는 `2022-02-18`과 같은 양식을 의미한다.

```python
# code syntax test
import time
time.sleep(10)

def say_hello():
    print("hi")

say_hello()
```