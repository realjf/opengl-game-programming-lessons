# 课06纹理更新

旧课程06使用glaux库加载纹理。但是glau已被弃用很长时间了。

现在我们来看一下SOIL（http://www.lonesock.net/soil.html），这是一个非常有用的库，仅需一行代码即可加载所需的任何图像格式！

我们必须对旧课程06的代码进行的唯一更改是包括SOIL的头文件，而不是glaux。在本教程中，我们将假定SOIL.h和库SOIL.lib与Lesson06.cpp位于同一文件夹中（新的示例代码包括它们）。因此，在Lesson06.cpp的顶部，我们将#include <gl / glaux.h>更改为#include“ SOIL.h”。

下一步是针对SOIL库进行**链接**。在Visual Studio 2010中，其工作方式如下：右键单击左侧树状视图中的项目名称，然后选择**属性**。在“配置属性->链接器->输入->其他依赖项”下，我们通过编辑字段来添加SOIL.lib。

那么，现在我们如何使用该库从硬盘驱动器加载纹理？没有比这更容易的了。从Lesson06.cpp中完全删除方法LoadBMP，然后将LoadGLTextures方法替换为以下内容：

```c++
int LoadGLTextures()                                    // Load Bitmaps And Convert To Textures
{
    /* load an image file directly as a new OpenGL texture */
    texture[0] = SOIL_load_OGL_texture
        (
        "Data/NeHe.bmp",
        SOIL_LOAD_AUTO,
        SOIL_CREATE_NEW_ID,
        SOIL_FLAG_INVERT_Y
        );
 
    if(texture[0] == 0)
        return false;
 
 
    // Typical Texture Generation Using Data From The Bitmap
    glBindTexture(GL_TEXTURE_2D, texture[0]);
    glTexParameteri(GL_TEXTURE_2D,GL_TEXTURE_MIN_FILTER,GL_LINEAR);
    glTexParameteri(GL_TEXTURE_2D,GL_TEXTURE_MAG_FILTER,GL_LINEAR);
 
    return true;                                        // Return Success
}
```

加载纹理的整个任务现在隐藏在调用SOIL_load_OGL_texture中。它的参数首先是图像文件的文件名，然后是格式说明符，在这里我们可以告诉SOIL为我们创建灰度图像等，但我们希望该图像与磁盘上的图像一样，因此为SOIL_LOAD_AUT。

下一个参数告诉SOIL为此纹理创建一个新的ID。回顾第06课中的解释，每个OpenGL纹理都通过其自己的ID进行访问，如果我们使用glaux，则必须自己生成此ID。但是，如果我们说SOIL_CREATE_NEW_ID，那么SOIL会为我们做到这一点。

最后一个参数允许很多选项，所有选项都记录在头文件SOIL.h中。但是现在我们只需要一个选项，那就是翻转图像的y轴。原因是，几乎所有图像格式都是从左上角开始存储的，但是OpenGL在左下角定义了图像的原点。因此，垂直翻转它可以解决此差异。


<a href="/src/06-update.cpp" target="_blank">完整代码</a>
