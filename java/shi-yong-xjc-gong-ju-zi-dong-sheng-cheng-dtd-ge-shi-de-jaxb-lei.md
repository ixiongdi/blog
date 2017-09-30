**文档类型定义（DTD）可定义合法的XML文档构建模块。它使用一系列合法的元素来定义文档的结构。**

**DTD 可被成行地声明于 XML 文档中，也可作为一个外部引用。**

我们先来看一下一个典型 的DTD文档定义：

```
<!ELEMENT note (to,from,heading,body)>
<!ELEMENT to (#PCDATA)>
<!ELEMENT from (#PCDATA)>
<!ELEMENT heading (#PCDATA)>
<!ELEMENT body (#PCDATA)>
```

新建个文件post.dtd

```bash
touch post.dtd
```

然后把刚才的文件定义写入文件。

xjc是一个java命令行工具，用户把XML格式定义的数据结构生成生成 JAXB 类。只要你安装了JDK就会有这个工具

我们先来看看xjc具有哪些功能

```
用法: xjc [-options ...] <schema file/URL/dir/jar> ... [-b <bindinfo>] ...
如果指定 dir, 将编译该目录中的所有模式文件。
如果指定 jar, 将编译 /META-INF/sun-jaxb.episode 绑定文件。
选项:
  -nv                :  不对输入模式执行严格验证
  -extension         :  允许供应商扩展 - 不严格遵循
                        JAXB 规范中的兼容性规则和应用程序 E.2
  -b <file/dir>      :  指定外部绑定文件 (每个 <file> 必须具有自己的 -b)
                        如果指定目录, 则将搜索 **/*.xjb
  -d <dir>           :  生成的文件将放入此目录中
  -p <pkg>           :  指定目标程序包
  -httpproxy <proxy> :  设置 HTTP/HTTPS 代理。格式为 [user[:password]@]proxyHost:proxyPort
  -httpproxyfile <f> :  作用与 -httpproxy 类似, 但在文件中采用参数来保护口令
  -classpath <arg>   :  指定查找用户类文件的位置
  -catalog <file>    :  指定用于解析外部实体引用的目录文件
                        支持 TR9401, XCatalog 和 OASIS XML 目录格式。
  -readOnly          :  生成的文件将处于只读模式
  -npa               :  禁止生成程序包级别注释 (**/package-info.java)
  -no-header         :  禁止生成带有时间戳的文件头
  -target (2.0|2.1)  :  行为与 XJC 2.0 或 2.1 类似, 用于生成不使用任何 2.2 功能的代码。
  -encoding <encoding> :  为生成的源文件指定字符编码
  -enableIntrospection :  用于正确生成布尔型 getter/setter 以启用 Bean 自测 apis 
  -contentForWildcard  :  为具有多个 xs:any 派生元素的类型生成内容属性
  -xmlschema         :  采用 W3C XML 模式处理输入 (默认值)
  -relaxng           :  采用 RELAX NG 处理输入 (实验性的, 不支持)
  -relaxng-compact   :  采用 RELAX NG 简洁语法处理输入 (实验性的, 不支持)
  -dtd               :  采用 XML DTD 处理输入 (实验性的, 不支持)
  -wsdl              :  采用 WSDL 处理输入并编译其中的模式 (实验性的, 不支持)
  -verbose           :  特别详细
  -quiet             :  隐藏编译器输出
  -help              :  显示此帮助消息
  -version           :  显示版本信息
  -fullversion       :  显示完整的版本信息


扩展:
  -Xinject-code      :  inject specified Java code fragments into the generated code
  -Xlocator          :  enable source location support for generated code
  -Xsync-methods     :  generate accessor methods with the 'synchronized' keyword
  -mark-generated    :  mark the generated code as @javax.annotation.Generated
  -episode <FILE>    :  generate the episode file for separate compilation
  -Xpropertyaccessors :  Use XmlAccessType PROPERTY instead of FIELD for generated classes

```

默认情况下xjc是吧.xsd格式的数据生成JAXB类，如果要使用.dtd生成JAXB类，需要使用参数\`-dtd\`

```bash
xjc -dtd post.dtd

```

然后生成了如下两个类：

```
generated/Note.java
generated/ObjectFactory.java

```

我们来看看生成的类Note.java

```java
//
// 此文件是由 JavaTM Architecture for XML Binding (JAXB) 引用实现 v2.2.8-b130911.1802 生成的
// 请访问 <a href="http://java.sun.com/xml/jaxb">http://java.sun.com/xml/jaxb</a> 
// 在重新编译源模式时, 对此文件的所有修改都将丢失。
// 生成时间: 2017.09.30 时间 04:04:53 PM CST 
//


package generated;

import javax.xml.bind.annotation.XmlAccessType;
import javax.xml.bind.annotation.XmlAccessorType;
import javax.xml.bind.annotation.XmlElement;
import javax.xml.bind.annotation.XmlRootElement;
import javax.xml.bind.annotation.XmlType;


/**
 * 
 */
@XmlAccessorType(XmlAccessType.FIELD)
@XmlType(name = "", propOrder = {
    "to",
    "from",
    "heading",
    "body"
})
@XmlRootElement(name = "note")
public class Note {

    @XmlElement(required = true)
    protected String to;
    @XmlElement(required = true)
    protected String from;
    @XmlElement(required = true)
    protected String heading;
    @XmlElement(required = true)
    protected String body;

    /**
     * 获取to属性的值。
     * 
     * @return
     *     possible object is
     *     {@link String }
     *     
     */
    public String getTo() {
        return to;
    }

    /**
     * 设置to属性的值。
     * 
     * @param value
     *     allowed object is
     *     {@link String }
     *     
     */
    public void setTo(String value) {
        this.to = value;
    }

    /**
     * 获取from属性的值。
     * 
     * @return
     *     possible object is
     *     {@link String }
     *     
     */
    public String getFrom() {
        return from;
    }

    /**
     * 设置from属性的值。
     * 
     * @param value
     *     allowed object is
     *     {@link String }
     *     
     */
    public void setFrom(String value) {
        this.from = value;
    }

    /**
     * 获取heading属性的值。
     * 
     * @return
     *     possible object is
     *     {@link String }
     *     
     */
    public String getHeading() {
        return heading;
    }

    /**
     * 设置heading属性的值。
     * 
     * @param value
     *     allowed object is
     *     {@link String }
     *     
     */
    public void setHeading(String value) {
        this.heading = value;
    }

    /**
     * 获取body属性的值。
     * 
     * @return
     *     possible object is
     *     {@link String }
     *     
     */
    public String getBody() {
        return body;
    }

    /**
     * 设置body属性的值。
     * 
     * @param value
     *     allowed object is
     *     {@link String }
     *     
     */
    public void setBody(String value) {
        this.body = value;
    }

}

```

这样一个标准的JAXB类就生成好了，可以序列化为xml文档了。

