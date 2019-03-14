# python调用dll
```python
    from ctypes inport *
    dll = CDLL("xxx.dll")
    print dll.~~function~~(  ,  )
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
---
### 在加载dll的时候会返回一个DLL对象，利用该对象就可以调用dll中的方法
