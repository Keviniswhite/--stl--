#STL标签使用    V_0.1
``请结合 具体的项目 及官方doc 查看本简单教程``
地址:https://www.siteserver.cn/docs/stl/
##引言
```html  
siteServer
的历史我也不太清楚,以前在学校学java时候，jsp页面使用了 c 的标记语言库，后来接触nodeJs,发现后端语言都会有动态标签		
```
##其他服务端语言渲染
- 例如 node.js
```html
	<block >
	</block >
	这个等同于 div
```
- 又比如 php
```html
	<include file="">
	我们在file 里面填页面路径，这样页面也就过来了，
	称之为  '公共的东西'
	也就意味着，写好一次就直接拉过来用
```
- 而siteServer 支持的  stl
```html
   1- 这种写法 可以传对应属性，特别多
	<stl:type="content" >
   2- 简写，简单粗暴
	{content.Content}
	后面逐一介绍
```
##为什么这样做
 - 我认为服务端渲染通过这些类似于``html``标记语言，可以实现页面的动态替换，我想这就是根本原因吧，毕竟说白了，动态其实也都是人为的替换。【当然这些东西归根结底还是存放在数据库当中，通过对应的后端接口拿过来数据，怎么放呢？就是那些html里的标记语言来划分】
 - 我们写好的代码，就是立下的``规则``，所有执行 都是在这套``规则``中完成。
 ##stl是怎么做的
 + 首先 谈 siteServer 的核心
   1 siteServer 从功能上看 是具备 cms 系统的 常见功能
   2 开发的后端语言 老牌 c# 【部分大学学生都是凭兴趣去学，毕竟比较难】
   3 后端提供的都是公共接口规则所以移植性强，放在哪都可以用
##stl 核心经脉
 - 如何操纵
		1 以栏目展开 划分
	2 每一个栏目相互独立
	3 栏目下就是一个 stl 标签环境，后端接口 也是通过读取栏目，再通过读取stl标签划分，为它渲染出来该出来的数据
# stlCms 核心使用
- 文件创建
```html
  首页栏目是自动生成的 不用关心
  它有快速创建的按钮，可以选这个

```

