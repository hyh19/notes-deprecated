# Tomcat 添加角色和用户

`apache-tomcat-8.5.27\conf\tomcat-users.xml`
```xml
<?xml version='1.0' encoding='utf-8'?>
<tomcat-users>
    <role rolename="manager"/>
    <role rolename="member"/>
    <user username="tom" password="secret"  roles="manager,member"/>
    <user username="jerry" password="secret" roles="member"/>
</tomcat-users>
```