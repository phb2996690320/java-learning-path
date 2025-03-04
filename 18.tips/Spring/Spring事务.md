### Spring 类内部非事务方法调用自身的事务方法。

只有通过 Spring 代理对象调用的方法，事务管理器才会生效。如果在同一个类的内部直接调用事务方法（例如通过 `this` 调用），事务注解将不会生效。

### **1. 动态代理的原理**

Spring 使用动态代理（默认是 JDK 动态代理或 CGLIB 代理）来实现事务管理。当一个方法被 `@Transactional` 注解标记时，Spring 的事务管理器会拦截该方法的调用，并在方法执行前后添加事务逻辑（如开启事务、提交事务或回滚事务）。

#### **关键点**

- **代理机制**：事务管理器只能拦截通过代理对象调用的方法。
- **内部调用**：如果在同一个类的内部直接调用事务方法（例如 `this.someMethod()`），不会通过代理，事务注解不会生效。

### **2. 示例说明**

#### **(1) 正确的调用方式**

假设你有两个服务类：`OrderService` 和 `InventoryService`。`InventoryService` 中的 `updateInventory` 方法被 `@Transactional` 注解标记。

java复制

```java
@Service
public class OrderService {

    @Autowired
    private InventoryService inventoryService;

    @Transactional
    public void createOrder(Order order) {
        // 主业务逻辑：创建订单
        saveOrder(order);

        // 通过代理调用事务方法
        inventoryService.updateInventory(order);
    }

    private void saveOrder(Order order) {
        // 保存订单逻辑
    }
}

@Service
public class InventoryService {

    @Transactional(propagation = Propagation.NESTED)
    public void updateInventory(Order order) {
        // 更新库存逻辑
        if (order.getQuantity() < 0) {
            throw new IllegalArgumentException("库存数量不能为负");
        }
        // 其他库存更新逻辑
    }
}
```

在这种情况下，`createOrder` 方法通过 `inventoryService` 代理对象调用 `updateInventory` 方法，事务注解会生效。

#### **(2) 错误的调用方式**

如果在同一个类的内部直接调用事务方法，事务注解不会生效。

java复制

```java
@Service
public class OrderService {

    @Transactional
    public void createOrder(Order order) {
        // 主业务逻辑：创建订单
        saveOrder(order);

        // 错误：直接通过 this 调用，不会通过代理
        this.updateInventory(order);
    }

    private void saveOrder(Order order) {
        // 保存订单逻辑
    }

    @Transactional(propagation = Propagation.NESTED)
    public void updateInventory(Order order) {
        // 更新库存逻辑
        if (order.getQuantity() < 0) {
            throw new IllegalArgumentException("库存数量不能为负");
        }
        // 其他库存更新逻辑
    }
}
```

在这种情况下，`updateInventory` 方法虽然有 `@Transactional` 注解，但由于是通过 `this` 调用的，不会通过 Spring 代理，事务注解不会生效。

### **3. 解决方法**

为了避免事务失效，有以下几种解决方法：

#### **(1) 使用 AOP 的 `aspect` 捕获内部调用**

可以通过 AOP 的 `aspect` 捕获内部调用，确保事务逻辑生效。但这种方法相对复杂，且可能影响性能。

#### **(2) 将事务方法提取到单独的服务类中**

将需要事务的方法提取到单独的服务类中，确保通过代理调用。

#### **(3) 使用 `AopContext`**

Spring 提供了 `AopContext`，可以通过它获取当前的代理对象，从而确保事务逻辑生效。

java复制

```java
@Service
public class OrderService {

    @Autowired
    private InventoryService inventoryService;

    @Transactional
    public void createOrder(Order order) {
        // 主业务逻辑：创建订单
        saveOrder(order);

        // 获取当前代理对象
        OrderService proxy = (OrderService) AopContext.currentProxy();
        proxy.updateInventory(order);
    }

    private void saveOrder(Order order) {
        // 保存订单逻辑
    }

    @Transactional(propagation = Propagation.NESTED)
    public void updateInventory(Order order) {
        // 更新库存逻辑
        if (order.getQuantity() < 0) {
            throw new IllegalArgumentException("库存数量不能为负");
        }
        // 其他库存更新逻辑
    }
}
```