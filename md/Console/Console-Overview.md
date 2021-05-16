# 控制台概述

Chrome DevTools 的控制台使开发网页更加容易. 它主要有两个作用:  [查看输出的消息](#查看输出的消息) 和 [运行JavaScript](#运行JavaScript).

## 查看输出的消息

Web开发者通常向控制台输出消息日志，以确保他们的JavaScript代码能够按预期运行。要实现这一点, 你需要将 `console.log('...some message')`插入到你的JavaScript程序中。 当浏览器执行到这句表达式时，就会向控制台输出这个信息。例如，假设你正在为一个网页编写如下的HTML和JavaScript：

```html
<!doctype html>
<html>
  <head>
    <title>Console Demo</title>
  </head>
  <body>
    <h1>Hello, World!</h1>
    <script>
      console.log('Loading!');
      const h1 = document.querySelector('h1');
      console.log(h1.textContent);
      console.assert(document.querySelector('h2'), 'h2 not found!');
      const artists = [
        {
          first: 'René',
          last: 'Magritte'
        },
        {
          first: 'Chaim',
          last: 'Soutine'
        },
        {
          first: 'Henri',
          last: 'Matisse'
        }
      ];
      console.table(artists);
      setTimeout(() => {
        h1.textContent = 'Hello, Console!';
        console.log(h1.textContent);
      }, 3000);
    </script>
  </body>
</html>
```

**图1** 展示了页面加载并等待3秒后控制台的样子。试着找出哪些代码使得控制台输出消息。

![控制台面板](https://developer-chrome-com.imgix.net/image/admin/dpOohQpnFAKdK0JpVvuv.png?auto=format)

**图 1**. 控制台面板

Web开发者通常在以下情况下会使用`console.log`：

- 确保代码以正确的顺序执行。

- 在代码执行过程中检查变量的值。

参见 [开始使用 console.log](https://developer.chrome.com/docs/devtools/console/log/) 获得有关日志记录的实际经验. 

参阅 [Console API 参考](https://developer.chrome.com/docs/devtools/console/api/) 以浏览`console`的方法的完整列表。这些方法之间的主要区别在于它们如何显示输出的消息。

## 运行JavaScript

控制台是一个["读取-求值-输出" 循环](https://zh.wikipedia.org/wiki/读取﹣求值﹣输出循环) (Read-Eval-Print Loop,REPL)[REPL](https://en.wikipedia.org/wiki/Read–eval–print_loop).您可以在控制台中运行JavaScript来与您正在检查的页面交互。例如, **图2** 显示了DevTools主页旁的控制台,**图3** 显示了使用控制台更改了文章标题后的同一页面.

![DevTools主页旁边的控制台面板](https://developer-chrome-com.imgix.net/image/admin/HsESeWdYC1Yck5qTtzkj.png?auto=format)

**图 2**. DevTools主页旁边的控制台面板

![使用控制台更改标题](https://developer-chrome-com.imgix.net/image/admin/Diu3Bq4TbPWb9Y5gr7HX.png?auto=format)

**图 3**. 使用控制台更改标题

可以从控制台修改页面，因为控制台可以完全访问页面 [`window`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window). DevTools有一些可以使检查页面变得更容易的函数。例如，假设你的JavaScript代码包含一个 `hideModal`函数. 运行`debug(hideModal)` 会使`hideModal`函数在下次被调用时被暂停在它的第一行。参阅 [Console Utilities API 参考](https://developer.chrome.com/docs/devtools/console/utilities#debug) 以查看实用函数的完整列表。

当你运行JavaScript时，你不需要与页面交互。你可以使用Console尝试与页面无关的新代码。例如，假设你刚刚学习了内置的JavaScript Array方法 [`map()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map), 并且想要试验它，那么控制台是一个测试该函数的好地方。

参见 [开始运行JavaScript](https://developer.chrome.com/docs/devtools/console/javascript/) 获得在控制台中运行JavaScript的实际经验。

