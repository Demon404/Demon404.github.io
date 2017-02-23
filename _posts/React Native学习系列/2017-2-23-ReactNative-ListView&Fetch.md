---
layout: post  
title: RN系列教程(四)ListView & fetch  
categories: [blog ]  
tags: [React Native基础系列, ]  
description: 
---  
###前言  
去年我写了一个教程，[手把手教你写一个RN小程序!](http://demon404.com/blog/%E6%89%8B%E6%8A%8A%E6%89%8B%E6%95%99%E4%BD%A0%E5%86%99%E4%B8%80%E4%B8%AARN%E5%B0%8F%E7%A8%8B%E5%BA%8F.html),里面其实对ListView已经做了一些详解，不过由于那个项目接口停止维护，再加上RN教程准备写一个系列，所以重新写一篇文章来完善ListView的使用。另外，ListView一般都是配合数据来使用的，所以这里我把网络请求也顺带简单的讲解一下。本文会使用豆瓣api来进行数据的解析。  
###fetch()  
React Native提供了和web标准一致的[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)，用于满足开发者访问网络的需求。那么如何使用呢？  
####fetch使用  
	fetch('https://api.douban.com/v2/movie/top250')//豆瓣电影Top250
从任意地址获取数据，只需要这么写就可以了，把地址传递给fetch()方法。  
	  
	  fetch('https://api.douban.com/v2/movie/top250')
        //ES6的写法左边代表输入的参数右边是逻辑处理和返回结果
        .then((response) => response.json())
        .then((responseData) => {
           this.setState({
             data: responseData,
             });
           })
        .done();  
以上是fetch通过get请求获取的数据，这个可以获取数据源data，那么我们登录注册一般都是用的post提交方式，那该如何写呢？  
fetch()还有可选的第二个参数用来指定请求的方法，你可以指定header参数，或是指定使用POST方法，又或是提交数据等等。   
	
	fetch('https://mywebsite.com/endpoint/', {
 	  method: 'POST',//指定POST方法
  	  headers: {//指定header参数
     	'Accept': 'application/json',
    	'Content-Type': 'application/json',
  	  },
  	 body: JSON.stringify({//设置提交的数据
    	firstParam: 'yourValue',
    	secondParam: 'yourOtherValue',
  	})
	}) 
	.then((response) => response.json())  
	.then((responseData) => {
		//responseData是服务器返回的data
	}
	
	})
      .done();  