![images](https://github.com/Keviniswhite/--stl--/blob/master/快速添加.png)
- 点击进去之后
![images](https://github.com/Keviniswhite/--stl--/blob/master/快速创建栏目.jpg)
- 创建的结果
![images](https://github.com/Keviniswhite/--stl--/blob/master/创建结果.jpg)
- 栏目
![images](https://github.com/Keviniswhite/--stl--/blob/master/栏目.jpg)
 补充对应关系 按照下面的来
 ```html
{channel.ChannelIndex}  栏目索引
{channel.Title}         栏目名称
{channel.Content}       栏目内容
{channel.Description}   栏目seo 的描述
{channel.Keywords}      栏目seo 的关键字
 ```
 - 内容
 ![images](https://github.com/Keviniswhite/--stl--/blob/master/内容.jpg)
 ![images](https://github.com/Keviniswhite/--stl--/blob/master/内容标签.jpg)
 补充对应关系 按照下面的来
 ```html
{content.Title}  内容标题
{content.ImgUrl}  内容图片
{content.Content}    内容内容
{content.DownloadUrl}  内容附件下载地址
{content.summary}        内容摘要
 ```
## 结合具体项目
- 导航栏怎么去做
1 上图
	 ![images](https://github.com/Keviniswhite/--stl--/blob/master/导航栏实现后图.jpg)
2 上代码
```html
<!-- 导航栏开始-->
<Header class="global-header clearfix">
   <div class="container clearfix">
	   <nav class="clearfix wow fadeInDown">
		   <div class="h-left-box navbar-left">
			   <a href="index.html"><img src="{stl.siteurl}/upload/images/22154736645.png" alt="logo"></a>

		   </div>
		   <div class="menu-yd c-hide " id="c_mask">
			   <label for="checkbox">
				   <svg t="1573002895088" class="icon" viewBox="0 0 1024 1024" version="1.1"
					   xmlns="http://www.w3.org/2000/svg" p-id="3283" width="32" height="32">
					   <path
						   d="M128 256l768 0 0 86.016-768 0 0-86.016zM128 553.984l0-84.010667 768 0 0 84.010667-768 0zM128 768l0-86.016 768 0 0 86.016-768 0z"
						   p-id="3284" fill="#ffffff"></path>
				   </svg>
			   </label>
		   </div>
		   <input class="c-hide g-hide" type="checkbox" id="checkbox">
		   <div class="h-right-box clearfix navbar-right" id="id_nav">
			   <ul class="clearfix prev-slider">
				   <!-- 判断当前栏目是否为首页 -->
				   <stl:if testType="ChannelName" testOperate="Equals" testValue="首页">
					   <stl:yes>
						   <li class="active">
							   <stl:a>
							   </stl:a>
						   </li>
					   </stl:yes>

					   <stl:no>
							   <li>
		   <a href="{stl.siteurl}/index.html">首页</a>
	   </li>
					   </stl:no>
				   </stl:if>


				   <stl:channels channelIndex="首页" channelName="首页" startNum="1" totalNum="5">
					   <stl:if type="UpChannelOrSelf">
						   <stl:yes>
							   <li class="active">
								   <stl:a>
									   {channel.Title}
									   <!--  判断 下级栏目是否有二级栏目，有就带图标 -->
									   <stl:if type="CountOfChannels" op="Equals" value="0">
										   <stl:yes>
										   </stl:yes>

										   <stl:no>
											   <i><svg t="1572941262885" class="icon" viewBox="0 0 1024 1024"
													   version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="1755"
													   width="14" height="14"
													   data-spm-anchor-id="a313x.7781069.0.i2">
													   <path
														   d="M209.656 344.031l298.604 335.938 306.084-335.839-604.688-0.099z"
														   p-id="1756" fill="#333333"></path>
												   </svg></i>
										   </stl:no>
									   </stl:if>
								   </stl:a>
								   <!-- 二级菜单 -->
								   <stl:if type="CountOfChannels" op="Equals" value="0">
									   <stl:yes>
									   </stl:yes>
									   <stl:no>
										   <div class="nav-sub-menu ">
											   <ul class="clearfix">
												   <stl:channels channelIndex={channelName} channelName={channelName}>
													   <li>
														   <stl:a>
															   <i>
																   <svg t="1574213356922" class="icon" viewBox="0 0 1024 1024" version="1.1"
																	   xmlns="http://www.w3.org/2000/svg" p-id="1405" width="14" height="14">
																	   <path
																		   d="M268.373 908.63l60.587 60.074 456.704-460.373L328.704 55.04l-60.075 60.587 396.374 393.216z"
																		   p-id="1406" fill="#797979"></path>
																   </svg>
															   </i>
															   <span>{channel.ChannelIndex}</span>
														   </stl:a>
													   </li>
												   </stl:channels>
											   </ul>
										   </div>
									   </stl:no>
								   </stl:if>
							   </li>
						   </stl:yes>
						   <stl:no>
							   <!--  不是当前选中项 -->
							   <li>
								   <stl:a>
									   {channel.Title}
									   <stl:if type="CountOfChannels" op="Equals" value="0">
										   <stl:yes>
										   </stl:yes>
										   <stl:no>
											   <i><svg t="1572941262885" class="icon" viewBox="0 0 1024 1024"
													   version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="1755"
													   width="14" height="14"
													   data-spm-anchor-id="a313x.7781069.0.i2">
													   <path
														   d="M209.656 344.031l298.604 335.938 306.084-335.839-604.688-0.099z"
														   p-id="1756" fill="#333333"></path>
												   </svg></i>
										   </stl:no>
									   </stl:if>
								   </stl:a>

								   <!-- 二级菜单 -->
								   <stl:if type="CountOfChannels" op="Equals" value="0">
									   <stl:yes>
									   </stl:yes>
									   <stl:no>
										   <div class="nav-sub-menu ">
											   <ul class="clearfix">
												   <stl:channels channelIndex={channelName} channelName={channelName}>
													   <li>
														   <stl:a>
															   <i>
														   <svg t="1574213356922" class="icon"
															   viewBox="0 0 1024 1024" version="1.1"
															   xmlns="http://www.w3.org/2000/svg" p-id="1405"
															   width="14" height="14">
															   <path
																   d="M268.373 908.63l60.587 60.074 456.704-460.373L328.704 55.04l-60.075 60.587 396.374 393.216z"
																   p-id="1406" fill="#797979"></path>
														   </svg>
													   </i>
															   <span>{channel.ChannelIndex}</span>
														   </stl:a>
													   </li>
												   </stl:channels>
											   </ul>
										   </div>
									   </stl:no>
								   </stl:if>
							   </li>
						   </stl:no>
					   </stl:if>
					   </li>
				   </stl:channels>
			   </ul>
		   </div>
	   </nav>
   </div>

</Header>
```

## stl 附加属性
```html
 他们把这个叫做非实体标签 ，实体标签就是简单粗暴那种{xxx.xxx}
 非实体就是：<stl:xxx  type="xxx" 其他一些属性>

```
- channel 的非实体
```html
<stl:channel
    type="展示的类型"
    channelIndex="栏目索引"
    channelName="栏目名称"
    parent="展示父栏目属性"
    upLevel="上级栏目的级别"
    topLevel="从首页向下的栏目级别"
    leftText="展示在信息前的文字"
    rightText="展示在信息后的文字"
    formatString="展示的格式"
    separator="展示多项时的分割字符串"
    startIndex="字符开始位置"
    length="指定字符长度"
    wordNum="展示字符的数目"
    ellipsis="文字超出部分展示的文字"
    replace="需要替换的文字，可以是正则表达式"
    to="替换replace的文字信息"
    isClearTags="是否清除HTML标签"
    isReturnToBr="是否将回车替换为HTML换行标签"
    isLower="是否转换为小写"
    isUpper="是否转换为大写">
</stl:channel>


```
- content 非实体
```html
<stl:content
    type="展示的类型"
    leftText="展示在信息前的文字"
    rightText="展示在信息后的文字"
    formatString="展示的格式"
    no="展示第几项"
    separator="展示多项时的分割字符串"
    startIndex="字符开始位置"
    length="指定字符长度"
    wordNum="展示字符的数目"
    ellipsis="文字超出部分展示的文字"
    replace="需要替换的文字，可以是正则表达式"
    to="替换replace的文字信息"
    isClearTags="是否清除HTML标签"
    isReturnToBr="是否将回车替换为HTML换行标签"
    isLower="是否转换为小写"
    isUpper="是否转换为大写"
    isOriginal="如果是引用内容，是否获取所引用内容的值">
</stl:content>
```
``总结：以上使用我们统称为你 拉了一个 东西过来``
？？ 那么如何拉更多的过来？
## 拉取更多
```html
<stl:channels
    isTotal="是否从所有栏目中选择"
    isAllChildren="是否展示所有级别的子栏目"
    channelIndex="栏目索引"
    channelName="栏目名称"
    upLevel="上级栏目的级别"
    topLevel="从首页向下的栏目级别"
    groupChannel="指定展示的栏目组"
    groupChannelNot="指定不展示的栏目组"
    totalNum="展示内容数目"
    startNum="从第几条信息开始展示"
    order="排序"
    where="获取内容列表的条件判断"
    columns="列数"
    direction="方向"
    height="指定列表布局方式"
    width="整体高度"
    align="整体宽度"
    itemHeight="整体对齐"
    itemWidth="项高度"
    itemAlign="项宽度"
    itemVerticalAlign="项水平对齐"
    itemClass="项垂直对齐"
    layout="项Css类">

    <span>{channel.Title}</span>
</stl:channels>

```
``这样就把 该大栏目下的所有小栏目的标题 循环遍历了出来，你有多少个子栏目 那么 span 就会生成多少个``
- content 也是这个道理
```html

<stl:contents
    channelIndex="栏目索引"
    channelName="栏目名称"
    upLevel="上级栏目的级别"
    topLevel="从首页向下的栏目级别"
    scope="内容范围"
    groupChannel="指定展示的栏目组"
    groupChannelNot="指定不展示的栏目组"
    groupContent="指定展示的内容组"
    groupContentNot="指定不展示的内容组"
    tags="指定标签"
    isTop="仅展示置顶内容"
    isRecommend="仅展示推荐内容"
    isHot="仅展示热点内容"
    isColor="仅展示醒目内容"
    totalNum="展示内容数目"
    startNum="从第几条信息开始展示"
    order="排序"
    isImage="仅展示图片内容"
    isVideo="仅展示视频内容"
    isFile="仅展示附件内容"
    isRelatedContents="展示相关内容列表"
    where="获取内容列表的条件判断"
    columns="列数"
    direction="方向"
    height="指定列表布局方式"
    width="整体高度"
    align="整体宽度"
    itemHeight="整体对齐"
    itemWidth="项高度"
    itemAlign="项宽度"
    itemVerticalAlign="项水平对齐"
    itemClass="项垂直对齐"
    layout="项Css类">
    <span>{content.Title}</span>
</stl:contents>
```
## 分页
- 看图
 ![images](https://github.com/Keviniswhite/--stl--/blob/master/分页.jpg)
- 上代码
```html
	1 分页内容
	 - channelIndex => 我这样写 是因为 之前说了，这个cms 是以 栏目为单位的
	所以服务器渲染时候 可以自动识别并把该页当前栏目名字加上，算是比较灵活的写法
	 - pageNum => 是一页显示多少个
	<stl:pageContents channelIndex="{channel.ChannelIndex}" channelName="{channel.Title}"  pageNum="8">
		   <li>
			 <stl:a href="">
			   <div class="img-box">
				 <img src="{content.ImageUrl}" alt="{content.Title}">
			   </div>
			   <h4 class="title">{content.Title}</h4>
			   <!-- 暂时无法匹配 -->
			   <span class="style-pro">
				 5-264
			   </span>
			 </stl:a>
		   </li>
		 </stl:pageContents>




	2  分页器
	<ul class="pagination  ">
		   <stl:pageItems>              
			 <ul class="pagination  ">
			   <li><a href="{PageItem.PreviousPage}">上一页</a></li>
			   <stl:pageItem type="PageNavigation">
				 <stl:yes>
				   <li><a href="{Current.Url}">{Current.Num}</a></li>
				 </stl:yes>
				 <stl:no>
				   <li class="active"><span>{Current.Num}</span></li>
				 </stl:no>
			   </stl:pageItem>
			   <li><a href="{PageItem.NextPage}">下一页</a></li>
			 </ul>

		   </stl:pageItems>
	   </ul>
	   【备注，如果pageNum = 8 ,那么产品总数 > 8 才会出现分页器】
```
