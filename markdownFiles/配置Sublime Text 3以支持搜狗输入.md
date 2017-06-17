###1. 在用户主目录～下新建文件sublime_imfix.c，并将一下代码保存到文件中去。
```c
#include <gtk/gtkimcontext.h>
void gtk_im_context_set_client_window (GtkIMContext *context,GdkWindow *window)
{
 GtkIMContextClass *klass;
 g_return_if_fail (GTK_IS_IM_CONTEXT (context));
 klass = GTK_IM_CONTEXT_GET_CLASS (context);
 if (klass->set_client_window)
   klass->set_client_window (context, window);
 g_object_set_data(G_OBJECT(context),"window",window);
 if(!GDK_IS_WINDOW (window))
   return;
 int width = gdk_window_get_width(window);
 int height = gdk_window_get_height(window);
 if(width != 0 && height !=0)
   gtk_im_context_focus_in(context);
}
```
###2. 打开终端，执行命令：
```c
gcc -shared -o libsublime-imfix.so sublime_imfix.c `pkg-config --libs --cflags gtk+-2.0` -fPIC
```
###3. 将编译生成的libsublime-imfix.so文件拷贝到sublime3目录里去
`mv libsublime-imfix.so /opt/sublime3/`
###4. 在/usr/bin目录里追加文件sublime3文件并将以下内容保存到文件中
```bash
#!/bin/sh
LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text "$@"
```
###5. 增加执行权限
`chmod a+x /usr/bin/sublime3`
###6. 在mainmenu里面添加新的sublime启动链接(如未安装mainmenu，执行`apt-get install alacarte`进行安装)
![图片1](http://img.blog.csdn.net/20161023110244357)   

![图片2](http://img.blog.csdn.net/20161023110139558)

---
###PS.
- 首先确认有无安装gcc，若还未安装，执行`apt-get install gcc`以安装。   
- 如果编译过程中报错说gtkimcontext.h找不到，可能没有安装gtk，执行`apt-get install libgtk+2.0-dev`