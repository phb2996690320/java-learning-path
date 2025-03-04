> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/csdn_20150804/article/details/121584855)

#### 文章目录

*   *   [一. 使用场景](#__2)
    *   [二. 使用姿势](#__9)
    *   *   [1. 实现 MetaObjectHandler 接口](#1MetaObjectHandler_14)
        *   [2. 实体类上添加注解](#2__55)
    *   [三. 不生效的场景](#__89)
    *   *   [1. 使用 mapper.xml 的 sql 不生效](#1_mapperxmlsql_91)
        *   [2. boolean update(Wrapper updateWrapper) 不生效](#2__boolean_updateWrapperT_updateWrapper__93)
        *   [3. @TableField 注解要加上](#3_TableField_108)
        *   [4. 其他](#4__112)

### 一. 使用场景

[MetaObjectHandler](https://so.csdn.net/so/search?q=MetaObjectHandler&spm=1001.2101.3001.7020) 是元对象字段填充控制器抽象类，实现公共字段自动写入。

比如通常，我们在建表时，会设置几个公共字段：创建人（creator）、更新人（uptater）、创建时间（create_time）、更新时间（update_time）。

每次将实体对象新增入库时，都要设置创建人和创建时间；每次更新实体对象时，都要设置更新人和更新时间；如果这是都放在业务代码中，很是繁琐，那么可不可以统一配置，自动帮我们添加这些属性呢？答案就是使用 MetaObjectHandler。

### 二. 使用姿势

官方说明：[https://mp.baomidou.com/guide/auto-fill-metainfo.html](https://mp.baomidou.com/guide/auto-fill-metainfo.html)  
注意：不同版本 api 略有不同，但是步骤是一样的，接口也是一样的，本文是 3.1.0 。

步骤如下：

#### 1. 实现 MetaObjectHandler 接口

MetaObjectHandler 接口有两个接口方法，需要我们自己去实现它：

```
/**
     * 插入元对象字段填充（用于插入时对公共字段的填充）
     *
     * @param metaObject 元对象
     */
    void insertFill(MetaObject metaObject);

    /**
     * 更新元对象字段填充（用于更新时对公共字段的填充）
     *
     * @param metaObject 元对象
     */
    void updateFill(MetaObject metaObject);
```

自定义实现类 MyMetaObjectHandler 如下：

```
@Slf4j
@Component
public class MyMetaObjectHandler implements MetaObjectHandler {

    @Override
    public void insertFill(MetaObject metaObject) {
        log.info("start insert fill ....");
          this.setFieldValByName("creator", bucUserBo.getEmpId(), metaObject);
         this.setFieldValByName("create_time", new Date(), metaObject);
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        log.info("start update fill ....");
        this.setFieldValByName("updater", "", metaObject);
        this.setFieldValByName("update_time", new Date(), metaObject);
    }
}
```

思路就是实现两个接口方法，一个更新，一个插入，然后对一些公共属性进行赋值操作；  
对于更新人和创建人这两个属性，一般从 Threadlocal 中取值。

#### 2. 实体类上添加注解

除了上面实现两个接口方法，还需要在对应实体的属性上添加注解，这样 mybatisplus 才会取进行赋值处理。

```
/**
 * 创建时间
 */
 @TableField(value = "create_time",fill = FieldFill.INSERT)
 @DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
private Date createTime;

/**
 * 创建者工号
 */
@TableField(value = "creator",fill = FieldFill.INSERT)
private String creator;

/**
 * 更新时间
 */
@TableField(value = "update_time",fill = FieldFill.INSERT_UPDATE)
@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
private Date updateTime;

/**
 * 更新者工号
 */
@TableField(value = "updater",fill = FieldFill.INSERT_UPDATE)
private String updater;
```

其中，FieldFill 枚举有 4 个属性，

*   DEFAULT ：默认不处理
*   INSERT ： 插入操作时进行填充字段
*   UPDATE ：更新操作时填充字段
*   INSERT_UPDATE ：插入和更新操作时填充字段

### 三. 不生效的场景

按照上文步骤配置了后，在新增和更新实体对象时，就会自动赋值了，但是，并不是所有场景都会自动填充的，有一些失效场景需要注意：

#### 1. 使用 mapper.xml 的 sql 不生效

必须使用 mybatis 的 api 进行插入和更新操作。

#### 2. boolean update(Wrapper [updateWrapper](https://so.csdn.net/so/search?q=updateWrapper&spm=1001.2101.3001.7020)) 不生效

不生效方式：

```
this.update( new UpdateWrapper<User>()
                .set("name", "张三"))
        );
```

生效方式：

```
this.update(new User(), new UpdateWrapper<User>()
                .set("name", "张三"))
        );
```

#### 3. @[TableField 注解](https://so.csdn.net/so/search?q=TableField%E6%B3%A8%E8%A7%A3&spm=1001.2101.3001.7020)要加上

@TableField(value = “update_time”, fill = FieldFill.UPDATE)  
实现接口后，此注解也要加，并且指定 fill 参数值，不然不会填充。

#### 4. 其他

如果属性有值则不覆盖, 如果 sql 中赋值了，自动填充又设置为其他值，则已 sql 中的值为准‘  
如果填充值为 null 则不填充；比如 this.setFieldValByName(“update_time”, null, metaObject); 实际是不会更新为 null 的；

3.1.0 版本的中的更新时间不生效，可以升级为 3.3.0 版本，然后使用如下方式：

```
this.strictUpdateFill(metaObject, "updateTime", LocalDateTime.class, LocalDateTime.now());
```

以上是 MetaObjectHandler 的使用方式以及需要注意的点；  
不同的版本使用方式略有不同，如果不生效，可以通过以下两个方法排查：

*   阅读所对应版本的官方使用文档；
*   代码调试，跟以下代码，一般就知道失效原因了。