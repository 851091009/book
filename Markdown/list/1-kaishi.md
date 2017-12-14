# MarkDown 使用方法

## 1. h1-h6 标签使用方法
# h1
## h2
### h3
#### h4
##### h5
###### h6

## 2. 区块引用 Blockquotes

> 这是区块引用

### a.区块引用还可以嵌套

> 区块引用
>> 还可以这样 ^_^!
>>> MORE

### b.区块引用还可以使用其它 Markdown 语法，包括标题、列表、代码块等...

> ## 这是一个标题。
> 
> 1.   这是第一行列表项。
> 2.   这是第二行列表项。
> 
> 给出一些例子代码：
> 
>     return shell_exec("echo $input | $markdown_script");

## 3.列表
Markdown 支持有序列表和无序列表

### a.无序列表使用 星号(*)、加号(+)、减号(-)作为列表标记
* 红色
* 蓝色
* 彩虹 ^_^

等同于：
+ 红色
+ 蓝色
+ 彩虹 ^_^

也等同于：
- 红色
- 蓝色
- 彩虹 ^_^

### b.有序列表则使用数组接着一个英文句点：

1. 红色
2. 蓝色
3. 彩虹 ^_^

### 很重要的一点是，你在列表标记上使用的数字并不会影响输出的 HTML 结果，上面的列表所产生的 HTML 标记为：
    <ol>
    <li>Bird</li>
    <li>McHale</li>
    <li>Parish</li>
    </ol>
### 如果你的列表标记写成：

1.  Bird
1.  McHale
1.  Parish

或甚至是：

3. Bird
1. McHale
8. Parish

你都会得到完全相同的 HTML 输出。重点在于，你可以让 Markdown 文件的列表数字和输出的结果相同，或是你懒一点，你可以完全不用在意数字的正确性。

如果你使用懒惰的写法，建议第一个项目最好还是从 1. 开始，因为 Markdown 未来可能会支持有序列表的 start 属性。

列表项目标记通常是放在最左边，但是其实也可以缩进，最多 3 个空格，项目标记后面则一定要接着至少一个空格或制表符。

要让列表看起来更漂亮，你可以把内容用固定的缩进整理好：

    *   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
     Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
     viverra nec, fringilla in, laoreet vitae, risus.
    *   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
    Suspendisse id sem consectetuer libero luctus adipiscing.

如果列表项目间用空行分开，在输出 HTML 时 Markdown 就会将项目内容用 <p> 标签包起来，举例来说：

    *   Bird
    *   Magic

但是这个：

    *   Bird

    *   Magic

会被转换为：

    <ul>
    <li><p>Bird</p></li>
    <li><p>Magic</p></li>
    </ul>

如果要在列表项目内放进引用，那 > 就需要缩进：
    
    *   A list item with a blockquote:
        > This is a blockquote
        > inside a list item.

## 4.代码区块

这是一个普通段落：

    这是一个代码区块。
Markdown 会转换成：

    <p>这是一个普通段落：</p>

    <pre><code>这是一个代码区块。
    </code></pre>

这个每行一阶的缩进（4 个空格或是 1 个制表符），都会被移除，例如：

    Here is an example of AppleScript:

        tell application "Foo"
        beep
        end tell

会被转换为：

    <p>Here is an example of AppleScript:</p>

    <pre><code>tell application "Foo"
        beep
    end tell
    </code></pre>

## 5.分隔线
你可以在一行中用三个以上星号、减号、底线来建立一个分割线，行内不能有其它内容,你可以在星号或者减号中间插入空格。下面的写法可以建立分割线。

    ***
    ---
    ******
    - - - -
    ------------

## 6.区段元素

### a.链接
Markdown 支持两种形式的链接形式的l链接语法:行内式和参考式两种形式.

不管是哪一种，链接文字都是用 [方括号] 来标记。

