

有问题可以微博找我，报错也可以找我（当然你要是能改好再告诉我再好不过了）微博@唐文_Ishtar，头像是白底的刺客信条图标

项目中的内容一共是四种

1. 保存我的喜欢/推荐/tag
2. 保存一个作者发布的所有内容
3. 单篇保存
4. 工具

所有程序要调的内容都在main里，划拉到程序最后面就是

保存作者和单篇保存是先写的，有些地方会比较粗糙



#### l13 保存我的喜欢/推荐/tag

程序 l13_like_share_tag

![InkedQQ截图20200713020059_LI](笔记图/README/InkedQQ截图20200713020059_LI.jpg)

##### 运行模式

共四种运行模式，like1,like2,share,tag

like1和like2：保存点过的喜欢，区别是2有一个额外的功能
share：保存所有点过的推荐
tag：保存一个tag里的内容

tag模式我只能爬到前1200条内容，后面的我弄不到了，有大佬知道怎么搞的话求告知，我爬的是这个

![1594571137411](笔记图/README/1594571137411.png)



##### 保存模式

lofter的博客有5种类型，这个爬虫可以爬到文字、图片、长文章。音乐和视频我没想弄。
其中文字我分类成了有标题的和无标题的，有标题的叫文章，无标题的叫文本。

![1593235470530](笔记图/README/1593235470530.png)



保存的图片的命名格式是 "作者名[作者三级域名] 发表时间(编号)"

![1593254388077](笔记图/README/1593254388077.png)

三级域名就是每个人主页 https://xxxxxxx.lofter.com/  xxxxxx那一段，整这个是因为作者可能经常改名，但三级域名很少改，通过文件找作者的时候会方便很多。

保存的文章、文本、长文章的文件命名格式都是 "标题 by 作者.txt"

![1593254326646](笔记图/README/1593254326646.png)

文件里有文件头信息和尾信息，头信息包括 标题，作者名，作者三级域名，发表时间，原文链接，该篇博客打的tag

<img src="笔记图/README/1593254052952.png" alt="1593254052952" style="zoom:50%;" />

如果文章里面带图片或者外链，链接会写在文件最后。有保存文章中图片的功能，后面说。

![1593254075293](笔记图/README/1593254075293.png)



##### 使用

基本的使用只要设置三个地方，url，mode，save_mode

url：
like1 like2 share模式的url是你个人主页的链接
tag模式需要的是tag链接。tag链接中有中文复制下来会变成编码，是正常的。

mode：就是模式，like1 like2 share 和 tag

save_mode：保存模式，要保存哪些内容，见图

![1594540653329](笔记图/README/1594540653329.png)

使用like1和share前需要把自己的lofter推荐和喜欢设为公开，非公开的话会爬不到

然后运行就行。

没学过编程的，我写过一篇教程，如何最快的让你运行一个python程序，lof现在不让贴外链，所以，我的微博叫 唐文_ishtar，搜'使用教程' 可以找到。



like2模式需要额外填写：

![1594541296291](笔记图/README/1594541296291.png)

跟like1不一样，like2爬的是 http://www.lofter.com/like ，这个页面是登录后访问的，须要登录信息，另外，只支持手机登录的，邮箱的登录请求有加密我解不开。手机号就是手机号密码就是密码，授权码是一个叫LOFTER-PHONE-LOGIN-AUTH的cookies。开始时间是功能，在功能里说。

不知道去哪找cookies的看下面

点开lofter主页或者喜欢页，按F12召唤开发者工具，按Network，按XHR，往下划拉几下会加载一个文件，点它

![1594541644246](笔记图/README/1594541644246.png)

点Cookies，找到一个叫LOFTER-PHONE-LOGIN-AUTH的，在它的值那里摁三下全选，拷到代码里就行

![1594542096984](笔记图/README/1594542096984.png)





##### 功能选项

所有功能如果在生效阶段运行完后想要修改，必须将进度重置，重新运行该阶段

所有模式可用

