---

---

# Jvppeteer
<p align = "left">
<a rel="nofollow" href="https://download-chromium.appspot.com/"><img src ="https://img.shields.io/badge/chromium%20download-latest-blue"  alt="下载最新版本的chromuim" style="max-width:100%;"></a> <a><img alt="Maven Central" src="https://img.shields.io/maven-central/v/io.github.fanyong920/jvppeteer"></a> <a href="https://github.com/fanyong920/jvppeteer/issues"><img alt="Issue resolution status" src="https://img.shields.io/github/issues/fanyong920/jvppeteer" style="max-width:100%;"></a>
    <a href="https://sonarcloud.io/dashboard?id=fanyong920_jvppeteer"><img alt="Quality Gate Status" src="https://sonarcloud.io/api/project_badges/measure?project=fanyong920_jvppeteer&metric=alert_status" style="max-width:100%;"></a>
</p>


## 注意
>通过maven导入的jar包，1.1.5及之前的版本，都存在linux上杀不死chrome的bug，可以通过<a href="https://github.com/fanyong920/jvppeteer/blob/master/1.1.5%E7%89%88%E6%9C%AC%E4%B9%8B%E5%89%8D%E7%9A%84%E5%86%85%E5%AD%98%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88.md" alt="链接"> 1.1.5版本之前的内存问题解决方案</a> 自行解决，仓库的代码已经将解决方案代码加上了，拉取下来打jar也可以用。


