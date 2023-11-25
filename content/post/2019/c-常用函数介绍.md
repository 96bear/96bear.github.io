---
title: "C <string.h>常用函数介绍"
date: "2019-08-31"
categories: 
  - "编程"
---

**1\. strcpy** char \*strcpy(char \*destin, char \*source); 功能：将source指向的字符串拷到destin。

1 int main() 2 { 3 4 char dest\[5\]; 5 char \*src="123456"; 6 strcpy(dest, src); 7 printf("dest= %s, %s, %s", dest, dest+4, dest+5); 8 9 return 0; 10 }

从结果可知确实将src的内容复制过去了，但是全部复制导致dest满了，使用不当就会出错！ 2. strncpy char \*strncpy(char \*destin, char \*source, int len); 功能：将source指向的len个字符串拷到destin。

1 int main() 2 { 3 4 char dest\[5\]; 5 char \*src="123456"; 6 strncpy(dest, src, 3); 7 dest\[3\]= '\\0'; 8 printf("dest= %s, %s, %s", dest, dest+4, dest+5); 9 10 return 0; 11 }

结果可知加上‘\\0’结束符后dest内容变的更安全，strcpy和strncpy要额外加字符结束符！ 3. strcat char\* strcat(char \* str1,char \* str2); 功能: 把字符串str2接到str1后面,str1最后的'\\0'被取消

1 int main() 2 { 3 4 char dest\[5\]="abcd"; 5 char \*src="123456"; 6 strcat(dest, src); 7 printf("dest= %s", dest); 8 9 return 0; 10 }

4\. strncat char \*strncat(char \*dest, const char \*src, size\_t maxlen) 功能: 将字符串src中前maxlen个字符连接到dest中

1 int main() 2 { 3 4 char dest\[10\]="abcd"; 5 char \*src="123456"; 6 strncat(dest, src, 8); 7 printf("dest= %s", dest); 8 9 return 0; 10 }

与strncpy不同，strncat会自动在末尾加‘\\0’，若指定长度超过源字符串长度，则只复制源字符串长度即停止，更安全！ 5. strcmp int strcmp(char \* str1,char \* str2); 功能: 比较两个字符串str1,str2 返回: str1<str2,返回负数;str1=str2,返回 0;str1>str2,返回正数

1 int main() 2 { 3 4 char dest\[10\]="abcd"; 5 char \*src="a23456"; 6 char d2\[8\]="abcd"; 7 int res; 8 res=strcmp(dest, src); 9 printf("res= %d \\n", res); 10 res=strcmp(dest, d2); 11 printf("res= %d \\n", res); 12 13 return 0; 14 }

结果可知每一位都要比较，且与原字符数组长度无关。 6. strncmp int strncmp(char \*str1,char \*str2,int count) 功能: 对str1和str2中的前count个字符按字典顺序比较 返回: 小于0：str1<str2，等于0：str1=str2，大于0：str1>str2

int main() {

char dest\[10\]="abcd"; char \*src="a23456"; char d2\[8\]="abcd"; int res; res=strncmp(dest, src, 1); printf("res= %d \\n", res); res=strncmp(dest, d2, 1); printf("res= %d \\n", res);

return 0; }

7\. strchr char\* strchr(char\* str,char ch); 功能: 找出str指向的字符串中第一次出现字符ch的位置 返回: 返回指向该位置的指针,如找不到,则返回空指针

1 int main() 2 { 3 4 char dest\[10\]="abcd"; 5 char\* rp; 6 char ch= 'c'; 7 rp=strchr(dest, ch); 8 if(NULL == rp) 9 printf("no %c exist", ch); 10 else11 printf("pos of %c is %d", ch, (int)(rp-dest+1)); 12 13 return 0; 14 }

8\. strrchr char \*strrchr(const char \*s, int c) 功能: 得到字符串s中最后一个含有c字符的位置指针 返回: 位置指针

