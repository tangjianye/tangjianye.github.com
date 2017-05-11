---
layout: post  
title:  Android代码规范   
date:   2017-02-19
tags: [Android, Standard]  
categories: Standard  
author: JayaTang  
description: 本文档定义了公司Android开发规范，包括代码规范、资源规范、命名规范和注释规范等方向。    
---
本文档定义了公司Android开发规范，包括代码规范、资源规范、命名规范和注释规范等方向。本文档的预期读者包括：公司Android系统设计人员、开发人员及测试人员。

## 书写规范

### 编码方式
编码方式统一用UTF-8. Android Studio默认已是UTF-8，只要不去改动它就可以了。

### 缩进统一
缩进统一为4个空格，将Tab size设置为4则可以保证tab键按4个空格缩进。保证切换到不同tab长度的环境时还能继续保持统一的4个空格的缩进样式。

### 单独成行
花括号不要单独一行，和它前面的代码同一行。而且，花括号与前面的代码之间用一个空格隔开。  
```Java
public void method() { // Good 
} 

public void method()
{ // Bad
}  

public void method(){ // Bad

}
```

### 空格隔开

- if、else、for、switch、while等逻辑关键字与后面的语句留一个空格隔开。  
```Java
  // Good
  if (booleanVariable) {
      // TODO while booleanVariable is true
  } else {
      // TODO else
  }

  // Bad
  if(booleanVariable) {
      // TODO while booleanVariable is true
  }else {
      // TODO else
  } 
```
- 运算符两边各用一个空格隔开。  
```Java
int result = a + b; //Good, = 和 + 两边各用一个空格隔开
int result=a+b; //Bad,=和+两边没用空格隔开
```
- 方法的每个参数之间用一个空格隔开。  
```Java
public void method(String param1, String param2); // Good，param1后面的逗号与String之间隔了一个空格
method(param1, param2); // Good，方法调用时，param1后面的逗号与param2之间隔了一个空格
method(param1,param2); // Bad，没有用一个空格隔开
```

### 空行使用  
将逻辑相关的代码段用空行隔开，以提高可读性。空行也只空一行，不要空多行。在以下情况需用一个空行：
- 两个方法之间  
- 方法内的两个逻辑段之间  
- 方法内的局部变量和方法的第一条逻辑语句之间  
- 常量和变量之间  

### 空格缩进
当一个表达式无法容纳在一行内时，可换行显示，另起的新行用8个空格缩进。
```Java
someMethod(longExpression1, longExpression2, longExpression3,  
        longExpression4, longExpression5);
```

### 一行一变量
一行声明一个变量，不要一行声明多个变量，这样有利于写注释。
```Java
private String param1; // 参数1
private String param2; // 参数2
```

### 行宽设置
行宽设置为100，设置格式化时自动断行到行宽位置。

### 自动格式化
使用快捷键进行代码自动格式化。
- Windows：CTRL+ALT+L  
- Mac：OPTION+COMMAND+L  
  
### 函数方法
理论上一个方法最多不要超过40行代码。  

### 资源规范

- 文字大小的单位统一用sp，元素大小的单位统一用dp。。
- 应用中的字符串统一在strings.xml中定义，然后在代码和布局文件中引用。
- 颜色值统一在colors.xml中定义，然后在代码和布局文件中引用。另外，不要在代码和布局文件中引用系统的颜色，除了透明色。

## 命名规范

### 包命名
**域名反写+项目名称+模块名称**，全部单词用小写字母。  
例如，奥昇平台项目的Config模块包名如下：
```Java
cn.aorise.platform.config
```
### 类和接口命名
使用大驼峰规则，用名词或名词词组命名，每个单词的首字母大写。  
- 共享基础类，命名以Base为前缀，如：BaseActivity、BaseFragment  
- activity类，命名以Activity为后缀，如：LoginActivity  
- fragment类，命名以Fragment为后缀，如：ShareDialogFragment  
- service类，命名以Service为后缀，如：DownloadService  
- adapter类，命名以Adapter为后缀，如：CouponListAdapter  
- 自定义view类，命名以View为后缀，如：CustomizeView  
- 模型类，命名以Model为后缀，如：CouponModel  
- 工具类，命名以Util为后缀，如：EncryptUtil  
- 接口实现类，命名以Impl为后缀，如：ApiImpl  
- 数据库类，命名以DBHelper后缀，如：NoteDBHelper  
- 解析类，命名以Hlr后缀，如：XmlPosterHlr  
- 公共方法类，命名以Tools或Manager为后缀，如：LogTools、ThreadPoolManager  
  
### 方法命名
使用小驼峰规则，用动词命名，第一个单词的首字母小写，其他单词的首字母大写。  
- 按钮点击方法，命名以to开头，例：toLogin  
- 设置方法，命名以set开头，例：setData  
- 具有返回值的获取方法，命名以get开头，例：getData  
- 通过异步加载数据的方法，命名以load开头，例：loadData  
- 布尔型的判断方法，命名以is或has，或具有逻辑意义的单词如equals，例：isEmpty  
  
### 控件缩写

|     控件     |   缩写   |      控件      |  缩写  |
| ------------ | -------- | -------------- | ------ |
| TextView     | txt      | EditText       | edt    |
| Button       | btn      | ImageButton    | ibtn   |
| ImageView    | img      | ListView       | list   |
| RadioGroup   | group    | RadioButton    | rbtn   |
| ProgressBar  | progress | SeekBar        | seek   |
| CheckBox     | chk      | Spinner        | row    |
| TableLayout  | table    | TableRow       | edt    |
| LinearLayout | ll       | RelativeLayout | rl     |
| ScrollView   | scroll   | SearchView     | search |
| TabHost      | host     | TabWidget      | widget |

