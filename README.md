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



##### JDBC 등장 이유 :facepunch:

애플리케이션을 개발할 때 중요한 데이터는 대부분 데이터베이스에 보관한다.

##### "클라이언트, 애플리케이션, 서버, DB"

![image](https://user-images.githubusercontent.com/76586084/187840738-164093fb-6e14-4877-9f51-e1eb6ceb7c40.png)

클라이언트가 애플리케이션 서버를 통해 데이터를 저장하거나 조회하면, 애플리케이션 서버는 다음 과정을 통해서 데이터베이스를 사용한다.



##### "애플리케이션 서버와 DB - 일반적인 사용법" :facepunch:

![image](https://user-images.githubusercontent.com/76586084/187841808-8ab57b57-c9a0-46e1-979e-9ff7a68d9d15.png)

1. 커넥션 연결 : 주로 TCP/IP를 사용해서 커넥션을 연결한다.
2. SQL 전달 : 애플리케이션 서버는 DB가 이해할 수 있는 SQL을 연결된 커넥션을 통해 DB에 전달한다.
3. 결과 응답 : DB는 전달된 SQL을 수행하고 극 결과를 응답한다. 애플리케이션 서버는 응답 결과를 활용한다.



##### "애플리케이션 서버와 DB - DB 변경" :facepunch:

![image](https://user-images.githubusercontent.com/76586084/187842238-fa0d84fa-4544-4156-8902-c5e8ede1fd56.png)

문제는 각각의 데이터베이스마다 커넥션을 연결하는 방법, SQL을 전달하는 방법, 그리고 결과를 응답 받는 방법이 모두 다르다는 점이다.



여기에는 2가지 큰 문제가 있다.

1. 데이터베이스를 다른 종류의 데이터베이스로 변경하면 애플리케이션 서버에 개발된 데이터베이스 사용 코드도 함께 변경해야 한다.
2. 개발자가 각각의 데이터베이스마다 커넥션 연결, SQL 전달, 그리고 그 결과를 응답 받는 방법을 새로 학습해야 한다.



##### 이런 문제를 해결하기 위해 JDBC라는 자바 표준이 등장했다



##### JDBC 표준 인터페이스 :facepunch:

> JDBC(Java Database Connectivity)는 자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API이다. JDBC는 데이터베이스에서 자료를 쿼리하거나 업데이트 하는 방법을 제공한다.



##### "JDBC 표준 인터페이스" :facepunch:

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

##### "JDBC 직접 사용" :facepunch:

![image](https://user-images.githubusercontent.com/76586084/187847780-baeb8627-baea-48a3-b6ef-8043550f17cd.png)



##### "SQL Mapper" :facepunch:

![image-20220901153912430](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220901153912430.png)

- SQL Mapper
  - 장점 : JDBC를 편리하게 사용하도록 도와준다.
    - SQL 응답 결과를 객체로 편리하게 변환해준다.
    - JDBC 반복 코드를 제거해준다.
  - 단점 : 개발자가 SQL을 직접 작성해야한다.
  - 대표 기술 : 스프링 Jdbc Template, MyBatis



##### "ORM 기술" :facepunch:

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



##### "ConnectionConst" :facepunch:

```java
public abstract class ConnectionConst{
    public static final String URL = "jdbc:h2:tcp://localhost/~/test";
    public static final String USERNAME = "sa";
    public static final String PASSWORD = "";
}
```

데이터베이스에 접속하는데 필요한 기본 정보를 편리하게 사용할 수 있도록 상수로 만들었다.



JDBC를 사용해서 실제 데이터베이스에 연결하는 코드를작성해보자.

##### "DBConnectionUtil" :facepunch:

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



##### "DBConnectionUtilTest" :facepunch:

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

##### "JDBC 커넥션 인터페이스와 구현" :facepunch:

![image](https://user-images.githubusercontent.com/76586084/187861781-e1eb983f-28e2-4f42-a029-8ea1b27165f0.png)

- JDBC는 ***```java.sql.Connection```*** 표준 커넥션 인터페이스를 정의한다.
- H2 데이터베이스 드라이버는 JDBC Connection 인터페이스를 구현한 ***```org.h2.jdbc.JdbcConnection```*** 구현체를 제공한다.



##### "DriverManager 커넥션 요청 흐름" :facepunch: 

![image-20220901171220956](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220901171220956.png)

JDBC가 제공하는 ***```DriverManger```*** 는 라이브러리에 등록된 DB 드라이버들을 관리하고, 커넥션을 획득하는 기능을 제공한다.



1. 애플리케이션 로직에서 커넥션이 필요하면 ***```DriverManager.getConnection()```*** 을 호출한다. :postal_horn:
2. ***```DriverManeger```*** 는 라이브러리에 등록된 드라이버 목록을 자동으로 인식한다. 이 드라이버들에게 순서대로 다음 정보를 넘겨서 커넥션을 획득할 수 있는지 확인한다.
   - URL : 예) ***```jdbc:h2:tcp://localhost/~/test```***-
   - 이름, 비밀번호 등 접속에 필요한 추가 정보
   - 여기서 각각의 드라이버는 자신이 처리할 수 있는 요청인지 확인하고 처리할 수 있는 요청이라면 실제 데이터베이스에 연결해서 커넥션을 획득하고 이 커넥션을 클라이언트에게 반환한다.
   - 이렇게 찾은 커넥션 구현체가 클라이언트에 반환된다.





## JDBC 개발 - 등록

##### "Member" :facepunch:

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



##### "MemberRepositoryV0" :facepunch:

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



##### "커넥션 획득" :confetti_ball:

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





##### "MemberRepositoryTest" :facepunch:

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



##### "MemberRepositoryV0 - 회원 조회 추가"  :facepunch:

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

##### "MemberRepositoryTest" :facepunch:

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

##### "memberRepositoryV0 - 회원 수정 추가"  :facepunch:

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



##### "memberRepositoryTest" :facepunch:

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



##### "memberRepositoryV0 - 회원 삭제 추가"  :facepunch:

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



##### "memberRepositoryTest" :facepunch:

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





## 2. 커넥션풀과 데이터소스 이해

##### "데이터베이스 커넥션을 매번 획득" :facepunch:

![image](https://user-images.githubusercontent.com/76586084/187911104-4e9c108a-bbde-4c20-9f8b-586e78af72d7.png)

데이터베이스 커넥션을 획득할 때는 다음과 같은 복잡한 과정을 거친다.

1. 애플리케이션 로직은 DB 드라이버를 통해 커넥션을 조회한다.
2. DB 드라이버는 DB와 ***```TCP/IP```*** 커넥션을 연결한다. 물론 이 과정에서 3 way handshake 같은 ***```TCP/IP```*** 연결을 위한 네트워크 동작이 발생한다.
3. DB 드라이버는 ***```TCP/IP```*** 커넥션이 연결되면 ID, PW 와 기타 부가정보를 DB에 전달한다.
4. DB는 ID, PW 를 통해 내부 인증을 완료하고, 내부에 DB 세션을 생성한다.
5. DB는 커넥션 생성이 완료되었다는 응답을 보낸다.
6. DB 드라이버는 커넥션 객체를 생성해서 클라이언트에 반환한다.

이렇게 커넥션을 새로 만드는 것은 과정도 복잡하고 시간도 많이 소모되는 일이다.
DB는 물론이고 애플리케이션 서버에서도 ***```TCP/IP```*** 커넥션을 새로 생성하기 위한 리소스를 매번 사용해야 한다.
결과적으로 응답 속도에 영향을 준다. 이것은 좋지 않다!! :rage:



:open_mouth: 이런 문제를 한번에 해결하는 아이디어가 바로 커넥션을 미리 생성해두고 사용하는 커넥션 풀이라는 방법이다.
커넥션 풀은 이름 그래도 커넥션을 관리하는 풀이다.



##### "커넥션 풀 초기화" :facepunch:

![image](https://user-images.githubusercontent.com/76586084/187914598-d7a7aec0-5f39-4a6d-9e87-f86c47d5165f.png)

애플리케이션을 시작하는 시점에 커넥션 풀은 필요한 만큼 커넥션을 미리 확보해서 풀에 보관한다. 보통 얼마나 보관할지는 서비스의 특징과 서버 스펙에 따라 다르지만 기본값을 10개이다.



##### "커넥션 풀의 연결 상태" :facepunch:

![image](https://user-images.githubusercontent.com/76586084/187916346-4becb329-24ba-41ce-a09a-411f90baf385.png)

커넥션 풀에 들어 있는 커넥션은 TCP/IP로 DB와 커넥션이 연결되어 있는 상태이기 때문에 언제든지 즉시 SQL을 DB에 전달할 수 있다.



##### "커넥션 풀 사용1" :facepunch:

![image](https://user-images.githubusercontent.com/76586084/187917561-7466ef28-18d0-445c-a720-55a6e43ef432.png)

- ㅉ애플리케이션 로직에서 이제는 DB 드라이버를 통해서 새로운 커넥션을 획득하는 것이 아니다.
- 이제는 커넥션 풀을 통해 이미 생성되어 있는 커넥션을 개개체 참조로 그냥 가져다 쓰기만 하면 된다.
- 커넥션 풀에 커넥션을 요청하면 커넥션 풀은 자신이 가지고 있는 커넥션 중에 하나를 반환한다.



##### "커넥션 풀 사용2" :punch:

![image](https://user-images.githubusercontent.com/76586084/187918616-82db6618-b3aa-4c34-b7b1-79f4fd647f64.png)

- 애플리케이션 로직은 커넥션 풀에서 받은 커넥션을 사용해서 SQL을 데이터베이스에 전달하고 그 결과를 받아서 처리한다.
- 커넥션을 모두 사용하고 나면 이제는 커넥션을 종료하는 것이 아니라, 다음에 다시 사용할 수 있도록 해당 커넥션을 그대로 커넥션풀에 반환하면 된다. (종료 :x: 반환 :o: )



성능과 사용의 편리함 측면에서 ***```hikariCP```*** 를 커넥션 풀 오픈소스로 사용한다. (스프링 2.0부터는 기본 커넥션 풀로 ***```hikariCP```*** 를 사용)







## DataSource 이해

커넥션을 얻는 방법은 앞서 학습한 JDBC ***```DriverManager```*** 를 직접 사용하거나, 커넥션 풀을 사용하는 등 다양한 방법이 존재한다.

##### DriverManager를 통해서 얻느냐 Connection pool을 통해서 얻느냐. (매번 새로 생성하느냐, 생성해 놓은것을 가지고 오느냐)

##### 

##### "커넥션을 획득하는 방법을 추상화" :punch:

![image](https://user-images.githubusercontent.com/76586084/187921818-92542aca-45ef-45ef-9db7-d21ecb1a07f7.png)

- 자바에서 이런 문제를 해결하기 위해 ***```javax.sql.DataSource```*** 라는 인터페이스를 제공한다.
- ***```DataSource```*** 는 ***커넥션을 획득하는 방법을 추상화*** 하는 인터페이스이다.
- 이 인터페이스의 핵심 기능은 커넥션 조회 하나이다.



##### "DataSource 핵심 기능만 축약" :punch:

```java
public interface DataSource{
    Connection getConnection() throws SELExcepion;
}
```



##### "ConnectionTest - 드라이브 매니저" :punch:

```java
@Test
void driveManager() throws SQLException {
    Connection con1 = DriverManager.getConnection(URL, USERNAME, PASSWORD);
    Connection con2 = DriverManager.getConnection(URL, USERNAME, PASSWORD);
    log.info("connection = {}, class={}", con1, con1.getClass());
    log.info("connection = {}, class={}", con2, con2.getClass());
}
```

##### 실행 결과

```
23:34:27.718 [Test worker] INFO hello.jdbc.connection.ConnectionTest - connection = conn0: url=jdbc:h2:tcp://localhost/~/test user=SA, class=class org.h2.jdbc.JdbcConnection
23:34:27.743 [Test worker] INFO hello.jdbc.connection.ConnectionTest - connection = conn1: url=jdbc:h2:tcp://localhost/~/test user=SA, class=class org.h2.jdbc.JdbcConnection
```



이번에는 스프링이 제공하는 ***```DataSource```*** 가 적용된 ***```DriveManager```*** 인 ***```DriveManagerDataSource```*** 를 사용해 보자.



##### "Connection Test - 데이터소스 드라이브 매니저" :punch:

```java
@Test
void dataSourceDriverManager() throws SQLException {
    //DriverManagerDataSource - 항상 새로운 커넥션을 획득
    DataSource dataSource = new DriverManagerDataSource(URL, USERNAME, PASSWORD);
    useDataSource(dataSource);
}

private void useDataSource(DataSource dataSource) throws SQLException {
    Connection con1 = dataSource.getConnection();
    Connection con2 = dataSource.getConnection();
    log.info("connection = {}, class={}", con1, con1.getClass());
    log.info("connection = {}, class={}", con2, con2.getClass());
}
```

##### 실행 결과

```
23:38:38.345 [Test worker] DEBUG org.springframework.jdbc.datasource.DriverManagerDataSource - Creating new JDBC DriverManager Connection to [jdbc:h2:tcp://localhost/~/test]
23:38:38.546 [Test worker] DEBUG org.springframework.jdbc.datasource.DriverManagerDataSource - Creating new JDBC DriverManager Connection to [jdbc:h2:tcp://localhost/~/test]
23:38:38.554 [Test worker] INFO hello.jdbc.connection.ConnectionTest - connection = conn0: url=jdbc:h2:tcp://localhost/~/test user=SA, class=class org.h2.jdbc.JdbcConnection
23:38:38.557 [Test worker] INFO hello.jdbc.connection.ConnectionTest - connection = conn1: url=jdbc:h2:tcp://localhost/~/test user=SA, class=class org.h2.jdbc.JdbcConnection
```

기존 코드와 비슷하지만 ***```DriverManagerDataSource```*** 는  ***```DataSource```*** 를 통해서 커넥션을 획득할 수 있다. 참고로 ***```DriverManagerDataSource```*** 는 스프링이 제공하는 코드이다.



##### 파라미터 차이 :dagger:

기존 ***```DriverManager```*** 를 통해서 커넥션을 획득하는 방법과 ***```DataSource```*** 를 통해서 커넥션을 획득하는 방법에는 큰 차이가 있다.

-  ***```DriverManager```*** 는 커넥션을 획득할 때 마다  ***```URL```*** , ***```USERNAME```*** , ***```PASSWORD```*** 같은 파라미터를 계속 전달해야 한다. 반면에 ***```DataSource```*** 를 사용하는 방식은 처음 객체를 생성할 때만 필요한 파라미터를 넘겨두고, 커넥션을 획득할 때는 단순히 ***```dataSource.getConnection()```*** 만 호출하면 된다.



##### 설정과 사용의 분리

- ***설정*** : ***```DataSource```*** 를 만들고 필요한 속성들을 사용해서***```URL```*** , ***```USERNAME```*** , ***```PASSWORD```*** 같은 부분을 입력하는 것을 말한다. 이렇게 설정과 관련된 속성들을 한 곳에 있는 것이 향후 변경에 더 유연하게 대처할 수 있다. :gear:
- ***사용*** : 설정은 신경쓰지 않고, ***```DataSource```***  의 ***```getConnection()```***  만 호출해서 사용하면 된다. :rocket:



##### 설정과 사용의 분리 설명

- 이 부분이 작아보이지만 큰 차이를 만들어내는데, 필요한 데이터를 ***```DataSource```***  가 만들어지는 시점에 미리 다 넣어두게 되면, ***```DataSource```***  를 사용하는 곳에서는 ***```dataSource.getConnection()```*** 만 호출하면 되므로, ***```URL```*** , ***```USERNAME```*** , ***```PASSWORD```***  같은 속성들에 의존하지 않아도 된다. 그냥 ***```DataSource```***  만 주입받아서 ***```getConnection()```*** 만 호출하면 된다.  :whale:
- 쉽게 이야기해서 리포지토리(Repository)는 ***```DataSource```***  만 의존하고, 이런 속성을 몰라도 된다. 애플리케이션을 개발해보면 보통 설정은 한 곳에서 하지만, 사용은 수 많은 곳에서 하게 된다. 덕분에 객체를 설정하는 부분과, 사용하는 부분을 좀 더 명확하게 분리할 수 있다. :whale2:





## DataSource 예제2 - 커넥션 풀

이번에는 ***```DataSource```*** 를 통해 커넥션 풀을 사용하는 예제를 알아보자.



##### "Connection Test - 데이터소스 커넥션 풀 추가"

```java
@Test
void dataSourceConnectionPool() throws SQLException, InterruptedException{
    //커넥션 풀링 : HikariProxyConnection(Proxy) -> JdbcConnection(Target)
    HikariDataSource dataSource = new HikariDataSource();
    dataSource.setJdbcUrl(URL);
    dataSource.setUsername(USERNAME);
    dataSource.setPassword(PASSWORD);
    dataSource.setMaximumPoolSize(10);
    dataSource.setPoolName("MyPool");
    
    useDataSource(dataSource);
    Thread.sleep(1000);
}
```

- HikariCP 커넥션 풀을 사용한다. ***```HikariDataSource```*** 는 ***```DataSource```*** 인터페이스를 구현하고 있다.
- 커넥션 풀 최대 사이즈를 10으로 지정하고, 풀의 이름을 ***```MyPool```*** 이라고 지정했다.
- 커넥션 풀에서 커넥션을 생성하는 작업은 애플리케이션 실행 속도에 영향을 주지 않기 위해 별도의 쓰레드에서 작동한다. 별도의 쓰레드에서 동작하기 때문에 테스트가 먼저 종료되어 버린다. 예제처럼 ***```Thread.sleep(1000)```*** 을 통해 대기 시간을 주어야 쓰레드 풀에 커넥션이 생성되는 로그를 확인할 수 있다.



##### "실행 결과"

```
12:54:12.844 [MyPool housekeeper] DEBUG com.zaxxer.hikari.pool.HikariPool - MyPool - Pool stats (total=2, active=2, idle=0, waiting=0)
12:54:12.859 [MyPool connection adder] DEBUG com.zaxxer.hikari.pool.HikariPool - MyPool - Added connection conn2: url=jdbc:h2:tcp://localhost/~/test user=SA
12:54:12.875 [MyPool connection adder] DEBUG com.zaxxer.hikari.pool.HikariPool - MyPool - Added connection conn3: url=jdbc:h2:tcp://localhost/~/test user=SA
12:54:12.884 [MyPool connection adder] DEBUG com.zaxxer.hikari.pool.HikariPool - MyPool - Added connection conn4: url=jdbc:h2:tcp://localhost/~/test user=SA
12:54:12.892 [MyPool connection adder] DEBUG com.zaxxer.hikari.pool.HikariPool - MyPool - Added connection conn5: url=jdbc:h2:tcp://localhost/~/test user=SA
12:54:12.907 [MyPool connection adder] DEBUG com.zaxxer.hikari.pool.HikariPool - MyPool - Added connection conn6: url=jdbc:h2:tcp://localhost/~/test user=SA
12:54:12.914 [MyPool connection adder] DEBUG com.zaxxer.hikari.pool.HikariPool - MyPool - Added connection conn7: url=jdbc:h2:tcp://localhost/~/test user=SA
12:54:12.928 [MyPool connection adder] DEBUG com.zaxxer.hikari.pool.HikariPool - MyPool - Added connection conn8: url=jdbc:h2:tcp://localhost/~/test user=SA
12:54:12.952 [MyPool connection adder] DEBUG com.zaxxer.hikari.pool.HikariPool - MyPool - Added connection conn9: url=jdbc:h2:tcp://localhost/~/test user=SA
12:54:12.953 [MyPool connection adder] DEBUG com.zaxxer.hikari.pool.HikariPool - MyPool - After adding stats (total=10, active=2, idle=8, waiting=0)
BUILD SUCCESSFUL in 1m 16s
4 actionable tasks: 2 executed, 2 up-to-date
오후 12:54:15: Task execution finished ':test --tests "hello.jdbc.connection.ConnectionTest.dataSourceConnectionPool"'.
```





## DataSource 적용

이번에는 애플리케이션에 ***```DataSource```*** 를 적용해보자

##### "MemberRepositoryV1"

```java
@Slf4j
public class MemberRepositoryV1 {

    private final DataSource dataSource;

    public MemberRepositoryV1(DataSource dataSource) {
        this.dataSource = dataSource;
    }

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

    private void close(Connection con, Statement stmt, ResultSet rs) {

        JdbcUtils.closeResultSet(rs);
        JdbcUtils.closeStatement(stmt);
        JdbcUtils.closeConnection(con);

    }

    private Connection getConnection() throws SQLException {
        Connection con = dataSource.getConnection();
        log.info("get connection = {}. class = {}");

        return con;
    }

}
```

- ***```DataSource```*** 의존관계 주입

  - 외부에서 ***```DataSource```*** 를 주입 받아서 사용한다. 이제 직접 만든 ***```DBConnectionUtil```*** 을 사용하지 않아도 된다.
  - ***```DataSource```*** 는 표준 인터페이스 이기 때문에 ***```DriverManagerDataSource```*** 로 변경 되어도 해당 코드를 변경하지 않아도 된다.

- ***```JdbcUtils```*** 편의 메서드

  - 스프링은 JDBC를 편리하게 다룰 수 있는 ***```JdbcUtils```*** 라는 편의 메서드를 제공한다.
  - ***```JdbcUtils```*** 을 사용하면 커넥션을 좀 더 편리하게 닫을 수 있다.

  

  ##### "MemberRepositoryV1Test"

  ```java
  @Slf4j
  public class MemberRepositoryV1Test {
  
      MemberRepositoryV1 repository;
  
      @BeforeEach
      void beforeEach(){
          //기본 DriverManager - 항상 새로운 커넥션을 획득
          DriverManagerDataSource dataSource = new DriverManagerDataSource(URL, USERNAME, PASSWORD);
          repository = new MemberRepositoryV1(dataSource);
      }
  
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

  ##### "실행 결과"

```
13:42:12.376 [Test worker] DEBUG org.springframework.jdbc.datasource.DriverManagerDataSource - Creating new JDBC DriverManager Connection to [jdbc:h2:tcp://localhost/~/test]
13:42:12.745 [Test worker] INFO hello.jdbc.repository.MemberRepositoryV1 - get connection = conn0: url=jdbc:h2:tcp://localhost/~/test user=SA. class = class org.h2.jdbc.JdbcConnection
13:42:12.760 [Test worker] INFO hello.jdbc.connection.DBConnectionUtil - get connection = conn1: url=jdbc:h2:tcp://localhost/~/test user=SA, class = class org.h2.jdbc.JdbcConnection
13:42:12.832 [Test worker] DEBUG org.springframework.jdbc.datasource.DriverManagerDataSource - Creating new JDBC DriverManager Connection to [jdbc:h2:tcp://localhost/~/test]
13:42:12.838 [Test worker] INFO hello.jdbc.repository.MemberRepositoryV1 - get connection = conn2: url=jdbc:h2:tcp://localhost/~/test user=SA. class = class org.h2.jdbc.JdbcConnection
13:42:12.844 [Test worker] INFO hello.jdbc.connection.DBConnectionUtil - get connection = conn3: url=jdbc:h2:tcp://localhost/~/test user=SA, class = class org.h2.jdbc.JdbcConnection
13:42:12.862 [Test worker] INFO hello.jdbc.repository.MemberRepositoryV1Test - findMember = Member(memberId=memberVO, money=10000)
13:42:13.151 [Test worker] DEBUG org.springframework.jdbc.datasource.DriverManagerDataSource - Creating new JDBC DriverManager Connection to [jdbc:h2:tcp://localhost/~/test]
13:42:13.158 [Test worker] INFO hello.jdbc.repository.MemberRepositoryV1 - get connection = conn4: url=jdbc:h2:tcp://localhost/~/test user=SA. class = class org.h2.jdbc.JdbcConnection
13:42:13.163 [Test worker] INFO hello.jdbc.connection.DBConnectionUtil - get connection = conn5: url=jdbc:h2:tcp://localhost/~/test user=SA, class = class org.h2.jdbc.JdbcConnection
13:42:13.168 [Test worker] INFO hello.jdbc.repository.MemberRepositoryV1 - resultSize=1
13:42:13.171 [Test worker] DEBUG org.springframework.jdbc.datasource.DriverManagerDataSource - Creating new 
```

계속 새로 생성해서 느리다! -> ConnectionPool을 쓰면 된다.



##### "MemberRepositoryV1Test" - @BeforeEach 수정

```java
@BeforeEach
void beforeEach(){
    //기본 DriverManager - 항상 새로운 커넥션을 획득
    //DriverManagerDataSource dataSource = new DriverManagerDataSource(URL, USERNAME, PASSWORD);

    //커넥션 풀링
    HikariDataSource dataSource = new HikariDataSource();
    dataSource.setJdbcUrl(URL);
    dataSource.setUsername(USERNAME);
    dataSource.setPassword(PASSWORD);
    repository = new MemberRepositoryV1(dataSource);
}
```





## 3. 트랜잭션 이해

##### 트랜잭션 - 개념 이해

데이터를 저장할 때 단순히 파일에 저장해도 되는데, 데이터베이스에 저장하는 이유는 무엇일까? 여러가지 이유가 있지만, 가장 대표적인 이유는 바로 데이터베이스는 트랙잭션이라는 개념을 지원하기 때문이다. :open_mouth:

트랜잭션을 이름 그대로 번역하면 거래라는 뜻이다. 이것을 쉽게 풀어서 이야기하면, 데이터베이스에서 트랜잭션은 하나의 거래를 안전하게 처리하도록 보장해주는 것을 뜻한다. 그런데 하나의 거래를 안전하게 처리하려면 생각보다 고려해야 할 점이 많다. 계좌이체를 하는 상황을 가정해 보자 A의 계좌에서 B의 계좌로 5000원을 계좌이체한다고 생각해보면 A의 잔고는 5000원 감소하고 B의 잔고는 5000원 증가해야 한다.

##### "5000원 계좌 이체"

1. A의 잔고를 5000원 감소
2. B의 잔고를 5000원 증가

계좌이체라는 거래는 이렇게 2가지 작업이 독장해야 한다. 만약 1번은 성공했는데 2번에서 시스템에 문제가 발생하면 계좌이체는 실패하고, A의 잔고만 5000원 감소하는 심각한 문제가 발생한다. :fearful:
데이터베이스가 제공하는 트랙잰색 기능을 사용하면 1,2 둘다 함께 성공해야 저장하고, 중간에 하나라도 실패하면 거래 전의 상태로 돌아갈 수 있다. 만약 1번은 성공했는데 2번에서 시스템에 문제가 발생하면 계좌이체는 실패가고, 거래 전의 상태로 완전히 돌아갈 수 있다.
결과적으로 A의 잔고가 감소하지 않는다.

모든 작업이 성공해서 데이터베이스에 정상 반영하는 것을 ***```커밋(Commit)```*** 이라 하고, 작업 중 하나라도 실패해서 거래 이전으로 되돌리는 것을 ***```롤백(Rollback)```*** 이라 한다.



##### 트랜잭션 ACID

트랜잭션은 ACID라 하는 원자성(Atomicity), 일관성(Consistency), 격리성(Isolation), 지속성(Durability)을 보장해야 한다.

- ***원자성*** : 트랜잭션 내에서 실행한 작업들은 마치 하나의 작업인 것처럼 모두 성공 하거나 모두 실패해야 한다. :atom_symbol:
- ***일관성*** : 모든 트랙잭션은 일관성 있는 데이터베이스 상태를 유지해야 한다. 예를 들어 데이터베이스에서 정한 무결성 제약 조건을 항상 만족해야 한다. :spider_web:
- ***격리성*** : 동시에 실행되는 트랜잭션들이 서로에게 영향을 미치지 않도록 격리한다. 예를 들어 동시에 같은 데이터를 수정하지 못하도록 해야 한다. 격리성은 동시성과 관련된 성능 이슈로 인해 트랜잭션 격리 수전(isolation level)을 선택할 수 있다. :european_castle:
- ***지속성*** : 트랜잰션을 성공적으로 끝내면 그 결과가 항상 기록되어야 한다. 중간에 시스템에 문제가 발생해도 데이터베이스 로그 등을 사용해서 겅공한 트랜잭션 내용을 복구해야 한다.

트랜잭션은 원자성, 일관성, 지속성을 보장한다. 문제는 격리성인데 트랜잭션 같에 격리성을 완변히 보장라혀면 트랜잭션을 거의 순서대로 실행해야 한다. 이렇게 하면 동시 처리 성능이 매우 나빠진다. 이런 문제로 인해 ANSI 표준은 트랜잭션의 격리 수준을 4단계로 나누어 정의했다.



##### 트랜잭션 격리 수준 - Isolation level :desktop_computer:

- READ UNCOMMITED(커밋되지 않은 읽기)
- READ COMMITED(커밋된 읽기)
- REPEATABLE READ(반복 가능한 읽기)
- SERIALIZABLE(직렬화 기능)



> ***참고*** : 강의에서는 일반적으로 사용하는 READ COMMITED(커밋된 읽기) 트랜잭션 격리 수준을 기준으로 설명한다.





## 데이터베이스 연결 구조와 DB 세션

트랜잭션을 더 자세히 이해하기 위해 데이터베이스 서버 연결 구조와 DB 세션에 대해 알아보자.



##### "데이터베이스 연결 구조1"

![image](https://user-images.githubusercontent.com/76586084/188096832-fee7f802-62c0-41d6-bbf2-bbbf192dbe13.png)

- 사용자는 웹 애플리케이션 서버(WAS)나 DB 접근 툴 같은 클라이언트를 사용해서 데이터베이스 서버에 접근할 수 있다. 클라이언트는 데이터베이스 서버에 연결을 요청하고 커넥션을 맺게 된다. 이때 데이터베이스 서버는 내부에 세션이라는 것을 만든다. 그리고 앞으로 해당 커넥션을 통한 모든 요청은 이 세션을 통해서 실행하게  된다.
- 쉽게 이야기해서 개발자가 클라이언트를 통해 SQL을 전달하면서 현재 커넥션에 연결된 세션이 SQL을 실행한다.
- 세션은 트랜잭션을 시작하고, 커밋 또는 롤백을 통해 트랜잭션을 종료한다. 그리고 이후에 새로운 트랜잭션을 다시 시작할 수 있다.
- 사용자가 커넥션을 닫거나, 또는 DBA(DB 관리자)가 세션을 강제로 종료하면 세션을 종료된다.



##### "데이터베이스 연결 구조2"

![image](https://user-images.githubusercontent.com/76586084/188099074-99baa4a8-af13-4cdc-b890-cc22b55bc301.png)

- 커넥션 풀이 10개의 커넥션을 생성하면, 세션도 10개 만들어진다.





바로 코드로 들어가보자.

##### table 준비 :golfing_man:

![image-20220902180050187](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220902180050187.png)

##### 자동 커밋

트랜잭션을 사용하려면 먼저 자동 커밋과 수동 커밋을 이해해야 한다.
자동 커밋으로 설정하면 각각의 쿼리 실행 직후에 자동으로 커밋을 호출한다. 따라서 커밋이나 롤백을 직접 호출하지 않아도 되는 편리함이 있다. 하지만 쿼리를 하나하나 실행할 때 마다 자동으로 커밋이 되어버리기 때문에 우리가 원하는 트랜잭션 기능을 제대로 사용할 수 없다.



##### 자동 커밋 설정 :oncoming_automobile:

```sql
set autocommit true;
insert into member(member_id, money) values ('data1', 10000);
insert into member(member_id, money) values ('data2', 20000);
```



따라서 ***```commit```*** , ***```rollback```*** 을 직접 호출하면서 트랜잭션 기능을 제대로 수행하려면 자동 커밈ㅅ을 끄고 수동 커밋을 사용해야 한다.

##### 수동 커밋 설정:traffic_light:

```java
set autocommit false;
insert into member(member_id, money) values ('data3', 10000);
insert into member(member_id, money) values ('data4', 10000);
```

보통  자동 커밋 모드가 기본으로 설전된 경우가 많기 때문에 ***수동 커밋 모드로 설정하는 것을 트랜잭션을 시작*** 한다고 표현할 수 있다.
수동 커밋 설정을 하면 이루헤 꼭 ***```commit```*** , ***```rollback```*** 을 호출해야한다.





## 트랜잭션 - 적용1

실제 애플리케이션에 DB 트랜잭션을 사용해서 계좌이체 같이 원자성이 중요한 비즈니스 로직을 어떻게 구현하는지 알아보자. 먼저 트랜잭션 없이 단순하게 계좌이체 비즈니스 로직만 구현해보자.

##### "MemberServiceV1"

```java
@RequiredArgsConstructor
public class MemberServiceV1 {

    private final MemberRepositoryV1 memberRepository;

    public void accountTransfer(String fromId, String toId, int money) throws SQLException {
        Member fromMember = memberRepository.findById(fromId);
        Member toMember = memberRepository.findById(toId);

        memberRepository.update(fromId, fromMember.getMoney() - money);
        validation(toMember);
        memberRepository.update(toId, toMember.getMoney() + money);
    }

    private void validation(Member toMember) {
        if (toMember.getMemberId().equals("ex")) {
            throw new IllegalStateException("이체중 예외 발생");
        }
    }
}
```

- ***```fromId```*** 의 회원을 조회해서 ***```toId```*** 의 회원에게 ***```money```*** 만큼의 돈을 계좌이체 하는 로직이다.
  - ***```fromId```*** 회원의 돈을  ***```money```*** 만큼 감소한다. :arrow_right: UPDATE SQL 실행
  - ***```toId```*** 회원의 돈을 ***```money```*** 만큼 증가한다. :arrow_right: UPDATE SQL 실행
- 예외 상황을 테스트해보기 위해 ***```toId```*** 가 ***```ex```***  인 경우 예외를 발생한다.



##### "MemberServiceV1Test"

```java
/**
 * 기본 동작, 트랜잭션이 없어서 문제 발생
 */
public class MemberServiceV1Test {

    public static final String MEMBER_A = "memberA";
    public static final String MEMBER_B = "memberB";
    public static final String MEMBER_EX = "ex";

    private MemberRepositoryV1 memberRepository;
    private MemberServiceV1 memberService;

    @BeforeEach
    void before() {
        DriverManagerDataSource dataSource = new DriverManagerDataSource(URL, USERNAME, PASSWORD);
        memberRepository = new MemberRepositoryV1(dataSource);
        memberService = new MemberServiceV1(memberRepository);
    }
    
    @AfterEach
    void after() throws SQLException {
        memberRepository.delete(MEMBER_A);
        memberRepository.delete(MEMBER_B);
        memberRepository.delete(MEMBER_EX);
    }

    @Test
    @DisplayName("정상 이체")
    void accountTransfer() throws SQLException {
        //given
        Member memberA = new Member(MEMBER_A, 10000);
        Member memberB = new Member(MEMBER_B, 10000);
        memberRepository.save(memberA);
        memberRepository.save(memberB);

        //when
        memberService.accountTransfer(memberA.getMemberId(), memberB.getMemberId(), 2000);

        //then
        Member findMemberA = memberRepository.findById(memberA.getMemberId());
        Member findMemberB = memberRepository.findById(memberB.getMemberId());
        assertThat(findMemberA.getMoney()).isEqualTo(8000);
        assertThat(findMemberB.getMoney()).isEqualTo(12000);
    }
    
    @Test
    @DisplayName("이체 중 예외 발생")
    void accountTransferEx() throws SQLException {
        //given
        Member memberA = new Member(MEMBER_A, 10000);
        Member memberEx = new Member(MEMBER_EX, 10000);
        memberRepository.save(memberA);
        memberRepository.save(memberEx);

        //when
        assertThatThrownBy(() -> memberService.accountTransfer(memberA.getMemberId(),
                                                               memberEx.getMemberId(), 2000))
                .isInstanceOf(IllegalStateException.class);

        //then
        Member findMemberA = memberRepository.findById(memberA.getMemberId());
        Member findMemberB = memberRepository.findById(memberEx.getMemberId());
        assertThat(findMemberA.getMoney()).isEqualTo(8000);
        assertThat(findMemberB.getMoney()).isEqualTo(10000);
    }

}
```



##### "정리"  :open_mouth:

이체중 예외가 발생하게 되면 ***```memberA```*** 의 금액은 10000원 :arrow_right: 8000원으로 2000원 감소한다. 그런데 ***```memberB```*** 의 돈은 그대로 10000원으로 남아있다. 결과적으로 ***```memberA```*** 의 돈만 2000원 감소한 것이다.





































































































