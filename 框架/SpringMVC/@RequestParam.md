- Spring会根据参数名字来封装注入
```Java
@RequestMapping("testRequestParam")    
   public String filesUpload(@RequestParam String inputStr, HttpServletRequest request) {    
    System.out.println(inputStr);  
      
    int inputInt = Integer.valueOf(request.getParameter("inputInt"));  
    System.out.println(inputInt);  
      
    // ......省略  
    return "index";  
   }     
```
- 可以对传入参数指定参数名
```Java
@RequestParam String inputStr  
// 下面的对传入参数指定为aa，如果前端不传aa参数名，会报错  
@RequestParam(value="aa") String inputStr  
```
- 可以通过required=false或者true来要求@RequestParam配置的前端参数是否一定要传
```Java
@RequestMapping("testRequestParam")    
    public String filesUpload(@RequestParam(value="aa", required=true) String inputStr, HttpServletRequest request)
```