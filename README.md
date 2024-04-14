# 利用github搭建个人maven仓库
简单来说，共有三步：
## 1. deploy到本地目录
库项目的pom.xml中设置发布配置，如下即发布到maven-repo的repository目录。
其中maven-repo是github上创建的一个代码工程的根目录。url指定了库工程deploy后，产生的jar等文件上传到本地的哪个目录。
<distributionManagement>
    <repository>
        <id>byebug-mvn-repo</id>
        <url>file:/Users/kali/maven-repo/repository/</url>
    </repository>
</distributionManagement>

## 2. 把本地目录提交到gtihub上
mvn deploy -Dmaven.test.skip=true -DaltDeploymentRepository=byebug-mvn-repo::default::file:/Users/kali/maven-repo/repository/
执行上面的命令后，会在repository目录生成库工程的jar，版本号是库工程里配置的版本号，如0.1。

cd maven-repo
git add .
git commit -m "0.1 version"
git push
执行上面的命令把本地生成的文件上传到github。

## 3. 配置github地址为仓库地址
业务工程进行如下配置，配置可下载依赖的mvn仓库信息。
<repositories>
    <repository>
        <id>byebug-mvn-repo</id>
        <url>https://raw.githubusercontent.com/zhlu32/maven-repo/master/repository</url>
    </repository>
</repositories>

业务工程，配置依赖mvn仓库中刚上传的库信息。
<dependency>
    <groupId>com.byebug.automation</groupId>
    <artifactId>byebug-automation-lib</artifactId>
    <version>0.1</version>
</dependency>
