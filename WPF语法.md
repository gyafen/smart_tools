#### 1. 入门

##### 1.1. APP.xaml和Application类

xaml类型的文件分为两部分，一部分以xaml扩展名为结尾的前端代码，另一部分以xaml.cs结尾的后端代码。

XAML——Extensible Application Markup Language。

```xaml
//前端代码
<Application x:Class="HelloWorld.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:HelloWorld"
             StartupUri="MainWindow.xaml">
    <Application.Resources>
         
    </Application.Resources>
</Application>
```

```c#
//后端代码
using System;
using System.Collections.Generic;
using System.Configuration;
using System.Data;
using System.Linq;
using System.Threading.Tasks;
using System.Windows;
 
namespace HelloWorld
{
    /// <summary>
    /// App.xaml 的交互逻辑
    /// </summary>
    public partial class App : Application
    {
    }
}
```

上述后端代码中，定义了App类型，并使用partial关键字修饰，表明它是一个局部类型，App继承Application父类。

> partial的作用：在C#中，partial关键字可以将一个类、结构体、接口或者方法分为多个部分进行声明，这些部分可以在同一个源文件中，也可以在不同的源文件中。

Q：前端如何定义类型？

A：在xaml代码中，可以看到有一个`<application>`的标签，同时还有一句话 `:x:Class="HelloWorld.App"`，它定义了一个叫App的类型，这个类型位于命名空间`HelloWorld`中，与后端代码的`namespace HelloWorld`保持一致。

> Application是个什么东西？

```C#
namespace System.Windows
{
    //
    // 摘要:
    //     封装 Windows Presentation Foundation (WPF) 应用程序。
    public class Application : DispatcherObject, IHaveResources, IQueryAmbient
    {
        [SecurityCritical]
        public Application();
        //获取或设置 System.Reflection.Assembly 提供包 统一资源标识符 (URI) 中的资源 WPF 应用程序。        
        public static Assembly ResourceAssembly { get; set; }
 
        //获取 System.Windows.Application 当前对象 System.AppDomain。
        public static Application Current { get; }
 
        //获取应用程序中实例化的窗口。
        public WindowCollection Windows { get; }
 
        //获取或设置该应用程序的主窗口。
        public Window MainWindow { get; set; }
 
        //获取或设置导致的情况， System.Windows.Application.Shutdown 来调用方法。
        public ShutdownMode ShutdownMode { get; set; }
 
        //获取或设置应用程序范围的资源，如样式和画笔的集合。
        [Ambient]
        public ResourceDictionary Resources { get; set; }
 
        //获取或设置 UI 一个应用程序启动时自动显示。
        public Uri StartupUri { get; set; }
 
        //获取应用程序作用域属性的集合。
        public IDictionary Properties { get; }
 
        
        public event EventHandler Deactivated;
        public event SessionEndingCancelEventHandler SessionEnding;
        public event DispatcherUnhandledExceptionEventHandler DispatcherUnhandledException;
        public event NavigatingCancelEventHandler Navigating;
        public event NavigatedEventHandler Navigated;
        public event NavigationProgressEventHandler NavigationProgress;
        public event NavigationFailedEventHandler NavigationFailed;
        public event LoadCompletedEventHandler LoadCompleted;
        public event EventHandler Activated;
        public event NavigationStoppedEventHandler NavigationStopped;
        public event FragmentNavigationEventHandler FragmentNavigation;
 
        public static StreamResourceInfo GetContentStream(Uri uriContent);
        public static string GetCookie(Uri uri);
        public static StreamResourceInfo GetRemoteStream(Uri uriRemote);
        public static StreamResourceInfo GetResourceStream(Uri uriResource);
        public static object LoadComponent(Uri resourceLocator);
        public static void LoadComponent(object component, Uri resourceLocator);
        public static void SetCookie(Uri uri, string value);
        public object FindResource(object resourceKey);
        public int Run(Window window);
        public int Run();
        public void Shutdown();
        public void Shutdown(int exitCode);
        public object TryFindResource(object resourceKey);
        protected virtual void OnActivated(EventArgs e);
        protected virtual void OnDeactivated(EventArgs e);
        protected virtual void OnExit(ExitEventArgs e);
        protected virtual void OnFragmentNavigation(FragmentNavigationEventArgs e);
        protected virtual void OnLoadCompleted(NavigationEventArgs e);
        protected virtual void OnNavigated(NavigationEventArgs e);
        protected virtual void OnNavigating(NavigatingCancelEventArgs e);
        protected virtual void OnNavigationFailed(NavigationFailedEventArgs e);
        protected virtual void OnNavigationProgress(NavigationProgressEventArgs e);
        protected virtual void OnNavigationStopped(NavigationEventArgs e);
        protected virtual void OnSessionEnding(SessionEndingCancelEventArgs e);
        protected virtual void OnStartup(StartupEventArgs e);
 
    }
}
```

