- @RequestBody  
将HTTP请求正文转换为适合的HttpMessageConverter对象。
```Java
@RequestMapping(value = "/person/login", method = RequestMethod.POST)  
    public @ResponseBody  
    Person login(@RequestBody Person person) {  
        return person;  
    }
```
- @ResponseBody  
将内容或对象作为HTTP响应正文返回，并调用适合HttpMessageConverter的Adapter转换对象，写入输出流。
- @PathVariable  
将uri中的变量注入参数
```Java
@RequestMapping(value = "/person/profile/{id}/{name}/{status}", method = RequestMethod.GET)  
    public @ResponseBody  
    Person porfile(@PathVariable int id, @PathVariable String name,  
            @PathVariable boolean status) {  
        return new Person(id, name, status);  
    }  
```
## 总结
- GET模式下，这里使用了@PathVariable绑定输入参数，非常适合Restful风格。因为隐藏了参数与路径的关系，可以提升网站的安全性，静态化页面，降低恶意攻击风险。
- POST模式下，使用@RequestBody绑定请求对象，Spring会帮你进行协议转换，将Json、Xml协议转换成你需要的对象。
- @ResponseBody可以标注任何对象，由Spring完成对象——协议的转换。