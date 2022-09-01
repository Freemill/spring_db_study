:star2: 학습 내용의 출처 : Inflearn, 스프링 DB 1편 :star2:

## 1. JDBC 이해

##### H2 데이터베이스 설정

H2 데이터베이스는 개발이나 테스트 용도로 사용하기 좋은 가볍고 편리한 DB이면서 SQL을 실행할 수 있는 웹 화면을 제공한다.

```sql
# 멤버 테이블을 한번 삭제한다.
drop table member if exists cascade;

# 멤버 테이블을 만든다.
create table member(
	member_id varchar(10),
	money integer not null default 0,
	primary key (member_id)
)

# 데이터를 추가해준다.
insert into member(member_id, money) values ('hi1', 10000);
insert into member(member_id, money) values ('hi2', 20000);
```



##### JDBC 등장 이유

애플리케이션을 개발할 때 중요한 데이터는 대부분 데이터베이스에 보관한다.

##### "클라이언트, 애플리케이션, 서버, DB"

![image](https://user-images.githubusercontent.com/76586084/187840738-164093fb-6e14-4877-9f51-e1eb6ceb7c40.png)

클라이언트가 애플리케이션 서버를 통해 데이터를 저장하거나 조회하면, 애플리케이션 서버는 다음 과정을 통해서 데이터베이스를 사용한다.



##### "애플리케이션 서버와 DB - 일반적인 사용법"

![image](https://user-images.githubusercontent.com/76586084/187841808-8ab57b57-c9a0-46e1-979e-9ff7a68d9d15.png)

1. 커넥션 연결 : 주로 TCP/IP를 사용해서 커넥션을 연결한다.
2. SQL 전달 : 애플리케이션 서버는 DB가 이해할 수 있는 SQL을 연결된 커넥션을 통해 DB에 전달한다.
3. 결과 응답 : DB는 전달된 SQL을 수행하고 극 결과를 응답한다. 애플리케이션 서버는 응답 결과를 활용한다.



##### "애플리케이션 서버와 DB - DB 변경"

![image](https://user-images.githubusercontent.com/76586084/187842238-fa0d84fa-4544-4156-8902-c5e8ede1fd56.png)

문제는 각각의 데이터베이스마다 커넥션을 연결하는 방법, SQL을 전달하는 방법, 그리고 결과를 응답 받는 방법이 모두 다르다는 점이다.



여기에는 2가지 큰 문제가 있다.

1. 데이터베이스를 다른 종류의 데이터베이스로 변경하면 애플리케이션 서버에 개발된 데이터베이스 사용 코드도 함께 변경해야 한다.
2. 개발자가 각각의 데이터베이스마다 커넥션 연결, SQL 전달, 그리고 그 결과를 응답 받는 방법을 새로 학습해야 한다.



##### 이런 문제를 해결하기 위해 JDBC라는 자바 표준이 등장했다



##### JDBC 표준 인터페이스

> JDBC(Java Database Connectivity)는 자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API이다. JDBC는 데이터베이스에서 자료를 쿼리하거나 업데이트 하는 방법을 제공한다.



##### "JDBC 표준 인터페이스"

![image](https://user-images.githubusercontent.com/76586084/187845404-544aec71-31db-4d00-8850-26b08e1cb6a3.png)

대표적으로 다음 3가지 기능을 표준 인터페이스로 정의해서 제공한다.

- ***```java.sql.Connection```*** - 연결
- ***```java.sql.Statement```*** -SQL을 담은 내용
- ***```java.sql.ResultSet```*** - SQL 요청 응답

각각의 DB벤더에서 자신의 DB에 맞도록 구현해서 라이브러리로 제공하는데, 이를 JDBC 드라이버라고 한다. 
ex) MySQL DB에 접근할 수 있는것은 MySQL JDBC 드라이버라고 하고, Oracle DB에 접근할 수 있는 것은 Oracle JDBC 드라이버라고 한다.

##### "Oracle 드라이버 사용"

![image](https://user-images.githubusercontent.com/76586084/187846897-7a59f65c-c98e-41f6-bae5-974458131c19.png)



## JDBC와 최신 데이터 접근 기술

##### "JDBC 직접 사용"

![image](https://user-images.githubusercontent.com/76586084/187847780-baeb8627-baea-48a3-b6ef-8043550f17cd.png)



##### "SQL Mapper"

![image-20220901153912430](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220901153912430.png)

- SQL Mapper
  - 장점 : JDBC를 편리하게 사용하도록 도와준다.
    - SQL 응답 결과를 객체로 편리하게 변환해준다.
    - JDBC 반복 코드를 제거해준다.
  - 단점 : 개발자가 SQL을 직접 작성해야한다.
  - 대표 기술 : 스프링 Jdbc Template, MyBatis



##### "ORM 기술"

![image](https://user-images.githubusercontent.com/76586084/187849244-32905a2c-783d-426b-beeb-70c7505b44ba.png)

- ORM 기술
  - ORM은 객체를 관계형 데이터베이스 테이블과 매핑해주는 기술이다. 이 기술 덕분에 개발자는 반복적인 SQL을 직접 작성하지 않고, ORM 기술이 개발자 대시에 SQL을 동적으로 만들어 실행해준다. 추가로 각각의 데이터베이스마다 다른 SQL을 사용하는 문제도 중간에서 해결해준다.
  - 대표 기술 : JPA, 하이버네이트, 이클립스링크
  - JPA는 자바 진영의 ORM 표준 인터체이스이고, 이것을 구현한 것으로 하이버네이트와 이클립스 링크 등의 구현 기술이 있다.



##### "SQL Mapper vs ORM 기술"

SQL Mapper와 ORM 기술 둘 다 각각 장단점이 있다.
SQL Mapper는 SQL만 직접 작성하면 나머지 번거로운 일은 SQL Mapper가 대신 해결해준다. SQL Mapper는 SQL만 작성할 줄 알면 금방 배워서 사용할 수 있다.

ORM기술은 SQL 자체를 작성하지 않아도 되어사 개발 생산성이 매우 높아진다. 편리한 반면에 쉬운 기술은 아니므로 실무에서 사용하려면 깊이있게 학습해야 한다.



## 데이터베이스 연결

애플리케이션과 데이터베이스를 연겨해보자.



##### "ConnectionConst"

```java
public abstract class ConnectionConst{
    public static final String URL = "jdbc:h2:tcp://localhost/~/test";
    public static final String USERNAME = "sa";
    public static final String PASSWORD = "";
}
```

데이터베이스에 접속하는데 필요한 기본 정보를 편리하게 사용할 수 있도록 상수로 만들었다.



JDBC를 사용해서 실제 데이터베이스에 연결하는 코드를작성해보자.

##### "DBConnectionUtil"

```java
@Slf4j
public class DBConnectionUtil {

    public static Connection getConnection(){
        try {
            Connection connection = DriverManager.getConnection(URL, USERNAME, PASSWORD);
            log.info("get connection = {}, class = {}", connection, connection.getClass());
            return connection;
        } catch (SQLException e) {
            throw new IllegalStateException(e);
        }
    }
}
```



##### "DBConnectionUtilTest"

```java
@Slf4j
public class DBConnectionUtilTest {

    @Test
    void connection(){
        Connection connection = DBConnectionUtil.getConnection();
        Assertions.assertThat(connection).isNotNull();
    }
}
```

##### "실행 결과"

```
16:38:50.418 [Test worker] INFO hello.jdbc.connection.DBConnectionUtil - get connection = conn0: url=jdbc:h2:tcp://localhost/~/test user=SA, class = class org.h2.jdbc.JdbcConnection
```

실행 결과를 보면 ***```class=class org.h2.jdbc.JdbcConnection```*** 부분을 확인할 수 있다. 이것이 바로 H2 데이터베이스 드라이버가 제공하는 H2 전용 커넥션이다. 물론 이 커넥션은 JDBC 표준 커넥션 인터체이스인 ***```jdbc.sql.Connection```*** 인터페이스를 구현하고 있다.



##### JDBC DriverMager 연결 이해

지금까지 코드로 확인해본 과정을 좀 더 자세히 알아보자

##### "JDBC 커넥션 인터페이스와 구현"

![image](https://user-images.githubusercontent.com/76586084/187861781-e1eb983f-28e2-4f42-a029-8ea1b27165f0.png)

- JDBC는 ***```java.sql.Connection```*** 표준 커넥션 인터페이스를 정의한다.
- H2 데이터베이스 드라이버는 JDBC Connection 인터페이스를 구현한 ***```org.h2.jdbc.JdbcConnection```*** 구현체를 제공한다.



##### "DriverManager 커넥션 요청 흐름"

![image-20220901171220956](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220901171220956.png)

JDBC가 제공하는 ***```DriverManger```*** 는 라이브러리에 등록된 DB 드라이버들을 관리하고, 커넥션을 획득하는 기능을 제공한다.



1. 애플리케이션 로직에서 커넥션이 필요하면 ***```DriverManager.getConnection()```*** 을 호출한다.
2. ***```DriverManeger```*** 는 라이브러리에 등록된 드라이버 목록을 자동으로 인식한다. 이 드라이버들에게 순서대로 다음 정보를 넘겨서 커넥션을 획득할 수 있는지 확인한다.
   - URL : 예) ***```jdbc:h2:tcp://localhost/~/test```***-
   - 이름, 비밀번호 등 접속에 필요한 추가 정보
   - 여기서 각각의 드라이버는 자신이 처리할 수 있는 요청인지 확인하고 처리할 수 있는 요청이라면 실제 데이터베이스에 연결해서 커넥션을 획득하고 이 커넥션을 클라이언트에게 반환한다.
   - 이렇게 찾은 커넥션 구현체가 클라이언트에 반환된다.





## JDBC 개발 - 등록

##### "Member"

```java
@Data
public class Member {
    private String memberId;
    private int money;
    
    public Member(){
    }

    public Member(String memberId, int money) {
        this.memberId = memberId;
        this.money = money;
    }
}
```



##### "MemberRepositoryV0"

```java
@Slf4j
public class MemberRepositoryV0 {
    public Member save(Member member) throws SQLException {
        String sql = "insert into member(member_id, money) values(?,?)";

        Connection con = null;
        PreparedStatement pstmt = null;

        try {
            con = getConnection();
            pstmt = con.prepareStatement(sql);
            pstmt.setString(1, member.getMemberId());
            pstmt.setInt(2, member.getMoney());
            pstmt.executeUpdate();
            return member;
        } catch (SQLException e) {
            log.error("db error", e);
            throw e;
        }finally{
            close(con, pstmt, null);
        }
    }

    private void close(Connection con, Statement stmt, ResultSet rs) {

        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {
                log.info("error", e);
            }
        }

        if (stmt != null) {
            try {
                stmt.close();
            } catch (SQLException e) {
                log.info("error", e);
            }
        }

        if (con != null) {
            try {
                con.close();
            } catch (SQLException e) {
                log.info("error", e);
            }

        }


    }

    private Connection getConnection() {
        return DBConnectionUtil.getConnection();
    }

}
```



##### "커넥션 획득"

- ***```getConntion()```*** :  이전에 만들어둔 ***```DBConnectionUtil```*** 를 통해서 데이터베이스 커넥션을 획득한다.
- ***```con.prepareStatement(sql)```***  : 데이터베이스에 전달할 SQL과 파라미터로 전달할 데이터들을 준비한다.
  - ***```sql```*** : ***```insert into member(member_id, money) values(?, ?)```***
  - ***```pstm.setString(1, member.getMemberId())```*** : SQL의 첫번째 ***```?```*** 에 값을 지정한다. 문자이므로 ***```setString```*** 을 사용한다.
  - ***```pstm.setString(1, member.getMemberId())```*** : SQL의 두번쨰 ***```?```*** 에 값을 지정한다. 숫자(Int)이므로 ***```setInt```*** 를 지지정한다.
- ***```pstmt.executeUpdate()```*** : ***```Statement```***  를 통해 준비된 SQL을 커넥션을 통해 실제 데이터베이스에 전달한다. 참고로 ***```exectuteUpdate()```*** 은 ***```int```*** 를 반환하는데 영향받은 DB row 수를 반환한다. 여기서는 하나의 row를 등록했으므로 1을 반환한다.



> ##### "주의"
>
> 리소스 정리는 꼭 해주어야한다. (예외가 발생하든 안하든)



> ##### "참고"
>
> ***```PreparedStatement```*** 는 ***```Statement```*** 의 자식 타입인데, ***```?```*** 를 통한 파라미터 바인딩을 가능하게 해준다.
>
> 참고로 SQL Injection 공격을 예발하려면 ***```PreparedStatement```*** 를 통한 파라미터 바인딩 방식을 사용해야 한다.





##### "MemberRepositoryTest"

```java
public class MemberRepositoryTest {
    MemberRepositoryV0 repository = new MemberRepositoryV0();

    @Test
    void crud() throws SQLException {
        Member member = new Member("memberVO", 10000);
        repository.save(member);
    }
}
```





##### "JDBC 개발 - 조회"



##### "MemberRepositoryV0 - 회원 조회 추가" 

```java
@Slf4j
public class MemberRepositoryV0 {
    public Member findById(String memberId) throws SQLException {
    	String sql = "select * from member where member_id = ?";

        Connection con = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;

        try {
            con = getConnection();
            pstmt = con.prepareStatement(sql);
            pstmt.setString(1, memberId);
            rs = pstmt.executeQuery();

            if (rs.next()) {
                Member member = new Member();
                member.setMemberId(rs.getString("member_id"));
                member.setMoney(rs.getInt("money"));
                return member;
            }else{
                throw new NoSuchElementException("member not found memberId=" + memberId);
            }
        } catch (SQLException e) {
            log.error("db error", e);
            throw e;
        } finally {
            close(con, pstmt, rs);
        }
    }

    private void close(Connection con, Statement stmt, ResultSet rs) {

        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {
                log.info("error", e);
            }
        }

        if (stmt != null) {
            try {
                stmt.close();
            } catch (SQLException e) {
                log.info("error", e);
            }
        }

        if (con != null) {
            try {
                con.close();
            } catch (SQLException e) {
                log.info("error", e);
            }

        }


    }

    private Connection getConnection() {
        return DBConnectionUtil.getConnection();
    }

}
```

##### "MemberRepositoryTest"

```java
@Slf4j
public class MemberRepositoryTest {
    MemberRepositoryV0 repository = new MemberRepositoryV0();

    @Test
    void crud() throws SQLException {
        //save
        Member member = new Member("memberVO", 10000);
        repository.save(member);

        //findById
        Member findMember = repository.findById(member.getMemberId());
        log.info("findMember = {}", findMember);
        Assertions.assertThat(findMember).isEqualTo(member);
    }


}
```





## JDBC 개발 - 수정, 삭제

##### "memberRepositoryV0 - 회원 수정 추가" 

```java
public void update(String memberId, int money) throws SQLException {
        String sql = "update member set money = ? where member_id =?";
        Connection con = null;
        PreparedStatement pstmt = null;

        try {
            con = getConnection();
            pstmt = con.prepareStatement(sql);
            pstmt.setInt(1, money);
            pstmt.setString(2, memberId);
            int resultSize = pstmt.executeUpdate();
            log.info("resultSize={}", resultSize);
        } catch (SQLException e) {
            log.error("db error", e);
            throw e;
        }finally {
            close(con, pstmt, null);
        }
    }
```



##### "memberRepositoryTest"

```java
@Test
void curd() throws SQLException{
    //save
    Member member = new Member("memberV4", 10000);
    repository.save(member);
    
    //findById
    Member findMember = repository.findById(member.getMemberId);
    log.info("findMember = {}", findMember);
    assertThat(findMember).isEqualTo(member);
    
    //update : money : 10000 -> 20000
    repository.update(member.getMemberId(), 20000);
    Member updatedMember = repository.findById(member.getMemberId());
    assertThat(updatedMember).isEqualTo(20000);
}
```



##### "memberRepositoryV0 - 회원 삭제 추가" 

```java
public void delete(String memberId) throws SQLException {
    String sql = "delete from member where member_id = ?";
    Connection con = null;
    PreparedStatement pstmt = null;

    try {
        con = getConnection();
        pstmt = con.prepareStatement(sql);
        pstmt.setString(1, memberId);
        pstmt.executeUpdate();
    } catch (SQLException e) {
        log.error("db error", e);
        throw e;
    }finally {
        close(con, pstmt, null);
    }
}
```



##### "memberRepositoryTest"

```java
@Slf4j
public class MemberRepositoryTest {
    MemberRepositoryV0 repository = new MemberRepositoryV0();

    @Test
    void crud() throws SQLException {
        //save
        Member member = new Member("memberVO", 10000);
        repository.save(member);

        //findById
        Member findMember = repository.findById(member.getMemberId());
        log.info("findMember = {}", findMember);
        assertThat(findMember).isEqualTo(member);

        //update : money : 10000 -> 20000
        repository.update(member.getMemberId(), 20000);
        Member updatedMember = repository.findById(member.getMemberId());
        assertThat(updatedMember.getMoney()).isEqualTo(20000);

        //delete
        repository.delete(member.getMemberId());
       	Assertions.assertThatThrownBy(() -> repository.findById(member.getMemberId()))
            .isInstanceOf(NoSuchElementException.class);
    }


}
```