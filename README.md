# Spring_ch2_guestbook

## 엔티티-관계 다이어그램(E-R Diagram)

## 엔티티-관계 모델(E-R Mode)

**개체 관계 다이어그램**

- 데이터 모델링 단계에서 개념적 설계 과정에서 주로 사용하는 모델
- 데이터베이스를 구성하는 **엔티티(Entitiy) 타입**과 **관계(Relationship) 타입 간의 구조** 또는 엔티티를 구성하는 **속성(Attribute) 등을 약속된 기호를 이용하여 표현함**으로서, 데이터베이스의 전반적인 구조를 이해하기 쉽도록 표현한 모델

### 엔티티

- 사람, 장소, 사물 등과 같이 **독립적으로 존재하면서 고유하게 식별이 가능한 실세계의 객체\**
- (그려와야함)

### 엔티티 관계도 확인하기

DBeaver → 원하는 테이블 선택 후 엔티티 관계도 선택

![Untitled]![Untitled (1)](https://user-images.githubusercontent.com/96537605/181761274-eae4f4b5-8795-4711-87a9-c6fe98d65fd0.png)
(https://s3-us-west-2.amazonaws.com/secure.notion-static.com/70ac418c-84d3-4c0a-a65b-f39001f6e13b/Untitled.png)

[[DBeaver]ERD 확인하기 : 네이버 블로그 (naver.com)](https://blog.naver.com/PostView.nhn?blogId=hj_kim97&logNo=222230861879&parentCategoryNo=&categoryNo=15&viewDate=&isShowPopularPosts=false&from=postView)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0e7d807a-7744-46cb-90f0-a4623dd839f6/Untitled.png)
![Untitled (2)](https://user-images.githubusercontent.com/96537605/181761234-ecde2dea-9f84-4d44-8cdd-51faae0ce690.png)


----


# 디자인 변경 - 색상 추가

- style을 이용하여 **색상을 변경**해줬다
- page 제목도 변경시켰다

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3662e17d-391a-4600-a13f-97133c70bd19/Untitled.png)
![Untitled](https://user-images.githubusercontent.com/96537605/181761171-f9f53587-b19d-4fec-b38b-e4e476762a51.png)


1. **list.html**

```java
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">

<th:block th:replace="~{/layout/basic :: setContent(~{this::content} )}" >

  <th:block th:fragment="content" >

    <h1 class ="mt-4" style="background: #80bdff">오류그만떠Page
      <span>
        <a th:href="@{/guestbook/register}">
          <button type="button" class="btn btn-outline-primary" style="background: #ffffa8">REGISTER
          </button>
        </a>
      </span>
    </h1>

    <form action="/guestbook/list" method="get" id="searchForm">
      <div class="input-group" style="background: #ffa4a4">
        <input type="hidden" name="page" value = "1">
        <div class="input-group-prepend" style="background: #ffb86c">
          <select class="custom-select" name="type" style="background: #ffffcc">
            <option th:selected="${pageRequestDTO.type == null}">-------</option>
            <option value="t" th:selected="${pageRequestDTO.type =='t'}" >제목</option>
            <option value="c" th:selected="${pageRequestDTO.type =='c'}"  >내용</option>
            <option value="w"  th:selected="${pageRequestDTO.type =='w'}" >작성자</option>
            <option value="tc"  th:selected="${pageRequestDTO.type =='tc'}" >제목 + 내용</option>
            <option value="tcw"  th:selected="${pageRequestDTO.type =='tcw'}" >제목 + 내용 + 작성자</option>
          </select>
        </div>
        <input class="form-control" name="keyword" th:value="${pageRequestDTO.keyword}" style="background: #d0ffbb">
        <div class="input-group-append" id="button-addon4" style="background: #97c7ff">
          <button class="btn btn-outline-secondary btn-search" type="button" style="background: #5eabff" >Search</button>
          <button class="btn btn-outline-secondary btn-clear" type="button" style="background: #efcbff">Clear</button>
        </div>
      </div>
    </form>

    <table class="table table-striped" style="background: #bbdbff">
      <thead>
      <tr>
        <th scope="col" style="background: #ff7474">#</th>
        <th scope="col" style="background: #ffa445">Title </th>
        <th scope="col" style="background: #fff64a">Writer </th>
        <th scope="col" style="background: #7fff6b">Regdate</th>
      </tr>
      </thead>
      <tbody>

      <tr th:each="dto : ${result.dtoList}">
        <th scope="row">
          <a th:href="@{/guestbook/read(gno = ${dto.gno},
                    page= ${result.page},
                    type=${pageRequestDTO.type} ,
                    keyword = ${pageRequestDTO.keyword})}">
            [[${dto.gno}]]
          </a>
        </th>
        <td style="background: #aae8ff"> [[${dto.title}]] </td>
        <td style="background: #a6a0ff"> [[${dto.writer}]] </td>
        <td style="background: #e7acff"> [[${#temporals.format(dto.regDate, 'yyyy/MM/dd')}]] </td>
      </tr>

      </tbody>
    </table>

    <ul class="pagination h-100 justify-content-center align-items-center">

      <li class="page-item " th:if="${result.prev}">
        <a class="page-link" th:href="@{/guestbook/list(page= ${result.start -1},
                    type=${pageRequestDTO.type} ,
                    keyword = ${pageRequestDTO.keyword} ) }" tabindex="-1">Previous</a>
      </li>

      <li th:class=" 'page-item ' + ${result.page == page?'active':''} " th:each="page: ${result.pageList}">
        <a class="page-link" th:href="@{/guestbook/list(page = ${page} ,
                   type=${pageRequestDTO.type} ,
                   keyword = ${pageRequestDTO.keyword}  )}">
          [[${page}]]
        </a>
      </li>

      <li class="page-item" th:if="${result.next}">
        <a class="page-link" th:href="@{/guestbook/list(page= ${result.end + 1} ,
                    type=${pageRequestDTO.type} ,
                    keyword = ${pageRequestDTO.keyword} )}">Next</a>
      </li>
    </ul>

    <div class="modal" tabindex="-1" role="dialog">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Modal title</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <p>Modal body text goes here.</p>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary">Save changes</button>
          </div>
        </div>
      </div>
    </div>

    <script th:inline="javascript">

      var msg = [[${msg}]];

      console.log(msg);

      if(msg){
        $(".modal").modal();
      }
      var searchForm = $("#searchForm");

      $('.btn-search').click(function(e){

        searchForm.submit();

      });

      $('.btn-clear').click(function(e){

        searchForm.empty().submit();

      });

    </script>
  </th:block>

</th:block>
```
