# 应用属性对象
## 描述
 * 属性读取优先级
 	* 命令行属性>系统属性>环境变量>应用当前目录属性文件>资源路径属性文件
 	* 高优先级属性覆盖低优先级属性
 * 多环境配置 `java -jar xxx.jar --profiles.active=<ActiveName>.`
	* 启用后对应属性文件:`application[-<ActiveName>].properties`
	* 不指定环境配置时可以没有`application.properties`属性文件
 * 命令行属性 `java -jar xxx.jar --<PropertyName>=[PropertyValue]...`
 	* String属性"--" 开头： `--<PropertyName>=[PropertyValue]`
 	* boolean属性：`--<PropertyName>` 读取时值为true
 * 系统属性 System.getProperties()
 	* java自动获取系统属性
 	* 命令行赋值String属性`-D<PropertyName>=[PropertyValue]`
 * 环境变量 System.getenv()
 	* 系统及环境变量
 * 应用当前目录属性文件
 	* 对应路径 `System.getProperty("user.dir") 或 java -jar xxx.jar --profiles.path=<profiles.path>`
 	* 如果指定了`profiles.active` 那么对应属性文件必须存在
 	* 属性文件名称：`application[-<profiles.active>].properties`
 * 资源路径属性文件
 	* 对应路径 `Properties.class.getResource("/")`
	* 激活的属性文件属性会覆盖application.properties中的同名属性
 	* 属性文件名称：`application[-<profiles.active>].properties`
 * 支持属性中引用直接引用属性 `${PropertyName}`,运行期间会替换为属性值
 	`PropertyNameX=PropertyValueX`
 	`<PropertyNameY>=${PropertyNameX}-ok`  解析后：`<PropertyNameY>=PropertyValueX-ok`
 * 支持自动属性随机值
 	`<PropertyName>=${random.value}` 32位随机字符串:415a29f9e5dd38e23f2d4fd39e79a821<br>
    `<PropertyName>=${random.uuid}` uuid随机字符串:505a3dd5-7742-4a51-bac9-e14291582e49<br>
 	`<PropertyName>=${random.long}` 随机long值正数:0-MaxLongValue<br>
 	`<PropertyName>=${random.int}` 随机int值正数:0-MaxIntValue<br>
 	`<PropertyName>=${random.int[MaxBound>]}` 随机int值正数:0-MaxBound<br>
 	`<PropertyName>=${random.int[<MiniBound>,<MaxBound>]}` 随机int值正数:MiniBound-MaxBound<br>
## 演示
* 命令行
```
java -jar xxx.jar --app.user=admin -Dhttp.useproxy=true
```
* 属性文件
```properties
#application.properties
app.port=80
node.id=${app.user}-${random.uuid}
```

* 程序内读取属性
```java
    String user = Property.get("app.user","default");
    int port = Property.get("app.port",80);
    boolean useProxy = Property.get("http.useproxy",false);
    String id = Property.get("node.id","");
```
