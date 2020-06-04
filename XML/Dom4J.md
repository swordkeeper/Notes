# DOM4J

Dom4J模型是 一种Java 操作XML的Java库。它是开源的。

Dom( Document Object Model) ，它定了了访问和操作 xml 文档树的方法。



1. Dom4J 将整个 XML当成 ``Document`` 节点
2. Dom4J 将XML中的元素当成``Element``节点



例子, 读

```java
import java.util.List;
import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

public class XMLReader {
	public void readXML() {
		String file = "/Users/chenrui/eclipse-workspace/myProject/src/com/xml/myXML.xml";
		
        // 用dom4j包下的 SAXReader 类，定义一个XML 阅读器
        SAXReader reader = new SAXReader();
        try {
            // 读一个XML文件，获得根节点，该节点就是Document类型，即整个文档为一个Document 类型的对象
            Document document = reader.read(file);
            // 获取根节点元素 Element的对象
            Element root = document.getRootElement();
            // 获取所有course 子节点
            List<Element> courses = root.elements("course");
            for(Element course: courses) {
                
                // 循环遍历每一个子节点
                Element courseNameElement = course.element("course-name");
                String courseName = courseNameElement.getText();
                String courseForm = course.elementText("course-form");
                String courseHour = course.elementText("course-hour");
                System.out.println(courseName+" "+courseForm+" "+courseHour);
                
                // 获取属性
                Attribute attr = course.attribute("id");
				String attrDetail = attr.getText();
				System.out.println(attrDetail+"\n--------------");
            }
        } catch (DocumentException e) {  // XML结构有问题，会报错
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
	}
	public static void main(String[] args) {
		new XMLReader().readXML();
	}
}

```



例子，写

```java
String file = "/Users/chenrui/eclipse-workspace/myProject/src/com/xml/myXML.xml";
		
			// 用dom4j包下的 SAXReader 类，定义一个XML 阅读器
			SAXReader reader = new SAXReader();
			try{
				// 读一个XML文件，获得根节点，该节点就是Document类型
				// 即整个文档为一个Document 类型的对象
				Document document = reader.read(file);
				// 获取根节点元素 Element的对象
				Element root = document.getRootElement();
				Element newCourse = root.addElement("course");
				newCourse.addAttribute("id", "332");
				Element newCourseName = newCourse.addElement("course-name");
				newCourseName.setText("美术");
				newCourse.addElement("course-form").setText("online");
				newCourse.addElement("course-hour").setText("2");
				// 定义一个写 字符流
				Writer writer = new OutputStreamWriter(new FileOutputStream(file), "UTF-8");
				document.write(writer);
				writer.close();
              }catch....
```