### 常量命名
全部为大写单词，单词之间用下划线分开。
```Java
public final static int PAGE_SIZE = 20;
```

### 变量命名
**{类型描述+}{范围描述+}意义描述**，用m+大驼峰规则命名。  
```Java
private TextView mTxtHeaderTitle; // 标题栏的标题
private Button mBtnLogin; // 登录按钮
```
### 控件id命名
**控件缩写_{范围_}意义**，范围可选，只在有明确定义的范围内才需要加上。
```xml
<!-- 这是标题栏的标题 -->
<TextView
    android:id="@+id/txt_header_title"
    ... />

<!-- 这是登录按钮 -->
<Button
    android:id="@+id/btn_login"
    ... />
```

### Layout文件命名
**组件类型_{范围_}功能**，范围可选，只在有明确定义的范围内才需要加上。  
- activity_{范围_}功能，为Activity的命名格式  
- fragment_{范围_}功能，为Fragment的命名格式  
- dialog_{范围_}功能，为Dialog的命名格式  
- include_{范围_}功能，为Include布局的命名格式（方案一）  
- content_{范围_}功能，为Include布局的命名格式（方案二）  
- item_list_{范围_}功能，为ListView的item命名格式  
- item_grid_{范围_}功能，为GridView的item命名格式  
- header_list_{范围_}功能，为ListView的HeaderView命名格式  
- footer_list_{范围_}功能，为ListView的FooterView命名格式  
  
### String命名
**类型_{范围_}功能**，范围可选。  
- 页面标题，命名格式为：title_页面  
- 按钮文字，命名格式为：btn_按钮事件  
- 标签文字，命名格式为：label_标签文字  
- 选项卡文字，命名格式为：tab_选项卡文字  
- 消息框文字，命名格式为：toast_消息  
- 编辑框的提示文字，命名格式为：hint_提示信息  
- 图片的描述文字，命名格式为：desc_图片文字  
- 对话框的文字，命名格式为：dialog_文字  
- menu的item文字，命名格式为：action_文字  
  
### Color命名
**前缀{_控件}{_范围}{_后缀}**，控件、范围、后缀可选，但控件和范围至少要有一个。  
- 背景颜色，添加bg前缀  
- 文本颜色，添加txt前缀  
- 分割线颜色，添加div前缀  
- 区分状态时，默认状态的颜色，添加normal后缀  
- 区分状态时，按下时的颜色，添加pressed后缀  
- 区分状态时，选中时的颜色，添加selected后缀  
- 区分状态时，不可用时的颜色，添加disable后缀  
  
### Drawable命名
**前缀{_控件}{_范围}{_后缀}**，控件、范围、后缀可选，但控件和范围至少要有一个。  
- 图标类，添加ic前缀  
- 背景类，添加bg前缀  
- 分隔类，添加div前缀  
- 默认类，添加def前缀  
- ~~多种状态的，添加selector前缀（一般为ListView的selector或按钮的selector）~~  
- 区分状态时，默认状态，添加normal后缀  
- 区分状态时，按下时的状态，添加pressed后缀  
- 区分状态时，选中时的状态，添加selected后缀  
- 区分状态时，不可用时的状态，添加disable后缀  
  
### Anim命名
**动画类型_动画方向**   
- fade_in，淡入  
- fade_out，淡出  
- push_down_in，从下方推入  
- push_down_out，从下方推出  
- slide_in_from_top，从头部滑动进入  
- zoom_enter，变形进入  
- shrink_to_middle，中间缩小  
  
## 注释规范

### 文件头注释
文件顶部统一添加版权声明，声明的格式如下：
```Java
/**
 * Copyright (c) 2016. Author Inc. All rights reserved.
 */
```

### 类和接口注释
类和接口统一添加javadoc注释，格式如下：
```Java
/**
 * 类或接口的描述信息
 *
 * @author ${USER}
 * @date ${DATE}
 */
```

### 方法注释
下面几种方法，都必须添加javadoc注释，说明该方法的用途和参数说明，以及返回值的说明。
- 接口中定义的所有方法  
- 抽象类中自定义的抽象方法  
- 抽象父类的自定义公用方法  
- 工具类的公用方法  

```Java
/**
 * 登录
 *
 * @param loginName 登录名
 * @param password  密码
 * @param listener  回调监听器
 */
public void login(String loginName, String password,    ActionCallbackListener<Void> listener);
```  

### 变量和常量注释
下面几种情况下的常量和变量，都要添加注释说明，优先采用右侧//来注释，若注释说明太长则在上方添加注释。
- 接口中定义的所有常量  
- 公有类的公有常量  
- 枚举类定义的所有枚举常量  
- 实体类的所有属性变量  

```Java
public static final int TYPE_CASH = 1; // 现金券
public static final int TYPE_DEBIT = 2; // 抵扣券
public static final int TYPE_DISCOUNT = 3; // 折扣券

private int id; // 券id
private String name; // 券名称
private String introduce; // 券简介
```

## 附录  

- 英文缩写  

|    名称    |                                        缩写                                         |
| ---------- | ----------------------------------------------------------------------------------- |
| icon       | ic（主要用在app的图标）                                                             |
| color      | cl（主要用于颜色值）                                                                |
| divider    | div（主要用于分隔线，不仅包括Listview中的divider，还包括普通布局中的线）            |
| selector   | sel（主要用于某一view多种状态，不仅包括Listview中的selector，还包括按钮的selector） |
| average    | avg                                                                                 |
| background | bg（主要用于布局和子布局的背景）                                                    |
| password   | pwd                                                                                 |
| error      | err                                                                                 |