简单的fetch()请求就介绍到这里，有想法的同学可以参考该文章：[【翻译】这个API很“迷人”——(新的Fetch API)](https://w3ctech.com/topic/854)  
>温馨提示，iOS默认不支持http请求，请在Xcode中设置  
>
>1、在Info.plist中添加NSAppTransportSecurity类型Dictionary。  
>
2、在NSAppTransportSecurity下添加NSAllowsArbitraryLoads类型Boolean,值设为YES    

###组件生命周期  
一般来说，一个组件类由 extends Component创建，并且提供一个 render方法以及其他可选的生命周期函数、组件相关的事件或方法来定义。  
>getInitialState()函数  初始化 this.state 的值，只在组件装载之前调用一次。  
> getDefaultProps()函数  只在组件创建时调用一次并缓存返回的对象,因为这个方法在实例初始化之前调用，所以在这个方法里面不能依赖 this 获取到这个组件的实例。  
> render()  组装生成这个组件的 HTML 结构 ,必须要有的函数  

生命周期函数  
>componentWillMount() 只会在装载之前调用一次，在 render 之前调用，你可以在这个方法里面调用 setState 改变状态  
>componentDidMount()  只会在装载完成之后调用一次，在 render 之后调用，从这里开始可以通过 ReactDOM.findDOMNode(this) 获取到组件的 DOM 节点。  
> componentWillUnmount()  卸载组件触发  
> 更新组件时触发的函数：  
> componentWillReceiveProps()  
> shouldComponentUpdate()  
> componentWillUpdate()  
> componentDidUpdate  

###ListView  
ListView是一个常用核心组件，用于高效地显示一个可以垂直滚动的变化的数据列表。通过创建一个ListView.DataSource数据源，然后给它传递一个普通的数据数组，再使用数据源来实例化一个ListView组件，并且定义它的renderRow回调函数，这个函数会接受数组中的每个数据作为参数，返回一个可渲染的组件（作为listview的每一行）。  
	
	cellRow(data) {
      return (
          <View >//这个是cell的视图
          </View>
      );  
 	 }

  	render() {
    	return (
     	 <View style={styles.container}>
       	 <ListView
        	    initiaListSize={1} //指定在组件刚挂载的时候渲染多少行数据。用来确保首屏显示合适数量的数据
        	    //onChangeVisibleRows={()=>{}}//当可见的行的集合变化的时候调用此回调函数  
        	    //onEndReached={()=>{}}//当所有的数据都已经渲染过，并且列表被滚动到距离最底部不足onEndReachedThreshold个像素的距离时调用。配合onEndReachedThreshold使用
        	    onEndReachedThreshold={10} //调用onEndReached之前的临界值，单位是像素。
        	    pageSize={3} //每次渲染的行数
        	    removeClippedSubviews={true}//用于提升大列表的滚动性能。需要给行容器添加样式overflow:'hidden'。（Android已默认添加此样式）。此属性默认开启。
        	    dataSource={this.state.dataSource} //列表依赖的数据源
         	   	renderRow={this.cellRow.bind(this)}//(rowData, sectionID, rowID, highlightRow) => renderable 从数据源(Data source)中接受一条数据，以及它和它所在section的ID。返回一个可渲染的组件来为这行数据进行渲染。
         	   	style={styles.listView}
       	 />
     	 </View>
   	 	);
 	 }  

renderSectionHeader()方法会为每个section提供一个粘性的标题  
scrollTo(...args),滚动到指定的x,y偏移处。  
###ListView和Fetch()结合使用  
####1.新建一个文件  
	
	import React, { Component } from 'react';
	import {
 	StyleSheet,
  	Text,
  	View,
	ListView
	} from 'react-native';
	export default class TestDemo extends Component {
  	render() {
    	return (
      	<View style={styles.container}>
      	</View>
    	);
  	}
	}
	const styles = StyleSheet.create({
	container:{
	flex:1,
	}
	});  

####2.引入ListView
	<ListView initiaListSize={2}
              pageSize={2}
              dataSource={this.state.dataSource}
              renderRow={this.cellRow.bind(this)}
              style={styles.listView}
   	/>  
然后根据文档添加数据源dataSource  
	
	constructor(props){
	super(props);
	const ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
	this.state = {dataSource: ds.cloneWithRows([{},])};
	}  
我们的Cell也必定不能少啊  
	
	cellRow(data) {
        return (
            <TouchableOpacity onPress={()=>this.rowPressed(data)}>
          <View>
            <View style={styles.cellStyle}>
                <Image style={{margin:10,width:100, resizeMode:'contain',height:(WIDTH-40)/2}}
                       source={{uri: data.images.large}}
                />
                <Text style={styles.title}>{data.title}</Text>
            </View>
          </View>
        </TouchableOpacity>
        );
    }  
OK基本上完工，说好的fetch呢？别着急，慢慢来  
	
	componentDidMount() {
        this.fetchData();
    }
	fetchData() {
        fetch(GET_URL)
            //ES6的写法左边代表输入的参数右边是逻辑处理和返回结果
            .then((response) => response.json())
            .then((responseData) => {
                if (responseData.subjects) {
                //我们需要在数据解析完成的时候来setdataSource
                    this.setState({
                        dataSource: this.state.dataSource.cloneWithRows(responseData.subjects),
                    });
                } else {
                    //do Something 
                    Alert.alert('暂时没有数据');
                }
            })

        .done
    }
恩，测试的话，GET_URL是[https://api.douban.com/v2/movie/top250](https://api.douban.com/v2/movie/top250),
至此基本上已经完工，大家可以试着做一下
![from Demon404](http://upload-images.jianshu.io/upload_images/2781235-6e67d8f62258fb5f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
###最后 
当然要附上我们的源码了，在我的github上:[React-Native-Study](https://github.com/Demon404/React-Native-Study)  
后续会更新带Header的ListView和用ListView写的九宫格，喜欢的同学可以支持一下，么么哒！

![from Demon404](http://upload-images.jianshu.io/upload_images/2781235-b945831fba3e7c01.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)