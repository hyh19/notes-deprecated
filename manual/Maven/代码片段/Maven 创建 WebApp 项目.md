# Maven 创建 WebApp 项目

```bash
mvn archetype:generate -B \
-DarchetypeGroupId=org.apache.maven.archetypes \
-DarchetypeArtifactId=maven-archetype-webapp \
-DgroupId=com.mycompany.app \
-DartifactId=mywebapp \
-Dversion=1.0-SNAPSHOT \
-Dpackage=com.mycompany.app.mywebapp
```

注意：包名不能用中划线 `- `，否则会出现编译错误，可以用下划线 `_`。