# python调用dll
```python
    from ctypes inport *
    dll = CDLL("xxx.dll")
    print dll.function(  ,  )
``` 
---
### *加载dll*
###   stdcall调用约定：两种加载方式 
```python
Objdll = ctypes.windll.LoadLibrary("dllpath")
Objdll = ctypes.WinDLL("dllpath")
   ```
###    cdecl调用约定：也有两种加载方式 
```python
  Objdll = ctypes.cdll.LoadLibrary("dllpath")
    Objdll = ctypes.CDLL("dllpath")
```
####    其实windll和cdll分别是WinDLL类和CDll类的对象。

### 在加载dll的时候会返回一个DLL对象，利用该对象就可以调用dll中的方法
---
## C基本类型和ctypes中实现的类型映射表
ctypes数据类型 | C数据类型
---|:--:|---:
c_char | char 
c_short   |   short 
c_int     |   int 
c_long    |   long 
c_ulong   |   unsign long 
c_float   |   float 
c_double  |   double 
c_void_p  |   void 
### 对应的指针类型是在后面加上"_p"，如int*是c_int_p等等。 
---
## DLL中的函数返回一个指针
### 返回的都是地址，把他们转换相应的python类型，再通过value属性访问
```python
pchar = dll.getbuffer()
szbuffer = c_char_p(pchar)
print szbuffer.value
```
---

## 处理C中的结构体类型 
### 在python里面申明一个类似c的结构体，要用到类，并且这个类必须继承自Structure

### *C代码*
```c
typedef struct 
{
char words[10];
}keywords;
typedef struct 
{
keywords *kws;
unsigned int len;
}outStruct;
extern "C"int __declspec(dllexport) test(outStruct *o);
int test(outStruct *o)
{
unsigned int i = 4;
o->kws = (keywords *)malloc(sizeof(unsigned char) * 10 * i);
strcpy(o->kws[0].words, "The First Data");
strcpy(o->kws[1].words, "The Second Data");
o->len = i;
return 1;
}
```

### *python代码*
```python
class keywords(Structure):
 
        _fields_ = [('words', c_char *10),]
 
class outStruct(Structure):
 
        _fields_ = [('kws', POINTER(keywords)),
 
                    ('len', c_int),]
 
o = outStruct()

dll.test(byref(o))
 
print o.kws[0].words;
 
print o.kws[1].words;
 
print o.len
```
