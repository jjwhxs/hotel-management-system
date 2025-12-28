### 系统介绍

基于SpringBoot和Vue实现的酒店管理系统采用前后端分离的架构方式，系统设计了两种角色，分别是用户、管理员，每种角色拥有不同的菜单权限，用户可以在系统前台中进行注册、登录、个人信息、我的预约、我的入住、首页、系统公告、用户留言、预约入住等功能模块的操作，管理员可以在系统后台中进行登录、控制面板、管理员管理、用户管理、房型管理、房间管理、预约管理、入住管理、留言管理、公告管理等功能模块的操作。

### 技术选型

开发工具：idea2022.3+webstorm2021.1

运行环境：jdk1.8+maven3.6.3+mysql8.0+nodejs16.0.0

服务端技术：springboot+mybatis-plus+jwt+fastjson

前端技术：html+css+vue+axios+element-ui+echarts

### 成果展示

系统登录/注册
<img width="1895" height="1021" alt="登录" src="https://github.com/user-attachments/assets/ad0e63a4-cff4-48a3-905e-65348ff59e01" />
<img width="1890" height="1015" alt="注册" src="https://github.com/user-attachments/assets/62ca600d-f58b-406b-8b8f-2f058c9f8e72" />

前台系统->首页
<img width="1899" height="1023" alt="首页-轮播图" src="https://github.com/user-attachments/assets/28e529ae-e27b-4109-a368-76984c7ed09d" />
<img width="1890" height="1020" alt="首页-客房展示" src="https://github.com/user-attachments/assets/7fac684a-b43e-43de-bee2-91d917d6f11f" />

前台系统->系统公告
<img width="1894" height="1018" alt="前台-系统公告" src="https://github.com/user-attachments/assets/db48cf9f-43d4-4132-9e39-d0fe52099f80" />

前台系统->用户留言
<img width="1894" height="1021" alt="前台-用户留言" src="https://github.com/user-attachments/assets/c4a6801c-a730-475c-b5a3-a949c5ce67bd" />

前台系统->预约入住
<img width="1898" height="1025" alt="前台-预约入住" src="https://github.com/user-attachments/assets/8c6a6324-cb45-4182-af1a-c13bf3ffc258" />

前台系统->我的预约
<img width="1901" height="1021" alt="前台-我的预约" src="https://github.com/user-attachments/assets/de21e670-4139-49c0-86e7-d61848cfeddf" />

前台系统->我的入住
<img width="1901" height="1029" alt="前台-我的入住" src="https://github.com/user-attachments/assets/58b89aab-14db-4971-8160-b4e0d7049003" />
<img width="1900" height="1021" alt="前台-我的入住2" src="https://github.com/user-attachments/assets/8669fe0c-ea65-4677-85c7-42bc0fe0267b" />

后台系统->控制面板
<img width="1900" height="1020" alt="后台-控制面版" src="https://github.com/user-attachments/assets/b3ea7d54-4b73-42c3-a7fc-a79716e7be9c" />

后台系统->用户管理
<img width="1900" height="1024" alt="后台-用户管理" src="https://github.com/user-attachments/assets/3f2848df-7c33-47c7-b53d-e7218b496ca9" />

后台系统->房型管理
<img width="1899" height="1025" alt="后台-房型管理" src="https://github.com/user-attachments/assets/82ee4423-0fee-4747-94ce-f22b38517f70" />

后台系统->房间管理
<img width="1900" height="1024" alt="后台-房间管理" src="https://github.com/user-attachments/assets/9c586ae2-cf4f-48bf-a687-8319e35aa46d" />

后台系统->预约管理
<img width="1901" height="1024" alt="后台-预约管理" src="https://github.com/user-attachments/assets/b3ce4c47-13f7-48a7-aa57-337f3354b564" />

后台系统->入住管理
<img width="1899" height="1021" alt="后台-入住管理" src="https://github.com/user-attachments/assets/83efc33e-9108-4b54-ad0e-ac8db71e5cb6" />

后台系统->留言管理
<img width="1900" height="1025" alt="后台-留言管理" src="https://github.com/user-attachments/assets/198dc439-8972-4383-bcd3-6a9f5d40645a" />

后台系统->公告管理
<img width="1900" height="1025" alt="后台-公告管理" src="https://github.com/user-attachments/assets/a8e38d6c-9de6-420b-bd5a-95c16583c205" />

### 源码展示

@RestController

@RequestMapping("/message")

public class MessageController {

@Autowired

private MessageService messageService;

//查询全部

@GetMapping

public Result findAll(){

    List<Message> messageList = messageService.findAll();
    return new Result(true, StatusCode.OK,"查询成功",messageList);
    
}

//根据条件查询

@GetMapping("search")

public Result search(Message message){

    List<Message> messageList = messageService.search(message);
    return new Result(true, StatusCode.OK,"查询成功",messageList);
    
}

//根据条件分页查询

@GetMapping("search/{current}/{size}")

public Result search(@PathVariable Integer current,@PathVariable Integer size ,Message message){

    Page<Message> page = messageService.search(new Page<Message>(current, size), message);
    return new Result(true, StatusCode.OK,"查询成功",page);
    
}

//用户留言

@PostMapping

public Result add(@RequestBody Message message, HttpServletRequest request){

    Long userId = JWTUtils.getUserId(request);
    message.setMemberId(userId.intValue());
    message.setCreateTime(new Date());
    messageService.add(message);
    return new Result(true, StatusCode.OK,"操作成功");
    
}

//回复留言

@PutMapping

public Result modify(@RequestBody Message message){

    message.setStatus(2);
    message.setReplyTime(new Date());
    messageService.modify(message);
    return new Result(true, StatusCode.OK,"操作成功");
    
}

//根据id查询

@GetMapping("{id}")

public Result findById(@PathVariable("id") Integer id){

    Message message  = messageService.findById(id);
    return new Result(true, StatusCode.OK,"查询成功",message);
    
}

//根据id删除

@DeleteMapping("{id}")

public Result removeById(@PathVariable("id") Integer id){

    messageService.removeById(id);
    return new Result(true, StatusCode.OK,"删除成功");
    
}

}

### 账号地址及其它说明

1、地址说明

登录页：localhost:9001

2、账号说明

用户：zhangsan/123456

管理员：admin/123456

3、目录结构展示

<img width="700" height="169" alt="目录结构" src="https://github.com/user-attachments/assets/f53e43ec-f4f2-4ae1-b1fd-5fa4a0068e8e" />

4、项目结构展示

<img width="1434" height="720" alt="项目结构" src="https://github.com/user-attachments/assets/a1b0728e-0772-4b91-8af6-95bb2fc80fb8" />

5、运行步骤

1）创建数据库、导入sql脚本

2）修改application.yml中的数据库配置文件，启动服务端

3）在hotely5d-web目录下打开cmd，执行npm install下载依赖

4）下载完毕后启动前端npm run dev，访问端口

### 获取方式(可远程调试)

访问链接(在浏览器中手动输入下图中的地址)：

<img width="1125" height="130" alt="链接" src="https://github.com/user-attachments/assets/503ccc70-0bf0-447f-ba25-59ac446c19ba" />

若资源获取失败，可添加happy35596339(vx)或1204901965(qq)进行交流
