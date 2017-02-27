#Java典型应用彻查1000例
##1 变量
###1.1 字符类型（char）
char类型以Unicode方式编码，用2个字节（byte），16位（bit）以'\u0000'~'\uFFFF'表示。可以表示ASCII和Unicode，表示ASCII时只用一个字节7位用于编码，1位作为检查码，左端用0填补成Unicode码  
参见com.wuji1626.themis.variable.PrimitiveTypeChar类  
###1.2 字节类型（byte）
byte类型变量占用1个字节（8位），范围-128~127，只能存储ASCII码数据，不能用于中文数据存储  
参见com.wuji1626.themis.variable.PrimitiveTypeByte类  
###1.3 短整型（short）
short变量占两个字节（16位），范围-32768~32767。除表示数之外，也可以处理Unicode字符数据  
参见com.wuji1626.themis.variable.PrimitiveTypeShort类  
###1.4 整型（int）
int类型占4个字节（32位）。可以处理英文字母、汉字字符、Unicode码、八进制整数、十进制整数、十六进制整数  