Application类继承于DispatcherObject父类，DispatcherObject是WPF中最终抽象类。

Application类是封装Windows Presentation Foundation的应用程序，我们开发一款 WPF应用程序，本质上是继承这个类。

> Application中的重要属性：
>
> - StartupUri——指定应用程序第一次启动时显示的用户界面
> - Resources——窗体的背景颜色、字体大小、模板样式等等，都放在Resources中

##### 1.2. Application的生命周期

StartupUri="MainWindow.xaml"表示程序启动的第一个窗体是MainWindow。

```C#
using System;
using System.Collections.Generic;
using System.Configuration;
using System.Data;
using System.Linq;
using System.Threading.Tasks;
using System.Windows;
 
namespace HelloWorld
{
    /// <summary>
    /// App.xaml 的交互逻辑
    /// </summary>
    public partial class App : Application
    {
        protected override void OnStartup(StartupEventArgs e)
        {
            base.OnStartup(e);
            Console.WriteLine("1.OnStartup被触发");
        }
 
        protected override void OnActivated(EventArgs e)
        {
            base.OnActivated(e);
            Console.WriteLine("2.OnActivated被触发");
        }
 
        protected override void OnDeactivated(EventArgs e)
        {
            base.OnDeactivated(e);
            Console.WriteLine("3.OnDeactivated被触发");
        }
 
        protected override void OnExit(ExitEventArgs e)
        {
            base.OnExit(e);
            Console.WriteLine("4.OnExit被触发");
        }
    }
}
```

Application的生命周期：OnStartup→OnActivated→OnDeactivated→OnExit。

##### 1.3. 窗体的生命周期

Window窗体，也是空间，窗体随着用户创建而生于内存，最后也被销毁于内存，GC垃圾回收器干活。

```C#
namespace HelloWorld
{
    /// <summary>
    /// MainWindow.xaml 的交互逻辑
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }
    }
}
```

MainWindow继承于Window类。

MianWindow的继承路线：MainWindow→Window→ContenControl→Control→FrameworkElement→UIElement→Visual→DepedencyObject→DispatcherObject。

| 事件              | 含义                                           |
| ----------------- | ---------------------------------------------- |
| SourceInitialized | 创建窗体源时发生此事件                         |
| Activated         | 当前窗体成为前台窗体时触发此事件               |
| Loaded            | 当前窗体内部所有元素完成布局和呈现时发生此事件 |
| ContentRendered   | 当前窗体的内容呈现之后发生此事件               |
| Closing           | 当前窗体关闭之后发生此事件                     |
| Deactivated       | 当前窗体成为后台窗体时发生此事件               |
| Closed            | 当前窗体关闭时发生此事件                       |
| Unloaded          | 当前窗体从元素树中删除时引发此事件             |

##### 1.4. window窗体的组成

window窗体本质上是一个控件，只不过和一般的控件有所区别，比如它有closing和closed事件，而一般控件是不可以关闭的，另外，window窗体可以容纳其他的控件。最后，窗体分为两个部分，工作区和非工作区。

###### 非工作区

包含：图标、标题、窗体菜单、最小化按钮、最大化按钮、关闭按钮、窗体边框、右下角鼠标拖动调整窗体尺寸。

###### 工作区

window窗体的工作区是指Content属性。Content属性表示窗体的内容，类型为object，即可以是任意的引用类型。需要注意的是Content属性并不在Window类中，而是在他的父类ContentControl中。

> 默认的`<window></window>`之中只能存在一个控件，就是因为Content是object类型，意思是只接受一个对象。如何向窗体增加多个控件？可以用grid布局控件。

#### 2. 控件父类

##### 2.1. 控件的父类们

C#的终极父类是Object，WPF中的终极父类是DispatcherObject，DispatcherObject继承于Object。

先看几个控件的继承关系：

> **Button→**
>
> ​	ButtonBase→
>
> ​			ContentControl→
>
> ​						Control→
>
> ​							FrameworkElement→UIElement→Visual→DependencyObject→DispatcherObject

