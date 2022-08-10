# SpringBoot整合SpringDataJPA

## SpringData JPA介绍

​		参考博客：[SpringBoot整合SpringDataJPA\_青衫-CSDN博客](https://blog.csdn.net/qq_39086296/article/details/90485645?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-2.control\&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-2.control "SpringBoot整合SpringDataJPA_青衫-CSDN博客")

*   Spring Data：其实SpringData是spring提供的一个操作数据的框架。而SpringData JPA只是SpringData框架下的一个
    `基于JPA标准操作数据`的模块。

*   SpringData JPA：基于JPA的标准数据进行操作。简化操作持久层的代码。只需要编写接口就可以。

## SpringBoot整合SpringData JPA

1.  导入依赖

    ```json
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    implementation group: 'mysql', name: 'mysql-connector-java', version: '8.0.23'
    ```

2.  application.properties文件中添加配置

    ```properties
    # 数据源配置
    spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
    spring.datasource.username=root
    spring.datasource.password=111111
    spring.datasource.url=jdbc:mysql://localhost:3306/jpa_test?characterEncoding=utf8&useSSL=false&serverTimezone=UTC

    # JPA配置

    # 开发阶段设置为true，开启了逆向工程。实际上线之后，设置为false
    # 数据库和Java的映射关系
    #   逆向工程：有了数据库的表，生成实体类
    #   正向工程：有了实体类，生成数据库的表
    spring.jpa.generate-ddl=true

    # （）=create：设置为create，每次运行程序，都会将原来的表删除，然后创建一个表。
    # （）=create-drop：每次运行创建一个表，使用完之后，就是在应用关闭的时候，也就是sessionFactory一关闭，会把表删除。
    # （）=none：功能不生效
    # （）=update：(常用)第一次启动根据实体建立表结构，之后启动会根据实体的（名字）改变更新表结构，之前的数据都在。
    #              name -> names 会添加一列新的names
    #              alter table user add column names varchar(255)
    # （）=validate：会验证创建数据库表结构，只会和数据库中的表进行比较，不会创建新表，但是会插入新值，
    #               运行程序会校验实体字段与数据库已有的表的字段类型是否相同，不同会报错
    spring.jpa.hibernate.ddl-auto=update

    # 生成sql语句
    spring.jpa.show-sql=true
    # 指定数据库的类型
    spring.jpa.database-platform=org.hibernate.dialect.MySQL5InnoDBDialect

    ```

3.  实体类
    Users
    Roles

    ```java
    @Data
    @NoArgsConstructor
    @AllArgsConstructor
    @Entity
    @Table(name = "t_users")
    public class Users {

        @Id    //主键id
        @GeneratedValue(strategy = GenerationType.IDENTITY)//主键生成策略
        @Column(name = "id")//数据库字段名
        private Integer id;

        @Column(name = "name")
        private String name;

        @Column(name = "age")
        private Integer age;

        @Column(name = "address")
        private String address;

        @ManyToOne(cascade = CascadeType.PERSIST)    //表示多方
        @JoinColumn(name = "role_id")    //维护一个外键，外键在Users一侧
        private Roles roles;

        @Override
        public String toString() {
            final StringBuffer sb = new StringBuffer("com.learn.springboot_jpa.entity.Users{");
            sb.append("id=").append(id);
            sb.append(", name='").append(name).append('\'');
            sb.append(", age=").append(age);
            sb.append(", address='").append(address).append('\'');
            sb.append(", roles=").append(roles);
            sb.append('}');
            return sb.toString();
        }
    }

    ```

    ```java
    @Data
    @NoArgsConstructor
    @AllArgsConstructor
    @Entity
    @Table(name = "t_roles")
    public class Roles {

        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        @Column(name = "role_id")
        private Integer roleId;
        @Column(name = "role_name")
        private String roleName;

        @OneToMany(mappedBy = "roles")
        private Set<Users> users=new HashSet<>();

    }
    ```

4.  Dao层接口

    ```java
    /**
     * Type parameters:
     * <T> – the domain type the repository manages
     * <ID> – the type of the id of the entity the repository manages
     */
    public interface UsersRepository extends JpaRepository<Users,Integer> {

    }
    ```

5.  测试

    ```java
        @Autowired
        UsersRepository usersRepository;
        @Test
        public void testSave(){
            Users users = Users.builder()
                    .name("张三")
                    .address("北京市")
                    .age(24)
                    .build();
            usersRepository.save(users);
        }
    ```

## SpringBoot JPA提供的核心接口

1.  Repository接口

2.  CrudRepository接口

3.  PagingAndSortingRepository接口

4.  JpaRepository接口

5.  JPASpecificationExecutor接口

**UML图如下：**

学习repository，最主要的是看**，它是一个样例实现类。**

![image-20210720094138578](https://gitee.com/zzursy/blog-image/raw/master/img/20210720094138.png "image-20210720094138578")

![JpaRepository](https://gitee.com/zzursy/blog-image/raw/master/img/20210720143447.png "JpaRepository")

## Repository接口

1.  提供了方法名称命名查询方式

2.  提供了基于@Query注解查询与更新

### 方法名称命名查询方式

1.  Dao层接口

    ```java
    public interface UsersRepositoryByName extends Repository<Users, Integer> {

        /**
         * 方法名遵循驼峰式命名规则
         * 查询例如：findBy+属性+查询条件
         *
         * @param name
         * @return
         */
        List<Users> findByName(String name);

        List<Users> findFirstByNameContains(String name);

        List<Users> findAllByNameLike(String name);
    }

    ```

2.  测试

    ```java
    @SpringBootTest
    class UsersRepositoryByNameTest {

        @Autowired
        UsersRepositoryByName repository;

        @Test
        void findByName() {
            List<Users> list = repository.findByName("张三");
            list.forEach((i) -> {
                System.out.println(i);
            });
        }

        @Test
        void findByNameAndAge() {
            List<Users> list = repository.findByNameAndAge("阿飞", 54);
            list.forEach((i) -> {
                System.out.println(i);
            });
        }

        @Test
        void findAllByNameLike() {
            List<Users> list = repository.findAllByNameLike("阿%");
            list.forEach((i) -> {
                System.out.println(i);
            });
        }
    }
    ```

![image-20210720112109532](https://gitee.com/zzursy/blog-image/raw/master/img/20210720112109.png "image-20210720112109532")

### 基于@Query注解查询与更新

1.  Dao层接口

    ```java
    public interface UsersRepositoryQueryAnnotation extends JpaRepository<Users, Integer> {
        @Query("from Users where name = ?1")
        List<Users> queryByNameUseHQL(String name);

        @Query(value = "select * from t_user where name= ?1", nativeQuery = true)
        List<Users> queryByNameUseSQL(String name);

        @Query("update Users set name= ?1 where id=?2")
        @Modifying
        // 需要执行一个更新操作
        void updateUsersNameById(String name, Integer id);
    }
    ```

2.  测试

    ```java
    	/**
    	 * Repository--@Query测试
    	 */
    	@Test
    	public void testQueryByNameUseSQL() {
    		List<Users> list = this.usersRepositoryQueryAnnotation.queryByNameUseSQL("张三");
    		for (Users users : list) {
    			System.out.println(users);
    		}
    	}

    	/**
    	 * Repository--@Query测试
    	 */
    	@Test
    	@Transactional // @Transactional与@Test 一起使用时 事务是自动回滚的。
    	@Rollback(false) // 取消自动回滚
    	public void testUpdateUsersNameById() {
    		this.usersRepositoryQueryAnnotation.updateUsersNameById("张三三", 1);
    	}

    ```

## CrudRepository接口

CrudRepository接口主要是完成一些增删改查的操作。注意：CrudRepository接口继承了Repository接口。

![image-20210720113021751](https://gitee.com/zzursy/blog-image/raw/master/img/20210720113021.png "image-20210720113021751")

## PagingAndSortingRepository接口

```java
public interface PagingAndSortingRepository<T, ID> extends CrudRepository<T, ID>{
    
}
```

CrudRepository 的扩展，以提供使用分页和排序抽象检索实体的方法。

1.  编写Dao层

    ```java
    public interface UsersRepositoryPagingAndSorting extends PagingAndSortingRepository<Users,Integer> {

    }
    ```

2.  测试

    ```java
    @Autowired
    UsersRepositoryPagingAndSorting usersRepositoryPagingAndSorting;
    @Test
    public void testPagingAndSortingRepositorySort() {
        //Order	定义了排序规则
        Sort.Order order = new Sort.Order(Sort.Direction.DESC, "age");

        //Sort对象封装了排序规则
        Sort sort = Sort.by(order);

        List<Users> lists = (List<Users>) usersRepositoryPagingAndSorting.findAll(sort);
        for (Users users : lists) {
            System.out.println(users);
        }
    }
    ```

![image-20210720121801354](https://gitee.com/zzursy/blog-image/raw/master/img/20210720121801.png "image-20210720121801354")

```java
@Test
public void testPagingAndSortingRepositoryPaging() {
    //Order	定义了排序规则
    Sort.Order order = new Sort.Order(Sort.Direction.DESC, "age");
    //Sort对象封装了排序规则
    Sort sort = Sort.by(order);

    //Pageable:封装了分页的参数，当前页，煤业显示的条数。注意：它的当前页是从0开始
    //PageRequest(page,size):page表示当前页，size表示每页显示多少条
    Pageable pageable = PageRequest.of(0, 5, sort);
    Page<Users> page = this.usersRepositoryPagingAndSorting.findAll(pageable);

    System.out.println("数据的总条数：" + page.getTotalElements());
    System.out.println("总页数：" + page.getTotalPages());
    List<Users> list = page.getContent();
    list.forEach(i -> {
        System.out.println(i);
    });
}
```

![image-20210720141829304](https://gitee.com/zzursy/blog-image/raw/master/img/20210720141829.png "image-20210720141829304")

## JpaRepository接口

```java
public interface JpaRepository<T, ID> extends 
    PagingAndSortingRepository<T, ID>, QueryByExampleExecutor<T>{
}
```

该接口继承了PagingAndSortingRepository。对继承的父接口中方法的返回值进行适配。

测试

```java
    @Autowired
    UsersRepository usersRepository;

    /**
     * JpaRepository	排序测试
     */
    @Test
    public void testJpaRepositorySort() {
        //Order	定义了排序规则
        Sort.Order order = new Sort.Order(Sort.Direction.DESC, "id");
        //Sort对象封装了排序规则
        Sort sort = Sort.by(order);
        List<Users> list = usersRepository.findAll(sort);
        for (Users users : list) {
            System.out.println(users);
        }
    }
```

## JPASpecificationExecutor接口

该接口主要是提供了多条件查询的支持，并且可以在查询中添加排序与分页。

注意JPASpecificationExecutor是单独存在的。

完全独立。

![SimpleJpaRepository](https://gitee.com/zzursy/blog-image/raw/master/img/20210720143413.png "SimpleJpaRepository")

### 使用接口

![image-20210720144512997](https://gitee.com/zzursy/blog-image/raw/master/img/20210720144513.png "image-20210720144512997")

先使用findOne方法：

1.  先去查看实现类

```java
@Override
public Optional<T> findOne(@Nullable Specification<T> spec) {

   try {
      return Optional.of(getQuery(spec, Sort.unsorted()).getSingleResult());
   } catch (NoResultException e) {
      return Optional.empty();
   }
}
```

1.  首先要创建一个Specification对象，查看构造器，发现只有默认构造器，直接new一个对象，实现里面的方法。

```java
Specification specification = new Specification() {
    @Override
    public Predicate toPredicate(Root root, CriteriaQuery query, CriteriaBuilder criteriaBuilder) {
        return null;
    }
};
```

1.  需要返回一个Predicate对象，`criteriaBuilder.equal(root.get("name"), "张三");`正好返回的是Predicate对象。

```java
Specification specification = new Specification() {
    @Override
    public Predicate toPredicate(Root root, CriteriaQuery query, CriteriaBuilder criteriaBuilder) {
        Predicate predicate = criteriaBuilder.equal(root.get("name"), "张三");
        return predicate;
    }
};
```

1.  接着使用

```java
Optional<Users> one = userRepositorySpecification.findOne(specification);
Users users = one.get();
System.out.println(users);
```

1.  输出日志
    com.learn.springboot\_jpa.entity.Users{id=1, name='张三', age=24, address='北京市', roles=null}

**测试：**

```java
	/**
     * JpaSpecificationExecutor		单条件查询
     */
    @Test
    public void testJpaSpecificationExecutor1() {
        /**
         * Specification:用于封装查查询条件
         */
        Specification<Users> spec = new Specification<Users>() {
            //Predicate：封装了单个查询条件

            /**
             * @param root        对查询对象属性的封装
             * @param criteriaQuery    封装了我们要执行的查询中的各个部分的信息，select from order
             * @param criteriaBuilder    查询条件的构造器
             * @return
             */
            @Override
            public Predicate toPredicate(Root<Users> root, CriteriaQuery<?> criteriaQuery, CriteriaBuilder criteriaBuilder) {
                //where name="张三"
                /**
                 * 参数一：查询的属性
                 * 参数二：条件的值
                 */
                Predicate predicate = criteriaBuilder.equal(root.get("name"), "张三");
                return predicate;
            }
        };
        List<Users> list = this.userRepositorySpecification.findAll(spec);
        for (Users users : list) {
            System.out.println(users);
        }
    }

    /**
     * JpaSpecificationExecutor		多条件查询方式一
     */
    @Test
    public void testJpaSpecificationExecutor2() {
        /**
         * Specification:用于封装查查询条件
         */
        Specification<Users> spec = new Specification<Users>() {
            //Predicate：封装了单个查询条件

            /**
             * @param root        对查询对象属性的封装
             * @param criteriaQuery    封装了我们要执行的查询中的各个部分的信息，select from order
             * @param criteriaBuilder    查询条件的构造器
             * @return
             */
            @Override
            public Predicate toPredicate(Root<Users> root, CriteriaQuery<?> criteriaQuery, CriteriaBuilder criteriaBuilder) {
                //where name="张三" and age=20
                /**
                 * 参数一：查询的属性
                 * 参数二：条件的值
                 */
                List<Predicate> list = new ArrayList<>();
                list.add(criteriaBuilder.equal(root.get("name"), "张三"));
                list.add(criteriaBuilder.equal(root.get("age"), 20));
                Predicate[] arr = new Predicate[list.size()];
                return criteriaBuilder.and(list.toArray(arr));
            }
        };
        List<Users> list = this.userRepositorySpecification.findAll(spec);
        for (Users users : list) {
            System.out.println(users);
        }
    }

    /**
     * JpaSpecificationExecutor		多条件查询方式二
     */
    @Test
    public void testJpaSpecificationExecutor3() {
        /**
         * Specification:用于封装查查询条件
         */
        Specification<Users> spec = new Specification<Users>() {
            //Predicate：封装了单个查询条件

            /**
             * @param root        对查询对象属性的封装
             * @param criteriaQuery    封装了我们要执行的查询中的各个部分的信息，select from order
             * @param criteriaBuilder    查询条件的构造器
             * @return
             */
            @Override
            public Predicate toPredicate(Root<Users> root, CriteriaQuery<?> criteriaQuery, CriteriaBuilder criteriaBuilder) {
                //where name="张三" and age=20
                /**
                 * 参数一：查询的属性
                 * 参数二：条件的值
                 */
				/*List<Predicate> list=new ArrayList<>();
				list.add(criteriaBuilder.equal(root.get("name"),"张三"));
				list.add(criteriaBuilder.equal(root.get("age"),20));
				Predicate[] arr=new Predicate[list.size()];*/
                //(name='张三' and age=20) or id=2
                return criteriaBuilder.or(criteriaBuilder.and(criteriaBuilder.equal(root.get("name"), "张三"), criteriaBuilder.equal(root.get("age"), 20)), criteriaBuilder.equal(root.get("id"), 1));
            }
        };

        Sort sort = Sort.by(new Sort.Order(Sort.Direction.DESC, "id"));
        List<Users> list = this.userRepositorySpecification.findAll(spec, sort);
        for (Users users : list) {
            System.out.println(users);
        }
    }
```

## 关联映射操作

### 一对多的关联关系

角色：一方

用户：多方

1.  Roles

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@Entity
@Table(name = "t_roles")
public class Roles {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "role_id")
    private Integer roleId;
    @Column(name = "role_name")
    private String roleName;

    @OneToMany(mappedBy = "roles")
    private Set<Users> users = new HashSet<>();

}

```

1.  Users

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
@Entity
@Table(name = "t_users")
public class Users {

    @Id    //主键id
    @GeneratedValue(strategy = GenerationType.IDENTITY)//主键生成策略
    @Column(name = "id")//数据库字段名
    private Integer id;

    @Column(name = "name")
    private String name;

    @Column(name = "age")
    private Integer age;

    @Column(name = "address")
    private String address;

    @ManyToOne(cascade = CascadeType.PERSIST)    //表示多方
    @JoinColumn(name = "role_id")    //维护一个外键，外键在Users一侧
    private Roles roles;

    @Override
    public String toString() {
        final StringBuffer sb = new StringBuffer("com.learn.springboot_jpa.entity.Users{");
        sb.append("id=").append(id);
        sb.append(", name='").append(name).append('\'');
        sb.append(", age=").append(age);
        sb.append(", address='").append(address).append('\'');
        sb.append(", roles=").append(roles);
        sb.append('}');
        return sb.toString();
    }
}
```

1.  Dao层编写

```java
/**
 * 参数一 T :当前需要映射的实体
 * 参数二 ID :当前映射的实体中的OID的类型
 *
 */
public interface UsersRepository extends JpaRepository<Users,Integer> {

}

```

1.  测试

### 多对多的关联关系

角色与菜单多对多关联关系

菜单：多方
角色：多方
