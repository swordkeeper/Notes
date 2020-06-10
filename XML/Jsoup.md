# Jsoup解析器

#### 使用方式：

1. 导入``jar``包
2. 获取``document``对象，代表整个DOM树形结构
3. 获取对应标签

```java
/**
 * 使用 Jsoup 包对xml进行解析
 * 使用步骤：
 *      1. 导入包
 *      2. 创建DOM对象
 *      3. 获得标签数据
 */
public class testJsoup {
    public static void main(String[] args) throws IOException {
        // 获取路径testDTD1.xml 的path
        String path = testJsoup.class.getClassLoader().getResource("xml/testDTD1.xml").getPath();
        Document document = Jsoup.parse(new File(path), "utf-8");// 接受一个参数，文件路径和文件路径名
        Elements names = document.getElementsByTag("name");
        for(Element name: names){
            System.out.println(name);
            System.out.println("***");
            System.out.println(name.text());
            System.out.println("-------------");
        }
    }
}

```



#### 方法：

1. ``Jsoup.parse()``解析xml的文档

2. ``Document``对象，代表内存中解析出的DOM树 

    方法： 

    - getElementsBytag(string)
    - getElementsByAttribute(String key, String value ) 根据属性名称来获取
    - getElementById()

    

3. ``Elements``对象，代表``Array<Element>``来使用，表示查询出的某个节点集合

4. ``Element``元素，代表一个XML节点

     方法：

    - attr(String key) 根据属性名称来获取属性值
    - String text(). 获取文本内容

5. ``Node``对象，是这些节点的父节点



#### 查询方式

查询方式有两种：

1. ``selector``

    使用方式：

    - ``Elements selector(String cssQuery)``，通过类似于css的选择器方式查询。通过这种选择器，可以大大简化元素的查询

2. ``xpath``

    XPATH 是一种专门用来查询 XML路径的语言。可以用来获取XML的节点 。需要添加``jar``包

    1. 创建一个``JXDocument``
    2. 语法：
        - ``//student``：查询所有student标签，不管位置 。双 ``//``代表所有，不论层级
        - ``//student/name``：查询所有student标签下的name， 不管student在哪。单``/``也是所有但是表示子元素。考虑层级
        - ``//student/name[@id]``：查询所有学生标签下的name标签，并且带有id属性