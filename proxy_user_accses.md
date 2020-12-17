## Oracle Proxy user를 사용한 DB 접근제어 및 패스워드 운영 강화 방안
### 1. Oracle Proxy user란?
- Oracle DB account 종류
- 다른 schema user에 연결하는 용도로만 사용
- 잇점 : 어플리케이션 투명하게 DB user(스키마 Owner)에 대한 비밀번호 변경이 수시로 가능하여 패스워드 노출로 인한 사고(데이터 유출, 파괴, 변조)에 대한 예방 보호가 가능함.

### 2. Proxy user 생성 방법
1. CREATE USER proxy_user_name IDENTIFIED BY …
2. GRANT connect TO proxy_user_name;
3. ALTER USER schema_user GRANT CONNECT THROUGH proxy_user_name;

### 3. Proxy user 사용시 유의/권고 사항
1. CREATE SESSION privilege만 허용 할 것
2. 다른 privileges 제공 금지

### 4. Proxy user 리스트 확인 방법
```sql
Select * from proxy_users;  -- 모든 proxy user 조회
```

### 5. User 세션에서 Proxy user 확인
```sql
select  sys_context('USERENV','PROXY_USER') proxy_user  from dual;  
```

### 6. Proxy login 방법
```sql
connect proxy_id[schema_id]/password1;
```
### 7. Application에서 Proxy user 사용 방법

```java
java.util.Properties prop = new java.util.Properties();
    prop.put(OracleConnection.PROXY_USER_NAME, ＂jeff＂);
    String[] roles = {＂role1＂, ＂role2＂};
    prop.put(OracleConnection.PROXY_ROLES, roles);
    conn.openProxySession(OracleConnection.PROXYTYPE_USER_NAME, prop);

```
