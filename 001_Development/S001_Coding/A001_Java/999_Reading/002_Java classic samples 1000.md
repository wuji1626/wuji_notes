**Java典型应用彻查1000例**
#1 变量
##1.1 基本数据类型
###1.1.1 字符类型（char）
char类型以Unicode方式编码，用2个字节（byte），16位（bit）以'\u0000'~'\uFFFF'表示。可以表示ASCII和Unicode，表示ASCII时只用一个字节7位用于编码，1位作为检查码，左端用0填补成Unicode码  
参见com.wuji1626.themis.variable.PrimitiveTypeChar类  
###1.1.2 字节类型（byte）
byte类型变量占用1个字节（8位），范围-128~127，只能存储ASCII码数据，不能用于中文数据存储  
参见com.wuji1626.themis.variable.PrimitiveTypeByte类  
###1.1.3 短整型（short）
short变量占两个字节（16位），范围-32768~32767。除表示数之外，也可以处理Unicode字符数据  
参见com.wuji1626.themis.variable.PrimitiveTypeShort类  
###1.1.4 整型（int）
int类型占4个字节（32位）。可以处理英文字母、汉字字符、Unicode码、八进制整数、十进制整数、十六进制整数  
###1.1.5 长整形（long）
long整形占用8各字节（64位）。使用时在右端用L标识  
参见com.wuji1626.themis.variable.PrimitiveTypeLong类  
###1.1.6 浮点型（float）
float占4字节（32位），double占8字节（64位）。浮点型数如果不加修饰会被识别为double类型，如果最右端有F/f修饰，被识别为float型  
float类型数据左侧连接未加f修饰的浮点类型，会出现double类型无法转换为float错误  
参考com.wuji1626.themis.variable.PrimitiveTypeFloat类  
###1.1.7 布尔型（boolean）
boolean型变量占用1字节（8位），内容true/false  

##1.2  引用数据类型（Reference Type）
变量无无确定长度，
 
 