| 功能          | 对应变量          | 变量类型 | 如何启动               | 具体描述                                                     | 生效阶段 | 备注                                                         |
| ------------- | ----------------- | -------- | ---------------------- | ------------------------------------------------------------ | :------- | ------------------------------------------------------------ |
| 按tag分类     | classify_by_tag   | 整型     | 1开启 0关闭            | 设作者打了 A,B,C 3个tag，启动该功能时会将该条博客的内容保存到名为A的文件夹下 | 阶段2    | 要爬的内容超过1000条时建议打开                               |
| 优先tag       | prior_tags        | 列表     | 列表非空时为启动该功能 | **未启动按tag分类时该功能无效**<br />设优先tag为C,B,D，作者打了A,B,C 三个tag，该条博客内容会按优先tag的顺序保存至文件夹 target/C<br />设优先tag为C,B,D，作者打了E,F,G 三个tag，按照作者第一顺位tag，保存至other/E下 | 阶段2    | 我自己整理了差不多200多个优先tag，个人建议顺序是cp - 角色 -作品 |
| 非优先tag聚合 | agg_non_prior_tag | 整型     | 1开启 0关闭            | **优先tag未启用时时该功能无效**<br />如果作者打的tag中不包含优先tag，博客内容保存到other文件夹下。1为启动，0为关闭。 | 阶段2    | /                                                            |



仅like2模式可用

| 功能         | 对应变量   | 变量类型 | 如何启动           | 具体描述                                           | 生效阶段 | 备注                                       |
| ------------ | ---------- | -------- | ------------------ | -------------------------------------------------- | -------- | ------------------------------------------ |
| 最早时间指定 | start_time | 字符串   | 非空字符串时为启动 | 保存从指定时间到现在喜欢的博客，空为不指定最早时间 | 阶段1    | 格式为"yyyy-MM-dd"<br />举例："2020-02-01" |



仅tag模式可用

| 功能         | 对应变量名 | 变量类型 | 如何启动           | 具体描述                           | 生效阶段 | 备注 |
| ------------ | ---------- | -------- | ------------------ | ---------------------------------- | -------- | ---- |
| 最小热度限定 | min_hot    | 整形     | 非空字符串时为启动 | 博客热度小于设定的热度则不会被爬取 | 阶段2    | /    |



其他选项

| 选项                               | 对应变量         | 变量类型 | 如何使用    | 具体描述                                                     | 生效阶段     | 备注                                                         |
| ---------------------------------- | ---------------- | -------- | ----------- | ------------------------------------------------------------ | ------------ | ------------------------------------------------------------ |
| 保存 文章/长文章/文本 中包含的图片 | save_img_in_text | 整形     | 1启动 0关闭 | 开启该功能后，会将文章中的图片保存到文章同已路径下，图片命名为文章标题+序号 | 阶段1        | 因为没有足够的文章用来测试所有这个功能出错的可能性挺高       |
| tag统计输出过滤                    | tag_filt_num     | 整形     | /           | 运行中会对所有博客的tag进行一个统计，输出出现次数超过设定次数的tag | 阶段3        | 可以当作优先tag的辅助功能，把输出的列表调整顺序后再放进代码里 |
| 输出等级                           | print_level      | 整形     | /           | 等级1的输出多，0的输出少                                     | 啥时候改都行 | 建议0，1会把每一条博客的信信息都输出来，除非你要debug不然0比较好。 |
| 文件设置                           | file_path        | 字符串   | /           | 所有文件的存放路径                                           | 阶段1        | 默认为./dir，一般不用改                                      |



##### 运行过程

运行完一个阶段会保存进度，会有文件标识这个阶段已经完成，可以删掉文件来手动调进度。



阶段1：获取喜欢/推荐/tag页面的原始数据，获取完成存到fav_info，然后从里面解析出需要的数据存到format_fav_info.json。
需要一点时间，我的一共8000多条，获取加解析大概6分钟。

format_fav_info.json文件出现表示阶段1已运行完成。



