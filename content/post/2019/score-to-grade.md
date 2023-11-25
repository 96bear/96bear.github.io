---
title: "Score to grade"
date: "2019-11-20"
categories: 
  - "zatan"
---

```
#include<stdio.h>
int main()
{
int score;
printf("Please input ur score\n");
scanf("%d",&score);
if(score>100||score<0)
{printf("error!\n");
return 0;
}
score/=10;
switch(score)
{
case 10:printf("A\n");break;
case 9:printf("A\n");break;
case 8:printf("B\n");break;
case 7:printf("C\n");break;
case 6:printf("D\n");break;
default:printf("E\n");break;
}
return 0;
}
```
