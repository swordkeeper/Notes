#   XML

XML 是 ``Extensible Markup Lanuage``的缩写，又称为可扩展标记语言。

XML 的主要用途是用来``存储``和``传递数据``的。它不同于HTML语言。HTML语言主要用于显示数据。

本质上XML 和 HTML 是相同的，都是有标签组织成的语言。



### XML 表头

```xml
<?xml version="1.0" encoding="UTF-8" ?>     <!--该代码必须出现在xml文件头-->
```



### XML 简单例子

```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<class>
    <student>
        <name>张三</name>
        <age>21</age>
        <gender>男</gender>
    </student>
    <student>
        <name>李四<</name>
         <age>16</age>
        <gender>女</gender>
    </student>
    <teacher id="21100">
        <name>老张</name>
        <title>教授</title>
        <course>体育</course>
     </teacher>
     <![CDATA[此处是CDTAT中的数据，其中的数据不会被转义，包括<  > & 等]]>
</class>
    
<!-- 对与 < > 等需要转义的字符. 需要用 &lt;  &gt; 等. ->
<!-- 对于这类特殊的字符。也可用CDATA标签来指明。  CDTAT标签中的字符，不会被转义 -->

```



### DTD 文档类型定义

DTD``(Document Type Definition)``，文档定义类型。DTD是对XML中定义的文档进行``约束``的一种元语言。

由于XML的语法很简单，只要满足基本标签配对即可满足XML的需求。但是，有的时候需要对XML所保存的数据进行的严格限制。比方说如上例，只有班级中才能有``<student>``标签和``<teacher>``标签。这两个标签不能脱离``<class>``标签存在。这种关系就可以用DTD来定义

DTD通常是以单独的文件来保存。以``.dtd``为扩展名.

#### DTD 子元素约束

```dtd
<!ELEMENT 父节点 (子节点) >    <!--标示父节点下可以拥有那种子节点， 例如-->
<!ELEMENT class (teacher, student) >   <!-- class 节点之下。有teacher和student节点，且只能有一个teacher和student  -->
<!ELEMENT class (teacher+, student*) > <!-- 通配符表示，有至少一个teacher，0～n个student  -->
<!ELEMENT class (blackboard?) >.  <!-- ？标示 0～1 个 -->
<!ElEMENT employee (name, age, salary, department) > <!-- 必须按照此顺序书写. -->

<!ELEMENT article (#PCDATA) >      <!-- #PCDATA 表示，article下面没有子元素，它自己是一个纯文本元素 -->
```

#### DTD 属性约束

```dtd
<!ATTLIST employee no CDATA "" > 
<!-- ATTLIST 标示对元素属性的约束，此处标示 employee标签的 有no属性，属性类型为 文本CDATA类型 默认为 ""空串 -->
```



### XML 中引用 DTD

```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE 根节点 SYSTEM "dtd文件路径" >

<!-- 例如。-->
<!DOCTYPE class SYSTEM "./class.dtd" > <!--加载当前目录下class.dtd文件下的 class元素的约束 到 STSTEM 本地范围 -->
<!-- 网络来的dtd -->
<!DOCTYPE class PUBLIC "class.dtd文件名" "dtd文件位置网络URL" > 
```



### XML Schema 

XML Schema 是一门更为严谨的约束语言，他比DTD有着更好的约束性

XML schema 例子如下：

```hr.xsd```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 文件的后缀为   .xsd   -->
<schema xmlns="http://www.w3.org/2001/XMLSchema" > <!-- 以schema 为跟节点 -->
	<element name="hr">
		<!-- 根节点，跟节点名字为hr -->
		<complexType>  
			<!-- 定义complexType标签，表示element-hr 
				下面包含的内容是一个复杂的多标签的类型，拥有子节点-->
			<sequence> <!-- 序列标签，表示里面的内容需按照顺序 -->
				<element name="employee" minOccurs="1" maxOccurs="999"> <!-- minOccurs 为element的个数提供范围 -->
                    <complexType>
                        <sequence>
							<element name="name" type="string"></element>
							<element name="age" type="integer">
                                <simpleType>  <!-- 对年龄进行 简单类型限制 限制整数 范围 18~60 -->
                                    <restriction base="integer">
                                        <minInclusive value="18"></minInclusive>
                                        <maxInclusive value="60"></maxInclusive>
                                    </restriction>
                                </simpleType>
                            </element>
							<element name="salary" type="integer"></element>
							<element name="department">
								<complexType>
									<sequence>
										<element name="dname" type="string"></element>
										<element name="address" type="string"></element>
									</sequence>
								</complexType>
							</element>
						</sequence>
                        <attribute name="no" type="string" use="required"></attribute>
                    <!-- 为employee标签添加属性，名为no ，字符串类型， use="required"表示该属性必须 -->
						
					</complexType>
				</element>
			</sequence>
		</complexType>
	</element>
</schema>
```

在xml文件中引用该schema文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<hr xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xsi:noNamespaceSchemaLocation="./hr.xsd" >
	<!-- 在根标签中添加   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			并添加 xsi:noNamespaceSchemaLocation="./hr.xsd"，表示查找约束文件路径 -->
</hr>
```

