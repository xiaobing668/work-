# work-
#include<stdio.h>
#include<stdlib.h> 
#include<string.h> 

/*********加密子函数开始************/ 
void add_file(char *in_fname,char *pwd,char *out_file) 
{ 
FILE *fp1,*fp2;
register char ch; 
int j=0; 
int jj=0; 


fp1=fopen(in_fname,"rb"); 
if(fp1==NULL)
 { 
  printf("文件打开失败！\n"); 
  exit(1);             //如果不能打开要加密的文件,便退出程序
 } 
fp2=fopen(out_file,"wb"); 
if(fp2==NULL)
 { 
  printf("新建文件失败！\n"); 
  exit(1);                //如果不能建立加密后的文件,便退出 
} 
while(pwd[++jj]) ; //算出密钥长度，保存至jj

   ch=fgetc(fp1); //加密算法开始

while(!feof(fp1)) //测试文件是否结束 
 { 
  fputc(ch^pwd[j>=jj?j=0:j++],fp2);//异或后写入fp2文件,加密和解密互逆
  ch=fgetc(fp1); 
 }

 fclose(fp1);//关闭源文件 
 fclose(fp2);//关闭目标文件 
} 

main(int argc,char *argv[])  
{ 
 char in_fname[30];//用户输入的要加密的文件名 
 char out_fname[30]; 
 char del_fname[36]="del "; //删除文件名及命令
 int i;
 char pwd[20];//用来保存密码 

printf("功  能:实现文件的加密和解密!\n注意:应用程序需跟文件放在同一个目录下!\n采用文件逐字节与密码异或方式对文件进行加密，密码需在8个字符或数字以内,当解密时，只需再运行一遍加密程序即可\n\n\n");

if(argc!=4) //容错处理
 {  
  printf("输入需要加密或者解密的文件(加后缀):\n"); 
  gets(in_fname);           //得到要加密的文件名  
  printf("输入密钥:\n"); 
  gets(pwd);               //得到密码 
  printf("输入解密或加密后的新文件名(加后缀):\n"); 
  gets(out_fname);//得到加密后你要的文件名 
  add_file(in_fname,pwd,out_fname); 
 } 
else                 //如果命令行参数正确,便直接运行程序
 {         
  strcpy(in_fname,argv[1]); 
  strcpy(pwd,argv[2]); 
  strcpy(out_fname,argv[3]); 
  add_file(in_fname,pwd,out_fname); 
 }

} 

