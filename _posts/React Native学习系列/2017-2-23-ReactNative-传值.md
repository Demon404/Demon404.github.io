---
layout: post  
title: RN系列教程(五)值传递  
categories: [blog ]  
tags: [React Native基础系列, ]  
description: 
---    

## 前言  
前段时间给大家介绍了一些常用的组件，这些其实能完成一些最基础的页面，那么今天给大家介绍一下组件参数传递。至于navigator,我以前写了一篇文章，简单的介绍了一下，想看的可以看一下这里，[react-native 简单的导航](http://www.jianshu.com/p/01d8c4f033a6),那么更详细的文章在这里[新手理解Navigator的教程](http://bbs.reactnative.cn/topic/20/%E6%96%B0%E6%89%8B%E7%90%86%E8%A7%A3navigator%E7%9A%84%E6%95%99%E7%A8%8B)。  

### 正向传值  
正向传值还是比较简单的，我们使用navigator的属性来传递给下一个页面。  
在我们push到下一个页面的时候我们可以把值包含在params里面  
  
```  
this.props.navigator.push({
			component: SecondPage,
			params: {
				secondValue: '这是传递到第二页的数据',   
				//第二页通过this.props.secondValue获取secondValue的值。
			}
		})
```    
然后我们可以在第二个页面来使用this.props.secondValue获取secondValue的值，这样我们就把第一页的值传递到第二页了。  
  
### 反向传值  
反向传值有两种方法，第一种是回调，第二种是通过EventEmitter实现事件机制。  
#### 回调  
我们需要在第一个页面navigator的push方法中的params里面加入回调函数    
  
```
this.props.navigator.push({
						component: Third,
						params: {
							callback: ((word) => {
								this.setState({
									next: word
								})
							}),   
							
							//这里同时还可以添加值传递到第二个页面
						},
					});
```  
callback: ((word) => {this.setState({next: word})});  
我们通过word来接收第二个页面传递过来的值，然后实现回调函数，这里是直接实现修改this.state.next的值。  

那么我们第二个页面如何实现把值传递过来呢？   
 
```
this.props.navigator.pop();  
this.props.callback('我是navigator反向传值');
```   
在第二个页面的返回方法中加入上面的语句就可以了，['我是navigator反向传值']()是我们传递给第一个页面的数据。  
#### EventEmitter事件机制传值  
其实也就是观察者模式。发送端发送通知，然后接收端接收通知改变状态。那么我们如何去做呢？  
首先，我们的两个页面需要引进DeviceEventEmitter组件，引入方法和Text、View等一样。  
  
```  
DeviceEventEmitter.addListener('test', (value) => {
			this.setState({
				name: value
			});
		})  
```  
我们需要在第一个页面中加入这个方法，'test'是我们在第二个页面注册的名称，value就是第二个页面传递过来的值，我们实现setState方法后，this.state.name就是我们第二个页面传递过来的值。那么第二个页面要如何做呢？  
  
```
DeviceEventEmitter.emit('test', '我是DeviceEventEmitter反向传值');
```  
我们只需要在返回的方法里面注册一下通知就可以了，test是注册的名称，后面跟的这个是我们需要传递给第一个页面的数据。   
### 组件之间的通讯  
#### 父组件传值给子组件  
这种传递可以用props来传递。
比如说我们的父组件是一个View,View里面包含一个我们自定义的组件，自定义组件里面有一个Text的文本，我们如何在父组件中修改Text的文本呢？  

```
export default class Fview extends Component {
   render() {
      return (
         <View>
         <MyView text='子组件'/>
      </View>
      );
   }
}
class MyView extends Component {
   render() {
      return (
         <Text>{this.props.text}</Text>{/*通过props传值*/}
      );
   }
}
```   
### 子组件传递给父组件  
类似于ListView的cell点击方法传递。  
比如说我们新建一个自定义组件。  

```  
//我们的子组件
'use strict';

import React, {
  Component
} from 'react';

import {
  StyleSheet,
  View,
  TouchableOpacity,
  Text
} from 'react-native';

class TestSend extends Component {
  render() {
    return (
      <TouchableOpacity onPress=?>{/*如何把这个点击方法传递给父组件？*/}
            <View style={styles.innerViewStyle}>
              <Image style={{width:70,height:70,marginTop:19}} source={{uri:data.image}}/>
              <Text style={{marginTop:6}}>{data.subjectName}</Text>
            </View>
        </TouchableOpacity>
    );
  }
}

const styles = StyleSheet.create({
innerViewStyle: {
    width: 50,
    height: 50,
    // justifyContent:'center',  
    alignItems: 'center',
  }
});


export default TestSend;
```  
我们的父组件要使用这个子组件，然后实现点击这个子组件触发某个方法，并且把子组件的值传递给父组件，该怎么写？  
  
```  
render() {
      return (
         <View>
            <TestSend/>{/*???*/}
         </View>
      );
   }  
```  
我们需要加入回调方法，子组件中写上    

```   
<TouchableOpacity onPress={()=>this.props.callBackFunc(data)}>  
</TouchableOpacity>
```  
其中data就是我们传递给父组件的数据  
父组件中    

```  
<TestSend callBackFunc={(rowdata)=>{this.popToList(rowdata)}}/>  
......  
popToList(rowdata) {
	//我们在父组件中实现的方法，rowdata是我们的子组件传递过来的数据。
}
```    
源码我会上传至[React-Native-Study](https://github.com/Demon404/React-Native-Study) 






