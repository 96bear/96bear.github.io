---
title: "JavaScript初学笔记1||打印 定义 运算符"
date: "2019-12-25"
categories: 
  - "javascript"
---

// JavaScript初学笔记1
// 打印 定义 运算符 

// 打印
// log("helloword");

// var let const
// var a=10;//var 全局变量
// let b=11;//let 局部变量
// const c=20;//const 常量
// log(a,b,c);

// 简单运算符：+-\*/=><%等
// 关系运算符：&& ||
// 自加等和C一致
//注意===
/\* //Examples 1
const right="成功",misktake="失败";
var a=1,b=3;
var result=a+b;
log(result);//计算赋值

//几句简单判断
if(a===b)
log(right);
else log(misktake);

if(a==b)
log(right);
else log(misktake);

if(a&&b)
log(right);
else log(misktake);

if(a||b)
log(right);
else log(misktake);

var result1=a++;
log(result1);
log(a);//观察变化
var result2=++b;
log(result2);//++在后先赋值后自增 \*/

// test 1 0 true false
/\* if(1&&0)
log("right");
else log("misktake"); \*/
/\* if(true||false)
log("right");
else log("misktake"); \*/
/\* if(true&&1)
log("right");
else log("misktake"); \*/

//test sanyuan
/\* var a=1,b=2;
log(a>b?1:2); \*/
