# QFileDialog之文件对话框

包含头文件：

```
#include <QFileDialog>
```
## 文件打开对话框

```
QString  getOpenFileName (  QWidget * parent = 0,     
                            const QString & caption = QString(), 
                            const QString & dir = QString(), 
                            const QString & filter = QString(), 
                            QString * selectedFilter = 0, 
                            Options options = 0 )

```
1. 第一个参数parent，用于指定父组件。注意，很多Qt组件的构造函数都会有这么一个parent参数，并提供一个默认值0；
2. 第二个参数caption，是对话框的标题；
3. 第三个参数dir，是对话框显示时默认打开的目录，"." 代表程序运行目录，"/" 代表当前盘符的根目录(Windows，Linux下/就是根目录了)，也可以是平台相关的，比如"C:\\"等；
4. 第四个参数filter，是对话框的后缀名过滤器，多个文件使用空格分隔：比如我们使用"Image Files(*.jpg *.png)"就让它只能显示后缀名是jpg或者png的文件。多个过滤使用两个分号分隔：如果需要使用多个过滤器，使用";;"分割，比如"JPEG Files(*.jpg);;PNG Files(*.png)"；
5. 第五个参数selectedFilter，是默认选择的过滤器；
6. 第六个参数options，是对话框的一些参数设定，比如只显示文件夹等等，它的取值是enum QFileDialog::Option，每个选项可以使用 | 运算组合起来。

如果我要想选择多个文件怎么办呢？Qt提供了getOpenFileNames()函数，其返回值是一个QStringList。你可以把它理解成一个只能存放QString的List，也就是STL中的list<string>。

示例：


```
QString file_name = QFileDialog::getOpenFileName(this,
        tr("Open File"), 
        "",
        "", 
        0);
    if (!fileName.isNull())
    {
        //fileName是文件名
        ...
    }
    else{
        //点的是取消
        ...
    }
    
```

## 文件保存对话框


```
QString    getSaveFileName ( QWidget * parent = 0, 
                             const QString & caption = QString(), 
                             const QString & dir = QString(),
                             const QString & filter = QString(),
                             QString * selectedFilter = 0, 
                             Options options = 0 )

```
示例


```
    QString path;
    path = QFileDialog::getSaveFileName(this,
                                        tr("Choose where to save the configuration header file"),        /* 对话框的标题 */
                                        ".",                  /*.打开程序运行的目录 */
                                        tr("h files (*.h)")); /* 保存类型h */

    if (path.isNull()) {
        return;              /* path不是文件名直接返回 */
    }
    
   /* path是文件名往下执行 */ 
    
```


