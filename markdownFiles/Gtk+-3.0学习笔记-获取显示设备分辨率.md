# Gtk+-3.0学习笔记-获取显示设备分辨率


最近开始学习Linux下Gtk+编程，试着按照Gtk+官网的sample写了个窗口，期间想按照显示屏的分辨率自动最大化窗口。

网上查了下Gtk+如何获取分辨率，看到一网贴中的方法：
```c
GdkScreen *screen;
gint width, height;
screen = gdk_screen_get_default();
width = gdk_screen_get_width(screen);
height = gdk_screen_get_height(screen);
```

编译的时候，gcc报警说gdk_screen_get_width和gdk_screen_get_height已被弃用，到Gtk+官网看了下这两个函数，有以下说明：

    gdk_screen_get_width has been deprecated since  version 3.22 and should not be used in newly-written code.
    Use per-monitor information instead

原来自Gtk+-3.22版本开始，这两个函数已被启用。虽然现在仍然能够使用，但还是使用新方法获取分辨率比较好。

但是官方文档中只说使用per-monitor information代替，但是该如何获取这个per-monitor information呢？

后来在stackoverflow上找到了解决方法：

```c
GdkDisplay *display;
GdkMonitor *monitor;
GdkRectangle geometry;
int width, height;
display = gdk_display_get_default();
monitor = gdk_display_get_monitor(display, 0);
gdk_monitor_get_geometry(monitor, &geometry);
width = geometry.width;
height = geometry.height;
```

stackoverflow原贴地址：
https://stackoverflow.com/questions/43225956/how-to-get-the-size-of-the-screen-with-gtk
