---
layout: post
title: RN系列教程(三)TextInput
categories: [blog ]
tags: [React Native基础系列, ]
description: 
---  

今天我们讲的是使用频率比较高的TextInput组件。  
对于TextInput组件大家肯定不陌生，QQ,微信，各个App都有登陆注册模块，需要大家输入信息，这就用到了TextInput！  

###   TextInput组件  

TextInput是一个允许用户在应用中通过键盘输入文本的基本组件，它有许多特性可以自己定制，今天我们就来讲解一下!  
####	First  
我们先来创建一个inputText.js文件   
	 
	import React, {
  	  Component
	} from 'react';
	import {
  	View,
  	StyleSheet,
  	Image,
  	TextInput,
  	Dimensions,
  	Text
	} from 'react-native';
	let ScreenHeight = 	Dimensions.get('window').height - 64;
	import NavBar from '../navbar';
	export default class TextInputView extends Component {
 	 render() {
    	return (
      	<View>
       	 <NavBar name='TextInput' back={()=>{this.props.navigator.pop()}}/>
       	 <View style={styles.container}>
          	<View style={{flex:1,justifyContent:'center'}}>
           		<TextInput style={[styles.baseStyle,styles.first]}>
           		</TextInput>
          	</View>
          	<View style={{flex:1,justifyContent:'center'}}>
            	<TextInput style={[styles.baseStyle,styles.second]}>
           		</TextInput>
          	</View>
          	<View style={{flex:1,justifyContent:'center'}}>
           		<TextInput style={[styles.baseStyle,styles.third]}>
            	</TextInput>
          	</View>

            <TextInput style={[styles.baseStyle,styles.fourth]}>{/**/}
            </TextInput>
        </View>
      </View>
   	 )
  	}
	}

	const styles = StyleSheet.create({
  	container: {
   	 height: ScreenHeight,
   	 backgroundColor: 'gainsboro',
   	 alignItems: 'center',
   	 justifyContent: 'center',
 	 },
 	 baseStyle: {
  	  height: 40,
  	  backgroundColor: 'white',
  	  width: 200
  	},
 	 first: {
	
 	 },
 	 second: {
		
 	 },
 	 third: {
	
  	 },
  	 fourth: {

  	 },
	});
	
这样，我们就创建了一个这样的页面  
![](http://upload-images.jianshu.io/upload_images/2781235-3ce0f8e5f73960bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
从上到下，依次是输入框1-4，那么我们现在就可以来学习如何使用了  

####	Second  
可能有人会问了，为什么最后一个位置不对呢？
都设置居中了啊，所以大家要注意，一般来说，设定TextInput的位置的时候，都是配合View来使用的！  

咱们先看一下大写切换属性autoCapitalize，   
 
	characters: 所有的字符都切换为大写，就是说，你输入框里的字符全都是大写。  
	words: 每个单词的第一个字符。比如说I Am Demon404!  
	sentences: 每句话的第一个字符切换为大写，这个是默认的。  
	none: 不自动切换任何字符为大写。    
autoCorrect={false}||autoCorrect={true},这个属性是拼写自动纠正提示的属性。  
autoFocus，也是一个bool值类型的属性，它的作用是在组件生命周期的componentDidMount后获得焦点。  
defaultValue：(string)设置输入框的初始值，当开始输入的时候值可以改变。  
editable:(bool)设置是否可以编辑。  
keyboardType：设置键盘类型，("default", 'numeric', 'email-address', "ascii-capable", 'numbers-and-punctuation', 'url', 'number-pad', 'phone-pad', 'name-phone-pad', 'decimal-pad', 'twitter', 'web-search')这些是它的类型  
maxLength:(int)限制文字输入的长度。    
multiline:(bool)设置是否可以输入多行文字,  
onBlur:(fun)当文本框失去焦点的时候触发该方法  
onChange:(fun)当文本框文字改变的时候触发该方法  
onChangeText:(fun)文本框文字改变的时候调用，会把输入的文字传递    
onEndEditing (fun) 当文本输入结束后调用此回调函数。  
onFocus (fun)当文本框获得焦点的时候调用此回调函数。  
onLayout (fun)当组件挂载或者布局变化的时候调用，参数为{x, y, width, height}。  
placeholder:(str)如果没有文字输入的话会显示此占位字符串  
placeholderTextColor:(str)更改placeholer占位字符串的文字颜色  
secureTextEntry (bool)设置是否加密，常用于密码输入  
selectionColor:(str)设置输入框高亮时的颜色  
安卓输入框默认的有下划线，可以设置这个属性来隐藏  
underlineColorAndroid='transparent'   
####	Final 
基本上常用的属性都介绍完了，那么它的样式呢？  
TextInput集成了Text的所有样式，如果有不明白的，可以看一下我的第一篇文章  




  

	
		
