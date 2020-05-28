# **Tomcat config**
```
<?xml version="1.0" encoding="UTF-8"?>
<Server>
  <Listener className="org.apache.catalina.startup.VersionLoggerListener"/>
  <Listener className="org.apache.catalina.core.AprLifecycleListener"/>
  <Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener"/>
  <Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener"/>
  <Listener className="org.apache.catalina.core.ThreadLocalLeakPreventionListener"/>
  <Listener className="org.apache.catalina.storeconfig.StoreConfigLifecycleListener"/>
  <GlobalNamingResources>
    <Resource auth="Container"
      description="User database that can be updated and saved" name="UserDatabase" type="org.apache.catalina.UserDatabase"
      factory="org.apache.catalina.users.MemoryUserDatabaseFactory" pathname="conf/tomcat-users.xml"/>
  </GlobalNamingResources>

  <Service name="Catalina">
    <Connector port="8080" redirectPort="8443" connectionTimeout="20000"
        keepAliveTimeout="20000" socket.soTimeout="20000">
    </Connector>
    <Engine defaultHost="localhost" name="Catalina">
      <Realm className="org.apache.catalina.realm.LockOutRealm">
        <Realm className="org.apache.catalina.realm.UserDatabaseRealm">
          <CredentialHandler className="org.apache.catalina.realm.MessageDigestCredentialHandler">
          </CredentialHandler>
        </Realm>
        <CredentialHandler className="org.apache.catalina.realm.MessageDigestCredentialHandler">
        </CredentialHandler>
      </Realm>
      <Host name="localhost">
        <Valve className="org.apache.catalina.valves.AccessLogValve" pattern="%h %l %u %t &quot;%r&quot; %s %b"
          prefix="localhost_access_log" suffix=".txt"/>
      </Host>
    </Engine>
  </Service>

  <Service name="Catalina1">
    <Connector port="8081" redirectPort="8443" connectionTimeout="20000"
        keepAliveTimeout="20000" socket.soTimeout="20000">
    </Connector>
    <Engine defaultHost="localhost" name="Catalina1">
      <Realm className="org.apache.catalina.realm.LockOutRealm">
        <Realm className="org.apache.catalina.realm.UserDatabaseRealm">
          <CredentialHandler className="org.apache.catalina.realm.MessageDigestCredentialHandler">
          </CredentialHandler>
        </Realm>
        <CredentialHandler className="org.apache.catalina.realm.MessageDigestCredentialHandler">
        </CredentialHandler>
      </Realm>
      <Host name="localhost" appBase="webapps1">
        <Valve className="org.apache.catalina.valves.AccessLogValve" pattern="%h %l %u %t &quot;%r&quot; %s %b"
          prefix="localhost_access_log" suffix=".txt"/>
		<!--
		<Context path="" docBase="C:\\ArchiveSW\\apache-tomcat-9.0.35\\sample.war" reloadable="true" />
		-->
      </Host>
    </Engine>
  </Service>
</Server>
```

## 总结:
- tomcat访问跟路径 "/" 是访问得appBase中的 ROOT 目录. 不管是手动创建还是tomcat自动生成都一样的效果.

### 配置 server.xml, 两种情况.
	1. war包在appBase目录下
	- 官方建议 不要设置 docBase 和 path, 此时, tomcat会自动解压war到war包得名字得目录.
		○ sample.war 会生成目录 appBase/sample. 访问得时需带上 /sample
		○ sample#test.war 会生成目录 appBase/sample#test, 访问是需带上 /sample/test
		○ ROOT.war, 会生成ROOT目录, 可以直接访问根路径 "/" ; 这也是为什么此种部署方式,常常修改war名字为 ROOT.war
	2. war包不在appBase目录下
	- 官方建议 设置 docBase 和 path, 此时, tomcat会自动解压war包到 以 path 内容创建的目录.
		○ sample.war, path = "/test" 会生成目录 appBase/test, 访问时需带上 /test
		○ sample.war, path = "/sample/test" 会生成目录 appBase/sample#test, 访问是需带上 /sample/test
		○ sample.war, path = "" 会生成目录 appBase/ROOT, 访问根目录即可 "/"

注意: 如果没有按照官方建议配置, 很有可能同时产生两次部署现象(double deployment is likely to result)



