---
url: https://blog.csdn.net/qq_66345100/article/details/131986713
title: 黑马点评项目学习笔记（15w 字详解，堪称史上最详细，欢迎收藏）-CSDN 博客
date: 2025-02-25 23:35:24
tag: 
summary: 文章浏览阅读 6w 次，点赞 366 次，收藏 1.2k 次。
---
## 黑马点评项目学习笔记

### 前言

 当前笔记是第二遍学习 [Redis 实战](https://so.csdn.net/so/search?q=Redis%E5%AE%9E%E6%88%98&spm=1001.2101.3001.7020)篇所写的，第一次学习要在 1 月份前了，当时刚学完 SpringBoot 练手项目瑞吉外卖，然后放松之时刷到 B 站 UP 主 鱼皮 的视频，他推荐大家如果学完 SpringBoot 后可以尝试着学习这门 Redis 课程。我也有幸知道了 B 站还有一门这么牛 X 的课程，于是当时一股脑连着肝了两周把这么这门课程看完了。当时是第一次接触到 Redis，被这么课程给深深打动了，不愧是被广大 B 友称之为 B 站最有含金量的一门课程，里面不仅大量运用了 Lambda 表达式、还有大量高并发相关知识点，感触最大的还是对于各种 Redis 相关知识的应用，比如如何解决数据不一致问题，如何解决缓存穿透、缓存击穿、缓存雪崩（这个只给出了解决思路，没有具体的解决方案），以及分布式下秒杀常见问题及其解决方案，如何使用 Jmeter 进行压测，还有 Redis 中各类数据结构的应用场景、如何应用等等。

  当时学起来还是有一点吃力的，对于一些代码为什么这么写、多线程下锁的使用，如何确保事务不失效等有点不明所以，然后我就去学习了一些 JUC 相关知识，后面也就是 6 月份，我开始打算重新学习一遍，争取把每一行代码的用意以及为什么要怎么写要搞懂，同时也算是为面试做准备，因为我打算在简历上项目中大量运用 Redis，于是再次历时一周重新学习了一遍，这一遍，我把每一个版本的迭代都使用 git 打上了 tag，这样我就能够随时看到我想要的了，并且重新调整了一下项目，梳理项目的业务逻辑，比如如何确保秒杀的正确性、如何保障分布式环境下不容易发生死锁、超卖等问题，同时按照阿里巴巴开发规约让整个项目显得更加规范。这一次我也没有像之前跟着老师敲，而是自己敲一遍，不懂的直接看 PPT、看老师提供的代码，对于一些模糊的知识点我就直接看视频复习一遍

**相关推荐**：

*   [黑马程序员 Redis 入门到实战教程，深度透析 redis 底层原理 + redis 分布式锁 + 企业解决方案 + 黑马点评实战项目](https://www.bilibili.com/video/BV1cr4y1671t/?spm_id_from=333.337.search-card.all.click)
    
*   [Redis 基础篇](https://blog.csdn.net/qq_66345100/article/details/130566275?spm=1001.2014.3001.5501)
    
*   [Lua 快速入门笔记](https://blog.csdn.net/qq_66345100/article/details/131617253?spm=1001.2014.3001.5501)
    
*   [自动化完成 1000 个用户的登录并获取 token 并生成 tokens.txt 文件](https://blog.csdn.net/qq_66345100/article/details/128913949)
    
*   [hm-dianping: Redis 练手项目：黑马点评 (gitee.com)](https://gitee.com/aghp/dianping)：本仓库共计打了 26 个 tag，你可以随时查看某一时段的版本，如果觉得对你有帮助，欢迎 start
    

最后，在此也感谢黑马、感谢[鱼皮](https://so.csdn.net/so/search?q=%E9%B1%BC%E7%9A%AE&spm=1001.2101.3001.7020)、感谢 B 站发弹幕提醒我的人、感谢互联网上那些写下相关文章的人，是你们让我遇到了这么课程，是你们让我能够解决中途遇到的 bug，是你们让我最终成功完成这篇文章，文章共计 **15 万字符**，如果觉得本文对你有所帮助，欢迎三连（点赞👍+ 收藏⭐+ 关注💖），如果文中存在错误或者描述不当的地方，恳请告知博主，博主将不胜感激，如果本文存在侵权，也恳请及时告知，博主将立刻修改

### 项目搭建

#### 导入数据库

![](<assets/1740497725130.png>)

![](<assets/1740497725452.png>)

**备注**：SQL 文件在项目的 Resource 目录下

#### 初始化项目

导入项目

配置 IDEA 环境

加载 Maven 依赖

更改配置

*   **Step1**：导入项目
    
    略……
    
*   **Step2**：配置好 IDEA 环境（**这一步很重要**）
    
    这一步主要是检查 JDK、Maven、编码三个方面
    
    1）JDK 保障是 1.8
    
    ![](<assets/1740497725767.png>)
    
    ![](<assets/1740497726029.png>)
    
    2）项目编码保障是 UTF-8
    
    ![](<assets/1740497726294.png>)
    
    3）Maven 版本不低于 3.4
    
    ![](<assets/1740497726758.png>)
    
    4）项目路径不要出现中文
    
    ![](<assets/1740497727026.png>)
    
*   **Step3**：加载 Maven 依赖，直接刷新 Maven
    
    **温馨提示**：建议配置阿里云镜像，不会的可以参考这篇文章 [一文带你快速上手项目开发神器 Maven_知识汲取者的博客 - CSDN 博客](https://blog.csdn.net/qq_66345100/article/details/126537091)
    
    ![](<assets/1740497727233.png>)
    
*   **Step4**：更改配置
    
    将 application.yml 文件中的账号密码换成自己的，比如 MySQL 的账号密码，Redis 的地址和密码。
    
    关于 Redis 的基础使用可以参考我都这篇文章：[Redis 基础篇_知识汲取者的博客 - CSDN 博客](https://blog.csdn.net/qq_66345100/article/details/130566275?spm=1001.2014.3001.5501)
    

#### 启动项目

##### 启动前端项目

*   **启动前端项目**
    
    解压前端项目，然后进入 Dos 命令窗口
    
    ![](<assets/1740497727570.png>)
    
    输入`start nginx.exe`启动 Nginx
    
    ![](<assets/1740497727756.png>)
    
    Nginx 是后台启动，我们直接打开浏览器访问`http://localhost:8080/`，如果看到以下页面则代表前端项目启动成功
    
    ![](<assets/1740497727995.png>)
    

##### 启动后端项目

*   **启动后端项目**
    
    方式一：
    
    ![](<assets/1740497728156.png>)
    
    方式二：
    
    ![](<assets/1740497728404.png>)
    
    然后访问打开浏览器访问`http://localhost:8081/shop/1`，如果能够看到以下页面就说明后端项目启动成功了
    
    ![](<assets/1740497728724.png>)
    
    注意：首先要保障数据库中的 shop 表中存在 id 为 1 的这条数据
    

### 基于 Session 实现短信验证码登录

#### 短信验证码登录

![](<assets/1740497729048.png>)

```
    
    @Override
    public Result sendCode(String phone, HttpSession session) {
        
        if (RegexUtils.isPhoneInvalid(phone)) {
            return Result.fail("手机号格式不正确");
        }
        
        String code = RandomUtil.randomNumbers(6);
        session.setAttribute(SystemConstants.VERIFY_CODE, code);
        
        log.info("验证码:{}", code);
        return Result.ok();
    }

    
    @Override
    public Result login(LoginFormDTO loginForm, HttpSession session) {
        String phone = loginForm.getPhone();
        String code = loginForm.getCode();
        
        if (RegexUtils.isPhoneInvalid(phone)) {
            return Result.fail("手机号格式不正确");
        }
        
        String sessionCode = (String) session.getAttribute(LOGIN_CODE);
        if (code == null || !code.equals(sessionCode)) {
            return Result.fail("验证码不正确");
        }
        
        User user = this.getOne(new LambdaQueryWrapper<User>()
                .eq(User::getPassword, phone));
        if (Objects.isNull(user)) {
            
            user = createUserWithPhone(phone);
        }
        
        session.setAttribute(LOGIN_USER, user);
        return Result.ok();
    }

    
    private User createUserWithPhone(String phone) {
        User user = new User();
        user.setPhone(phone);
        user.setNickName(SystemConstants.USER_NICK_NAME_PREFIX + RandomUtil.randomString(10));
        this.save(user);
        return user;
    }

```

#### 配置登录拦截器

![](<assets/1740497729287.png>)

登录拦截器：

```
public class LoginInterceptor implements HandlerInterceptor {
    
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        HttpSession session = request.getSession();
        
        User user = (User) session.getAttribute(LOGIN_USER);
        if (Objects.isNull(user)){
            
            response.setStatus(HttpStatus.HTTP_UNAUTHORIZED);
            return false;
        }
        
        
        ThreadLocalUtls.saveUser(user);

        return HandlerInterceptor.super.preHandle(request, response, handler);
    }
}

```

配置完拦截器后，还需要将我们自定义的拦截器添加到 SpringMVC 的拦截器列表中，才能生效：

```
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        
        registry.addInterceptor(new LoginInterceptor())
                
                .excludePathPatterns(
                        "/user/code",
                        "/user/login",
                        "/blog/hot",
                        "/shop/**",
                        "/shop-type/**",
                        "/upload/**",
                        "/voucher/**"
                );
    }
}


```

#### 数据脱敏

1.  封装 UserDTO，返回给前端的`Entity`数据使用`BeanUtil`工具类转成 DTO
2.  存储到 ThreadLocal 中的数据也进行数据托名

```
@Data
public class UserDTO {
    private Long id;
    private String nickName;
    private String icon;
}

```

### Session 集群共享问题

*   **什么是 Session 集群共享问题**？
    
    在分布式集群环境中，会话（Session）共享是一个常见的挑战。默认情况下，Web 应用程序的会话是保存在单个服务器上的，当请求不经过该服务器时，会话信息无法被访问。
    
*   **Session 集群共享问题造成哪些问题**？
    
    *   服务器之间无法实现会话状态的共享。比如：在当前这个服务器上用户已经完成了登录，Session 中存储了用户的信息，能够判断用户已登录，但是在另一个服务器的 Session 中没有用户信息，无法调用显示没有登录的服务器上的服务
*   **如何解决 Session 集群共享问题**？
    
    *   **方案一**：**Session 拷贝**（不推荐）
        
        Tomcat 提供了 Session 拷贝功能，通过配置 Tomcat 可以实现 Session 的拷贝，但是这会增加服务器的额外内存开销，同时会带来数据一致性问题
        
    *   **方案二**：**Redis 缓存**（推荐）
        
        Redis 缓存具有 Session 存储一样的特点，基于内存、存储结构可以是 key-value 结构、数据共享
        
*   **Redis 缓存相较于传统 Session 存储的优点**：
    
    *   **高性能和可伸缩性**：Redis 是一个内存数据库，具有快速的读写能力。相比于传统的 Session 存储方式，将会话数据存储在 Redis 中可以大大提高读写速度和处理能力。此外，Redis 还支持集群和分片技术，可以实现水平扩展，处理大规模的并发请求。
    *   **可靠性和持久性**：Redis 提供了持久化机制，可以将内存中的数据定期或异步地写入磁盘，以保证数据的持久性。这样即使发生服务器崩溃或重启，会话数据也可以被恢复。
    *   **丰富的数据结构**：Redis 不仅仅是一个键值存储数据库，它还支持多种数据结构，如字符串、列表、哈希、集合和有序集合等。这些数据结构的灵活性使得可以更方便地存储和操作复杂的会话数据。
    *   **分布式缓存功能**：Redis 作为一个高效的缓存解决方案，可以用于缓存会话数据，减轻后端服务器的负载。与传统的 Session 存储方式相比，使用 Redis 缓存会话数据可以大幅提高系统的性能和可扩展性。
    *   **可用性和可部署性**：Redis 是一个强大而成熟的开源工具，有丰富的社区支持和活跃的开发者社区。它可以轻松地与各种编程语言和框架集成，并且可以在多个操作系统上运行。
    
    PS：但是 Redis 费钱，而且增加了系统的复杂度
    

### 基于 Redis 实现短信验证码登录

![](<assets/1740497729593.png>)

从前面的分析来看，显然 Redis 是要优于 Session 的，但是 Redis 中有很多数据结构，我们应该选择哪种数据结构来存储用户信息才能够更优呢？可能大多数同学都会想到使用 String 类型的数据据结构，但是这里我推荐使用 Hash 结构！

*   **Hash 结构与 String 结构类型的比较**：
    *   String 数据结构是以 JSON 字符串的形式保存，更加直观，操作也更加简单，但是 JSON 结构会有很多非必须的内存开销，比如双引号、大括号，内存占用比 Hash 更高
    *   Hash 数据结构是以 Hash 表的形式保存，可以对单个字段进行 CRUD，更加灵活
*   **Redis 替代 Session 需要考虑的问题**：
    *   选择合适的数据结构，了解 Hash 比 String 的区别
    *   选择合适的 key，为 key 设置一个业务前缀，方便区分和分组，为 key 拼接一个 UUID，避免 key 冲突防止数据覆盖
    *   选择合适的存储粒度，对于验证码这类数据，一般设置 TTL 为 3min 即可，防止大量缓存数据的堆积，而对于用户信息这类数据可以稍微设置长一点，比如 30min，防止频繁对 Redis 进行 IO 操作

#### 短信验证登录

```
    
    @Override
    public Result sendCode(String phone, HttpSession session) {
        
        if (RegexUtils.isPhoneInvalid(phone)) {
            return Result.fail("手机号格式不正确");
        }
        
        String code = RandomUtil.randomNumbers(6);
        stringRedisTemplate.opsForValue().set(LOGIN_CODE_KEY + phone, code,
                RedisConstants.LOGIN_CODE_TTL, TimeUnit.MINUTES);
        
        log.info("验证码:{}", code);
        return Result.ok();
    }

    
    @Override
    public Result login(LoginFormDTO loginForm, HttpSession session) {
        String phone = loginForm.getPhone();
        String code = loginForm.getCode();
        
        if (RegexUtils.isPhoneInvalid(phone)) {
            return Result.fail("手机号格式不正确");
        }
        
        String redisCode = stringRedisTemplate.opsForValue().get(LOGIN_CODE_KEY + phone);
        if (code == null || !code.equals(redisCode)) {
            return Result.fail("验证码不正确");
        }
        
        User user = this.getOne(new LambdaQueryWrapper<User>()
                .eq(User::getPhone, phone));
        if (Objects.isNull(user)) {
            
            user = createUserWithPhone(phone);
        }
        
        UserDTO userDTO = BeanUtil.copyProperties(user, UserDTO.class);
        
        Map<String, Object> userMap = BeanUtil.beanToMap(userDTO, new HashMap<>(),
                CopyOptions.create().setIgnoreNullValue(true).
                        setFieldValueEditor((fieldName, fieldValue) -> fieldValue.toString()));
        String token = UUID.randomUUID().toString(true);
        String tokenKey = LOGIN_USER_KEY + token;
        stringRedisTemplate.opsForHash().putAll(tokenKey, userMap);
        stringRedisTemplate.expire(tokenKey, LOGIN_USER_TTL, TimeUnit.MINUTES);

        return Result.ok(token);
    }

    
    private User createUserWithPhone(String phone) {
        User user = new User();
        user.setPhone(phone);
        user.setNickName(SystemConstants.USER_NICK_NAME_PREFIX + RandomUtil.randomString(10));
        this.save(user);
        return user;
    }

```

#### 配置登录拦截器

![](<assets/1740497729780.png>)

**单独配置一个拦截器用户刷新 Redis 中的 token**：在基于 Session 实现短信验证码登录时，我们只配置了一个拦截器，这里需要另外再配置一个拦截器专门用户刷新存入 Redis 中的 token，因为我们现在改用 Redis 了，为了防止用户在操作网站时突然由于 Redis 中的 token 过期，导致直接退出网站，严重影响用户体验。那为什么不把刷新的操作放到一个拦截器中呢，因为之前的那个拦截器只是用来拦截一些需要进行登录校验的请求，对于哪些不需要登录校验的请求是不会走拦截器的，刷新操作显然是要针对所有请求比较合理，所以单独创建一个拦截器拦截一切请求，刷新 Redis 中的 Key

登录拦截器：

```
public class LoginInterceptor implements HandlerInterceptor {

    
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        
        if (ThreadLocalUtls.getUser() == null){
            
            response.setStatus(HttpStatus.HTTP_UNAUTHORIZED);
            return false;
        }
        
        return true;
    }
}

```

刷新 token 的拦截器：

```
public class RefreshTokenInterceptor implements HandlerInterceptor {

    
    
    private StringRedisTemplate stringRedisTemplate;

    public RefreshTokenInterceptor(StringRedisTemplate stringRedisTemplate) {
        this.stringRedisTemplate = stringRedisTemplate;
    }

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        
        String token = request.getHeader("authorization");
        if (StrUtil.isBlank(token)){
            
            return true;
        }
        
        String tokenKey = LOGIN_USER_KEY + token;
        Map<Object, Object> userMap = stringRedisTemplate.opsForHash().entries(tokenKey);
        if (userMap.isEmpty()){
            
            return true;
        }
        
        UserDTO userDTO = BeanUtil.fillBeanWithMap(userMap, new UserDTO(), false);
        ThreadLocalUtls.saveUser(BeanUtil.copyProperties(userMap, UserDTO.class));
        
        stringRedisTemplate.expire(token, LOGIN_USER_TTL, TimeUnit.MINUTES);
        return true;
    }
}

```

将自定义的拦截器添加到 SpringMVC 的拦截器表中，使其生效：

```
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

    
    
    @Resource
    private StringRedisTemplate stringRedisTemplate;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        
        registry.addInterceptor(new LoginInterceptor())
                
                .excludePathPatterns(
                        "/user/code",
                        "/user/login",
                        "/blog/hot",
                        "/shop/**",
                        "/shop-type/**",
                        "/upload/**",
                        "/voucher/**"
                ).order(1); 
        
        registry.addInterceptor(new RefreshTokenInterceptor(stringRedisTemplate)).addPathPatterns("/**").order(0);
    }
}

```

### 店铺数据查询

*   **什么是缓存**？
    
    **缓存**就是数据交换的缓冲区（称作 Cache [kæʃ] ），是存贮数据的临时地方，一般读写性能较高。
    

![](<assets/1740497730154.png>)

#### 根据 id 查询商铺缓存

![](<assets/1740497730473.png>)

```
    
    @Override
    public Result queryById(Long id) {
        String key = CACHE_SHOP_KEY + id;
        
        String shopJson = stringRedisTemplate.opsForValue().get(key);

        Shop shop = null;
        
        if (StrUtil.isNotBlank(shopJson)) {
            
            shop = JSONUtil.toBean(shopJson, Shop.class);
            return Result.ok(shop);
        }
        
        shop = this.getById(id);

        
        if (Objects.isNull(shop)) {
            
            return Result.fail("店铺不存在");
        }
        
        stringRedisTemplate.opsForValue().set(key, JSONUtil.toJsonStr(shop));
        return Result.ok(shop);
    }

```

对于店铺的详细数据，这种数据变化比较大，店家可能会随时修改店铺的相关信息（比如宣传语，店铺名等），所以对于这类变动较为频繁的数据，我们是直接存入 Redis 中，并且设置合适的有效期（后面还会进行优化，确保 Redis 和 MySQL 的**数据一致性**，以及解决**缓存常见的三大问题**）

![](<assets/1740497730742.png>)

#### 查询店铺类型

对于店铺类型数据，一般变动会比较小，所以这里我们直接将店铺类型的数据持久化存储到 Redis 中

![](<assets/1740497730976.png>)

```
    
    @Override
    public Result queryTypeList() {
        
        String key = CACHE_SHOP_TYPE_KEY + UUID.randomUUID().toString(true);
        String shopTypeJSON = stringRedisTemplate.opsForValue().get(key);

        List<ShopType> typeList = null;
        
        if (StrUtil.isNotBlank(shopTypeJSON)) {
            
            typeList = JSONUtil.toList(shopTypeJSON, ShopType.class);
            return Result.ok(typeList);
        }
        
        typeList = this.list(new LambdaQueryWrapper<ShopType>()
                .orderByAsc(ShopType::getSort));

        
        if (Objects.isNull(typeList)) {
            
            return Result.fail("店铺类型不存在");
        }
        
        stringRedisTemplate.opsForValue().set(key, JSONUtil.toJsonStr(typeList),
                CACHE_SHOP_TYPE_TTL, TimeUnit.MINUTES);
        return Result.ok(typeList);
    }

```

#### 数据一致性问题

缓存的使用**降低了后端负载**，**提高了读写的效率**，**降低了响应的时间**，这么看来缓存是不是就一本万利呢？答案是否定的！并不是说缓存有这么多优点项目中就可以无脑使用缓存了，我们还需要考虑缓存带来的问题，比如：缓存的添加**提高了系统的维护成本**，同时也**带来了数据一致性问题**…… 总的来讲，系统使用引入缓存需要经过前期的测试、预算，判断引入缓存后带来的价值是否会超过引入缓存带来的代价

那么我们该如何解决数据一致性问题呢？首先我们需要明确数据一致性问题的主要原因是什么，从主要原因入手才是解决问题的关键！数据一致性的根本原因是 **缓存和数据库中的数据不同步**，那么我们该如何让 缓存 和 数据库 中的数据尽可能的即时同步？这就需要选择一个比较好的**缓存更新策略**了

*   **常见的缓存更新策略**：
    
    ![](<assets/1740497731238.png>)
    
    *   **内存淘汰**（全自动）。利用 **Redis 的内存淘汰机制**实现缓存更新，Redis 的内存淘汰机制是当 Redis 发现内存不足时，会根据一定的策略自动淘汰部分数据
        
        Redis 中常见的淘汰策略：
        
        *   **noeviction**（**默认**）：当达到内存限制并且客户端尝试执行写入操作时，Redis 会返回错误信息，拒绝新数据的写入，保证数据完整性和一致性
        *   **allkeys-lru**：从所有的键中选择最近最少使用（Least Recently Used，LRU）的数据进行淘汰。即优先淘汰最长时间未被访问的数据
        *   **allkeys-random**：从所有的键中随机选择数据进行淘汰
        *   **volatile-lru**：从设置了过期时间的键中选择最近最少使用的数据进行淘汰
        *   **volatile-random**：从设置了过期时间的键中随机选择数据进行淘汰
        *   **volatile-ttl**：从设置了过期时间的键中选择剩余生存时间（Time To Live，TTL）最短的数据进行淘汰
    *   **超时剔除**（半自动）。手动给缓存数据添加 TTL，到期后 Redis 自动删除缓存
        
    *   **主动更新**（手动）。手动编码实现缓存更新，在修改数据库的同时更新缓存
        
        *   **双写方案**（Cache Aside Pattern）：人工编码方式，缓存调用者在更新完数据库后再去更新缓存。使用困难，灵活度高。
            
            1）**读取**（Read）：当需要读取数据时，首先检查缓存是否存在该数据。如果缓存中存在，直接返回缓存中的数据。如果缓存中不存在，则从底层数据存储（如数据库）中获取数据，并将数据存储到缓存中，以便以后的读取操作可以更快地访问该数据。
            
            2）**写入**（Write）：当进行数据写入操作时，首先更新底层数据存储中的数据。然后，根据具体情况，可以选择直接更新缓存中的数据（使缓存与底层数据存储保持同步），或者是简单地将缓存中与修改数据相关的条目标记为无效状态（缓存失效），以便下一次读取时重新加载最新数据
            
            使用双写方案需要考虑以下几个问题：
            
            是使用**更新缓存模式**还是使用**删除缓存模式**？
            
            *   **更新缓存模式**：每次更新数据库都更新缓存，无效写操作较多（不推荐使用）
                
                假如我们执行上百次更新数据库操作，那么就要执行上百次写入缓存的操作，而在这期间并没有查询请求，那么这上百次写入缓存的操作就显得没有什么意义
                
            *   **删除缓存模式**：更新数据时更新数据库并删除缓存，查询时更新缓存，无效写操作较少（**推荐使用**）
                
                选择使用删除缓存模式，那么是**先操作缓存**还是**先操作数据库**？
                
                *   **先操作缓存**：先删缓存，再更新数据库（不推荐使用，详细原因看 P38）
                    
                    当线程 1 删除缓存到更新数据库之间的时间段，会有其它线程进来查询数据，由于没有加锁，且前面的线程将缓存删除了，这就导致请求会直接打到数据库上，给数据库带来巨大压力。这个事件发生的概率很大，因为缓存的读写速度块，而数据库的读写较慢。
                    
                    这种方式的不足之处：存在缓存击穿问题，且概率较大
                    
                *   **先操作数据库**：先更新数据库，再删缓存（**推荐使用**，详细原因看 P38）
                    
                    当线程 1 在查询缓存且未命中，此时线程 1 查询数据，查询完准备写入缓存时，由于没有加锁线程 2 乘虚而入，线程 2 在这期间对数据库进行了更新，此时线程 1 将旧数据返回了，出现了脏读，这个事件发生的概率很低，因为先是需要满足缓存未命中，且在写入缓存的那段事件内有一个线程进行更新操作，缓存的查询很快，这段空隙时间很小，所以出现脏读现象的概率也很低
                    
                    这种方式的不足之处：存在脏读现象，但概率较小
                    
                    选择先更新数据库，再删除缓存。那么**如何保证缓存与数据库的操作的原子性**（同时成功或失败）？
                    
                    *   对于单体系统，将缓存与数据库操作放在同一个事务中（当前项目就是一个单体项目，所以选择这种方式）
                    *   对于分布式系统，利用 TCC（Try-Confirm-Cancel）等分布式事务方案
        *   **读写穿透方案**（Read/Write Through Pattern）：将读取和写入操作首先在缓存中执行，然后再传播到数据存储
            
            1）**读取穿透**（Read Through）：当进行读取请求时，首先检查缓存。如果所请求的数据在缓存中找到，直接返回数据。如果缓存中没有找到数据，则将请求转发给数据存储以获取数据。获取到的数据随后存储在缓存中，然后返回给调用者。
            
            2）**写入穿透**（Write Through）：当进行写入请求时，首先将数据写入缓存。缓存立即将写操作传播到数据存储，确保缓存和数据存储之间的数据保持一致。这样保证了后续的读取请求从缓存中返回更新后的数据。
            
        *   **写回方案**（Write Behind Caching Pattern）：调用者只操作缓存，其他线程去异步处理数据库，实现最终一致
            
            1）**读取**（Read）：先检查缓存中是否存在数据，如果不存在，则从底层数据存储中获取数据，并将数据存储到缓存中。
            
            2）**写入**（Write）：先更新底层数据存储，然后将待写入的数据放入一个缓存队列中。在适当的时机，通过批量操作或异步处理，将缓存队列中的数据写入底层数据存储
            
*   **主动更新策略中三种方案的比较**：
    
    *   **双写方案** 和 **读写穿透方案** 在写入数据时都会直接更新缓存，以保持缓存和底层数据存储的一致性。而 **写回方案** 延迟了缓存的更新操作，将数据先放入缓存队列，然后再进行批量或异步写入。
    *   **读写穿透方案** 和 **写回方案** 相比，**写回方案** 具有更高的写入性能，因为它通过批量和异步操作减少了频繁的写入操作。但是 **写回方案** 带来了数据一致性的考虑，需要确保缓存和底层数据存储在某个时间点上保持一致，而 **读写穿透方案** 将数据库和缓存整合为一个服务，由服务来维护缓存与数据库的一致性，调用者无需关心数据一致性问题，降低了系统的可维护性，但是实现困难
*   **主动更新策略中三种方案的应用场景**：
    
    *   **双写方案** 较适用于读多写少的场景，数据的一致性由应用程序主动管理
    *   **读写穿透方案** 适用于数据实时性要求较高、对一致性要求严格的场景
    *   **写回方案** 适用于追求写入性能的场景，对数据的实时性要求相对较低、可靠性也相对低
*   **更新策略的应用场景**：
    
    *   对于低一致性需求，可以使用内存淘汰机制。例如店铺类型数据的查询缓存
    *   对于高一致性需求，可以采用主动更新策略，并以超时剔除作为兜底方案。例如店铺详情数据查询的缓存

#### 缓存主动更新策略的实现

上一节，我们了解了数据一致性问题，并了解了如何解决数据一致性问题的几种常见策略，最终经过我们的讨论得出采用**缓存主动更新**来解决数据一致性问题，是相较于其它两种方案更好的选择，同时也选择使用**双写方案**的**删除缓存模式**来减少线程安全问题发生的概率，采用 TTL 过期 + 内存淘汰机制作为兜底方案，同时将缓存和数据库的操作放到同一个**事务**来保障操作的原子性，现在就让我们来通过下面的案例进行实现

![](<assets/1740497731522.png>)

在启动类添加`@EnableTransactionManagement`注解开启事务，

然后使用缓存主动更新策略（采用删除缓存模式，并且先操作数据库再操作缓存，同时添加事务保证数据库操作和缓存操作的原子性）解决数据一致性问题：

```
    
    @Override
    public Result queryById(Long id) {
        String key = CACHE_SHOP_KEY + id;
        
        String shopJson = stringRedisTemplate.opsForValue().get(key);

        Shop shop = null;
        
        if (StrUtil.isNotBlank(shopJson)) {
            
            shop = JSONUtil.toBean(shopJson, Shop.class);
            return Result.ok(shop);
        }
        
        shop = this.getById(id);

        
        if (Objects.isNull(shop)) {
            
            return Result.fail("店铺不存在");
        }
        
        stringRedisTemplate.opsForValue().set(key, JSONUtil.toJsonStr(shop), CACHE_SHOP_TTL, TimeUnit.MINUTES);
        return Result.ok(shop);
    }

    
    @Transactional
    @Override
    public Result updateShop(Shop shop) {
        

        
        boolean f = this.updateById(shop);
        if (!f){
            
            throw new RuntimeException("数据库更新失败");
        }
        
        f = stringRedisTemplate.delete(CACHE_SHOP_KEY + shop.getId());
        if (!f){
            
            throw new RuntimeException("缓存删除失败");
        }
        return Result.ok();
    }

```

#### 缓存穿透的解决方案

**缓存穿透**是指客户端请求的数据在缓存中和数据库中都不存在，这样缓存永远不会生效，这些请求都会打到数据库。

PS：本篇文章主要是记录实现代码，关于缓存穿透详情见这篇文章：Redis 缓存常见问题以及解决方案

![](<assets/1740497731771.png>)

*   **常见解决缓存穿透的解决方案**：
    
    *   **缓存空对象**
        
        *   优点：实现简单，维护方便
            
        *   缺点：额外的内存消耗，可能造成短期的不一致
            
    *   **布隆过滤**
        
        *   优点：内存占用较少，没有多余 key
        *   缺点：实现复杂，存在误判可能（有穿透的风险），无法删除数据
    
    上面两种方式都是被动的解决缓存穿透方案，此外我们还可以采用主动的方案预防缓存穿透，比如：**增强 id 的复杂度避免被猜测 id 规律**、**做好数据的基础格式校验**、**加强用户权限校验**、**做好热点参数的限流**
    

![](<assets/1740497732128.png>)

这里使用方案一（缓存空对象）解决缓存穿透问题：

![](<assets/1740497732364.png>)

```
    
    @Override
    public Result queryById(Long id) {
        String key = CACHE_SHOP_KEY + id;
        
        String shopJson = stringRedisTemplate.opsForValue().get(key);

        Shop shop = null;
        
        if (StrUtil.isNotBlank(shopJson)) {
            
            shop = JSONUtil.toBean(shopJson, Shop.class);
            return Result.ok(shop);
        }

        
        if (Objects.nonNull(shopJson)){
            
            return Result.fail("店铺不存在");
        }
        
        shop = this.getById(id);

        
        if (Objects.isNull(shop)) {
            
            stringRedisTemplate.opsForValue().set(key, "", CACHE_NULL_TTL, TimeUnit.SECONDS);
            return Result.fail("店铺不存在");
        }
        
        stringRedisTemplate.opsForValue().set(key, JSONUtil.toJsonStr(shop), 
                                              CACHE_SHOP_TTL, TimeUnit.MINUTES);
        return Result.ok(shop);
    }

```

第一次查询数据库中和缓存中不存在的数据，请求经过了数据库，但是缓存了空字符串：

![](<assets/1740497732674.png>)

第二次查询（短期内），发先请求未经过数据库，通过 Debug 的方式可以发现请求直接再 **2.2.1** 处返回了

![](<assets/1740497733129.png>)

#### 缓存雪崩的解决方案

**缓存雪崩**是指在同一时段大量的缓存 key 同时失效或者 Redis 服务宕机，导致大量请求到达数据库，带来巨大压力。

PS：本篇文章主要是记录实现代码，关于缓存雪崩详情见这篇文章：Redis 缓存常见问题以及解决方案

![](<assets/1740497733517.png>)

*   **缓存雪崩的常见解决方案**：
    *   **给不同的 Key 的 TTL 添加随机值**
    *   **利用 Redis 集群提高服务的可用性**
    *   **给缓存业务添加降级限流策略**，比如快速失败机制，让请求尽可能打不到数据库上
    *   **给业务添加多级缓存**

#### 缓存击穿的解决方案

**缓存击穿问题**也叫热点 Key 问题，就是一个被**高并发访问**并且**缓存重建业务较复杂**的 key 突然失效了，无数的请求访问会在瞬间给数据库带来巨大的冲击。

PS：本篇文章主要是记录实现代码，关于缓存击穿详情见这篇文章：Redis 缓存常见问题以及解决方案

![](<assets/1740497733817.png>)

*   **缓存击穿的常见解决方案**：
    
    *   **互斥锁**（时间换空间）
        *   优点：内存占用小，一致性高，实现简单
        *   缺点：性能较低，容易出现死锁
    *   **逻辑过期**（空间换时间）
        *   优点：性能高
        *   缺点：内存占用较大，容易出现脏读
    
    两者相比较，互斥锁更加易于实现，但是容易发生死锁，且锁导致并行变成串行，导致系统性能下降，逻辑过期实现起来相较复杂，且需要耗费额外的内存，但是通过开启子线程重建缓存，使原来的同步阻塞变成异步，提高系统的响应速度，但是容易出现脏读
    

![](<assets/1740497734115.png>)

##### 基于互斥锁解决缓存击穿

![](<assets/1740497734396.png>)

```
    
    @Override
    public Result queryById(Long id) {
        String key = CACHE_SHOP_KEY + id;
        
        Result result = getShopFromCache(key);
        if (Objects.nonNull(result)) {
            
            return result;
        }
        try {
            
            String lockKey = LOCK_SHOP_KEY + id;
            boolean isLock = tryLock(lockKey);
            if (!isLock) {
                
                Thread.sleep(50);
                return queryById(id);
            }
            
            result = getShopFromCache(key);
            if (Objects.nonNull(result)) {
                
                return result;
            }

            
            Shop shop = this.getById(id);
            if (Objects.isNull(shop)) {
                
                stringRedisTemplate.opsForValue().set(key, "", CACHE_NULL_TTL, TimeUnit.SECONDS);
                return Result.fail("店铺不存在");
            }

            
            stringRedisTemplate.opsForValue().set(key, JSONUtil.toJsonStr(shop),
                    CACHE_SHOP_TTL, TimeUnit.MINUTES);
            return Result.ok(shop);
        }catch (Exception e){
            throw new RuntimeException("发生异常");
        } finally {
            
            unlock(key);
        }
    }

    
    private Result getShopFromCache(String key) {
        String shopJson = stringRedisTemplate.opsForValue().get(key);
        
        if (StrUtil.isNotBlank(shopJson)) {
            
            Shop shop = JSONUtil.toBean(shopJson, Shop.class);
            return Result.ok(shop);
        }
        
        if (Objects.nonNull(shopJson)) {
            
            return Result.fail("店铺不存在");
        }
        
        return null;
    }


    
    private boolean tryLock(String key) {
        Boolean flag = stringRedisTemplate.opsForValue().setIfAbsent(key, "1", 10, TimeUnit.SECONDS);
        
        return BooleanUtil.isTrue(flag);
    }

    
    private void unlock(String key) {
        stringRedisTemplate.delete(key);
    }

```

**备注**：

1.  这里使用 Redis 中的`setnx`指令实现互斥锁，只有当值不存在时才能进行`set`操作
    
2.  锁的有效期更具体业务有关，需要灵活变动，一般锁的有效期是业务处理时长 10~20 倍
    
3.  线程获取锁后，还需要查询缓存（也就是所谓的双检），这样才能够真正有效保障缓存不被击穿
    

**测试**

通过 Jmeter 进行压测，在 5 秒内发送 2000 个请求（**qps** 高达 400，也就是每秒钟发送 400 个请求），最终**吞吐量**是 331.3（也就是每秒钟处理 134.8 个请求），系统性能还是比较好的

![](<assets/1740497734874.png>)

##### 基于逻辑过期解决缓存击穿

所谓的逻辑过期，类似于逻辑删除，并不是真正意义上的过期，而是新增一个字段，用来标记 key 的过期时间，这样能能够避免 key 过期而被自动删除，这样数据就永不过期了，从根本上解决因为热点 key 过期导致的缓存击穿。一般搞活动时，比如抢优惠券，秒杀等场景，请求量比较大就可以使用逻辑过期，等活动一过就手动删除逻辑过期的数据

![](<assets/1740497735305.png>)

创建一个逻辑过期数据类：

```
@Data
public class RedisData {
    
    private LocalDateTime expireTime;
    
    private Object data;
}

```

ShopService 钟的代码：

```
    
    public static final ExecutorService CACHE_REBUILD_EXECUTOR = Executors.newFixedThreadPool(10);

    
    @Override
    public Result queryById(Long id) {
        String key = CACHE_SHOP_KEY + id;
        
        String shopJson = stringRedisTemplate.opsForValue().get(key);
        if (StrUtil.isBlank(shopJson)) {
            
            return Result.fail("店铺数据不存在");
        }
        
        RedisData redisData = JSONUtil.toBean(shopJson, RedisData.class);
        
        JSONObject data = (JSONObject) redisData.getData();
        Shop shop = JSONUtil.toBean(data, Shop.class);
        LocalDateTime expireTime = redisData.getExpireTime();
        if (expireTime.isAfter(LocalDateTime.now())) {
            
            return Result.ok(shop);
        }

        
        String lockKey = LOCK_SHOP_KEY + id;
        boolean isLock = tryLock(lockKey);
        if (isLock) {
            
            CACHE_REBUILD_EXECUTOR.submit(() -> {
                try {
                    this.saveShopToCache(id, CACHE_SHOP_LOGICAL_TTL);
                } finally {
                    unlock(lockKey);
                }
            });
        }

        
        shopJson = stringRedisTemplate.opsForValue().get(key);
        if (StrUtil.isBlank(shopJson)) {
            
            return Result.fail("店铺数据不存在");
        }
        
        redisData = JSONUtil.toBean(shopJson, RedisData.class);
        
        data = (JSONObject) redisData.getData();
        shop = JSONUtil.toBean(data, Shop.class);
        expireTime = redisData.getExpireTime();
        if (expireTime.isAfter(LocalDateTime.now())) {
            
            return Result.ok(shop);
        }

        
        return Result.ok(shop);
    }

    
    public void saveShopToCache(Long id, Long expireSeconds) {
        
        Shop shop = this.getById(id);
        
        RedisData redisData = new RedisData();
        redisData.setData(shop);
        redisData.setExpireTime(LocalDateTime.now().plusSeconds(expireSeconds));
        
        stringRedisTemplate.opsForValue().set(CACHE_SHOP_KEY + id, JSONUtil.toJsonStr(redisData));
    }

    
    private boolean tryLock(String key) {
        Boolean flag = stringRedisTemplate.opsForValue().setIfAbsent(key, "1", 10, TimeUnit.SECONDS);
        
        return BooleanUtil.isTrue(flag);
    }

    
    private void unlock(String key) {
        stringRedisTemplate.delete(key);
    }

```

注意：逻辑过期一定要先进行数据预热，将我们热点数据加载到缓存中

数据预热可以编写一个`CommandLineRunner`继承类，然后 SpringBoot 程序启动时就执行启动的`run`方法，实现数据预热，或者像下面一样编写一个测试累，实现数据预热

```
    
    @Test
    public void testSaveShopToCache(){
        shopService.saveShopToCache(1L, RedisConstants.CACHE_SHOP_LOGICAL_TTL);
    }

```

**备注**：

1.  逻辑过期时间根据具体业务而定，逻辑过期过长，会造成缓存数据的堆积，浪费内存，过短造成频繁缓存重建，降低性能，所以设置逻辑过期时间时需要实际测试和评估不同参数下的性能和资源消耗情况，可以通过观察系统的表现，在业务需求和性能要求之间找到一个平衡点

**测试**

先数据预热，由于预热数据的逻辑过期的时间我设置是 10s 钟，所以等待 10s 后，在数据库中修改 id 为 1 的店铺数据，使用 Jmeter 进行压力测试（可以延迟一下重建缓存的子线程，比如延迟个 1s 钟，让效果更加明显），此时我们 11s 去查询，发现还是旧数据，等待 12s 左右再去测试，可以发现缓存数据发生了更新（因为此时缓存已经重建好了），所以这种逻辑过期的缺点很明显，容易出现脏读现象

![](<assets/1740497735557.png>)

可以发现 5s 钟发送 2000 个请求（**qps 是 400**），**吞吐量是 333**，发先逻辑过期并没有我们想象中性能要比互斥锁方案高多少，，这不是测试的问题，而是 Redis 的问题，因为 Redis 的 IO 操作太快了，Redis 重建缓存的几乎耗费的时间占比很少，所以说实验是检验真理的唯一标准，但是对于重建缓存耗时特别大的操作，比如批量重建大量的缓存数据，这个时候我觉得采用逻辑过期的耗时可能会比互斥锁快很多（具体我就没有测了🤣）

#### 缓存工具类的封装

![](<assets/1740497735803.png>)

PS：方法 1 与方法 3 对应，负责常见的普通的缓存，用于解决缓存穿透；方法 2 与方法 4 对应，负责热点缓存，用于解决缓存击穿

使用工具类后，ShopServiceImpl 的代码：

可以看到现在 ShopServiceImpl 中的代码就显得十分清爽干净了😄

```
    
    @Override
    public Result queryById(Long id) {
        





        
        
        Shop shop = cacheClient.handleCacheBreakdown(CACHE_SHOP_KEY, id, Shop.class,
                this::getById, CACHE_SHOP_TTL, TimeUnit.SECONDS);
        if (Objects.isNull(shop)) {
            return Result.fail("店铺不存在");
        }
        
        return Result.ok(shop);
    }

```

工具类：

PS：如果看不太懂的，可以参考前面的代码，比对着来看，我注释都没有改过

```
@Component
@Slf4j
public class CacheClient {

    private final StringRedisTemplate stringRedisTemplate;

    public CacheClient(StringRedisTemplate stringRedisTemplate) {
        this.stringRedisTemplate = stringRedisTemplate;
    }

    
    public void set(String key, Object value, Long timeout, TimeUnit unit) {
        stringRedisTemplate.opsForValue().set(key, JSONUtil.toJsonStr(value), timeout, unit);
    }

    
    public void setWithLogicalExpire(String key, Object value, Long timeout, TimeUnit unit) {
        RedisData redisData = new RedisData();
        redisData.setData(value);
        
        redisData.setExpireTime(LocalDateTime.now().plusSeconds(unit.toSeconds(timeout)));
        stringRedisTemplate.opsForValue().set(key, JSONUtil.toJsonStr(value), timeout, unit);
    }

    
    public <T, ID> T handleCachePenetration(String keyPrefix, ID id, Class<T> type,
                                            Function<ID, T> dbFallback, Long timeout, TimeUnit unit) {
        String key = keyPrefix + id;
        
        String jsonStr = stringRedisTemplate.opsForValue().get(key);

        T t = null;
        
        if (StrUtil.isNotBlank(jsonStr)) {
            
            t = JSONUtil.toBean(jsonStr, type);
            return t;
        }

        
        if (Objects.nonNull(jsonStr)) {
            
            return null;
        }
        
        t = dbFallback.apply(id);

        
        if (Objects.isNull(t)) {
            
            this.set(key, "", CACHE_NULL_TTL, TimeUnit.SECONDS);
            return null;
        }
        
        this.set(key, t, timeout, unit);
        return t;
    }

    
    public static final ExecutorService CACHE_REBUILD_EXECUTOR = Executors.newFixedThreadPool(10);

    
    public <T, ID> T handleCacheBreakdown(String keyPrefix, ID id, Class<T> type,
                                          Function<ID, T> dbFallback, Long timeout, TimeUnit unit) {
        String key = keyPrefix + id;
        
        String jsonStr = stringRedisTemplate.opsForValue().get(key);
        if (StrUtil.isBlank(jsonStr)) {
            
            return null;
        }
        
        RedisData redisData = JSONUtil.toBean(jsonStr, RedisData.class);
        
        JSONObject data = (JSONObject) redisData.getData();
        T t = JSONUtil.toBean(data, type);
        LocalDateTime expireTime = redisData.getExpireTime();
        if (expireTime.isAfter(LocalDateTime.now())) {
            
            return t;
        }

        
        String lockKey = LOCK_SHOP_KEY + id;
        boolean isLock = tryLock(lockKey);
        if (isLock) {
            
            CACHE_REBUILD_EXECUTOR.submit(() -> {
                try {
                    
                    T t1 = dbFallback.apply(id);
                    
                    this.setWithLogicalExpire(key, t1, timeout, unit);
                } finally {
                    unlock(lockKey);
                }
            });
        }

        
        jsonStr = stringRedisTemplate.opsForValue().get(key);
        if (StrUtil.isBlank(jsonStr)) {
            
            return null;
        }
        
        redisData = JSONUtil.toBean(jsonStr, RedisData.class);
        
        data = (JSONObject) redisData.getData();
        t = JSONUtil.toBean(data, type);
        expireTime = redisData.getExpireTime();
        if (expireTime.isAfter(LocalDateTime.now())) {
            
            return t;
        }

        
        return t;

    }

    
    private boolean tryLock(String key) {
        Boolean flag = stringRedisTemplate.opsForValue().setIfAbsent(key, "1", 10, TimeUnit.SECONDS);
        
        return BooleanUtil.isTrue(flag);
    }

    
    private void unlock(String key) {
        stringRedisTemplate.delete(key);
    }
}

```

#### 小结

为了解决数据一致性问题，我们可以选择适当的缓存更新策略：

以**缓存主动更新**（**双写方案** + **删除缓存模式** + **先操作数据库后操作缓存** + **事务**）为主，**超时剔除**为辅

1.  **查询时**，先查询缓存，缓存命中直接返回，缓存未命中查询数据库并重建缓存，返回查询结果
2.  **更新时**，先修改数据删除缓存，使用事务保证缓存和数据操作两者的原子性

除了会遇到数据一致性问题意外，我们还会遇到缓存穿透、缓存雪崩、缓存击穿等问题

1.  对于缓存穿透，我们采用了 ** 缓存空对象 ** 解决
2.  对于缓存击穿，我们分别演示了**互斥锁**（`setnx`实现方式）和**逻辑过期**两种方式解决

最后我们通过抽取出一个工具类，并且利用泛型编写几个通用方法，形成最终的形式

### 优惠券秒杀

#### 自增 ID 存在的问题

当用户抢购时，就会生成订单并保存到`tb_voucher_order`这张表中，而订单表如果使用数据库自增 ID 就存在一些问题：

1.  **id 的规律性太明显**，容易出现信息的泄露，被不怀好意的人伪造请求
    
2.  **受单表数据量的限制**，MySQL 中表能够存储的数据有限，会出现分库分表的情况，id 不能够一直自增
    

当 ID 规律过于明显时，存在以下一些缺点：

1.  **安全性问题**：如果 ID 规律太明显，可能会使系统容易受到恶意攻击，例如暴力破解等。攻击者可以通过分析 ID 规律来推断出其他用户的 ID，从而进行未授权的访问或操纵。
2.  **隐私泄露风险**：如果 ID 规律太明显，可能导致用户的个人信息或敏感数据被曝光。攻击者可以根据规律推测出其他用户的 ID，并通过这些 ID 获取到相应的数据，进而侵犯用户的隐私。
3.  **数据可预测性**：当 ID 规律太明显时，使用这些规律的攻击者可以很轻易地猜测出其他实体（如订单、交易等）的 ID。这可能破坏系统的数据安全性和防伪能力。
4.  **扩展性受限**：如果 ID 规律太明显，可能会对系统的扩展性造成一定影响。当系统需要处理大量并发操作时，如果 ID 规律过于明显，可能导致多个操作同时对同一资源进行竞争，从而增加冲突和性能瓶颈。
5.  **维护困难**：当 ID 规律太明显时，系统可能需要额外的资源和机制来保持规律的更新和变化，以确保安全性和数据完整性。这会增加系统的复杂度，并给维护带来挑战。

在 MySQL 中，表最多可以存储的记录数取决于多个因素，包括数据库版本、操作系统和硬件配置等。下面是一些常见的限制：

1.  行数限制：在 MySQL 5.7 及之前的版本中，InnoDB 和 XtraDB 存储引擎的行数限制为最大约为 **64 亿**（ 2 32 − 1 2^{32}-1 232−1），即 4 , 294 , 967 , 295 4,294,967,295 4,294,967,295 行。而在 MySQL 8.0 及以后的版本中，它们的行数限制可达到理论上的最大值，大约是 **1844 万亿**（ 2 64 − 1 2^{64}-1 264−1）行。
2.  数据库文件大小限制：每个 InnoDB 表的存储大小受到所使用文件系统的限制。对于 InnoDB 表，默认情况下，数据库文件的大小限制取决于操作系统和文件系统，通常在**几 TB** 或更高。但是，这也可能受到特定的操作系统和文件系统的限制。
3.  硬件资源限制：实际上，表的记录数还受到可用硬件资源，如磁盘空间、内存和处理能力的限制。当数据库文件较大时，磁盘空间变得关键，而在执行查询时，内存和处理能力可影响读写性能。

业界流传是 **500 万行**。超过 500 万行就要考虑分表分库了。阿里巴巴《Java 开发手册》提出单表行数超过 **500 万行**或者**单表容量超过 2GB**，就需要考虑分库分表了

那么该如何解决呢？我们需要使用**分布式 ID**（也可以叫**全局唯一 ID**），分布式 ID 满足以下特点：

1.  **全局唯一性**：分布式 ID 保证在整个分布式系统中唯一性，不会出现重复的标识符。这对于区分和追踪系统中的不同实体非常重要。
2.  **高可用性**：分布式 ID 生成器通常被设计为高可用的组件，可以通过水平扩展、冗余备份或集群部署来确保服务的可用性。即使某个节点或组件发生故障，仍然能够正常生成唯一的 ID 标识符。
3.  **安全性**：分布式 ID 生成器通常是独立于应用程序和业务逻辑的。它们被设计为一个单独的组件或服务，可以被各种应用程序和服务所共享和使用，使得各个应用程序之间的 ID 生成过程互不干扰。
4.  **高性能**：分布式 ID 生成器通常要求在很短的时间内生成唯一的标识符。为了实现低延迟，设计者通常采用高效的算法和数据结构，以及优化的网络通信和存储策略。
5.  **递增性**：分布式 ID 通常可以被设计成可按时间顺序排序，以便更容易对生成的 ID 进行索引、检索或排序操作。这对于一些场景，如日志记录和事件溯源等，非常重要。

#### 分布式 ID 的实现

分布式 ID 的实现方式：

1.  UUID
2.  Redis 自增
3.  数据库自增
4.  snowflake 算法（雪花算法）

这里我们使用自定义的方式实现：**时间戳** + **序列号** + **数据库自增**

为了增加 ID 的安全性，我们可以不直接使用 Redis 自增的数值，而是拼接一些其它信息，比如时间戳、UUID、业务关键词

![](<assets/1740497736111.png>)

*   符号位：1bit，永远为 0（表示正数）
*   时间戳：31bit，以秒为单位，可以使用 69 年（ 2 31 / 3600 / 24 / 365 ≈ 69 2^{31}/3600/24/365≈69 231/3600/24/365≈69）
*   序列号：32bit，秒内的计数器，支持每秒产生 2^32 个不同 ID

分布式 ID 生成器：

```
@Component
public class RedisIdWorker {

    @Resource
    private StringRedisTemplate stringRedisTemplate;
    
    private static final long BEGIN_TIMESTAMP = 1640995200;
    
    private static final int COUNT_BITS = 32;

    
    public long nextId(String keyPrefix){
        
        LocalDateTime now = LocalDateTime.now();
        long nowSecond = now.toEpochSecond(ZoneOffset.UTC);
        long timestamp = nowSecond - BEGIN_TIMESTAMP;
        
        
        String date = now.format(DateTimeFormatter.ofPattern("yyyyMMdd"));
        Long count = stringRedisTemplate.opsForValue().increment(ID_PREFIX + keyPrefix + ":" + date);
        
        return timestamp << COUNT_BITS | count;
    }

    public static void main(String[] args) {
        LocalDateTime time = LocalDateTime.of(2022, 1, 1, 0, 0, 0);
        long second = time.toEpochSecond(ZoneOffset.UTC);
        System.out.println("second = " + second);
    }
}

```

测试类：

```
@SpringBootTest
public class RedisIdWorkerTest {

    @Resource
    private RedisIdWorker redisIdWorker;

    private ExecutorService es = Executors.newFixedThreadPool(500);

    
    @Test
    public void testNextId() throws InterruptedException {
        
        CountDownLatch latch = new CountDownLatch(300);
        
        Runnable task = () -> {
            for (int i = 0; i < 100; i++) {
                long id = redisIdWorker.nextId("order");
                System.out.println("id = " + id);
            }
            
            latch.countDown();
        };
        long begin = System.currentTimeMillis();
        
        for (int i = 0; i < 300; i++) {
            es.submit(task);
        }
        
        latch.await();
        long end = System.currentTimeMillis();
        System.out.println("生成3w个id共耗时" + (end - begin) + "ms");
    }
}

```

![](<assets/1740497736373.png>)

#### 优惠券秒杀接口实现

**前提**：需要先有优惠券，打开数据库然后添加秒杀券（普通券抢购功能前端未实现）

![](<assets/1740497736701.png>)

**业务流程**：

![](<assets/1740497737040.png>)

```
    
    @Transactional
    @Override
    public Result seckillVoucher(Long voucherId) {
        
        SeckillVoucher voucher = seckillVoucherService.getById(voucherId);
        
        if (voucher.getBeginTime().isAfter(LocalDateTime.now())) {
            
            return Result.fail("秒杀尚未开始");
        }
        if (voucher.getEndTime().isBefore(LocalDateTime.now())) {
            
            return Result.fail("秒杀已结束");
        }
        if (voucher.getStock() < 1) {
            return Result.fail("秒杀券已抢空");
        }
        
        boolean flag = seckillVoucherService.update(new LambdaUpdateWrapper<SeckillVoucher>()
                .eq(SeckillVoucher::getVoucherId, voucherId)
                .setSql("stock = stock -1"));
        if (!flag){
            throw new RuntimeException("秒杀券扣减失败");
        }
        
        VoucherOrder voucherOrder = new VoucherOrder();
        long orderId = redisIdWorker.nextId(SECKILL_VOUCHER_ORDER);
        voucherOrder.setId(orderId);
        voucherOrder.setUserId(ThreadLocalUtls.getUser().getId());
        voucherOrder.setVoucherId(voucherOrder.getId());
        flag = this.save(voucherOrder);
        if (!flag){
            throw new RuntimeException("创建秒杀券订单失败");
        }
        
        return Result.ok(orderId);
    }

```

![](<assets/1740497737315.png>)

![](<assets/1740497737693.png>)

#### 单体下一人多单超卖问题

上一节我们通过分布式 ID + 事务成功完成了优惠券秒杀功能，并且在测试后发现逻辑跑通了，看上去已经成功的解决了秒杀优惠券功能。但是前面我们只是正常的测试，那如果换到高并发的场景下能否成功解决？现在就让我们使用 Jmeter 来进行压力测试看看吧！

![](<assets/1740497737996.png>)

经过测试，可以发现在 **qps400** 时，异常率高达 **95.80%**，也就是说 2000 个请求只有 84 个请求正常响应，其它的 1916 个请求失败，这是很糟糕的，如果发生在公司的系统（特别是大公司）那将是不可估量的损失！经过多次压测，库存居然还惊现了负值！

![](<assets/1740497738312.png>)

为什么会产生超卖问题呢？

![](<assets/1740497738568.png>)

线程 1 查询库存，发现库存充足，创建订单，然后准备对库存进行扣减，但此时线程 2 和线程 3 也进行查询，同样发现库存充足，然后线程 1 执行完扣减操作后，库存变为了 0，线程 2 和线程 3 同样完成了库存扣减操作，最终导致库存变成了负数！这就是超卖问题的完整流程

那么我们该如何有效防止超卖问题的发生呢，以下提供几种常见的解决方案

*   **超卖问题的常见解决方案**：
    
    *   **悲观锁**，认为线程安全问题一定会发生，因此操作数据库之前都需要先获取锁，确保线程串行执行。常见的悲观锁有：`synchronized`、`lock`
    *   **乐观锁**，认为线程安全问题不一定发生，因此不加锁，只会在更新数据库的时候去判断有没有其它线程对数据进行修改，如果没有修改则认为是安全的，直接更新数据库中的数据即可，如果修改了则说明不安全，直接抛异常或者等待重试。常见的实现方式有：版本号法、CAS 操作、乐观锁算法
*   **悲观锁和乐观锁的比较**
    
    *   悲观锁比乐观锁的**性能**低：悲观锁需要先加锁再操作，而乐观锁不需要加锁，所以乐观锁通常具有更好的性能。
    *   悲观锁比乐观锁的**冲突处理能力**低：悲观锁在冲突发生时直接阻塞其他线程，乐观锁则是在提交阶段检查冲突并进行重试。
    *   悲观锁比乐观锁的**并发度**低：悲观锁存在锁粒度较大的问题，可能会限制并发性能；而乐观锁可以实现较高的并发度。
    *   应用场景：两者都是互斥锁，悲观锁适合写入操作较多、冲突频繁的场景；乐观锁适合读取操作较多、冲突较少的场景。

**拓展**：CAS

CAS（Compare and Swap）是一种并发编程中常用的原子操作，用于解决多线程环境下的数据竞争问题。它是乐观锁算法的一种实现方式。

CAS 操作包含三个参数：内存地址 V、旧的预期值 A 和新的值 B。CAS 的执行过程如下：

1.  **比较**（Compare）：将内存地址 V 中的值与预期值 A 进行比较。
2.  **判断**（Judgment）：如果相等，则说明当前值和预期值相等，表示没有发生其他线程的修改。
3.  **交换**（Swap）：使用新的值 B 来更新内存地址 V 中的值。

CAS 操作是一个原子操作，意味着在执行过程中不会被其他线程中断，保证了线程安全性。如果 CAS 操作失败（即当前值与预期值不相等），通常会进行重试，直到 CAS 操作成功为止。

CAS 操作适用于精细粒度的并发控制，可以避免使用传统的加锁机制带来的性能开销和线程阻塞。然而，CAS 操作也存在一些限制和注意事项：

1.  **ABA 问题**：CAS 操作无法感知到对象值从 A 变为 B 又变回 A 的情况，可能会导致数据不一致。为了解决 ABA 问题，可以引入版本号或标记位等机制。
2.  **自旋开销**：当 CAS 操作失败时，需要不断地进行重试，会占用 CPU 资源。如果重试次数过多或者线程争用激烈，可能会引起性能问题。
3.  **并发性限制**：如果多个线程同时对同一内存地址进行 CAS 操作，只有一个线程的 CAS 操作会成功，其他线程需要重试或放弃操作。

在 Java 中，提供了相关的 CAS 操作支持，如`AtomicInteger`、`AtomicLong`、`AtomicReference`等类，可以实现基于 CAS 操作的线程安全操作。

#### 乐观锁解决一人多单超卖问题

*   **实现方式一**：版本号法
    
    ![](<assets/1740497738818.png>)
    
    首先我们要为 tb_seckill_voucher 表新增一个版本号字段 version ，线程 1 查询完库存，在进行库存扣减操作的同时将版本号 + 1，线程 2 在查询库存时，同时查询出当前的版本号，发现库存充足，也准备执行库存扣减操作，但是需要判断当前的版本号是否是之前查询时的版本号，结果发现版本号发生了改变，这就说明数据库中的数据已经发生了修改，需要进行重试（或者直接抛异常中断）
    
*   **实现方式二**：CAS 法
    
    ![](<assets/1740497739101.png>)
    
    CAS 法类似与版本号法，但是不需要另外在添加一个 version 字段，而是直接使用库存替代版本号，线程 1 查询完库存后进行库存扣减操作，线程 2 在查询库存时，发现库存充足，也准备执行库存扣减操作，但是需要判断当前的库存是否是之前查询时的库存，结果发现库存数量发生了改变，这就说明数据库中的数据已经发生了修改，需要进行重试（或者直接抛异常中断）
    

综上所述，使用 CAS 法要更加好，能够避免额外的内存开销，下面就让我们用代码来实现吧 (●ˇ∀ˇ●)

```
        
        boolean flag = seckillVoucherService.update(new LambdaUpdateWrapper<SeckillVoucher>()
                .eq(SeckillVoucher::getVoucherId, voucherId)
                .eq(SeckillVoucher::getStock, voucher.getStock())
                .setSql("stock = stock -1"));

```

可以看到实现起来也是相当简单，只需要添加一个`eq(SeckillVoucher::getStock, voucher.getStock())`即可，现在我们再来使用 Jmeter 测试

![](<assets/1740497739312.png>)

**qps200**，**异常 90%**，**吞吐量 1652.9**

这又是什么原因呢？这就是乐观锁的弊端，我们只要发现数据修改就直接终止操作了，我们只需要修改一下判断条件，即只要库存大于 0 就可以进行修改，而不是库存数据修改我们就终止操作

```
        
        boolean flag = seckillVoucherService.update(new LambdaUpdateWrapper<SeckillVoucher>()
                .eq(SeckillVoucher::getVoucherId, voucherId)
                .gt(SeckillVoucher::getStock, 0)
                .setSql("stock = stock -1"));

```

![](<assets/1740497739647.png>)

**qps200**，**异常 50%**，**吞吐量 498.8**

#### 单体下的一人一单超卖问题

![](<assets/1740497740039.png>)

```
    
    @Transactional
    @Override
    public Result seckillVoucher(Long voucherId) {
        
        SeckillVoucher voucher = seckillVoucherService.getById(voucherId);
        
        if (voucher.getBeginTime().isAfter(LocalDateTime.now())) {
            
            return Result.fail("秒杀尚未开始");
        }
        if (voucher.getEndTime().isBefore(LocalDateTime.now())) {
            
            return Result.fail("秒杀已结束");
        }
        if (voucher.getStock() < 1) {
            return Result.fail("秒杀券已抢空");
        }
        
        int count = this.count(new LambdaQueryWrapper<VoucherOrder>()
                .eq(VoucherOrder::getUserId, ThreadLocalUtls.getUser().getId()));
        if (count >= 1) {
            
            return Result.fail("用户已购买");
        }
        
        boolean flag = seckillVoucherService.update(new LambdaUpdateWrapper<SeckillVoucher>()
                .eq(SeckillVoucher::getVoucherId, voucherId)
                .gt(SeckillVoucher::getStock, 0)
                .setSql("stock = stock -1"));
        if (!flag) {
            throw new RuntimeException("秒杀券扣减失败");
        }
        
        VoucherOrder voucherOrder = new VoucherOrder();
        long orderId = redisIdWorker.nextId(SECKILL_VOUCHER_ORDER);
        voucherOrder.setId(orderId);
        voucherOrder.setUserId(ThreadLocalUtls.getUser().getId());
        voucherOrder.setVoucherId(voucherOrder.getId());
        flag = this.save(voucherOrder);
        if (!flag) {
            throw new RuntimeException("创建秒杀券订单失败");
        }
        
        return Result.ok(orderId);
    }
}

```

现在是不是可以成功完成了一人一单的要求呢？让我们来使用 Jmeter 测一下吧

![](<assets/1740497740379.png>)

通过测试，发现并没有达到我们想象中的目标，一个人只能购买一次，但是发现一个用户居然能够购买 8 次。这说明还是存在超卖问题！

**问题原因**：出现这个问题的原因和前面库存为负数数的情况是一样的，线程 1 查询当前用户是否有订单，当前用户没有订单准备下单，此时线程 2 也查询当前用户是否有订单，由于线程 1 还没有完成下单操作，线程 2 同样发现当前用户未下单，也准备下单，这样明明一个用户只能下一单，结果下了两单，也就出现了超卖问题

**解决方案**：一般这种超卖问题可以使用下面两种常见的解决方案

1.  悲观锁
2.  乐观锁

#### 悲观锁解决超卖问题

乐观锁需要判断数据是否修改，而当前是判断当前是否存在，所以无法像解决库存超卖一样使用 CAS 机制，但是可以使用版本号法，但是版本号法需要新增一个字段，所以这里为了方便，就直接演示使用悲观锁解决超卖问题

![](<assets/1740497740627.png>)

代码实现：

```
    
    @Transactional
    @Override
    public Result seckillVoucher(Long voucherId) {
        
        SeckillVoucher voucher = seckillVoucherService.getById(voucherId);
        
        if (voucher.getBeginTime().isAfter(LocalDateTime.now())) {
            
            return Result.fail("秒杀尚未开始");
        }
        if (voucher.getEndTime().isBefore(LocalDateTime.now())) {
            
            return Result.fail("秒杀已结束");
        }
        if (voucher.getStock() < 1) {
            return Result.fail("秒杀券已抢空");
        }
        
        Long userId = ThreadLocalUtls.getUser().getId();
        synchronized (userId.toString().intern()) {
            
            IVoucherOrderService proxy = (IVoucherOrderService) AopContext.currentProxy();
            return proxy.createVoucherOrder(userId, voucherId);
        }
    }

    
    @Transactional
    public Result createVoucherOrder(Long userId, Long voucherId) {

        
        int count = this.count(new LambdaQueryWrapper<VoucherOrder>()
                .eq(VoucherOrder::getUserId, userId));
        if (count >= 1) {
            
            return Result.fail("用户已购买");
        }
        
        boolean flag = seckillVoucherService.update(new LambdaUpdateWrapper<SeckillVoucher>()
                .eq(SeckillVoucher::getVoucherId, voucherId)
                .gt(SeckillVoucher::getStock, 0)
                .setSql("stock = stock -1"));
        if (!flag) {
            throw new RuntimeException("秒杀券扣减失败");
        }
        
        VoucherOrder voucherOrder = new VoucherOrder();
        long orderId = redisIdWorker.nextId(SECKILL_VOUCHER_ORDER);
        voucherOrder.setId(orderId);
        voucherOrder.setUserId(ThreadLocalUtls.getUser().getId());
        voucherOrder.setVoucherId(voucherOrder.getId());
        flag = this.save(voucherOrder);
        if (!flag) {
            throw new RuntimeException("创建秒杀券订单失败");
        }
        
        return Result.ok(orderId);

    }

```

![](<assets/1740497740963.png>)

**实现细节**：

1.  锁的范围尽量小。`synchronized`尽量锁代码块，而不是方法，锁的范围越大性能越低
    
2.  锁的对象一定要是一个不变的值。我们不能直接锁 `Long` 类型的 userId，每请求一次都会创建一个新的 userId 对象，synchronized 要锁不变的值，所以我们要将 Long 类型的 userId 通过 toString() 方法转成 `String` 类型的 userId，`toString()`方法底层（可以点击去看源码）是直接 new 一个新的 String 对象，显然还是在变，所以我们要使用 `intern()` 方法从常量池中寻找与当前 字符串值一致的字符串对象，这就能够保障一个用户 发送多次请求，每次请求的 userId 都是不变的，从而能够完成锁的效果（并行变串行）
    
3.  我们要锁住整个事务，而不是锁住事务内部的代码。如果我们锁住事务内部的代码会导致其它线程能够进入事务，当我们事务还未提交，锁一旦释放，仍然会存在超卖问题
    
4.  Spring 的`@Transactional`注解要想事务生效，必须使用动态代理。Service 中一个方法中调用另一个方法，另一个方法使用了事务，此时会导致`@Transactional`失效，所以我们需要创建一个代理对象，使用代理对象来调用方法。
    
    这篇文章对 @Transactional 注解事务失效的常见场景进行了一个总结：[【Java 面试篇】Spring 中 @Transactional 注解事务失效的常见场景](https://blog.csdn.net/qq_66345100/article/details/129322325)
    
    让代理对象生效的步骤：
    
    ①引入 AOP 依赖，动态代理是 AOP 的常见实现之一
    
    ```
            <dependency>
                <groupId>org.aspectj</groupId>
                <artifactId>aspectjweaver</artifactId>
            </dependency>
    
    ```
    
    ②暴露动态代理对象，默认是关闭的
    
    ```
    @EnableAspectJAutoProxy(exposeProxy = true)
    
    ```
    

#### 集群下的一人一单超卖问题

1）**搭建集群并实现负载均衡**

首先，在 IDEA 中启动两个 SpringBoot 程序，一个端口号是 8081，另一个端口是 8082：

![](<assets/1740497741450.png>)

![](<assets/1740497741706.png>)

然后在 Nginx 中配置负载均衡：

![](<assets/1740497742000.png>)

这里遇到 [bug2](#2)

2）**准备两个接口**，用于模拟集群下的多用户重复下单

![](<assets/1740497742256.png>)

在发送请求之前，现在锁的内部打一个断点

![](<assets/1740497742603.png>)

3）**使用接口发送请求**

由于 Nginx 负载均衡（轮询策略）的原因，请求 1 会被 8081 接收，请求 2 会被 8082 接收

![](<assets/1740497742907.png>)

可以看到，两者都进入了锁的内部，这个`synchronized`锁形同虚设，这是由于`synchronized`是本地锁，只能提供线程级别的同步，每个 JVM 中都有一把 synchronized 锁，不能跨 JVM 进行上锁，当一个线程进入被 `synchronized` 关键字修饰的方法或代码块时，它会尝试获取对象的内置锁（也称为监视器锁）。如果该锁没有被其他线程占用，则当前线程获得锁，可以继续执行代码；否则，当前线程将进入阻塞状态，直到获取到锁为止。而现在我们是创建了两个节点，也就意味着有两个 JVM，所以`synchronized`会失效！

从而出现超卖问题：

![](<assets/1740497743252.png>)

#### 分布式锁

*   分布式锁：满足分布式系统或集群模式下多进程可见并且互斥的锁

前面`sychronized`锁失效的原因是由于每一个 JVM 都有一个独立的锁监视器，用于监视当前 JVM 中的`sychronized`锁，所以无法保障多个集群下只有一个线程访问一个代码块。所以我们直接将使用一个分布锁，在整个系统的全局中设置一个锁监视器，从而保障不同节点的 JVM 都能够识别，从而实现集群下只允许一个线程访问一个代码块

![](<assets/1740497743565.png>)

![](<assets/1740497744044.png>)

*   **分布式锁的特点**：
    
    *   **多线程可见**。
    *   **互斥**。分布式锁必须能够确保在任何时刻只有一个节点能够获得锁，其他节点需要等待。
    *   **高可用**。分布式锁应该具备高可用性，即使在网络分区或节点故障的情况下，仍然能够正常工作。（容错性）当持有锁的节点发生故障或宕机时，系统需要能够自动释放该锁，以确保其他节点能够继续获取锁。
    *   **高性能**。分布式锁需要具备良好的性能，尽可能减少对共享资源的访问等待时间，以及减少锁竞争带来的开销。
    *   **安全性**。（可重入性）如果一个节点已经获得了锁，那么它可以继续请求获取该锁而不会造成死锁。（锁超时机制）为了避免某个节点因故障或其他原因无限期持有锁而影响系统正常运行，分布式锁通常应该设置超时机制，确保锁的自动释放。
    
    ……
    
*   **分布式锁的常见实现方式**：
    
    ![](<assets/1740497744395.png>)
    
    *   **基于关系数据库**：可以利用数据库的事务特性和唯一索引来实现分布式锁。通过向数据库插入一条具有唯一约束的记录作为锁，其他进程在获取锁时会受到数据库的并发控制机制限制。
    *   **基于缓存**（如 Redis）：使用分布式缓存服务（如 Redis）提供的原子操作来实现分布式锁。通过将锁信息存储在缓存中，其他进程可以通过检查缓存中的锁状态来判断是否可以获取锁。
    *   **基于 ZooKeeper**：ZooKeeper 是一个分布式协调服务，可以用于实现分布式锁。通过创建临时有序节点，每个请求都会尝试创建一个唯一的节点，并检查自己是否是最小节点，如果是，则表示获取到了锁。
    *   **基于分布式算法**：还可以利用一些分布式算法来实现分布式锁，例如 Chubby、DLM（Distributed Lock Manager）等。这些算法通过在分布式系统中协调进程之间的通信和状态变化，实现分布式锁的功能。
*   **`setnx`指令的特点**：setnx 只能设置 key 不存在的值，值不存在设置成功，返回 1 ；值存在设置失败，返回 0
    
*   **获取锁**：
    
    *   方式一：
        
        ```
        setnx [key] [value]
        
        expire [key] [time]
        
        ```
        
    *   方式二：这种方式更加推荐，因为将上面两个指令变成一个指令，从而保障指令的原子性
        
        ```
        set [key] [value] ex [time] nx
        
        ```
        
*   **释放锁**：
    
    ```
    del [key]
    
    ```
    

#### 分布式锁解决超卖问题

由于本项目是专门学习 Redis 的，所以我们这里将会使用 Redis 的`setnx`指令实现分布式锁解决超卖问题

![](<assets/1740497744682.png>)

1）**创建分布式锁**：

```
public class SimpleRedisLock implements Lock {

    
    private StringRedisTemplate stringRedisTemplate;

    
    private String name;

    public SimpleRedisLock(StringRedisTemplate stringRedisTemplate, String name) {
        this.stringRedisTemplate = stringRedisTemplate;
        this.name = name;
    }


    
    @Override
    public boolean tryLock(long timeoutSec) {
        String id = Thread.currentThread().getId() + "";
        
        Boolean result = stringRedisTemplate.opsForValue()
                .setIfAbsent("lock:" + name, id, timeoutSec, TimeUnit.SECONDS);
        return Boolean.TRUE.equals(result);
    }

    
    @Override
    public void unlock() {
        stringRedisTemplate.delete("lock:" + name);
    }
}

```

2）**使用分布式锁**。改造前面 VoucherOrderServiceImpl 中的代码，将之前使用`sychronized`锁的地方，改成我们自己实现的分布式锁：

```
        
        Long userId = ThreadLocalUtls.getUser().getId();
        SimpleRedisLock lock = new SimpleRedisLock(stringRedisTemplate, "order:" + userId);
        boolean isLock = lock.tryLock(1200);
        if (!isLock) {
            
            return Result.fail("一人只能下一单");
        }
        try {
            
            IVoucherOrderService proxy = (IVoucherOrderService) AopContext.currentProxy();
            return proxy.createVoucherOrder(userId, voucherId);
        } finally {
            lock.unlock();
        }

```

PS：完整代码参考[前面](#beiguansuo)

同样，重启程序，然后在 postman 中使用同一个用户的 token 发送两次请求，可以发现只有有一个用户获取锁成功

![](<assets/1740497744968.png>)

**实现细节**:

1.  `try...finally...`确保发生异常时锁能够释放，注意这给地方不要使用`catch`，A 事务方法内部调用 B 事务方法，A 事务方法不能够直接 catch，否则会导致事务失效，详情可以参考这篇文章：[【Java 面试篇】Spring 中 @Transactional 注解事务失效的常见场景](https://blog.csdn.net/qq_66345100/article/details/129322325)

#### 分布式锁优化

##### 分布式锁优化 1

本次优化主要解决了锁超时释放出现的超卖问题

上一节，我们实现了一个简单的分布式锁，但是会存在一个问题：当线程 1 获取锁后，由于业务阻塞，线程 1 的锁超时释放了，这时候线程 2 趁虚而入拿到了锁，然后此时线程 1 业务完成了，然后把线程 2 刚刚获取的锁给释放了，这时候线程 3 又趁虚而入拿到了锁，这就导致又出现了**超卖问题**！（但是这个在小项目（并发数不高）中出现的概率比较低，在大型项目（并发数高）情况下是有一定概率的）

![](<assets/1740497745295.png>)

备注：我们可以把锁的有效期降低一点，这样就能够测试上面哪种情况了 (●’◡’●)

如何解决呢？我们为分布式锁添加一个线程标识，在释放锁时判断当前锁是否是自己的锁，是自己的就直接释放，不是自己的就不释放锁，从而解决多个线程同时获得锁的情况导致出现超卖

![](<assets/1740497745574.png>)

我们只需要修改一下锁的实现，业务代码不需要修改：

```
package com.hmdp.utils.lock.impl;

import cn.hutool.core.lang.UUID;
import com.hmdp.utils.lock.Lock;
import org.springframework.data.redis.core.StringRedisTemplate;

import java.util.concurrent.TimeUnit;


public class SimpleRedisLock implements Lock {

    
    private StringRedisTemplate stringRedisTemplate;

    
    private String name;
    
    public static final String KEY_PREFIX = "lock:";
    
    public static final String ID_PREFIX = UUID.randomUUID().toString(true) + "-";

    public SimpleRedisLock(StringRedisTemplate stringRedisTemplate, String name) {
        this.stringRedisTemplate = stringRedisTemplate;
        this.name = name;
    }


    
    @Override
    public boolean tryLock(long timeoutSec) {
        String threadId = ID_PREFIX + Thread.currentThread().getId() + "";
        
        Boolean result = stringRedisTemplate.opsForValue()
                .setIfAbsent(KEY_PREFIX + name, threadId, timeoutSec, TimeUnit.SECONDS);
        return Boolean.TRUE.equals(result);
    }

    
    @Override
    public void unlock() {
        
        String currentThreadFlag = ID_PREFIX + Thread.currentThread().getId();
        String redisThreadFlag = stringRedisTemplate.opsForValue().get(KEY_PREFIX + name);
        if (currentThreadFlag != null || currentThreadFlag.equals(redisThreadFlag)) {
            
            stringRedisTemplate.delete(KEY_PREFIX + name);
        }
        
    }
}

```

##### 分布式锁优化 2

本次优化主要解决了释放锁时的原子性问题。说到底也是锁超时释放的问题

在上一节中，我们通过给锁添加一个线程标识，并且在释放锁时添加一个判断，从而防止锁超时释放产生的超卖问题，一定程度上解决了超卖问题，但是仍有可能发生超卖问题（出现超卖概率更低了）：当线程 1 获取锁，执行完业务然后并且判断完当前锁是自己的锁时，但就在此时发生了阻塞，结果锁被超时释放了，线程 2 立马就趁虚而入了，获得锁执行业务，但就在此时线程 1 阻塞完成，由于已经判断过锁，已经确定锁是自己的锁了，于是直接就删除了锁，结果删的是线程 2 的锁，这就又导致线程 3 趁虚而入了，从而继续发生**超卖问题**

**备注**：我们可以在判断删除锁的那行代码上打一个断点，然后 user1 发送一个请求，获取锁，手动把锁删了，模拟锁超时释放，然后使用 user2 发送一个请求，成功获取锁，从而模拟上诉过程，检验超卖问题

![](<assets/1740497745942.png>)

PS：虽然这个情况发生的概率较低，但是根据**墨菲定律**，我们最好不要抱有侥幸心理，不然最终我们会在这个细微的问题上付诸沉重的代价！你可能还会想，判断锁和释放锁在同一个方法中，并且两者之间没有别的代码，为什么会发生阻塞呢？JVM 的垃圾回收机制会导致短暂的阻塞（我个人感觉这种情况发生的概率真的不高，但是我也没有实际接触过真正的大型高并发项目，所以具体也只能靠揣摩）

那么我们该如何保障 **判断锁** 和 **释放锁** 这连段代码的原子性呢？答案是使用 Lua 脚本，关于 Lua 脚本相关知识可以参考这篇文章：[Lua 快速入门笔记客](https://blog.csdn.net/qq_66345100/article/details/131617253?spm=1001.2014.3001.5501)

那么 Lua 脚本是如何确保原子性的呢？Redis 使用（支持）相同的 Lua 解释器，来运行所有的命令。Redis 还保证脚本以原子方式执行：在执行脚本时，不会执行其他脚本或 Redis 命令。这个语义类似于`MULTI`（开启事务）/`EXEC`（触发事务，一并执行事务中的所有命令）。从所有其他客户端的角度来看，脚本的效果要么仍然不可见，要么已经完成。

注意：虽然 Redis 在单个 Lua 脚本的执行期间会暂停其他脚本和 Redis 命令，以确保脚本的执行是原子的，但如果 Lua 脚本本身出错，那么无法完全保证原子性。也就是说 Lua 脚本中的 Redis 指令出错，会发生回滚以确保原子性，但 Lua 脚本本身出错就无法保障原子性

**代码实现**

![](<assets/1740497746250.png>)

释放锁的业务流程是这样的：

1.  获取锁中的线程标示
    
2.  判断是否与指定的标示（当前线程标示）一致
    
3.  如果一致则释放锁（删除）
    
4.  如果不一致则什么都不做
    

![](<assets/1740497746603.png>)

注意：在 IDEA 中编写 Lua 脚本，需要先下载一个 Lua 脚本插件 **`Tarantool-EmmyLua`**

编写 Lua 脚本：

```


if (redis.call('get', KEYS[1]) == ARGV[1]) then
    
    return redis.call('del', KEYS[1])
end

return 0

```

编写 Java 代码，使用 Lua 实现释放锁：

```
package com.hmdp.utils.lock.impl;

import cn.hutool.core.lang.UUID;
import com.hmdp.utils.lock.Lock;
import org.springframework.core.io.ClassPathResource;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.data.redis.core.script.DefaultRedisScript;

import java.util.Collections;
import java.util.concurrent.TimeUnit;


public class SimpleRedisLock implements Lock {

    
    private StringRedisTemplate stringRedisTemplate;

    
    private String name;
    
    private static final String KEY_PREFIX = "lock:";
    
    private static final String ID_PREFIX = UUID.randomUUID().toString(true) + "-";

    public SimpleRedisLock(StringRedisTemplate stringRedisTemplate, String name) {
        this.stringRedisTemplate = stringRedisTemplate;
        this.name = name;
    }


    
    @Override
    public boolean tryLock(long timeoutSec) {
        String threadId = ID_PREFIX + Thread.currentThread().getId() + "";
        
        Boolean result = stringRedisTemplate.opsForValue()
                .setIfAbsent(KEY_PREFIX + name, threadId, timeoutSec, TimeUnit.SECONDS);
        return Boolean.TRUE.equals(result);
    }

    
    private static final DefaultRedisScript<Long> UNLOCK_SCRIPT;

    static {
        UNLOCK_SCRIPT = new DefaultRedisScript<>();
        UNLOCK_SCRIPT.setLocation(new ClassPathResource("lua/unlock.lua"));
        UNLOCK_SCRIPT.setResultType(Long.class);
    }

    
    @Override
    public void unlock() {
        
        stringRedisTemplate.execute(
                UNLOCK_SCRIPT,
                Collections.singletonList(KEY_PREFIX + name),
                ID_PREFIX + Thread.currentThread().getId()
        );
    }
}

```

业务代码不需要变动，锁的使用方法也不需要改变，因为我们之改动了锁的实现，使用方式没有改变

![](<assets/1740497746938.png>)

现在我们的分布式锁满足了：

1.  **多线程可见**，将锁放到 Redis 中，所有的 JVM 都可以同时看到
2.  **互斥**，`set ex nx`指令互斥
3.  **高可用**，层层优化，即使是特别极端的情况下照样可以防止超卖
4.  **高性能**，Redis 的 IO 速度很快，Lua 脚本的性能也很快
5.  **安全性**，这个不用多说了，通过给锁夹线程标识 + Lua 封装 Redis 指令充分保障了线程安全，不那么容易出现并发安全问题，同时采用超时释放避免死锁

#### Redisson

经过优化 1 和优化 2，我们实现的分布式锁已经达到**生产可用级别**了，但是还不够完善，比如：

1.  分布式锁**不可重入**：不可重入是指同一线程不能重复获取同一把锁。比如，方法 A 中调用方法 B，方法 A 需要获取分布式锁，方法 B 同样需要获取分布式锁，线程 1 进入方法 A 获取了一次锁，进入方法 B 又获取一次锁，由于锁不可重入，所以就会导致死锁
    
2.  分布式锁**不可重试**：获取锁只尝试一次就返回 false，没有重试机制，这会导致数据丢失，比如线程 1 获取锁，然后要将数据写入数据库，但是当前的锁被线程 2 占用了，线程 1 直接就结束了而不去重试，这就导致数据发生了丢失
    
3.  分布式锁**超时释放**：超市释放机机制虽然一定程度避免了死锁发生的概率，但是如果业务执行耗时过长，期间锁就释放了，这样存在安全隐患。锁的有效期过短，容易出现业务没执行完就被释放，锁的有效期过长，容易出现死锁，所以这是一个大难题！
    
    我们可以设置一个较短的有效期，但是加上一个 心跳机制 和 自动续期：在锁被获取后，可以使用心跳机制并自动续期锁的持有时间。通过定期发送心跳请求，显示地告知其他线程或系统锁还在使用中，同时更新锁的过期时间。如果某个线程持有锁的时间超过了预设的有效时间，其他线程可以尝试重新获取锁。
    
4.  **主从一致性问题**：如果 Redis 提供了主从集群，主从同步存在延迟，线程 1 获取了锁
    

我们如果想要更进一步优化分布式锁，当然是可以的，但是没必要，除非是迫不得已，我们完全可以直接使用已经造好的轮子，比如：[Redisson](https://github.com/redisson/redisson)。Redssion 是一个十分成熟的 Redis 框架，功能也很多，比如：**分布式锁**和**同步器**、**分布式对象**、**分布式集合**、**分布式服务**，各种 Redis 实现分布式的解决方案。简而言之 Redisson 就是一个使用 Redis 解决分布式问题的方案的集合，当然它不仅仅是解决分布式相关问题，还包含其它的一些问题。

所以说分布式锁的究极优化就是使用别人造好的轮子🤣

##### Redisson 实现分布式锁

1）引入 Redisson 依赖

```
        <dependency>
            <groupId>org.redisson</groupId>
            <artifactId>redisson</artifactId>
            <version>3.13.6</version>
        </dependency>

```

2）配置 Redisson 客户端

```
@Configuration
public class RedissonConfig {

    @Value("${spring.redis.host}")
    private String host;
    @Value("${spring.redis.port}")
    private String port;
    @Value("${spring.redis.password}")
    private String password;

    
    @Bean
    public RedissonClient redissonClient() {
        
        Config config = new Config();
        
        config.useSingleServer().setAddress("redis://" + this.host + ":" + this.port)
                .setPassword(this.password);
        
        return Redisson.create(config);
    }
}

```

**温馨提示**：此外还有一种引入方式，可以引入 redission 的 starter 依赖，然后在 yml 文件中配置 Redisson，但是不推荐这种方式，因为他会替换掉 Spring 官方 提供的这套对 Redisson 的配置

3）我们只需要修改一下使用锁的地方，其它的业务代码都不需要该

```
        
        Long userId = ThreadLocalUtls.getUser().getId();
        RLock lock = redissonClient.getLock(RedisConstants.LOCK_ORDER_KEY + userId);
        boolean isLock = lock.tryLock();

```

PS：完整代码参考[前面](#beiguansuo)

*   **`tryLock`方法介绍**
    *   `tryLock()`：它会使用默认的超时时间和等待机制。具体的超时时间是由 Redisson 配置文件或者自定义配置决定的。
        
    *   `tryLock(long time, TimeUnit unit)`：它会在指定的时间内尝试获取锁（等待 time 后重试），如果获取成功则返回 true，表示获取到了锁；如果在指定时间内（Redisson 内部默认指定的）未能获取到锁，则返回 false。
        
    *   `tryLock(long waitTime, long leaseTime, TimeUnit unit)`：指定等待时间为 watiTime，如果超过 leaseTime 后还没有获取锁就直接返回失败
        

总的来讲自上而下，tryLock 的灵活性逐渐提高，无参 tryLock 时，`waitTime`的默认值是 - 1，代表不等待，`leaseTime`的默认值是 30，`unit`默认值是 seconds ，也就是锁超过 30 秒还没有释放就自动释放

##### 可重入锁的原理

现在我们的分布式锁就具有了**可重入性**，现在让我们来看一下 Redissson 可重入锁的原理吧 (●ˇ∀ˇ●)

![](<assets/1740497747276.png>)

Redisson 内部释放锁，并不是直接执行`del`命令将锁给删除，而是将锁以`hash`数据结构的形式存储在 Redis 中，每次获取锁，都将`value`的值 + 1，每次释放锁，都将 value 的值 - 1，只有锁的 value 值归 0 时才会真正的释放锁，从而确保锁的可重入性

###### 测试锁的可重入性

```
@SpringBootTest
@Slf4j
public class RedissonLockTest {

    @Resource
    private RedissonClient redissonClient;

    private RLock lock;
    
    @Test
    void method1() {
        boolean isLock = false;
        
        lock = redissonClient.getLock("lock");
        try {
            isLock = lock.tryLock();
            if (!isLock) {
                log.error("获取锁失败，1");
                return;
            }
            log.info("获取锁成功，1");
            method2();
        } finally {
            if (isLock) {
                log.info("释放锁，1");
                lock.unlock();
            }
        }
    }

    
    void method2() {
        boolean isLock = false;
        try {
            isLock = lock.tryLock();
            if (!isLock) {
                log.error("获取锁失败, 2");
                return;
            }
            log.info("获取锁成功，2");
        } finally {
            if (isLock) {
                log.info("释放锁，2");
                lock.unlock();
            }
        }
    }
}

```

![](<assets/1740497747499.png>)

![](<assets/1740497747789.png>)

###### 源码流程解析

详情见 P67

![](<assets/1740497748056.png>)

**Redisson 分布式锁原理**：

*   **如何解决可重入问题**：利用 hash 结构记录线程 id 和重入次数。
    
*   **如何解决可重试问题**：利用信号量和`PubSub`功能实现等待、唤醒，获取锁失败的重试机制。
    
*   **如何解决超时续约问题**：利用`watchDog`，每隔一段时间（releaseTime / 3），重置超时时间。
    
*   **如何解决主从一致性问题**：利用 Redisson 的`multiLock`，多个独立的 Redis 节点，必须在所有节点都获取重入锁，才算获取锁成功
    
    缺陷：运维成本高、实现复杂
    

源码流程示意图（这里展示的是无参构造器的源码执行流程，我们也可以看有构造器的源码执行流程）：

![](<assets/1740497748337.png>)

![](<assets/1740497748575.png>)

![](<assets/1740497748937.png>)

![](<assets/1740497749198.png>)

![](<assets/1740497749597.png>)

获取剩余有效期

![](<assets/1740497750027.png>)

Redisson 底层通过利用 Lua 脚本确保 **判断锁是否存在**、**添加锁的有效期**、**添加线程标识**这些的操作全部封装到了一个 Lua 脚本（确保了锁的**原子性**和**可重入性**）

![](<assets/1740497750535.png>)

有参构造器的源码执行流程：

![](<assets/1740497750846.png>)

底层通过**信号量** + **发布订阅模式**实现可重试机制（解决了锁的**可重试性**）

![](<assets/1740497751153.png>)

如何确保锁超时释放问题？线程 1 获取锁，由于业务阻塞导致锁超时释放了该如何解决呢？这就需要使用**看门狗机制**了，确保我们的业务是执行完释放，而不是阻塞释放

![](<assets/1740497751492.png>)

![](<assets/1740497751913.png>)

那么如何取消任务呢，不可能这个任务一直调用，让锁永不过期呢？

![](<assets/1740497752209.png>)

![](<assets/1740497752427.png>)

###### 测试主从节点锁的一致性

1）注入三个 RedissonClient

```
    @Bean
    public RedissonClient redissonClient() {
        
        Config config = new Config();
        
        config.useSingleServer().setAddress("redis://url")
                .setPassword(this.password);
        
        return Redisson.create(config);
    }
    @Bean
    public RedissonClient2 redissonClient() {
        
        Config config = new Config();
        
        config.useSingleServer().setAddress("redis://url")
                .setPassword(this.password);
        
        return Redisson.create(config);
    }
    @Bean
    public RedissonClient3 redissonClient() {
        
        Config config = new Config();
        
        config.useSingleServer().setAddress("redis://url")
                .setPassword(this.password);
        
        return Redisson.create(config);
    }

```

2）编写测试类：

```
    @BeforeEach
    void setUp(){
        RLock lock1 = redissonClient.getLock("order");
        RLock lock2 = redissonClient2.getLock("order");
        RLock lock3 = redissonClient3.getLock("order");
        
        redissonClient.getMultiLock(lock1, lock2, lock3);
    }

```

获取锁和释放锁代码都是一样的，执行代码，可以在节点 1、节点 2、节点 3 中查看到锁

#### 编码实现可重入锁

上一节，我们使用 Redisson 实现分布式锁，并且了解了 Redssion 可重入锁的原理，现在让我们手动来实现一下吧🤭

![](<assets/1740497752758.png>)

可以看到，可重入锁需要进行一系列的逻辑判断，这些逻辑代码我们最好将它们全都封装到一个 Lua 脚本 中，以确保操作的原子性，从而确保线程安全（Redisson 底层也是这么干的）

1）编写获取锁的 Lua 脚本

```


local key = KEYS[1];

local threadId = ARGV[1];

local releaseTime = ARGV[2];


if (redis.call('EXISTS', key) == 0) then
    
    redis.call('HSET', key, threadId, '1');
    
    redis.call('EXPIRE', key, releaseTime);
    return 1; 
end


if (redis.call('HEXISTS', key, threadId) == 1) then
    
    redis.call('HINCRBY', key, threadId, '1');
    
    redis.call('EXPIRE', key, releaseTime);
    return 1; 
end


return 0;

```

2）编写释放锁的 Lua 脚本

```


local key = KEYS[1];

local threadId = ARGV[1];

local releaseTime = ARGV[2];


if (redis.call('HEXISTS', key, threadId) == 0) then
    
    return nil; 
end

local count = redis.call('HINCRBY', key, threadId, -1);


if (count > 0) then
    
    redis.call('EXPIRE', key, releaseTime);
    return nil;
else
    
    redis.call('DEL', key);
    return nil;
end

```

3）编写可重入锁：

```
public class ReentrantLock implements Lock {
    
    private StringRedisTemplate stringRedisTemplate;
    
    private String name;
    
    private static final String KEY_PREFIX = "lock:";
    
    private static final String ID_PREFIX = UUID.randomUUID().toString(true) + "-";
    
    public long timeoutSec;

    public ReentrantLock(StringRedisTemplate stringRedisTemplate, String name) {
        this.stringRedisTemplate = stringRedisTemplate;
        this.name = name;
    }

    
    private static final DefaultRedisScript<Long> TRYLOCK_SCRIPT;

    static {
        TRYLOCK_SCRIPT = new DefaultRedisScript<>();
        TRYLOCK_SCRIPT.setLocation(new ClassPathResource("lua/re-trylock.lua"));
        TRYLOCK_SCRIPT.setResultType(Long.class);
    }

    
    @Override
    public boolean tryLock(long timeoutSec) {
        this.timeoutSec = timeoutSec;
        
        Long result = stringRedisTemplate.execute(
                TRYLOCK_SCRIPT,
                Collections.singletonList(KEY_PREFIX + name),
                ID_PREFIX + Thread.currentThread().getId(),
                Long.toString(timeoutSec)
        );
        return result != null && result.equals(1L);
    }

    
    private static final DefaultRedisScript<Long> UNLOCK_SCRIPT;

    static {
        UNLOCK_SCRIPT = new DefaultRedisScript<>();
        UNLOCK_SCRIPT.setLocation(new ClassPathResource("lua/re-unlock.lua"));
        UNLOCK_SCRIPT.setResultType(Long.class);
    }

    
    @Override
    public void unlock() {
        
        stringRedisTemplate.execute(
                UNLOCK_SCRIPT,
                Collections.singletonList(KEY_PREFIX + name),
                ID_PREFIX + Thread.currentThread().getId(),
                Long.toString(this.timeoutSec)
        );
    }
}

```

4）编写测试类

```
@SpringBootTest
@Slf4j
public class ReentrantLockTest {

    @Resource
    private StringRedisTemplate stringRedisTemplate;

    private ReentrantLock lock;

    
    @Test
    void method1() {
        boolean isLock = false;
        
        lock = new ReentrantLock(stringRedisTemplate, "order:" + 1);
        try {
            isLock = lock.tryLock(1200);
            if (!isLock) {
                log.error("获取锁失败，1");
                return;
            }
            log.info("获取锁成功，1");
            method2();
        } finally {
            if (isLock) {
                log.info("释放锁，1");
                lock.unlock();
            }
        }
    }

    
    void method2() {
        boolean isLock = false;
        try {
            isLock = lock.tryLock(1200);
            if (!isLock) {
                log.error("获取锁失败, 2");
                return;
            }
            log.info("获取锁成功，2");
        } finally {
            if (isLock) {
                log.info("释放锁，2");
                lock.unlock();
            }
        }
    }
}

```

![](<assets/1740497753128.png>)

在 VoucherOrderServiceImpl 中修改锁的实现方式，将之前的 Redssion 分布式锁改成我们自己写的可重入锁

```
        
        Long userId = ThreadLocalUtls.getUser().getId();
        ReentrantLock lock = new ReentrantLock(stringRedisTemplate, "order:" + userId);
        boolean isLock = lock.tryLock(1200);
        if (!isLock) {
            
            return Result.fail("一人只能下一单");
        }
        try {
            
            IVoucherOrderService proxy = (IVoucherOrderService) AopContext.currentProxy();
            return proxy.createVoucherOrder(userId, voucherId);
        } finally {
            lock.unlock();
        }

```

PS：完整代码参考[前面](#beiguansuo)

#### 秒杀优化

最开始我们的遇到自增 ID 问题，我们通过实现分布式 ID 解决了问题；后面我们在单体系统下遇到了一人多单超卖问题，我们通过乐观锁解决了；我们对业务进行了变更，将一人多单变成了一人一单，结果在高并发场景下同一用户发送相同请求仍然出现了超卖问题，我们通过悲观锁解决了；由于用户量的激增，我们将单体系统升级成了集群，结果由于锁只能在一个 JVM 中可见导致又出现了，在高并发场景下同一用户发送下单请求出现超卖问题，我们通过实现分布式锁成功解决集群下的超卖问题；由于我们最开始实现的分布式锁比较简单，会出现超时释放导致超卖问题，我们通过给锁添加线程标识成功解决了；但是释放锁时，判断锁是否是当前线程 和 删除锁两个操作不是原子性的，可能导致超卖问题，我们通过将两个操作封装到一个 Lua 脚本成功解决了；为了解决锁的不可重入性，我们通过将锁以 hash 结构的形式存储，每次释放锁都 value-1，获取锁 value+1，从而实现锁的可重入性，并且将释放锁和获取锁的操作封装到 Lua 脚本中以确保原子性。最最后，我们发现可以直接使用现有比较成熟的方案 Redisson 来解决上诉出现的所有问题🤣，什么不可重试、不可重入、超市释放、原子性等问题 Redisson 都提供相对应的解决方法（。＾▽＾）

所以现在锁的优化基本上已经到了极致，我们现在就要对 ** 性能 ** 和**稳定性**进行进一步的优化😄

##### 异步秒杀优化

*   **同步**（Synchronous）是指程序按照顺序依次执行，每一步操作完成后再进行下一步。在同步模式下，当一个任务开始执行时，程序会一直等待该任务完成后才会继续执行下一个任务。
*   **异步**（Asynchronous）是指程序在执行任务时，不需要等待当前任务完成，而是在任务执行的同时继续执行其他任务。在异步模式下，任务的执行顺序是不确定的，程序通过回调、事件通知等方式来获取任务执行的结果。

显然异步的性能是要高于同步的，但是会牺牲掉一定的数据一致性，所以也不是无脑用异步，要根据具体业务进行分析，这里的下单是可以使用异步的，因为下单操作比较耗时，后端操作步骤多，可以进行拆分

###### 分析

之前秒杀业务的流程：

![](<assets/1740497753396.png>)

1 秒钟 1000 个用户同时发起请求：

![](<assets/1740497753706.png>)

可以看到 **qps1000**、**平均值 1921**、**异常率 37.2%**、**吞吐量 315.3**，对比[优化后](#yibuyouhua)

![](<assets/1740497753995.png>)

可以看到这个流程是同步执行的，同步是比较耗费时间的，我们直接将同步变成异步，从而大幅提高秒杀业务的性能，具体如何做呢？我们可以将一部分的工作交给 Redis，并且不能直接去调用 Redis，而是通过开启一个独立的子线程去异步执行，从而大大提高效率

![](<assets/1740497754307.png>)

###### 实现

![](<assets/1740497754630.png>)

![](<assets/1740497754983.png>)

![](<assets/1740497755316.png>)

**细节**

1.  库存判断放到 Redis 中，我们应该使用哪一种数据结构存储订单的库存呢？可以直接使用 `String` 类型的数据结构，Redis 的 IO 操作是单线程的，所以能够充分保障线程安全。
2.  一人一单的判断也是由 Redis 完成的，所以我们需要在 Redis 中存储订单信息，而订单是唯一的，所以我们可以使用 `Set`类型的数据结构
3.  lua 脚本中，接收的参数都是 String 类型的，String 类型的数据无法进行比较，我们需要利用`tonumber`函数将 String 转成 Number
4.  `stringRedisTemplate.execute`这个方法，第二个参数是应该 List 集合，标识传入 Lua 脚本中的的 key，如果我们没有传 key，那么直接使用`Collections.emptyList()`，而不是直接使用`null`，是因为在 stringRedisTemplate.execute 方法内部可能对参数进行了处理，如果传递 null 可能引发 NPE 异常
5.  异步线程无法从 ThreadLocal 中获取 userId，我们需要从 voucherOrder 中获取 userId
6.  `AopContext.currentProxy()`底层也是利用 ThreadLocal 获取的，所以异步线程中也无法使用。解决方案有两种，第一种是将代理对象和订单一起放入阻塞队列中，第二种是将代理对象的作用域提升，变成一个成员变量（我采用了第二种方式）

1）编写 Lua 脚本：

```


local voucherId = ARGV[1];

local userId = ARGV[2];


local stockKey = 'seckill:stock:' .. voucherId;

local orderKey = 'seckill:order:' .. voucherId;


local stock = redis.call('GET', stockKey);
if (tonumber(stock) <= 0) then
    
    return 1;
end


if (redis.call('SISMEMBER', orderKey, userId) == 1) then
    
    return 2;
end


redis.call('INCRBY', stockKey, -1);
redis.call('SADD', orderKey, userId);

return 0;

```

2）在 VoucherOrderServiceImpl 中编写 Java 代码：

```
@Service
public class VoucherOrderServiceImpl extends ServiceImpl<VoucherOrderMapper, VoucherOrder> implements IVoucherOrderService {

    @Resource
    private ISeckillVoucherService seckillVoucherService;

    @Resource
    private RedisIdWorker redisIdWorker;

    @Resource
    private StringRedisTemplate stringRedisTemplate;

    @Resource
    private RedissonClient redissonClient;

    
    @PostConstruct
    private void init() {
        
        SECKILL_ORDER_EXECUTOR.submit(new VoucherOrderHandler());
    }

    
    private BlockingQueue<VoucherOrder> orderTasks = new ArrayBlockingQueue<>(1024 * 1024);

    
    private static final ExecutorService SECKILL_ORDER_EXECUTOR = Executors.newSingleThreadExecutor();

    
    private class VoucherOrderHandler implements Runnable {
        @Override
        public void run() {
            while (true) {
                
                try {
                    VoucherOrder voucherOrder = orderTasks.take();
                    handleVoucherOrder(voucherOrder);
                } catch (Exception e) {
                    log.error("处理订单异常", e);
                }
            }
        }
    }

    
    private void handleVoucherOrder(VoucherOrder voucherOrder) {
        Long userId = voucherOrder.getUserId();
        RLock lock = redissonClient.getLock(RedisConstants.LOCK_ORDER_KEY + userId);
        boolean isLock = lock.tryLock();
        if (!isLock) {
            
            log.error("一人只能下一单");
            return;
        }
        try {
            
            proxy.createVoucherOrder(voucherOrder);
        } finally {
            lock.unlock();
        }
    }

    
    private static final DefaultRedisScript<Long> SECKILL_SCRIPT;
    static {
        SECKILL_SCRIPT = new DefaultRedisScript<>();
        SECKILL_SCRIPT.setLocation(new ClassPathResource("lua/seckill.lua"));
        SECKILL_SCRIPT.setResultType(Long.class);
    }

    
    private IVoucherOrderService proxy;

    
    @Transactional
    @Override
    public Result seckillVoucher(Long voucherId) {
        
        Long result = null;
        try {
            result = stringRedisTemplate.execute(
                    SECKILL_SCRIPT,
                    Collections.emptyList(),
                    voucherId.toString(),
                    ThreadLocalUtls.getUser().getId().toString()
            );
        } catch (Exception e) {
            log.error("Lua脚本执行失败");
            throw new RuntimeException(e);
        }
        if (result != null && !result.equals(0L)) {
            
            int r = result.intValue();
            return Result.fail(r == 2 ? "不能重复下单" : "库存不足");
        }
        
        long orderId = redisIdWorker.nextId(SECKILL_VOUCHER_ORDER);
        
        VoucherOrder voucherOrder = new VoucherOrder();
        voucherOrder.setId(orderId);
        voucherOrder.setUserId(ThreadLocalUtls.getUser().getId());
        voucherOrder.setVoucherId(voucherId);
        
        orderTasks.add(voucherOrder);
        
        IVoucherOrderService proxy = (IVoucherOrderService) AopContext.currentProxy();
        this.proxy = proxy;
        return Result.ok();
    }

    
    @Transactional
    @Override
    public void createVoucherOrder(VoucherOrder voucherOrder) {
        Long userId = voucherOrder.getUserId();
        Long voucherId = voucherOrder.getVoucherId();
        
        int count = this.count(new LambdaQueryWrapper<VoucherOrder>()
                .eq(VoucherOrder::getUserId, userId));
        if (count >= 1) {
            
            log.error("当前用户不是第一单");
            return;
        }
        
        boolean flag = seckillVoucherService.update(new LambdaUpdateWrapper<SeckillVoucher>()
                .eq(SeckillVoucher::getVoucherId, voucherId)
                .gt(SeckillVoucher::getStock, 0)
                .setSql("stock = stock -1"));
        if (!flag) {
            throw new RuntimeException("秒杀券扣减失败");
        }
        
        flag = this.save(voucherOrder);
        if (!flag) {
            throw new RuntimeException("创建秒杀券订单失败");
        }
    }
}

```

###### 测试

**测试一**：测试同一用户在高并发场景下发送大量请求，是否发生超卖问题，或者其它线程安全问题

![](<assets/1740497755492.png>)

**qps200**，**吞吐量 1242**，**异常率 96%**，可以发现成功请求数量为 200 ∗ 4 200*4%=8 200∗4，居然有八个请求同时成功！那是不是又发生了超卖问题 w(ﾟДﾟ)w

相较于前面同步逻辑，我们添加了异步机制之后，qps 由原来的

我们看看数据库和缓存中库存的情况：

![](<assets/1740497755743.png>)

可以看到虽然请求有 8 次成功，但是库存只减少了 1，并且数据库和缓存中订单也只创建了一个。那么为什么会出现问题呢？原因很简单，请求成功不以为这下单成功。对于大量请求，SpringBoot 无法同时处理大量请求，Jemter 的请求只具有短期有效，SpringBoot 在处理某一个请求时，其它请求由于超时直接就返回失败了，我们也可以看到哪些成功请求具有一定的规律性，基本上是每隔固定的失败请求就会有一个成功请求，只有第一个成功请求，返回是 success ，后面虽然请求成功了，但是返回的是 不能重复下单 的提示信息

![](<assets/1740497756053.png>)

**测试二**：测试不同用户在高并发场景下发送大量请求，是否发生超卖问题，或者其它线程安全问题

PS：关于 Jmeter 的 Tokens 文件获取，可以参考这篇文章[自动化完成 1000 个用户的登录并获取 token 并生成 tokens.txt 文件](https://blog.csdn.net/qq_66345100/article/details/128913949)

![](<assets/1740497756372.png>)

生成 1000 个用户的 token 文件后，直接导入到 Jmeter，然后进行压测，相当于 1000 个用户同时对系统进行请求

![](<assets/1740497756646.png>)

可以看到 **qps1000**、**吞吐量 455.6**、**异常率 0%**，可以看出当前系统的这一个秒杀功能已经达到了企业可用级别了😄

对比[优化前](#tongbu)，平均值由 1921 **降低至 930**，吞吐量由 315.3 **提高至 445.6**，异常率由 37.2% **降低至 0%**

**PS**：这个性能看起来比较差，这是因为我当前电脑是轻薄本，本身性能不太好，并且同时开启了两个虚拟机、QQ、微信、两个浏览器、一个 Jmeter、Navicat、一个 IDEA、一个 ApiPost、FinalShell、RESP、有道翻译等等（老师的吞吐量直接 1300😫）快去测测你的并发性能吧 (●’◡’●)

再去看看数据库和 Redis 中是否发生了超卖问题：

![](<assets/1740497757156.png>)

##### 消息队列优化

###### 分析

前面我们使用 Java 自带的阻塞队列 BlockingQueue 实现消息队列，这种方式存在以下几个严重的弊端：

1.  **信息可靠性没有保障**，BlockingQueue 的消息是存储在内存中的，无法进行持久化，一旦程序宕机或者发生异常，会直接导致消息丢失
2.  **消息容量有限**，BlockingQueue 的容量有限，无法进行有效扩容，一旦达到最大容量限制，就会抛出 OOM 异常

所以这里我们可以选择采用其它成熟的的（和之前分布式锁一样）MQ，比如：RabbitMQ、RocketMQ、Kafka 等，但是本项目是为了学习 Redis 而设计的，所以这里我们将要学习如何使用 Redis 实现一个相对可靠的消息队列（自己实现的肯定没法和别人成熟的产品相比）

PS：关于消息队列可以参考这篇文章 [RabbitMQ 入门学习笔记](https://blog.csdn.net/qq_66345100/article/details/131317096?spm=1001.2014.3001.5502)

那么我们该如何实现呢？首先我们需要了解 MQ 的特点（这里不再赘述了，MQ 详情看上面那篇文章），根据 MQ 的特点选取相对应的数据结构，Redis 中能够实现 MQ 效果的主要由以下三种方式：

![](<assets/1740497757498.png>)

*   **`list`结构**：基于 List 结构模拟消息队列（`BRPOP+BLPOP`实现阻塞队列）
    
    *   **生产消息**：`BRPUSH key value [value ...]` 将一个或多个元素推入到指定列表的头部。如果列表不存在，BRPUSH 命令会自动创建一个新的列表
        
    *   **消费消息**：`BRPOP key [key ...] timeout` 从指定的一个或多个列表中弹出最后一个元素。如果 list 列表为空，BRPOP 命令会导致客户端阻塞，直到有数据可用或超过指定的超时时间
        
    
    **优点**：不会内存超限、可以持久化、消息有序性；**缺点**：无法避免数据丢失、只支持单消费者
    
*   **`pubsub`**：发布订阅模式，基本的点对点消息模型（redis2.0 引入）
    
    *   **生产消息**：
        
        ```
        PUBLISH channel message 
        
        ```
        
    *   **消费消息**：
        
        ```
        SUBSCRIBE channel [channel]
        
        UNSUBSCRIBE [channel [channel ...]]
        
        PSUBSCRIBE pattern [pattern ...]
        
        PUNSUBSCRIBE [pattern [pattern ...]]
        
        ```
        
    
    **优点**：支持多生产、多消费者；**缺点**：不支持持久化、无法比避免数据丢失，消息堆积有上限（消费者会缓存消息），超出会丢失消息
    
*   **`stream`**：比较完善的消息队列模型（redis5.0 引入，我的是 redis6.0😄）
    
    stream 是一种数据类型，专门为消息队列设计的，相较于前面两种方式能够更加完美实现一个消息队列
    
    *   **生产消息**：用于向指定的 Stream 流中添加一个消息
        
        ```
        XADD key *|ID value [value ...]
        
        
        127.0.0.1:6379> XADD users * name jack age 21
        "1644805700523-0"
        
        ```
        
        key 就是消息队列，key 不存 (*) 在会自动创建（默认），ID 是消息表示，value 是消息的内容
        
    *   **消费消息**：
        
        ```
        XREAD [COUNT count] [BLOCK milliseconds] STREAMS key [key ...] ID ID
        
        
        XREAD COUNT 1 STREAMS users 0
        
        XREAD COUNT 1 BLOCK 1000 STREAMS users $
        
        ```
        
        注意：当我们指定起始 ID 为`$`时代表读取最后一条消息（读取最新的消息）ID 为`0`时代表读最开始的一条消息（读取最旧的消息），如果我们处理一条消息的过程中，又有超过 1 条以上的消息到达队列，则下次获取时也只能获取到最新的一条，会出现漏读消息的问题
        
    
    **优点**：消息可回溯、一个消息可以被多个消费者消费、可以阻塞读取；**缺点**：有消息漏读的风险
    

上面我们介绍的消费方式都是**单消费方式**，容易发生消息堆积导致消息丢失，所以我们需要改用消费者组的模式

*   **消费者组**（Consumer Group）：将多个消息划分到一个组中，监听同一队列
    
*   **消费者组的特点**：
    
    *   **消息分流**：队列中的消息会分流给组内的不同消费者，而不是重复消费，从而加快消息处理的速度
    *   **消息标识**：消费者组会维护一个标示，记录最后一个被处理的消息，哪怕消费者宕机重启，还会从标示之后读取消息。确保每一个消息都会被消费
    *   **消息确认**：消费者获取消息后，消息处于`pending`（待处理）状态，并存入一个`pending-list`。当处理完成后需要通过`XACK`来确认消息，标记消息为已处理，才会从 pending-list 移除。

```
XGROUP CREATE key groupName ID

XGROUP DESTORY key groupName

XGROUP CREATECONSUMER key groupName consumerName

XGROUP DELCONSUMER key groupName consumerName

XREADGROUP GROUP

```

![](<assets/1740497757849.png>)

![](<assets/1740497758261.png>)

`stream`类型消息队列的`XREADGROUP`命令特点：

*   消息可回溯
*   可以多消费者争抢消息，加快消费速度
*   可以阻塞读取
*   没有消息漏读的风险
*   有消息确认机制，保证消息至少被消费一次

###### 实现

![](<assets/1740497758571.png>)

1）创建队列。在 Redis CLI 中执行下面指令

```
XGROUP CREATE stream.orders g1 0 MKSTREAM

```

我的建议是直接创建一个任务类，在 Java 代码中实现创建队列

```
    
    @PostConstruct
    private void init() {
        
        DefaultRedisScript<Long> mqScript = new DefaultRedisScript<>();
        mqScript.setLocation(new ClassPathResource("lua/stream-mq.lua"));
        mqScript.setResultType(Long.class);
        Long result = null;
        try {
            result = stringRedisTemplate.execute(mqScript,
                    Collections.emptyList(),
                    QUEUE_NAME,
                    GROUP_NAME);
        } catch (Exception e) {
            log.error("队列创建失败", e);
            return;
        }
        int r = result.intValue();
        String info = r == 1 ? "队列创建成功" : "队列已存在";
        log.debug(info);
        
        SECKILL_ORDER_EXECUTOR.submit(new VoucherOrderHandler());
    }

```

**备注**：当时我还想着使用`CommandLineRunner`来初始化队列，但是发现`CommandLineRunner`的初始化时机在`@PostConstruct`之后，这就导致队列还未初始化，就已经执行线程任务获取队列中的消息，从而报错，所以干脆直接放到执行线程之前

2）在 VoucherOrderServiceImpl 中编写 Java 代码：

```
@Service
public class VoucherOrderServiceImpl extends ServiceImpl<VoucherOrderMapper, VoucherOrder> implements IVoucherOrderService {

    @Resource
    private ISeckillVoucherService seckillVoucherService;

    @Resource
    private RedisIdWorker redisIdWorker;

    @Resource
    private StringRedisTemplate stringRedisTemplate;

    @Resource
    private RedissonClient redissonClient;

    
    @PostConstruct
    private void init() {
        
        SECKILL_ORDER_EXECUTOR.submit(new VoucherOrderHandler());
    }

    
    private static final ExecutorService SECKILL_ORDER_EXECUTOR = Executors.newSingleThreadExecutor();

    
    private static final String queueName = "stream.orders";

    
    private class VoucherOrderHandler implements Runnable {
        @Override
        public void run() {
            while (true) {
                try {
                    
                    List<MapRecord<String, Object, Object>> messageList = stringRedisTemplate.opsForStream().read(
                            Consumer.from("g1", "c1"),
                            StreamReadOptions.empty().count(1).block(Duration.ofSeconds(1)),
                            StreamOffset.create(queueName, ReadOffset.lastConsumed())
                    );
                    
                    if (messageList == null || messageList.isEmpty()) {
                        
                        continue;
                    }
                    
                    
                    MapRecord<String, Object, Object> record = messageList.get(0);
                    Map<Object, Object> messageMap = record.getValue();
                    VoucherOrder voucherOrder = BeanUtil.fillBeanWithMap(messageMap, new VoucherOrder(), true);
                    handleVoucherOrder(voucherOrder);
                    
                    stringRedisTemplate.opsForStream().acknowledge(queueName, "g1", record.getId());
                } catch (Exception e) {
                    log.error("处理订单异常", e);
                    
                    handlePendingList();
                }
            }
        }
    }

    private void handlePendingList() {
        while (true) {
            try {
                
                List<MapRecord<String, Object, Object>> messageList = stringRedisTemplate.opsForStream().read(
                        Consumer.from("g1", "c1"),
                        StreamReadOptions.empty().count(1).block(Duration.ofSeconds(1)),
                        StreamOffset.create(queueName, ReadOffset.from("0"))
                );
                
                if (messageList == null || messageList.isEmpty()) {
                    
                    break;
                }
                
                
                MapRecord<String, Object, Object> record = messageList.get(0);
                Map<Object, Object> messageMap = record.getValue();
                VoucherOrder voucherOrder = BeanUtil.fillBeanWithMap(messageMap, new VoucherOrder(), true);
                handleVoucherOrder(voucherOrder);
                
                stringRedisTemplate.opsForStream().acknowledge(queueName, "g1", record.getId());
            } catch (Exception e) {
                log.error("处理订单异常", e);
                
                try {
                    Thread.sleep(20);
                } catch (InterruptedException ex) {
                    log.error("线程休眠异常", ex);
                }
            }
        }
    }

    
    private void handleVoucherOrder(VoucherOrder voucherOrder) {
        Long userId = voucherOrder.getUserId();
        RLock lock = redissonClient.getLock(RedisConstants.LOCK_ORDER_KEY + userId);
        boolean isLock = lock.tryLock();
        if (!isLock) {
            
            log.error("一人只能下一单");
            return;
        }
        try {
            
            proxy.createVoucherOrder(voucherOrder);
        } finally {
            lock.unlock();
        }
    }

    
    private static final DefaultRedisScript<Long> SECKILL_SCRIPT;

    static {
        SECKILL_SCRIPT = new DefaultRedisScript<>();
        SECKILL_SCRIPT.setLocation(new ClassPathResource("lua/stream-seckill.lua"));
        SECKILL_SCRIPT.setResultType(Long.class);
    }

    
    private IVoucherOrderService proxy;

    
    @Transactional
    @Override
    public Result seckillVoucher(Long voucherId) {
        Long userId = ThreadLocalUtls.getUser().getId();
        long orderId = redisIdWorker.nextId(SECKILL_VOUCHER_ORDER);

        
        Long result = null;
        try {
            result = stringRedisTemplate.execute(
                    SECKILL_SCRIPT,
                    Collections.emptyList(),
                    voucherId.toString(),
                    userId.toString(),
                    String.valueOf(orderId)
            );
        } catch (Exception e) {
            log.error("Lua脚本执行失败");
            throw new RuntimeException(e);
        }
        if (result != null && !result.equals(0L)) {
            
            int r = result.intValue();
            return Result.fail(r == 2 ? "不能重复下单" : "库存不足");
        }

        
        
        IVoucherOrderService proxy = (IVoucherOrderService) AopContext.currentProxy();
        this.proxy = proxy;
        return Result.ok();
    }

    
    @Transactional
    @Override
    public void createVoucherOrder(VoucherOrder voucherOrder) {
        Long userId = voucherOrder.getUserId();
        Long voucherId = voucherOrder.getVoucherId();
        
        int count = this.count(new LambdaQueryWrapper<VoucherOrder>()
                .eq(VoucherOrder::getUserId, userId));
        if (count >= 1) {
            
            log.error("当前用户不是第一单");
            return;
        }
        
        boolean flag = seckillVoucherService.update(new LambdaUpdateWrapper<SeckillVoucher>()
                .eq(SeckillVoucher::getVoucherId, voucherId)
                .gt(SeckillVoucher::getStock, 0)
                .setSql("stock = stock -1"));
        if (!flag) {
            throw new RuntimeException("秒杀券扣减失败");
        }
        
        flag = this.save(voucherOrder);
        if (!flag) {
            throw new RuntimeException("创建秒杀券订单失败");
        }
    }
}

```

###### 测试

和前面的测试一样，测试 1000 个人在一秒内同时下单，观察是否发生超卖，并且记录系统性能

![](<assets/1740497758891.png>)

可以看到数据库和缓存都没有发生超卖问题 (●ˇ∀ˇ●)

![](<assets/1740497759111.png>)

**qps1000**、**平均值 889**、**异常率 0%**、**吞吐量 474.2**，可以看到`stream`实现的消息队列并没有比 Java 的`BlockingQueue`实现的消息队列高多少性能，但是 stream 实现的信息队列比 BlockingQueue 实现的消息队列的**可靠性**和**灵活性**要高一大截

期间遇到 [bug3](#3)、[bug4](#4)

### 达人探店

#### 发布探店笔记

这个直接已经实现了，不用作任何修改即可使用，并且没用使用到 Redis

![](<assets/1740497759329.png>)

**温馨提示**：记得修改图片上传地址，建议直接放到你的 Ngixn 下的 imgs 目录

![](<assets/1740497759696.png>)

#### 查看探店笔记

这个功能和发布博客功能一样，也是一个非重点功能，没有使用到 Redis

![](<assets/1740497759971.png>)

在编写查询博客详情接口的同时，重构了一下查询热点博客的接口

```
    
    @Override
    public Result queryBlogById(Long id) {
        
        Blog blog = this.getById(id);
        if (Objects.isNull(blog)){
            return Result.fail("笔记不存在");
        }
        
        queryUserByBlog(blog);
        return Result.ok(blog);
    }

    
    @Override
    public Result queryHotBlog(Integer current) {
        
        Page<Blog> page = this.query()
                .orderByDesc("liked")
                .page(new Page<>(current, SystemConstants.MAX_PAGE_SIZE));
        
        List<Blog> records = page.getRecords();
        
        records.forEach(this::queryUserByBlog);
        return Result.ok(records);
    }

    
    private void queryUserByBlog(Blog blog) {
        Long userId = blog.getUserId();
        User user = userService.getById(userId);
        blog.setName(user.getNickName());
        blog.setIcon(user.getIcon());
    }

```

#### Set 实现点赞功能

![](<assets/1740497760427.png>)

现在存在一个问题，一个用户可以无限点赞，这显然是不合理的，所以我们需要对点赞功能进行一个优化，实现一人只能点赞一次。

对于点赞这种高频变化的数据，如果我们使用 MySQL 是十分不理智的，因为 MySQL 慢、并且并发请求 MySQL 会影响其它重要业务，容易影响整个系统的性能，继而降低了用户体验。那么如何我们要使用 Redis，那么我们又该选择哪种数据结构才更加合理呢？

这里我推荐使用`Set`，因为 Set 类型的数据结构具有

1.  **不重复**，符合业务的特点，一个用户只能点赞一次
2.  **高性能**，Set 集合内部实现了高效的数据结构 (Hash 表)
3.  **灵活性**，Set 集合可以实现一对多，一个用户可以点赞多个博客，符合实际的业务逻辑

当然也可以选择使用`Hash`（Hash 占用空间比 Set 更小），如果想要点赞排序也可以选用`Sorted Set`

![](<assets/1740497760707.png>)

```
@Service
public class BlogServiceImpl extends ServiceImpl<BlogMapper, Blog> implements IBlogService {

    @Resource
    private IUserService userService;

    @Resource
    private StringRedisTemplate stringRedisTemplate;

    
    @Override
    public Result queryBlogById(Long id) {
        
        Blog blog = this.getById(id);
        if (Objects.isNull(blog)) {
            return Result.fail("笔记不存在");
        }
        
        queryUserByBlog(blog);
        
        isBlogLiked(blog);
        return Result.ok(blog);
    }

    
    private void isBlogLiked(Blog blog) {
        Long userId = ThreadLocalUtls.getUser().getId();
        String key = BLOG_LIKED_KEY + blog.getId();
        Boolean isMember = stringRedisTemplate.opsForSet().isMember(key, userId.toString());
        blog.setIsLike(BooleanUtil.isTrue(isMember));
    }

    
    @Override
    public Result queryHotBlog(Integer current) {
        
        Page<Blog> page = this.query()
                .orderByDesc("liked")
                .page(new Page<>(current, SystemConstants.MAX_PAGE_SIZE));
        
        List<Blog> records = page.getRecords();
        
        records.forEach(blog -> {
            this.queryUserByBlog(blog);
            this.isBlogLiked(blog);
        });
        return Result.ok(records);
    }

    
    @Override
    public Result likeBlog(Long id) {
        
        Long userId = ThreadLocalUtls.getUser().getId();
        String key = BLOG_LIKED_KEY + blog.getId();
        
        Boolean isMember = stringRedisTemplate.opsForSet().isMember(key, userId.toString());
        boolean result;
        if (BooleanUtil.isFalse(isMember)) {
            
            result = this.update(new LambdaUpdateWrapper<Blog>()
                    .eq(Blog::getId, id)
                    .setSql("liked = liked + 1"));
            if (result) {
                
                stringRedisTemplate.opsForSet().add(key, userId.toString());
            }
        } else {
            
            result = this.update(new LambdaUpdateWrapper<Blog>()
                    .eq(Blog::getId, id)
                    .setSql("liked = liked - 1"));
            if (result) {
                
                stringRedisTemplate.opsForSet().remove(key, userId.toString());
            }
        }
        return Result.ok();
    }

    
    private void queryUserByBlog(Blog blog) {
        Long userId = blog.getUserId();
        User user = userService.getById(userId);
        blog.setName(user.getNickName());
        blog.setIcon(user.getIcon());
    }
}

```

![](<assets/1740497760998.png>)

#### SortedSet 实现点赞排行榜

![](<assets/1740497761299.png>)

![](<assets/1740497761540.png>)

在平常我们所使用的软件中（比如微信、QQ、抖音）的点赞功能都会默认按照时间顺序对点赞的用户进行一个排序，后点赞的用户会排在最前面，而`Set`是无需的，无法满足这个需求，虽然 `List`有序，但是不唯一，查找效率也比较低，所以也不推荐使用，此时我们就可以选择使用`SortedSet`这个数据结构，它完美的满足了我们所有的需求：**唯一**、**有序**、**查找效率高**。

相较于 Set 集合，SortedList 有以下不同之处：

1.  对于 Set 集合我们可以使用 `isMember`方法判断用户是否存在，对于 SortedList 我们可以使用`ZSCORE`方法判断用户是否存在
2.  Set 集合没有提供范围查询，无法获排行榜前几名的数据，SortedList 可以使用`ZRANGE`方法实现范围查询

```
@Service
public class BlogServiceImpl extends ServiceImpl<BlogMapper, Blog> implements IBlogService {

    @Resource
    private IUserService userService;

    @Resource
    private StringRedisTemplate stringRedisTemplate;

    
    @Override
    public Result queryBlogById(Long id) {
        
        Blog blog = this.getById(id);
        if (Objects.isNull(blog)) {
            return Result.fail("笔记不存在");
        }
        
        queryUserByBlog(blog);
        
        isBlogLiked(blog);
        return Result.ok(blog);
    }

    
    private void isBlogLiked(Blog blog) {
        UserDTO user = ThreadLocalUtls.getUser();
        if (Objects.isNull(user)){
            
            return;
        }
        Long userId = user.getId();
        String key = BLOG_LIKED_KEY + blog.getId();
        Double score = stringRedisTemplate.opsForZSet().score(key, userId.toString());
        blog.setIsLike(Objects.nonNull(score));
    }

    
    @Override
    public Result queryHotBlog(Integer current) {
        
        Page<Blog> page = this.query()
                .orderByDesc("liked")
                .page(new Page<>(current, SystemConstants.MAX_PAGE_SIZE));
        
        List<Blog> records = page.getRecords();
        
        records.forEach(blog -> {
            this.queryUserByBlog(blog);
            this.isBlogLiked(blog);
        });
        return Result.ok(records);
    }

    
    @Override
    public Result likeBlog(Long id) {
        
        Long userId = ThreadLocalUtls.getUser().getId();
        String key = BLOG_LIKED_KEY + id;
        
        Double score = stringRedisTemplate.opsForZSet().score(key, userId.toString());
        boolean result;
        if (score == null) {
            
            result = this.update(new LambdaUpdateWrapper<Blog>()
                    .eq(Blog::getId, id)
                    .setSql("liked = liked + 1"));
            if (result) {
                
                stringRedisTemplate.opsForZSet().add(key, userId.toString(), System.currentTimeMillis());
            }
        } else {
            
            result = this.update(new LambdaUpdateWrapper<Blog>()
                    .eq(Blog::getId, id)
                    .setSql("liked = liked - 1"));
            if (result) {
                
                stringRedisTemplate.opsForZSet().remove(key, userId.toString());
            }
        }
        return Result.ok();
    }

    
    @Override
    public Result queryBlogLikes(Long id) {
        
        Long userId = ThreadLocalUtls.getUser().getId();
        String key = BLOG_LIKED_KEY + id;
        Set<String> top5 = stringRedisTemplate.opsForZSet().range(key, 0, 4);
        if (top5 == null || top5.isEmpty()) {
            return Result.ok(Collections.emptyList());
        }
        List<Long> ids = top5.stream().map(Long::valueOf).collect(Collectors.toList());
        List<UserDTO> userDTOList = userService.listByIds(ids).stream()
                .map(user -> BeanUtil.copyProperties(user, UserDTO.class))
                .collect(Collectors.toList());
        return Result.ok(userDTOList);
    }

    
    private void queryUserByBlog(Blog blog) {
        Long userId = blog.getUserId();
        User user = userService.getById(userId);
        blog.setName(user.getNickName());
        blog.setIcon(user.getIcon());
    }
}

```

![](<assets/1740497761805.png>)

如果我们的需求是，先点赞的排在前面，后点赞的排在后面该如何实现？这就需要涉及到 MySQL 的一些相关知识了，在 MySQL 中如果我们使用`in`进行条件查询，我们的查询默认是数据库顺序查询，数据库中的记录默认都是按照 ID 自增的，所以查出来的结果默认是按照 ID 自增排序的

![](<assets/1740497762153.png>)

解决方法：

```
select id, phone,password,nick_name,icon,create_time,update_time
from tb_user
where id in(1, 5)
order by field(id, 5, 1)

```

注意：根据某一个字段进行排序，oder by 字段的 id 顺序即为查询的顺序

![](<assets/1740497762382.png>)

```
    
    @Override
    public Result queryBlogLikes(Long id) {
        
        Long userId = ThreadLocalUtls.getUser().getId();
        String key = BLOG_LIKED_KEY + id;
        Set<String> top5 = stringRedisTemplate.opsForZSet().range(key, 0, 4);
        if (top5 == null || top5.isEmpty()) {
            return Result.ok(Collections.emptyList());
        }
        List<Long> ids = top5.stream().map(Long::valueOf).collect(Collectors.toList());
        String idStr = StrUtil.join(",", ids);
        
        List<UserDTO> userDTOList = userService.list(new LambdaQueryWrapper<User>()
                        .in(User::getId, ids)
                        .last("order by field (id," + idStr + ")"))
                .stream()
                .map(user -> BeanUtil.copyProperties(user, UserDTO.class))
                .collect(Collectors.toList());
        return Result.ok(userDTOList);
    }

```

![](<assets/1740497762659.png>)

### 好友关注

#### 关注和取关

![](<assets/1740497762973.png>)

![](<assets/1740497763364.png>)

```
@RestController
@RequestMapping("/follow")
public class FollowController {

    @Resource
    private IFollowService followService;

    
    @PutMapping("/{id}/{isFollow}")
    public Result follow(@PathVariable("id") Long followUserId, @PathVariable Boolean isFollow){
        return followService.follow(followUserId, isFollow);
    }

    
    @GetMapping("/or/not/{id}")
    public Result isFollow(@PathVariable("id") Long followUserId){
        return followService.isFollow(followUserId);
    }
}

```

```
@Service
public class FollowServiceImpl extends ServiceImpl<FollowMapper, Follow> implements IFollowService {

    
    @Override
    public Result follow(Long followUserId, Boolean isFollow) {
        Long userId = ThreadLocalUtls.getUser().getId();
        if (isFollow) {
            
            Follow follow = new Follow();
            follow.setUserId(userId);
            follow.setFollowUserId(followUserId);
            this.save(follow);
        } else {
            
            this.remove(new LambdaQueryWrapper<Follow>()
                    .eq(Follow::getUserId, userId)
                    .eq(Follow::getFollowUserId, followUserId));
        }
        return Result.ok();
    }

    
    @Override
    public Result isFollow(Long followUserId) {
        Long userId = ThreadLocalUtls.getUser().getId();
        int count = this.count(new LambdaQueryWrapper<Follow>()
                .eq(Follow::getUserId, userId)
                .eq(Follow::getFollowUserId, followUserId));
        return Result.ok(count > 0);
    }
}

```

![](<assets/1740497763681.png>)

#### Set 实现共同关注

![](<assets/1740497764008.png>)

![](<assets/1740497764306.png>)

我们想要查询出两个用户的共同关注对象，这就需要使用求交集，对于求交集，我们可以使用 Set 集合

```
@Service
public class FollowServiceImpl extends ServiceImpl<FollowMapper, Follow> implements IFollowService {

    @Resource
    private StringRedisTemplate stringRedisTemplate;

    @Resource
    private IUserService userService;

    
    @Override
    public Result follow(Long followUserId, Boolean isFollow) {
        Long userId = ThreadLocalUtls.getUser().getId();
        String key = FOLLOW_KEY + userId;
        if (isFollow) {
            
            Follow follow = new Follow();
            follow.setUserId(userId);
            follow.setFollowUserId(followUserId);
            boolean isSuccess = this.save(follow);
            if (isSuccess) {
                
                stringRedisTemplate.opsForSet().add(key, followUserId.toString());
            }
        } else {
            
            boolean isSuccess = this.remove(new LambdaQueryWrapper<Follow>()
                    .eq(Follow::getUserId, userId)
                    .eq(Follow::getFollowUserId, followUserId));
            if (isSuccess) {
                stringRedisTemplate.opsForSet().remove(key, followUserId.toString());
            }
        }
        return Result.ok();
    }

    
    @Override
    public Result isFollow(Long followUserId) {
        Long userId = ThreadLocalUtls.getUser().getId();
        int count = this.count(new LambdaQueryWrapper<Follow>()
                .eq(Follow::getUserId, userId)
                .eq(Follow::getFollowUserId, followUserId));
        return Result.ok(count > 0);
    }

    
    @Override
    public Result followCommons(Long id) {
        Long userId = ThreadLocalUtls.getUser().getId();
        String key1 = FOLLOW_KEY + userId;
        String key2 = FOLLOW_KEY + id;
        
        Set<String> intersect = stringRedisTemplate.opsForSet().intersect(key1, key2);
        if (Objects.isNull(intersect) || intersect.isEmpty()) {
            return Result.ok(Collections.emptyList());
        }
        List<Long> ids = intersect.stream().map(Long::valueOf).collect(Collectors.toList());
        
        List<UserDTO> userDTOList = userService.listByIds(ids).stream()
                .map(user -> BeanUtil.copyProperties(user, UserDTO.class))
                .collect(Collectors.toList());
        return Result.ok(userDTOList);
    }
}

```

![](<assets/1740497764623.png>)

![](<assets/1740497764960.png>)

遇到 [bug5](#5)

#### Feed 流关注推送

##### 分析

*   **什么是 Feed 流**？
    
    关注推送也叫做 Feed 流，直译为投喂。为用户持续的提供 “沉浸式” 的体验，通过无限下拉刷新获取新的信息。Feed 流是一种基于用户个性化需求和兴趣的信息流推送方式，常见于社交媒体、新闻应用、音乐应用等互联网平台。Feed 流通过算法和用户行为数据分析，动态地将用户感兴趣的内容以流式方式呈现在用户的界面上。
    
    ![](<assets/1740497765257.png>)
    
*   **Feed 流产品有两种常见模式**：
    
    *   **时间排序**（`Timeline`）：不做内容筛选，简单的按照内容发布时间排序，常用于好友或关注。例如朋友圈
        
        *   优点：信息全面，不会有缺失。并且实现也相对简单
            
        *   缺点：信息噪音较多，用户不一定感兴趣，内容获取效率低
            
    *   **智能排序**：利用智能算法屏蔽掉违规的、用户不感兴趣的内容。推送用户感兴趣信息来吸引用户
        
        *   优点：投喂用户感兴趣信息，用户粘度很高，容易沉迷
            
        *   缺点：如果算法不精准，可能起到反作用
            
    
    本例中的个人页面，是基于关注的好友来做 Feed 流，因此采用`Timeline`的模式。该模式的实现方案有三种：
    
    ![](<assets/1740497765571.png>)
    
    1.  **拉模式**：也叫做**读扩散**。在拉模式中，终端用户或应用程序主动发送请求来获取最新的数据流。它是一种按需获取数据的方式，用户可以在需要时发出请求来获取新数据。在 Feed 流中，数据提供方将数据发布到实时数据源中，而终端用户或应用程序通过订阅或请求来获取新数据。
        
        **优点**：节约空间，可以减少不必要的数据传输，只需要获取自己感兴趣的数据，因为赵六在读信息时，并没有重复读取，而且读取完之后可以把他的收件箱进行清楚。
        
        **缺点**：延迟较高，当用户读取数据时才去关注的人里边去读取数据，假设用户关注了大量的用户，那么此时就会拉取海量的内容，对服务器压力巨大。
        
        ![](<assets/1740497765871.png>)
        
    2.  **推模式**：也叫做**写扩散**。在推模式中，数据提供方主动将最新的数据推送给终端用户或应用程序。数据提供方会实时地将数据推送到终端用户或应用程序，而无需等待请求。
        
        **优点**：数据延迟低，不用临时拉取
        
        **缺点**：内存耗费大，假设一个大 V 写信息，很多人关注他， 就会写很多份数据到粉丝那边去
        
        ![](<assets/1740497766164.png>)
        
    3.  **推拉结合**：也叫做**读写混合**，兼具推和拉两种模式的优点。在推拉结合模式中，数据提供方会主动将最新的数据推送给终端用户或应用程序，同时也支持用户通过拉取的方式来获取数据。这样可以实现实时的数据更新，并且用户也具有按需获取数据的能力。推拉模式是一个折中的方案，站在发件人这一段，如果是个普通的人，那么我们采用写扩散的方式，直接把数据写入到他的粉丝中去，因为普通的人他的粉丝关注量比较小，所以这样做没有压力，如果是大 V，那么他是直接将数据先写入到一份到发件箱里边去，然后再直接写一份到活跃粉丝收件箱里边去，现在站在收件人这端来看，如果是活跃粉丝，那么大 V 和普通的人发的都会直接写入到自己收件箱里边来，而如果是普通的粉丝，由于他们上线不是很频繁，所以等他们上线时，再从发件箱里边去拉信息
        
        ![](<assets/1740497766455.png>)
        

##### 实现

1）基于模式实现关注推送功能

![](<assets/1740497766786.png>)

当前项目用户量比较小，所以这里我们选择使用推模式，延迟低、内存占比也没那么大

由于我们需要实现分页查询功能，这里我们可以选择 `list` 或者 `SortedSet`，而不能使用`Set`，因为`Set`是无需的， `list`是有索引的，`SortedSet` 是有序的，那么我们该如何选择呢？

如果我们选择 `list` 会存在索引漂移现象（这个在 Vue 中也存在），从而导致读取重复数据，所以我们不能选择使用 list

![](<assets/1740497767066.png>)

我们可以选择使用**滚动分页**，我们使用`SortedSet`，如果使用排名和使用角标是一样的，但是`SortedSet`可以按照`Score`排序（Score 默认按照时间戳生成，所以是固定的），每次我们可以选择比之前 Score 较小的，这样就能够实现滚动排序，从而防止出现问题

![](<assets/1740497767361.png>)

**代码实现**：在 BlogServiceImpl 中修改原有的保存探店笔记的方法：

```
    
    @Override
    public Result saveBlog(Blog blog) {
        Long userId = ThreadLocalUtls.getUser().getId();
        blog.setUserId(userId);
        
        boolean isSuccess = this.save(blog);
        if (!isSuccess){
            return Result.fail("笔记保存失败");
        }
        
        List<Follow> follows = followService.list(new LambdaQueryWrapper<Follow>()
                .eq(Follow::getFollowUserId, userId));
        
        for (Follow follow : follows) {
            
            Long id = follow.getUserId();
            
            String key = FEED_KEY + id;
            stringRedisTemplate.opsForZSet().add(key, blog.getId().toString(), System.currentTimeMillis());
        }
        return Result.ok(blog.getId());
    }

```

2）实现关注推送页面的分页查询

![](<assets/1740497767755.png>)

**代码实现**：

```
    
    @Override
    public Result queryBlogOfFollow(Long max, Integer offset) {
        
        Long userId = ThreadLocalUtls.getUser().getId();
        String key = FEED_KEY + userId;
        
        Set<ZSetOperations.TypedTuple<String>> typedTuples = stringRedisTemplate.opsForZSet()
                .reverseRangeByScoreWithScores(key, 0, max, offset, 2);
        
        if (typedTuples == null || typedTuples.isEmpty()) {
            return Result.ok();
        }

        
        List<Long> ids = new ArrayList<>(typedTuples.size());
        long minTime = 0; 
        int os = 1; 
        for (ZSetOperations.TypedTuple<String> tuple : typedTuples) { 
            
            ids.add(Long.valueOf(tuple.getValue()));
            
            long time = tuple.getScore().longValue();
            if (time == minTime) {
                
                os++;
            } else {
                
                minTime = time;
                os = 1;
            }
        }

        
        String idStr = StrUtil.join(",", ids);
        List<Blog> blogs = this.list(new LambdaQueryWrapper<Blog>().in(Blog::getId, ids)
                .last("ORDER BY FIELD(id," + idStr + ")"));
        
        for (Blog blog : blogs) {
            
            queryUserByBlog(blog);
            
            isBlogLiked(blog);
        }

        
        ScrollResult scrollResult = new ScrollResult();
        scrollResult.setList(blogs);
        scrollResult.setOffset(os);
        scrollResult.setMinTime(minTime);

        return Result.ok(scrollResult);
    }

```

![](<assets/1740497768072.png>)

![](<assets/1740497768537.png>)

### 附近商铺搜索

#### GEO 数据结构

GEO 就是 Geolocation 的简写形式，代表地理坐标。Redis 在 3.2 版本中加入了对 GEO 的支持，允许存储地理坐标信息，帮助我们根据经纬度来检索数据。常见的命令有：

*   [`GEOADD`](https://redis.io/commands/geoadd)：添加一个地理空间信息，包含：经度（longitude）、纬度（latitude）、值（member）
    
*   [`GEODIST`](https://redis.io/commands/geodist)：计算指定的两个点之间的距离并返回
    
*   [`GEOHASH`](https://redis.io/commands/geohash)：将指定 member 的坐标转为 hash 字符串形式并返回
    
*   [`GEOPOS`](https://redis.io/commands/geopos)：返回指定 member 的坐标
    
*   [`GEORADIUS`](https://redis.io/commands/georadius)：指定圆心、半径，找到该圆内包含的所有 member，并按照与圆心之间的距离排序后返回。6.2 以后已废弃
    
*   [`GEOSEARCH`](https://redis.io/commands/geosearch)：在指定范围内搜索 member，并按照与指定点之间的距离排序后返回。范围可以是圆形或矩形。6.2. 新功能
    
*   [`GEOSEARCHSTORE`](https://redis.io/commands/geosearchstore)：与 GEOSEARCH 功能一致，不过可以把结果存储到一个指定的 key。 6.2. 新功能
    

**练习**

![](<assets/1740497768740.png>)

```
GEOADD g1 116.378248 39.865275 bjnz 116.42803 39.903738 bjz 116.322287 39.893729 bjxz

GEODIST g1 bjnz bjxz km

GEOSEARCH g1 FROMLONLAT 116.397904 39.909005 BYRADIUS 10 km WITHDIST

```

![](<assets/1740497768968.png>)

![](<assets/1740497769301.png>)

**备注**：

1.  一定要登录 Redis，如果有密码一定要输入密码登录，否则添加数据会报错 `unauthenticated multibulk length`
2.  `GEODIST`计算距离，默认的单位是米

#### 附近商户搜索

![](<assets/1740497769620.png>)

**细节**：

1.  GEO 存储经度（longitude）和维度（latitude）还有值（member），为了节约内存，我们在 memboer 中值存储店铺 id
2.  由于前端传来一个 type 参数，但是 GEO 没有 type 数据，所以我们按照商铺类型进行分组，类型相同的商户分为一组，以 typeId 作为 key 同时存入一个 GEO 集合中

1）数据预热。将店铺数据按照 typeId 批量存入 Redis

```
    
    @Test
    public void loadShopListToCache() {
        
        List<Shop> shopList = shopService.list();
        











        
        Map<Long, List<Shop>> shopMap = shopList.stream()
                .collect(Collectors.groupingBy(Shop::getTypeId));

        
        for (Map.Entry<Long, List<Shop>> shopMapEntry : shopMap.entrySet()) {
            
            Long typeId = shopMapEntry.getKey();
            List<Shop> values = shopMapEntry.getValue();
            
            String key = SHOP_GEO_KEY + typeId;
            




            
            List<RedisGeoCommands.GeoLocation<String>> locations = new ArrayList<>();
            for (Shop shop : values) {
               locations.add(new RedisGeoCommands.GeoLocation<>(shop.getId().toString(),
                       new Point(shop.getX(), shop.getY())));
            }
            stringRedisTemplate.opsForGeo().add(key, locations);
        }
    }

```

2）在 ShopServiceImpl 中编写查询代码

```
@Override
    public Result queryShopByType(Integer typeId, Integer current, Double x, Double y) {
        
        if (x == null || y == null) {
            
            Page<Shop> page = query()
                    .eq("type_id", typeId)
                    .page(new Page<>(current, SystemConstants.DEFAULT_PAGE_SIZE));
            
            return Result.ok(page.getRecords());
        }

        
        int from = (current - 1) * SystemConstants.DEFAULT_PAGE_SIZE;
        int end = current * SystemConstants.DEFAULT_PAGE_SIZE;

        
        String key = SHOP_GEO_KEY + typeId;
        GeoResults<RedisGeoCommands.GeoLocation<String>> results = stringRedisTemplate.opsForGeo() 
                .search(
                        key,
                        GeoReference.fromCoordinate(x, y),
                        new Distance(5000),
                        RedisGeoCommands.GeoSearchCommandArgs.newGeoSearchArgs().includeDistance().limit(end)
                );
        
        if (results == null) {
            return Result.ok(Collections.emptyList());
        }
        List<GeoResult<RedisGeoCommands.GeoLocation<String>>> list = results.getContent();
        if (list.size() <= from) {
            
            return Result.ok(Collections.emptyList());
        }
        
        List<Long> ids = new ArrayList<>(list.size());
        Map<String, Distance> distanceMap = new HashMap<>(list.size());
        list.stream().skip(from).forEach(result -> {
            
            String shopIdStr = result.getContent().getName();
            ids.add(Long.valueOf(shopIdStr));
            
            Distance distance = result.getDistance();
            distanceMap.put(shopIdStr, distance);
        });
        
        String idStr = StrUtil.join(",", ids);
        List<Shop> shops = query().in("id", ids).last("ORDER BY FIELD(id," + idStr + ")").list();
        for (Shop shop : shops) {
            shop.setDistance(distanceMap.get(shop.getId().toString()).getValue());
        }
        
        return Result.ok(shops);
    }

```

注意：还需要再 Controller 层中新增两个参数，经度 x 和纬度 y

![](<assets/1740497769913.png>)

**备注**：前端数据是写死的，无论是哪一个用户，发送的地理坐标都是一样的，数据库中的店铺数据的坐标也是也是假的

![](<assets/1740497770211.png>)

期间遇到 [bug6](#6)

### 用户签到和连续签到统计

#### BitMap 基本使用

![](<assets/1740497770522.png>)

Redis 中是利用 string 类型数据结构实现 BitMap**，** 因此最大上限是 512M，转换为 bit 则是 2 32 2^{32} 232 个 bit 位。

BitMap 的操作命令有：

*   [`SETBIT`](https://redis.io/commands/setbit)：向指定位置（offset）存入一个 0 或 1
    
*   [`GETBIT`](https://redis.io/commands/getbit) ：获取指定位置（offset）的 bit 值
    
*   [`BITCOUNT`](https://redis.io/commands/bitcount) ：统计 BitMap 中值为 1 的 bit 位的数量
    
*   [`BITFIELD`](https://redis.io/commands/bitfield) ：操作（查询、修改、自增）BitMap 中 bit 数组中的指定位置（offset）的值
    
*   [`BITFIELD_RO`](https://redis.io/commands/bitfield_ro) ：获取 BitMap 中 bit 数组，并以十进制形式返回
    
*   [`BITOP`](https://redis.io/commands/bitop) ：将多个 BitMap 的结果做位运算（与 、或、异或）
    
*   [`BITPOS`](https://redis.io/commands/bitpos) ：查找 bit 数组中指定范围内第一个 0 或 1 出现的位置
    

**示例一**：总共 7 天，前三天（星期一到星期三）签到，中间两天不签到（星期四和星期五），后面三天又全都签到

```
SETBIT key offset value

```

![](<assets/1740497770831.png>)

**示例二**：查看数据

```
BITFIELD key

BITPOS key value
BITPOS bm1 1 
BITPOS bm1 0 

BITFIELD key GET type offset

BITFIELD bm1 get u2 0

```

备注：`u`表示无符号，`i`表示有符号，这里读取到 3，是因为 u2 表示获取两个 bit 位，0 表示从 0 开始计数，前面我们存入的数据是 11100111，从 0 开始计数，往后数两个 bit 位 就是 11 ，11 代表的数字就是 3，如果是 BITFIELD bm1 get u3 0 对应的就是 111，代表的数据就是 7，BITFIELD bm1 get u5 0 对应的数字也是 7（11100）

#### 签到功能

![](<assets/1740497771135.png>)

**注意**：Redis 客户端（RESP），必须使用 2020 以前版本，或者 2022.2 之后的版本，2021 不支持二进制数据的展示

```
    
    @Override
    public Result sign() {
        
        Long userId = ThreadLocalUtls.getUser().getId();
        
        LocalDateTime now = LocalDateTime.now();
        
        String keySuffix = now.format(DateTimeFormatter.ofPattern(":yyyyMM"));
        String key = USER_SIGN_KEY + userId + keySuffix;
        
        int dayOfMonth = now.getDayOfMonth();
        
        stringRedisTemplate.opsForValue().setBit(key, dayOfMonth - 1, true);
        return Result.ok();
    }

```

由于这个功能并没有前端实现，所以这里采用接口测试的方法：

![](<assets/1740497771564.png>)

#### 签到统计

*   **问题 1**：什么叫做连续签到天数？
    
    从最后一次签到开始向前统计，直到遇到第一次未签到为止，计算总的签到次数，就是连续签到天数。
    
*   **问题 2**：如何得到本月到今天为止的所有签到数据？
    
    `BITFIELD key GET u[dayOfMonth] 0`
    
*   **问题 3**：如何从后向前遍历每个 bit 位？
    
    与 1 做与运算，就能得到最后一个 bit 位。随后右移 1 位，下一个 bit 位就成为了最后一个 bit 位。
    

![](<assets/1740497771823.png>)

```
    
    @Override
    public Result signCount() {
        
        
        Long userId = ThreadLocalUtls.getUser().getId();
        
        LocalDateTime now = LocalDateTime.now();
        
        String keySuffix = now.format(DateTimeFormatter.ofPattern(":yyyyMM"));
        String key = USER_SIGN_KEY + userId + keySuffix;
        
        int dayOfMonth = now.getDayOfMonth();
        
        List<Long> result = stringRedisTemplate.opsForValue().bitField(
                key,
                BitFieldSubCommands.create()
                        .get(BitFieldSubCommands.BitFieldType.unsigned(dayOfMonth)).valueAt(0)
        );
        
        if (result == null || result.isEmpty()) {
            
            return Result.ok(0);
        }
        
        
        Long num = result.get(0);
        if (num == null || num == 0) {
            
            return Result.ok(0);
        }
        
        int count = 0;
        while (true) {
            
            if ((num & 1) == 0) {
                
                break;
            } else {
                
                count++;
            }
            
            num >>>= 1;
        }
        return Result.ok(count);
    }

```

由于这个功能并没有前端实现，所以这里采用接口测试的方法，此时由于只有一天起到，所以这里只会显示连续签到 1 天

我们可以通过`SETBIT`命令进行补签，然后再来统计一下签到天数

```
SETBIT sign:2:202307 0 1

SETBIT sign:2:202307 0 1

SETBIT sign:2:202307 0 1

SETBIT sign:2:202307 20 1

SETBIT sign:2:202307 19 1

```

现在我们当前是 22 号，当前连续签到是 3 天

![](<assets/1740497772034.png>)

### UV 统计

首先我们搞懂两个概念：

*   **UV**：全称 **U**nique **V**isitor，也叫独立访客量，是指通过互联网访问、浏览这个网页的自然人。1 天内同一个用户多次访问该网站，只记录 1 次。
    
*   **PV**：全称 **P**age **V**iew，也叫页面访问量或点击量，用户每访问网站的一个页面，记录 1 次 PV，用户多次打开页面，则记录多次 PV。往往用来衡量网站的流量。
    

UV 统计在服务端做会比较麻烦，因为要判断该用户是否已经统计过了，需要将统计过的用户信息保存。但是如果每个访问的用户都保存到 Redis 中，数据量会非常恐怖。

#### HyperLogLog 用法

Hyperloglog(HLL) 是从 Loglog 算法派生的概率算法，用于确定非常大的集合的基数，而不需要存储其所有值。相关算法原理大家可以参考：[https://juejin.cn/post/6844903785744056333#heading-0](https://juejin.cn/post/6844903785744056333)

Redis 中的`HLL`是基于`string`结构实现的，单个 HLL 的内存永远小于 **16kb**，内存占用低的令人发指！作为代价，其测量结果是概率性的，有小于 **0.81％** 的误差。不过对于 UV 统计来说，这完全可以忽略。

*   **HyperLogLog 常用指令**：
    
    *   `PFADD key element [element...]` ：添加指定元素到 HyperLogLog 中
        
    *   `PFCOUNT key [key ...]`：返回给定 HyperLogLog 的基数估算值
        
    *   `PFMERGE destkey sourcekey [sourcekey ...]`：将多个 HyperLogLog 合并为一个 HyperLogLog
        
*   **HyperLogLog 的作用**：做海量数据的统计工作
    
*   **HyperLogLog 的优缺点**：
    
    *   **优点**：内存占用极低、性能非常好
        
    *   **缺点**：有一定的误差
        

#### 实现 UV 统计

由于当前系统并没有足够的用户数据量，所以这里我们需要模拟实现 UV 统计 (●’◡’●)

1）我们先来测试一下 UV 统计的内存占用情况

内存查看前，我们先查看当前 Redis 占用内存情况

```
info memory

```

![](<assets/1740497772327.png>)

可以看到记录存储前：2.07MB（ 2.07 ∗ 2 10 ∗ 2 10 = 2174664 B y t e 2.07*2^{10}*2^{10}=2174664Byte 2.07∗210∗210=2174664Byte）

2）存储模拟用户数据

```
    
    @Test
    public void testHyperLogLog() {
        String[] values = new String[1000];
        
        int j = 0;
        for (int i = 0; i < 1000000; i++) {
            j = i % 1000;
            values[j] = "user_" + i;
            if (j == 999) {
                
                stringRedisTemplate.opsForHyperLogLog().add("hl2", values);
            }
        }
        
        Long count = stringRedisTemplate.opsForHyperLogLog().size("hl2");
        System.out.println("count = " + count);
    }

```

![](<assets/1740497772540.png>)

使用`info memory`查看记录存储后内存：2.09MB（2174664Byte），可以看到，存储 100w 条用户记录，但是内存至多占用了 0.02MB（恐怖如斯😱，太 TM 牛了），再来看看误差率 ( 1000000 − 997593 ) / 1000000 ≈ 0.002407 % (1000000-997593)/1000000≈0.002407\% (1000000−997593)/1000000≈0.002407%，远远低于官方说的 0.81%

### Bug 记录

*   **bug**：
    
    **问题背景**：
    
    **问题原因**：
    
    **问题解决**：
    
*   **bug1**：测试类运行失败
    
    **问题背景**：在【基于逻辑过期解决缓存击穿】时，需要提前预热数据，我使用测试类进行 Shop 数据预热，但是最终 NPE
    
    **问题原因**：详情见下方文章链接
    
    **问题解决**：
    
    1.  方案一：提高 SpringBoot 版本（不推荐）
    2.  方案二：在测试类上添加`@RunWith(SpringJunit4ClassRunner.class)`注解
    
    参考文章：[SpringBoot 程序测试时出现 NullPointerException（空指针异常）- 知识汲取者的博客 - CSDN 博客](https://blog.csdn.net/qq_66345100/article/details/128821656)
    
*   **bug2**：Nginx 负载均衡失败
    
    **问题背景**：在搭建集群环境，测试一人一单超卖问题时，配置完 Nginx 的负载均衡后，发现 Nginx 一直无法实现负载均衡的效果
    
    **问题原因**：ngxin 配置没有生效，修改了 nginx 的配置后，一定要记得重启 nginx
    
    **问题解决**：杀死所有 8080 端口的进程，然后执行`nginx.exe -s reload`，或者直接执行`nginx.exe -s reload`
    
    ```
    netstat -ano | findstr "8080"
    taskkill /pid 进程id -f
    
    ```
    
*   **bug3**：包不存在
    
    **问题背景**：在消息队列优化的时候，突然发现 redisson 这个 jar 包找不到了，真的很疑惑，明明我根本就没有动过这个依赖啊？？
    
    ![](<assets/1740497772863.png>)
    
    **问题原因**：IDEA 的锅
    
    **问题解决**：
    
    ![](<assets/1740497773173.png>)
    
    "Delegate IDE build/run actions to Maven" 是一种配置选项，通常在集成开发环境（IDE）中可用。它的作用是将 IDE 中的构建和运行操作委托给 Maven 进行处理。将 IDE 中的构建和运行操作委托给 Maven 可以提供更稳定、可靠和一致的构建过程，并使得项目的构建和依赖管理更加高效和可控
    
    参考文章： [Java——程序包不存在【三种解决方法】](https://blog.csdn.net/Pan_peter/article/details/126774921)
    
*   **bug4**：SpringBoot 启动后循环报错
    
    **问题背景**：在对消息队列进行优化时，然后启动 SpringBoot 程序后没有报错，但是过一段时间就会循环报错`java.lang.IllegalStateException: LettuceConnectionFactory was destroyed and cannot be used anymore`
    
    **问题原因**：这个异常信息表明在使用一个已被销毁的 LettuceConnectionFactory 实例。LettuceConnectionFactory 是 Spring Data Redis 对 Lettuce 客户端库的一个封装，用于创建和管理与 Redis 服务器的连接。
    
    这个异常通常在以下情况下发生：
    
    1.  **情况一**：在已经销毁的 LettuceConnectionFactory 实例上尝试进行 Redis 操作。
    2.  **情况二**：在多线程环境下共享同一个 LettuceConnectionFactory 实例，并在另一个线程销毁了该实例。（**我属于这种情况**）
    
    我属于第二种情况，我开启了两个 SpringBoot 程序，但是两个 SpringBoot 共用了一个 LettuceConnectionFactory 实例，同时我开启了热部署，每次我修改代码后，导致销毁和使用交叉进行，一个线程销毁了 LettuceConnectionFactory，另一个线程立马去取，从而引发线程安全问题
    
    **问题解决**：
    
    *   **方案一**：使用 ThreadLocal：将 LettuceConnectionFactory 实例放入 ThreadLocal 中，确保每个线程都使用自己的连接工厂实例。这样可以避免同时访问同一个连接工厂，减少线程间的竞争条件。
        
    *   **方案二**：使用连接池：使用连接池管理连接并提供线程安全的访问。Lettuce 支持连接池（如 LettucePool），可以配置连接池的大小来满足并发需求。
        
    *   **方案三**：使用连接工厂池：如果需要多个 LettuceConnectionFactory 实例，可以使用连接工厂池。可以使用 Apache Commons Pool 等开源库来管理连接工厂的池化对象，并确保池中连接工厂的线程安全访问。（推荐）
        
    *   **方案四**：同步访问：如果必须在多个线程中共享同一个 LettuceConnectionFactory 实例，那么需要在访问该实例时添加适当的同步机制，如使用 synchronized 同步块。这将确保只有一个线程能够使用连接工厂，从而避免线程间的竞争条件。
        
    *   **方案五**：使用连接工厂的连接：确保在操作完 Redis 后，将连接归还到连接工厂或连接池中。这样可以避免在一个线程中完成操作后，另一个线程从已关闭的连接工厂获取连接并尝试使用它。
        
    *   **方案六**：关闭热部署（**我采用的方法**）
        
    *   **方案七**：重启项目，但是不关闭热部署，重启项目还是会报错
        
    *   **方案八**：让当前项目处于单体状态，关闭一个 Tomcat，但是需要配置一下 Nginx，不让 Nginx 处于负载均衡状态
        
    
    参考文章：
    
    *   [java.lang.IllegalStateException: LettuceConnectionFactory was destroyed and cannot be used anymore](https://blog.csdn.net/weixin_52383177/article/details/126182003)
    *   ChatGPT
    
*   **bug5**：查询共同评论接口无法获取 id
    
    **问题背景**：在实现共同关注功能时，发现发送请求时，发现无法获取用户 id
    
    **问题原因**：漏写了一个接口，我们需要通过这个接口查询出用户信息，然后将用户信息存入 user 中
    
    ![](<assets/1740497773412.png>)
    
    我一开始还以为是前端出错了，没注意到这个请求居然是 404 了
    
    **问题解决**：在 UserController 中添加这个接口，返回用户信息，用户信息一定要包括用户 id
    
*   **bug6**</：Redis 查询报错`ERR unknown command 'GEORADIUS'`
    
    **问题背景**：在实现附近商户搜索功能时，利用`GEORADIUS`命令查询 Redis 报错
    
    **问题原因**：Redis 版本太低了，`GEORADIUS`命令是 Redis6.2 提供的。我的 SpringBoot 版本是 2.3，2.3 对应的 SpringDataRedis 是 2.3.9，该版本不支持 Redis6.2 提供的`GEORADIUS`指令
    
    **问题解决**：
    
    1.  方案一：使用低版本 Redis 的查询指令（可以自行百度）
    2.  方案二：将 Redis 换成高版本的（**采用**）
    
    ![](<assets/1740497773617.png>)
    
*   **bug7**：SpringBoot 启动失败
    
    **问题背景**：在实现了签到统计之后，为了方便接口测试，我不打算使用 postman 了，而是直接集成 Knife4j 直接再浏览器上进行接口测试，这样做的好处是能够避免编写接口，减少一定的工作量，使用 Knife4j 页显得更加简单，结果导入 Knife4j 之后报错`org.springframework.data.redis.RedisSystemException: Redis exception; nested exception is io.lettuce.core.RedisException: Connection closed`，这个原有是由于一个错误导致 Redis 连接断开，一开始我被这个错误迷惑了双眼，一顿 Debug 发现还是没有发现错误，于是就打算先将报错的地方注释起来，然后再次启动，结果有报错`Failed to start bean 'documentationPluginsBootstrapper'; nested exception is java.lang.NullPointerException` ，这个错误一看就明白了，Knife4j 的问题🤣。之所以前面循环报错，就是由于我要读取 stream.orders 中的数据，结果 Knife4j 报错导致 Redis 中断，我开辟了一个新线程去读取消息队列中的消息，导致循环报错
    
    **问题原因**：这次报错是由于没有注意到 Knife4j 与 SpringBoot 的版本兼容性问题，Knife4j3.0.x 版本不在兼容 SpringBoot2.6 之前的版本，我当前项目的 SpringBoot 版本是 2.4，所以就报错了
    
    **问题解决**：
    
    1.  方案一：改动版本。要么提高 SpringBoot 的版本（成本较高，不推荐），要么降低 Knife4j 的版本（推荐）
        
    2.  方案二：在配置文件中添加如下配置（**采用**）
        
        ```
        spring:
          mvc:
            pathmatch:
              matching-strategy: ant_path_matcher 
        
        ```
        
    
    **参考资料**：
    
    *   [Failed to start bean ‘documentationPluginsBootstrapper‘； nested exception is java.lang.NullPointerEx](https://blog.csdn.net/Shipley_Leo/article/details/129100908)
    
    配置成功之后，启动没有报错了，但是访问 \doc.html 出现 401，401 表示没有访问权限，这么一看就明白了，是拦截器在作怪┭┮﹏┭┮，需要将 Knife4j 的资源添加到白名单，加入一个 Knife4j 真不容易啊
    
    ```
                            // knife4j接口文档请求
                            "/doc.html",
                            "/webjars/**",
                            "/swagger-resources",
                            "/v2/api-docs"
    
    ```
    
    配置成功后，访问`http://localhost:8081/doc.html`即可
    

### 结语

详情见：**黑马点评项目总结. xmind**

**参考资料**：

*   [黑马程序员 Redis 入门到实战教程](https://www.bilibili.com/video/BV1cr4y1671t/?spm_id_from=333.337.search-card.all.click)
    
*   GPT3.5
    
*   [单体架构系统与分布式系统区别对比](https://blog.csdn.net/weixin_45393094/article/details/104632343)
    
*   [Redis 使用 Lua 脚本时为什么能保证原子性?](https://blog.csdn.net/weixin_49118892/article/details/116078430)
    
*   [redisson.tryLock() 的参数的理解](https://blog.csdn.net/m0_56356631/article/details/127735699)