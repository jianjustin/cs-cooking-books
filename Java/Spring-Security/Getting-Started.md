# Getting Started

Spring Security是一个功能强大和高度可定制的身份验证和访问控制框架

**Spring Security包含模块：**

* **Core**｜`spring-security-core.jar`
* **Remoting**｜`spring-security-remoting.jar`
* **Web**｜`spring-security-web.jar`
* **Config**｜`spring-security-config.jar`
* **LDAP**｜`spring-security-ldap.jar`
* **OAuth 2.0 Core**｜`spring-security-oauth2-core.jar`
* **OAuth 2.0 Client**｜`spring-security-oauth2-client.jar`
* **OAuth 2.0 JOSE**｜`spring-security-oauth2-jose.jar`
* **OAuth 2.0 Resource Server**｜`spring-security-oauth2-resource-server.jar`
* **ACL**｜`spring-security-acl.jar`
* **CAS**｜`spring-security-cas.jar`
* **OpenID**｜`spring-security-openid.jar`
* **Test**｜`spring-security-test.jar`

## Spring Security教程

* 01-编写简单Spring Security应用

## 核心组件

* `SecurityContextHolder` - 存放当前应用的安全上下文
* `SecurityContext` - 保存应用认证等安全相关信息
* `Authentication` - 用户在应用中的安全认证实体
* `UserDetailsService` - 通过用户名等进行认证获取UserDetails实体
* `GrantedAuthority` - 反映用户在应用范围内的授权信息
* `UserDetails` - 提供必要的信息以构建你的应用DAO或安全的其他数据源的认证对象