获取阶段的实际返回条数比请求条数少，因为你点过喜欢/推荐/tag中曾经存在的内容，有些已经被屏蔽或者作者删除的，也会计算进去，但实际上已经请求不到了，一般来说时间约久消失的条数越多。（我的少了1/10，fuck）



<img src="笔记图/README/1593243488551.png" alt="1593243488551" style="zoom:67%;" />





阶段2：

添加自动整理的信息，然后按照 图片、文章、文本、长文章分类，分类完后写到classified_fav_info.json。
大概一两秒。

classified_fav_info.json文件生成说明阶段2完成。



阶段3：对博客进行类型统计和tag统计，统计完后输出。很快，一两秒。

没有文件，目的是输出，每次运行都有这个阶段。

第三行是字典，每个tag具体出现了多少次，第四行是列表。![1593245140407](笔记图/README/1593245140407.png)
这有个小附加功能，用来辅助生成prior_tags的，具体看小工具 tags_tolist。

阶段3结束之后会暂停让你输个ok，退出就随便输点啥。



![1593245455891](笔记图/README/1593245455891.png)



阶段4：保存

保存文章、保存文本、保存长文章很快，如果没开保存文本中的图片的话只要几秒就能运行完。
这三项没有进度文件，所以保存中断再启动会从头开始保存。

保存图片能自己记录进度，保存图片的时候断掉重启的话会自动读上次的进度，进度在img_save_info.json。
需要的时间比较长，查不多20-40分钟1000条图片博客，主要看图片多少。

如果图片存到一半，停止，修改了自动整理选项再启动的话，进度会自己重置到阶段2（这是整个程序唯一自动调进度的地方）



保存完成后有个问你删不删文件的，要是保存完了yes就行。
要是比如说你第一次没存长文章，跑完了突然又想存就no，再启动会从阶段3开始跑。

![1594574380460](笔记图/README/1594574380460.png)





#### l4 l9 保存作者主页内容

l4_author_img.py 和 l9_author_txt.py

l4是保存作者发布的所有图片，l9是保存所有文章

l4_author_img

![1594572545362](笔记图/README/1594572545362.png)

 l9_author_txt.py![1594573557525](笔记图/README/1594573557525.png)

这两挺像，因为l9就是拿l4改的，还从l4调了一堆方法

功能在注释里都写了，不做自定义设置的话把作者主页链接拷到author_url，运行就行

l9只能爬到有标题的文章（保存我的喜欢那个可以爬到没标题的），以后可能会做升级

 爬到的图片会在./dir/img/作者名 里面，图片命名方式是 作者名[作者lof的三级域名]-发表时间(序号) 
文章在dir/article里，文件命名格式是 标题by作者

以后有空可能会把这两合并一下 



#### l8 l10单篇保存

l8_blogs_img.py 和 l10_blogs_txt.py

这两没有要改代码的地方，改的是对应的文件，文件在./dir中

l8 对应 img_list
l10 对应 txt_list

使用方法相同，把单篇的链接放进文件里，一行一个，直接运行程序就行

![1594575313811](笔记图/README/1594575313811.png)

这两个是我以前用得最多的，不过保存喜欢的写出来之后就没怎么用了



#### 工具

parse_template.py：这个是l9和l10的xpath匹配模板，放着就行

useragentutil.py：随机获取请求头，放着就行



tags_tolist.py：

这个是对l13生成prior_tag的一个辅助功能

在l13运行了一次阶段3之后会在./dir里面生成一个prior_tags.txt文件，里面是所有博客中tag数从多到少的排序。里面的tag是tag过滤后的。

![1594576152102](笔记图/README/1594576152102.png)

可以在里面排序/删除，排完运行tags_tolist.py会把你排好的变成列表（这里示范一下，没排），把列表直接拷到prior_tag就行

![1594576516451](笔记图/README/1594576516451.png)为了避免排好之后忘记导出来就运行l13把文件覆盖掉，prior_tags.txt这个文件只会在文件不存在时生成，重新生成需要把文件删掉。