要建立一个行内式的链接，只要在方块括号后面紧接着圆括号并插入网址链接即可，如果你还想要加上链接的 title 文字，只要在网址后面，用双引号把 title 文字包起来即可，例如：

    This is [an example](http://example.com/ "Title") inline link.

    [This link](http://example.net/) has no title attribute.

会产生：

    <p>This is <a href="http://example.com/" title="Title">
    an example</a> inline link.</p>

    <p><a href="http://example.net/">This link</a> has no
    title attribute.</p>

如果你是要链接到同样主机的资源，你可以使用相对路径：

    See my [About](/about/) page for details.

参考式的链接是在链接文字的括号后面再接上另一个方括号，而在第二个方括号里面要填入用以辨识链接的标记：

    This is [an example][id] reference-style link.

你也可以选择性地在两个方括号中间加上一个空格：

    This is [an example] [id] reference-style link.

接着，在文件的任意处，你可以把这个标记的链接内容定义出来：

    [id]: http://example.com/  "Optional Title Here"

链接内容定义的形式为：

* 方括号（前面可以选择性地加上至多三个空格来缩进），里面输入链接文字
* 接着一个冒号
* 接着一个以上的空格或制表符
* 接着链接的网址
* 选择性地接着 title 内容，可以用单引号、双引号或是括弧包着

下面这三种链接的定义都是相同：

    [foo]: http://example.com/  "Optional Title Here"
    [foo]: http://example.com/  'Optional Title Here'
    [foo]: http://example.com/  (Optional Title Here)

请注意：有一个已知的问题是 Markdown.pl 1.0.1 会忽略单引号包起来的链接 title。

链接网址也可以用方括号包起来：

    [id]: <http://example.com/>  "Optional Title Here"


你也可以把 title 属性放到下一行，也可以加一些缩进，若网址太长的话，这样会比较好看：

    [id]: http://example.com/longish/path/to/resource/here
    "Optional Title Here"

网址定义只有在产生链接的时候用到，并不会直接出现在文件之中。

链接辨别标签可以有字母、数字、空白和标点符号，但是并不区分大小写，因此下面两个链接是一样的：

    [link text][a]
    [link text][A]

隐式链接标记功能让你可以省略指定链接标记，这种情形下，链接标记会视为等同于链接文字，要用隐式链接标记只要在链接文字后面加上一个空的方括号，如果你要让 "Google" 链接到 google.com，你可以简化成：

    [Google][]

然后定义链接内容：

    [Google]: http://google.com/

由于链接文字可能包含空白，所以这种简化型的标记内也许包含多个单词：

    Visit [Daring Fireball][] for more information.

然后接着定义链接：
    
    [Daring Fireball]: http://daringfireball.net/

链接的定义可以放在文件中的任何一个地方，我比较偏好直接放在链接出现段落的后面，你也可以把它放在文件最后面，就像是注解一样。

下面是一个参考式链接的范例：

    I get 10 times more traffic from [Google] [1] than from
    [Yahoo] [2] or [MSN] [3].

    [1]: http://google.com/        "Google"
    [2]: http://search.yahoo.com/  "Yahoo Search"
    [3]: http://search.msn.com/    "MSN Search"

如果改成用链接名称的方式写：

    I get 10 times more traffic from [Google][] than from
    [Yahoo][] or [MSN][].

    [google]: http://google.com/        "Google"
    [yahoo]:  http://search.yahoo.com/  "Yahoo Search"
    [msn]:    http://search.msn.com/    "MSN Search"

上面两种写法都会产生下面的 HTML。

    <p>I get 10 times more traffic from <a href="http://google.com/"
    title="Google">Google</a> than from
    <a href="http://search.yahoo.com/" title="Yahoo Search">Yahoo</a>
    or <a href="http://search.msn.com/" title="MSN Search">MSN</a>.</p>

下面是用行内式写的同样一段内容的 Markdown 文件，提供作为比较之用：

    I get 10 times more traffic from [Google](http://google.com/ "Google")
    than from [Yahoo](http://search.yahoo.com/ "Yahoo Search") or
    [MSN](http://search.msn.com/ "MSN Search").

参考式的链接其实重点不在于它比较好写，而是它比较好读，比较一下上面的范例，使用参考式的文章本身只有 81 个字符，但是用行内形式的却会增加到 176 个字元，如果是用纯 HTML 格式来写，会有 234 个字元，在 HTML 格式中，标签比文本还要多。

使用 Markdown 的参考式链接，可以让文件更像是浏览器最后产生的结果，让你可以把一些标记相关的元数据移到段落文字之外，你就可以增加链接而不让文章的阅读感觉被打断。

## 7.强调
