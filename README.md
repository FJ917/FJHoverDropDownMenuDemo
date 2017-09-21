---
title: 栗子——嵌套组合实现筛选菜单滑动吸顶悬停
date: 2016-08-13 23:25:20
categories: "Android栗子"
tags:
	 - Android栗子
	 - Android
	 - 栗子
---

![栗子配图.png](http://upload-images.jianshu.io/upload_images/2071764-876a337c79b7c77d.png)


<!--more-->

---

> ### 栗子惯例，先上GIF

![栗子.gif](http://upload-images.jianshu.io/upload_images/2071764-e6ff8674caec4bba.gif)

---

好了，现在来说下这个栗子了，在以往实现这种效果是很麻烦的，现在就不同了，自从新特性控件出来后，各种happy，可以轻松实现各种炫酷效果！

---

> ### 使用到的控件

使用前需要在Gradle加入Support Design Library：
**compile 'com.android.support:design:25.0.1'**

#### CoordinatorLayout
`CoordinatorLayout`通过协调子布局的形式，产生联动效果。通过设置子View的Behaviors来协调子View。

#### AppBarLayout
`AppBarLayout`中的一个属性`android:fitsSystemWindows="true"`，是为了调整系统窗口布局以适应布局。
`AppBarLayout`里面的View，是通过`app:layout_scrollFlags`属性来控制，其中有4种Flag的类型：

 - `Scroll`：向下滚动时,被指定了这个属性的View会被滚出屏幕范围直到完全不可见的位置。
 - `enterAlways`：向上滚动时,这个View会随着滚动手势出现,直到恢复原来的位置。
 - `enterAlwaysCollapsed`： 当视图已经设置minHeight属性又使用此标志时，视图只能以最小高度进入，只有当滚动视图到达顶部时才扩大到完整高度。
 - `exitUntilCollapsed`： 滚动退出屏幕，最后折叠在顶端。

#### CollapsingToolbarLayout
用来协调`AppBarLayout`来实现滚动隐藏`ToolBar`的效果。

#### Toolbar
Toolbar在v7包中，设置`layout_collapseMode`协调`CollapsingToolbarLayout`达到滑动视图的视觉差效果：

 - `pin`：固定模式，在折叠的时候最后固定在顶端。
 - `parallax`：视差模式，在折叠的时候会有个视差折叠的效果。

---

> ### main.xml
```
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout
	xmlns:android="http://schemas.android.com/apk/res/android"
	xmlns:app="http://schemas.android.com/apk/res-auto"
	android:layout_width="match_parent"
	android:layout_height="match_parent">
	<android.support.design.widget.AppBarLayout
		android:layout_width="match_parent"
		android:layout_height="wrap_content"
		android:fitsSystemWindows="true"
		android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar">
		<android.support.design.widget.CollapsingToolbarLayout
			android:id="@+id/collapsingToolbar"
			android:layout_width="match_parent"
			android:layout_height="match_parent"
			android:fitsSystemWindows="true"
			app:contentScrim="?attr/colorPrimary"
			app:navigationIcon="@drawable/back"
			app:layout_scrollFlags="scroll|exitUntilCollapsed">
			<LinearLayout
				android:layout_width="match_parent"
				android:layout_height="wrap_content"
				android:scaleType="centerInside"
				app:layout_collapseMode="parallax"
				android:fitsSystemWindows="true"
				android:orientation="vertical">
				<ImageView
					android:layout_width="match_parent"
					android:layout_height="180dp"
					android:background="@drawable/image" />
			</LinearLayout>
			<android.support.v7.widget.Toolbar
				android:id="@+id/toolbar"
				android:layout_width="match_parent"
				android:layout_height="?attr/actionBarSize"
				app:titleTextColor="#ffffff"
				app:theme="@style/ToolbarTheme"
				android:gravity="center_vertical"
				android:background="#00ffffff"
				app:navigationIcon="@drawable/back"
				app:layout_collapseMode="pin"
				app:popupTheme="@style/AppTheme.PopupOverlay" />
		</android.support.design.widget.CollapsingToolbarLayout>
		<LinearLayout
			app:layout_scrollFlags="exitUntilCollapsed"
			android:layout_width="match_parent"
			android:layout_height="wrap_content"
			android:orientation="vertical">
			<LinearLayout
				android:layout_width="match_parent"
				android:layout_height="48dp"
				android:background="@color/white"
				android:gravity="center">
				<fj.hoverdropdownmenu.demo.view.DropdownButton
					android:id="@+id/chooseType"
					android:layout_width="0px"
					android:layout_height="match_parent"
					android:layout_weight="1" />
				<View
					android:layout_width="0.5dp"
					android:layout_height="18dp"
					android:background="#dddddd" />
				<fj.hoverdropdownmenu.demo.view.DropdownButton
					android:id="@+id/chooseLanguage"
					android:layout_width="0px"
					android:layout_height="match_parent"
					android:layout_weight="1" />
			</LinearLayout>
			<View
				android:layout_width="match_parent"
				android:layout_height="0.5dp"
				android:background="@color/divide" />
		</LinearLayout>
	</android.support.design.widget.AppBarLayout>
	<FrameLayout
		android:layout_width="match_parent"
		android:layout_height="match_parent"
		app:layout_behavior="@string/appbar_scrolling_view_behavior">
		<android.support.v7.widget.RecyclerView
			android:id="@+id/mRecyclerView"
			android:layout_width="match_parent"
			android:layout_height="match_parent" />
		<View
			android:id="@+id/mask"
			android:layout_width="match_parent"
			android:layout_height="match_parent"
			android:background="#80000000" />
		<LinearLayout
			android:layout_width="match_parent"
			android:layout_height="match_parent"
			android:layout_marginBottom="200dp"
			android:orientation="vertical">
			<fj.hoverdropdownmenu.demo.view.DropdownListView
				android:id="@+id/dropdownType"
				android:layout_width="match_parent"
				android:layout_height="wrap_content"
				android:orientation="vertical" />
			<fj.hoverdropdownmenu.demo.view.DropdownListView
				android:id="@+id/dropdownLanguage"
				android:layout_width="match_parent"                
				android:layout_height="wrap_content"                
				android:orientation="vertical" />
		</LinearLayout>
	</FrameLayout>
</android.support.design.widget.CoordinatorLayout>
```

---


**分析说明：**
1. 最外层是由一个`CoordinatorLayout`嵌套
2. 内层是由`AppBarLayout`和`FrameLayout`组成
3. `AppBarLayout`里的view是通过`layout_scrollFlags`来控制
4. `FrameLayout`里的view是通过`layout_behavior`来控制的，只要设置其`@string/appbar_scrolling_view_behavior`属性就ok了
5. `FrameLayout`里的布局是由`RecyclerView`和灰色透明的view,以及一组`DropdownListView`组成，这就是我选择这个筛选控件的原因，可以拆分出来独立的组件，而不是组合起来的一个新控件。

---

**JAVA代码中都是些简单的数据绑定，及一些设置，具体的可以看代码，有问题的话可以联系我~**

---

> ### 总结
> 学会新控件的使用这是最基本的，然后实现一些炫酷的效果~

---

参考如下，感谢作者：
参考：[过滤功能的下拉菜单FilterDropDownMenu](https://github.com/leerduo/FilterDropDownMenu)

> ### 源码地址
> **[https://github.com/FJ917/FJHoverDropDownMenuDemo](https://github.com/FJ917/FJHoverDropDownMenuDemo)**
> **有用的话，请给个star支持下，谢谢~**

---

> **个人博客：[WWW.FJ917.COM](http://www.fj917.com)**
> **简书：[www.jianshu.com/u/3d2770e6e489](http://www.jianshu.com/u/3d2770e6e489)**
> **CSDN：[blog.csdn.net/fj917](http://blog.csdn.net/fj917)**


|欢迎加入QQ交流群657206000[点我加入](http://shang.qq.com/wpa/qunwpa?idkey=9b454a6f01bd94d97e4c3f2771447a989ec77794eb5a563422263153c00f700d)|
|:---:|
|![QQ交流群：657206000](http://upload-images.jianshu.io/upload_images/2071764-bce605159bbceb2a.png)|