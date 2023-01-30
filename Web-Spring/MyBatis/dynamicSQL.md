## Dynamic SQL

- [if](#if절)
- [choose, when, otherwise](#choose-when-oterwise)
- [trim, where, set](#trim-where-set)
- [foreach](#foreach)
- [script](#script)
- [bind](#bind)
- [Multi-db vendor support](#multi-db-vendor-support)
- [Pluggable Scripting Languages For Dynamic SQL](#pluggable-scripting-languages-for-dynamic-sql)

> &nbsp; MyBatis의 강력한 기능 중 하나는 `Dynamic SQL`이다. 만약 JDBC 또는 그와 유사한 프레임워크를 사용한 경험이 있다면 `조건부`로 SQL을 연결하여 사용하는 것이 얼마나 힘든지 알 것이다. MyBatis는 매핑된 SQL문 안에서 사용할 수 있는 강력한 Dynamic SQL 언어을 제공한다.
>
>&nbsp; Dynamic SQL 요소는 `JSTL`이나 유사한 `XML` 기반 텍스트 프로세서를 사용했던 개발자들에게 친숙할 것이다. 이전 버전의 MyBatis는 너무 많은 요소를 가지고 있었지만, `MyBatis3`는 이러한 부분을 크게 개선했다. 또 `OGNL 기반`의 강력한 표현식을 사용하고 있다.
<div align=center >
    <h5>&lt;MyBatis Document에 기재된 서론&gt;</h5>
</div>

<br>

> ## if절

- SQL의 where절 일부를 `조건부`로 포함
    - Parameter Value에 `마스킹` 관련 문자가 포함돼야 함
- 문자열 비교
    - a `==` b, a `!=` b
    - a `eq` b
    - a`.equals(`b`)`
- 대소문자 관계없이 비교
    - a`.equalsIgnoreCase(`b`)`
- 숫자 비교
    - a `>` 3, a `>=` 3
    - a `<` 3, a `<=` 3
- or문
    - `||`가 아닌 `or` 사용
- and문
    - `&&`가 아닌 `and` 사용 

```xml
<!-- Sample -->
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * 
  FROM BLOG 
  WHERE state = ‘ACTIVE’
  <if test="title != null">
    AND title like #{title}
  </if>
  <if test="author != null and author.name != null">
    AND author_name like #{author.name}
  </if>
</select>
```

<br>

> ## choose, when, oterwise

- switch-case문 기능 제공
- swith → choose
- case → when
- default → otherwise

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG 
  WHERE state = ‘ACTIVE’
  <choose>
    <when test="title != null">
      AND title like #{title}
    </when>
    <when test="author != null and author.name != null">
      AND author_name like #{author.name}
    </when>
    <otherwise>
      AND featured = 1
    </otherwise>
  </choose>
</select>
```

<br>

> ## trim, where, set

- ### where
    - 그렇지 않은 경우 어떻게 할지 정의
    - `AND`와 `OR`의 문제점 해결
    - where 태그를 쓰지 않은 경우 문제 발생 가능
        - 조건이 하나도 맞지 않으면 where 이후 수식이 `공백`이 됨
            - SELECT * FROM BLOG WHERE
        - 두 번째 조건만 충족해도 문제가 됨
            - SELECT * FROM BLOG WHERE `AND` title like 'someTitle'

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG
  <where>
    <if test="state != null">
         state = #{state}
    </if>
    <if test="title != null">
        AND title like #{title}
    </if>
    <if test="author != null and author.name != null">
        AND author_name like #{author.name}
    </if>
  </where>
</select>
```

- ### set
    - Dynamic Update문에 대한 솔루션
    - 수정할 열을 동적으로 포함하고 다른 열은 제외 가능

```xml
<update id="updateAuthorIfNecessary">
    update Author
    <set>
      <if test="username != null">username=#{username},</if>
      <if test="password != null">password=#{password},</if>
      <if test="email != null">email=#{email},</if>
      <if test="bio != null">bio=#{bio}</if>
    </set>
  where id=#{id}
</update>
```

- ### trim

- where절에서의 AND, OR, set절에서의 불필요한 쉼표 제거 가능

```xml
<!-- where절 -->
<trim prefix="WHERE" prefixOverrides="AND |OR ">
  ...
</trim>

<!-- set절 -->
<trim prefix="SET" suffixOverrides=",">
  ...
</trim>

```

<br>

> ## foreach

- `IN`절 구축을 위해 컬렉션을 활용할 때 사용
- 노드와 인덱스 변수 선언 가능
- Array와 Iterable, Map 사용 가능
    - Map은 `index`가 key, `item`이 value

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
  SELECT *
  FROM POST P
  <where>
    <foreach item="item" index="index" collection="list"
        open="ID in (" separator="," close=")" nullable="true">
          #{item}
    </foreach>
  </where>
</select>
```

<br>

> ## script

- Mapper 클래스에서 Dynamic SQL을 사용할 때 사용

```java
. . .
    @Update({"<script>",
      "update Author",
      "  <set>",
      "    <if test='username != null'>username=#{username},</if>",
      "    <if test='password != null'>password=#{password},</if>",
      "    <if test='email != null'>email=#{email},</if>",
      "    <if test='bio != null'>bio=#{bio}</if>",
      "  </set>",
      "where id=#{id}",
      "</script>"})
    void updateAuthorValues(Author author);
. . .
```

<br>

> ## bind

- OGNL 표현식으로 변수 선언 및 초기화
- context에 바인딩

```xml
<select id="selectBlogsLike" resultType="Blog">
  <bind name="pattern" value="'%' + _parameter.getTitle() + '%'" />
  SELECT * 
  FROM BLOG
  WHERE title LIKE #{pattern}
</select>
```

<br>

> ## Multi-db vendor support

- DatabaseIdProvider가 구성된 경우 `_databaseId` 변수를 활용
- DB 벤더에 따라 다양한 명령문 작성 가능

```xml
<insert id="insert">
  <selectKey keyProperty="id" resultType="int" order="BEFORE">
    <if test="_databaseId == 'oracle'">
      select seq_users.nextval 
      from dual
    </if>
    <if test="_databaseId == 'db2'">
      select nextval for seq_users 
      from sysibm.sysdummy1"
    </if>
  </selectKey>
  insert into users values (#{id}, #{name})
</insert>
```

<br>

> ## Pluggable Scripting Languages For Dynamic SQL

- MyBatis 3.2 버전부터 지원
- Language Driver 플러그 
- 해당 언어로 Dynamic SQL 쿼리를 작성 가능

```java
// LanguageDriver
public interface LanguageDriver {
    ParameterHandler createParameterHandler(MappedStatement mappedStatement, Object parameterObject, BoundSql boundSql);
    SqlSource createSqlSource(Configuration configuration, XNode script, Class<?> parameterType);
    SqlSource createSqlSource(Configuration configuration, String script, Class<?> parameterType);
}
```

- Config 파일에서 `기본값`으로 설정 가능

```xml
<typeAliases>
  <typeAlias type="org.sample.MyLanguageDriver" alias="myLanguage"/>
</typeAliases>
<settings>
  <setting name="defaultScriptingLanguage" value="myLanguage"/>
</settings>
```

- 기본값으로 설정하는 대신, 특정 명령문에 언어 지정

```xml
<select id="selectBlog" lang="myLanguage">
  . . .
</select>
```

- `@Lang` 어노테이션을 사용해 Mapper에도 사용 가능

```java
public interface Mapper {
    @Lang(MyLanguageDriver.class)
    @Select("SELECT * FROM BLOG")
  List<Blog> selectBlog();
}
```

<br>

---

### References

- [MyBatis Document](https://mybatis.org/mybatis-3/dynamic-sql.html)