# FlashExcel
FlashExcel（闪电导表工具）是一个界面化的表格导出工具。根据游戏引擎和项目需求，程序可以很方便的编写自定义的导出器，
如果不想编写，那么也可以直接使用工具提供的几种默认的导出器。  

![image](https://github.com/gmhevinci/FlashExcel/raw/master/Docs/Image/img1.png)

## 工程
VS2017 && .net framework 4.6
1. NuGet package : NPOI
2. NuGet package : SharpZipLib

## 导出器
1. **TXT文本导出器**  
生成一个包含所有表格数据的纯文本，一般多用于C++逻辑服务器。  

2. L**UA脚本导出器**  
生成一个包含所有表格数据的Lua脚本。  

3. **ILR脚本导出器**  
生成一个包含所有表格数据的CS脚本，一般多用于支持ILRuntime的客户端框架。  

4. **BYTE文件导出器**  
生成一个包含所有表格数据的二进制格式的文件，需要CS脚本导出器配合。  

5. **CS脚本导出器**（需要MotionEngine.IO库）  
配合BYTE文件导出器，生成一个支持读取二进制数据的CS脚本。该CS脚本可以实现解析零GC。

## 自定义导出器
创建自定义的导出器非常简单，在工程目录里创建一个继承自BaseExporter的CS类，通过ExportAttribute属性可以设置导出器名称。脚本准备完毕之后我们再次运行程序，会发现导出器列表里已经有了我们自定义的导出器。
```C#
[ExportAttribute("导出TXT文件")]
class TextExporter : BaseExporter
{
	public TextExporter(SheetData sheetData)
	  : base(sheetData)
	{
	}
  
	public override void ExportFile(string path, string createLogo)
	{
          //可以参考默认自带的导出器来编写你的代码
	}
}
```

## 设置
整形和浮点型的数值单元格，如果策划忘记填写而为空，那么导出的时候工具会报错。我们也可以设置数值单元格自动补齐功能，来规避这个问题。

![image](https://github.com/gmhevinci/FlashExcel/raw/master/Docs/Image/img4.png)

## 多语言支持
工具提供了完整的多语言解决方案，自动生成多语言汇总表格。在我们导出选择的表格的时候，工具会自动把这些表格里的所有多语言列汇总到多语言总表。如果我们想导出所有表格里的多语言内容，那么可以点击界面里的[**多语言 自动化**]按钮，它会检查所有表格里的多语言列并进行汇总。

## 公式支持
单元格支持数值公式和字符串公式。例如：同一个Excel里的不同页签里的单元格可以通过Excel公式互相引用关联。

## 表格结构
表格结构支持定义各种类型：整型，浮点型，布尔，枚举，多语言，字符串，列表，自定义类型。  

![image](https://github.com/gmhevinci/FlashExcel/raw/master/Docs/Image/img2.png)

![image](https://github.com/gmhevinci/FlashExcel/raw/master/Docs/Image/img3.png)

1. 第一行为类型
2. 第二行为名称
3. 第三行为导出标记：客户端(C)，战斗服务器(B)，逻辑服务器(S)。

## 注意事项
Excel内可以设立多个页签，前缀t_的页签会被识别并导出，其它页签可以作为策划的辅助页签或备注页签