**本库的灵感来自 [Puppeteer(Node.js)](https://github.com/puppeteer/puppeteer), API 也与其基本上保持一致，做这个库是为了方便使用 Java 操控 Chrome 或 Chromium**




   >Jvppeteer 通过 [DevTools](https://chromedevtools.github.io/devtools-protocol/) 控制 Chromium 或 Chrome。
   >默认情况下，以 headless 模式运行，也可以通过配置运行'有头'模式。


你可以在浏览器中手动执行的绝大多数操作都可以使用 Jvppeteer 来完成！ 下面是一些示例：

- 生成页面 PDF。
- 抓取 SPA（单页应用）并生成预渲染内容（即“SSR”（服务器端渲染））。
- 自动提交表单，进行 UI 测试，键盘输入等。
- 创建一个时时更新的自动化测试环境。 使用最新的 JavaScript 和浏览器功能直接在最新版本的 Chrome 中执行测试。
- 捕获网站的 [timeline trace](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference)，用来帮助分析性能问题。
- 测试浏览器扩展。

## 开始使用



**注意：Mac必须withExcutablePath是用來指定启动Chrome.exe的路径。在Mac下BrowserFetcher.downloadIfNotExist(null)有问题。**



### 以下是使用依赖管理工具（如 maven 或 gradle）的简要指南。

#### Maven
要使用 maven,请将此依赖添加到pom.xml文件中：

```xml
<dependency>
  <groupId>io.github.fanyong920</groupId>
  <artifactId>jvppeteer</artifactId>
  <version>1.1.6</version>
</dependency>
```

#### Gradle

要使用 Gradle，请将 Maven 中央存储库添加到您的存储库列表中:

```
mavenCentral（）
```

然后，您可以将最新版本添加到您的构建中。

```xml
compile "io.github.fanyong920:jvppeteer:1.1.5"
```

#### Logging

该库使用 [SLF4J](https://www.slf4j.org/) 进行日志记录，并且不附带任何默认日志记录实现。

调试程序将日志级别设置为 TRACE。

#### 独立 jar

如果您不使用任何依赖项管理工具，则可以在[此处](https://github.com/fanyong920/jvppeteer/releases/latest)找到最新的独立 jar 。

### 快速开始

#### 1、启动浏览器

```java
	//设置基本的启动配置,这里选择了‘有头’模式启动
	ArrayList<String> argList = new ArrayList<>();
    //withExecutablePath 是指定chrome的启动路径
    //chrome的启动路径可以从以下几方面之一去设置：
    //1.直接指定。使用LaunchOptions的withExecutablePath方法
    //2.配置环境变量。在Constant.EXECUTABLE_ENV可以找到配置环境变量的key
    //3.使用BrowserFetcher.downloadIfNotExist()下载。如果使用使用BrowserFetcher.downloadIfNotExist()下载了chrome,在1和2选项都没有配置的情况下，才会使用下载的chrome路径;
    //4.搜索电脑上采用默认安装的chrome路径。 1、2、3选项都没有配置的情况下，会自动搜索，此项不用任何操作
    LaunchOptions options = new LaunchOptionsBuilder().withArgs(argList).withHeadless(false).withExecutablePath("C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe").build();
    argList.add("--no-sandbox");
    argList.add("--disable-setuid-sandbox");
    Puppeteer.launch(options);
```

在这个例子中，我们明确指明了启动路径，程序就会根据指明的路径启动对应的浏览器，如果没有明确指明路径，那么程序会尝试启动默认安装路径下的 Chrome 浏览器

#### 2、导航至某个页面

```java
	//withExecutablePath 是指定chrome的启动路径
    ArrayList<String> argList = new ArrayList<>();
    LaunchOptions options = new LaunchOptionsBuilder().withArgs(argList).withExecutablePath("C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe").withHeadless(false).build();
    argList.add("--no-sandbox");
    argList.add("--disable-setuid-sandbox");
    Browser browser = Puppeteer.launch(options);
    Browser browser2 = Puppeteer.launch(options);
    Page page = browser.newPage();
    page.goTo("https://www.taobao.com/about/");
    browser.close();

    Page page1 = browser2.newPage();
    page1.goTo("https://www.taobao.com/about/");
```

这个例子中，浏览器导航到具体某个页面后关闭。在这里并没有指明启动路径。argList是放一些额外的命令行启动参数的，在下面资源章节中我会给出相关资料。

#### 3、生成页面的 PDF

```java

    ArrayList<String> arrayList = new ArrayList<>();
    //生成pdf必须在无厘头模式下才能生效
    LaunchOptions options = new LaunchOptionsBuilder().withArgs(arrayList).withHeadless(true).withExecutablePath("C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe").build();
    arrayList.add("--no-sandbox");
    arrayList.add("--disable-setuid-sandbox");
    Browser browser = Puppeteer.launch(options);
    Page page = browser.newPage();
    page.goTo("https://www.baidu.com/?tn=98012088_10_dg&ch=3");
    PDFOptions pdfOptions = new PDFOptions();
    pdfOptions.setPath("test.pdf");
    page.pdf(pdfOptions);
    page.close();
    browser.close();
```

在这个例子中，导航到某个页面后，将整个页面截图，并写成PDF文件。注意，生成PDF必须在headless模式下才能生效

#### 4、TRACING 性能分析

```java

    //即使不指定chrome路径，程序也会寻找默认安装chrome的路径，如果安chrome修改了默认路径，则寻找不到chrome
    ArrayList<String> argList = new ArrayList<>();
    LaunchOptions options = new LaunchOptionsBuilder().withArgs(argList).withHeadless(true).build();
    argList.add("--no-sandbox");
    argList.add("--disable-setuid-sandbox");
    Browser browser = Puppeteer.launch(options);
    Page page = browser.newPage();
    //开启追踪
    page.tracing().start("C:\\Users\\howay\\Desktop\\trace.json");
    page.goTo("https://www.baidu.com/?tn=98012088_10_dg&ch=3");
    page.tracing().stop();
```

在这个例子中，将在页面导航完成后，生成一个 json 格式的文件，里面包含页面性能的具体数据，可以用 Chrome 浏览器开发者工具打开该 json 文件，并分析性能。

#### 5、页面截图

```java
    BrowserFetcher.downloadIfNotExist(null);       
    ArrayList<String> arrayList = new ArrayList<>();
    LaunchOptions options = new LaunchOptionsBuilder().withArgs(arrayList).withHeadless(true).build();
    arrayList.add("--no-sandbox");
    arrayList.add("--disable-setuid-sandbox");
    Browser browser = Puppeteer.launch(options);
    Page page = browser.newPage();
    page.goTo("https://www.baidu.com/?tn=98012088_10_dg&ch=3");
    ScreenshotOptions screenshotOptions = new ScreenshotOptions();
    //设置截图范围
    Clip clip = new Clip(1.0,1.56,400,400);
    screenshotOptions.setClip(clip);
    //设置存放的路径
    screenshotOptions.setPath("test.png");
    page.screenshot(screenshotOptions);
```

页面导航完成后，设置截图范围以及图片保存路径，即可开始截图。

**更多的例子请看**[这里](https://github.com/fanyong920/jvppeteer/tree/master/example/src/main/java/com/ruiyun/example)

### 资源

1. [Puppeteer中文文档](https://zhaoqize.github.io/puppeteer-api-zh_CN/#/)
2. [DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/)
3. [Chrome命令行启动参数](https://peter.sh/experiments/chromium-command-line-switches/)

### 执照

此仓库中找到的所有内容均已获得 Apache 许可。有关详细信息，请参见`LICENSE`文件
