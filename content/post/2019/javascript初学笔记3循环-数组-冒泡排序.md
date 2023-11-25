---
title: "JavaScript初学笔记3||循环 数组 冒泡排序"
date: "2019-12-25"
categories: 
  - "javascript"
---

 // JavaScript初学笔记3||循环 数组 冒泡排序

/\* for(;;)
log("LOL\\n"); \*/
/\* while(true)
log("LOL\\n"); \*/
/\* do log("LOL\\n");
while(true); \*/
/\* var a=3
for(;;)
log("LOL\\n"+a);//log的复合打印写法 和 牛X的不加;也能运行 \*/

/\* var number =\[1,2,3,4,5,6\];
//数组的表示
/\* log(number\[0\],number\[1\]+number\[2\]);//尝试输出1 5
log(number\[0\],number\[1\],number\[2\]);//right
log(number.length);//length \*/
/\* for(let count=0;count<number.length;count++)
log(number\[count\]); //以上二者的结合\*/

// number.push(100);//数组放入
// number.shift();//删除左侧第一个 队列先进先出?
// number.pop();//删除右侧最后一个 所以什么结构？\*/
//log(number)  

/\* var messages="hello|world";
var result=messages.split("|");
log (result\[0\]+"\\n");
log (result\[1\]);//转为了两个小的字符串，分别存入 好奇内在
 \*/

/\* //由大到小冒泡排序
var number =\[16,23,3,48,15,46,75,123,54,11,2,8,33\];
//while(number.length===100)number.push(random(1,140));
//var kk=number.length;
for(let i=0;i<number.length-1;i++)
{
    //for(let j=0;j<kk-1;j++)//kk的自减可以改成
    for(let j=0;j<number.length-1-i;j++)
    {
        if(number\[j\]<number\[j+1\])//如果小到大这里改为>
        {let a=number\[j\];
            number\[j\]=number\[j+1\];
            number\[j+1\]=a;
        }
    }
}
log(number); \*/