> **StackPanel→**
>
> ​		Panel→
>
> ​			FrameworkElement→UIElement→Visual→DependencyObject→DispatcherObject

> **Rectangle→**
>
> ​		Shape→
>
> ​			FrameworkElement→UIElement→Visual→DependencyObject→DispatcherObject

​			

发现Button、StackPanel、Rectangle三个控件都继承于**FrameworkElement→UIElement→Visual→DependencyObject→DispatcherObject**

因此可以得出结论，控件的父类们至少有如下几个类型：

- DispatcherObject
- DependencyObject
- Visual
- UIElement
- FrameworkElement

WPF几乎所有的控件都继承于这5个父类：

<img src="assets\2023081203432433.png" alt="img" style="zoom: 80%;" />

##### 2.2. DispatcherObject类

.NET为WPF准备了两个线程，分别用于呈现界面（后台线程）和管理界面（UI线程）。后台线程一直隐藏于后台默默运行，我们感知不到，我们能感知到的是UI线程。

绝大多数控件是在UI线程上创建，而且，其他后台子线程不能直接访问UI线程上的控件，那么后台线程非要访问UI线程上的控件，该咋办？微软提供了一个中间商：Dispatcher，将Dispatcher放到一个抽象类DispatcherObject，然后保证所有的控件都从DispatcherObject继承，这样当后台线程要访问控件时，就可以通过Dispatcher来访问了。Dispatcher是DispatcherObject的成员，提供了Invoke和BeginInvoke供我们安全访问UI线程中的控件。

看如下的例子：

前端代码：

```xaml
<Window x:Class="lrc_convert.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:lrc_convert"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <Button x:Name="button" Content="START"/>
    </Grid>
</Window>

```

后端代码：

```C#
namespace lrc_convert
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            Task.Factory.StartNew(() =>
            {
                Task.Delay(3000).Wait();
                button.Dispatcher.Invoke(() =>
                {
                    button.Content = "SMART";
                });
            });

        }
    }
}
```

运行之后3秒，按钮的内容从“START”变成了“SMART”。

<img src="assets\image-20231220230940742.png" alt="image-20231220230940742" style="zoom: 50%;" />

Task工厂创建子线程，然后，调用了button的Dispatcher调度员，其Invoke方法中传入了一个匿名函数，这个匿名函数中改变了button的content属性。

我们主要关心：button继承了DispatcherObject类，因此其中含有Dispatcher成员。

**DispatcherObject类的两个作用：**

- 提供对对象所关联的当前的Dispatcher的访问权限，意思就是谁继承了它，谁就拥有了Dispatcher。

- 提供方法以检查 (CheckAccess) 和验证 (VerifyAccess) 某个线程是否有权访问对象（派生于 DispatcherObject）。CheckAccess 与 VerifyAccess 的区别在于 CheckAccess 返回一个布尔值，表示当前线程是否有可以使用的对象，而 VerifyAccess 则在线程无权访问对象的情况下引发异常。

##### 2.3. DependencyObject类

微软在WPF框架中，推出了数据驱动模式。数据驱动模式是指控件的属性不再被直接赋值，而是绑定了另外一个变量，当这个变量发生改变时 ，控件的属性也会跟着改变，这样的属性被称为是依赖属性。WPF的所有控件都可以使用数据驱动模式，因为所有的控件都继承了DependencyObject这个基类。

> DependencyObject类表示参与依赖属性系统的对象。属性系统的主要功能是计算属性的值，并提供有关已经更改的值的系统通知。参与属性系统的另一个类DependencyProperty允许将依赖属性注册到属性系统，并提供有关每个依赖属性的标识和信息，而DependencyObject为基类，使对象能够使用此依赖属性。

DependencyObject的定义中，比较常用的是GetValue和SetValue。GetValue表示获取某一个依赖属性的值，由于不确定这个值是什么类型，因此微软把这个函数的返回值设置为Object。SetValue表示设置某一个依赖属性的值，它有两个参数，第一个参数dp表示要设置的依赖属性，第二个参数value表示新值。

##### 2.4. Visual类

Visual是WPF框架中的第三个父类，主要是为WPF中的呈现提供支持，其中包含命中测试 、坐标转换和边界框计算它位于程序集：PresentationCore.dll库文件中，他的命名空间：System.Windows.Media。

