1 int main() 2 { 3 4 char dest\[10\]="abcdabc"; 5 char\* rp; 6 char ch= 'c'; 7 rp=strrchr(dest, ch); 8 if(NULL == rp) 9 printf("no %c exist", ch); 10 else11 printf("pos of %c is %d", ch, (int)(rp-dest+1)); 12 13 return 0; 14 }

strrchr 比strchr多的 r 意指反向寻找，位置都是从1开始计数（非从0开始） 9. strstr char\* strstr(char\* str1,char\* str2); 功能:找出str2字符串在str1字符串中第一次出现的位置(不包括str2的串结束符) 返回: 返回该位置的指针,如找不到,返回空指针

1 int main() 2 { 3 4 char dest\[10\]="abcdabc"; 5 char\* rp; 6 char ch1\[\]= "c"; 7 char str2\[\]= "cda"; 8 rp=strstr(dest, ch1); 9 if(NULL == rp) 10 printf("no %s exist", ch1); 11 else12 printf("substring is %s \\n", rp); 13 14 rp=strstr(dest, str2); 15 if(NULL == rp) 16 printf("no %s exist", str2); 17 else18 printf("substring is %s ", rp); 19 20 return 0; 21 }

可以找单个字符串（字符不符合参数要求） 10. strnset char \*strnset(char \*s, int ch, size\_t n) 功能: 将字符串s中前n个字符设置为ch的值 返回: 指向s的指针

1 int main() 2 { 3 4 char dest\[10\]="abcdabc"; 5 char\* rp; 6 char ch= 'F'; 7 rp=strnset(dest, ch, 4); 8 printf("after strnset dest is %s \\n", rp); 9 10 return 0; 11 }

11\. strset char \*strset(char \*s, int ch) 功能: 将字符串s中所有字符设置为ch的值 返回: 指向s的指针

1 int main() 2 { 3 4 char dest\[10\]="abcdabc"; 5 char\* rp; 6 char ch= 'F'; 7 rp=strset(dest, ch); 8 printf("after strnset dest is %s \\n", rp); 9 printf("after strnset dest is %s \\n", dest); 10 return 0; 11 }

结果的 rp和dest 都被修改为同一内容！ 12. strtok char \*strtok(char \*s1, const char \*s2) 功能:分解s1字符串，用特定分隔符(s2)分隔成多个字符串 返回: 字符串s1中首次出现s2中的字符前的子字符串指针 strtok()在参数s1的字符串中发现参数s2中包涵的分割字符时，则会将该字符改为\\0字符。在第一次调用时，strtok()必需给予参数s1字符串，往后的调用则将参数s1设置成 NULL。每次调用成功则返回指向被分割出片段的指针。

1 int main() 2 { 3 4 char dest\[\]="ab,cd,ef,c"; 5 char\* rp; 6 char ch\[\]= ","; 7 rp=strtok(dest, ch); 8 while(NULL != rp) 9 { 10 printf("dest: %s ", dest); 11 printf("rp: %s \\n", rp); 12 rp=strtok(NULL, ch); 13 } 14 15 return 0; 16 }

说明：尽量使用可重入版的strtok，Windows平台下为strtok\_s，Linux平台下为strtok\_r。 牢记strtok函数族的分隔规则：忽略字符串前后的分隔符，连续的分隔符被当做一个处理。 在使用strtok前，请对源字符串进行备份，除非你可以接受字符串被修改这一事实（修改为分隔的第一个字符串）。 13. strupr char \*strupr(char \*s) 功能: 将字符串s中的字符变为大写

1 int main() 2 { 3 4 char dest\[\]="ab,cd,EF,c"; 5 char\* rp; 6 rp=strupr(dest); 7 printf("dest: %s, rp: %s", dest, rp); 8 9 return 0; 10 }

原字符串dest 也被修改！！，对符号和大写字符无影响。 char \*strlwr(char \*s)与它相反，将字符串中的字符变为小写字符 还有一些 memxxx() 函数下次单独说明，有问题欢迎评论~~
