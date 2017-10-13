+++
title = "Java中的Proxy"
description = ""
categories = [
    "Web"
]
tags = [
    "Java",
]
date = "2015-09-08 17:26:58"
+++


Java中的静态代理属于硬编码方式，在这里就不记录了。这里主要写下jdk动态代理和使用cglib实现动态代理的方法。

### jdk动态代理

jdk实现动态代理必须通过接口(面向接口编程)。

IProductService.java

    public interface IProductService {
        void save();
    }

ProductServiceImpl.java

    public class ProductServiceImpl implements IProductService {
        @Override
        public void save() {
            System.out.println("saved a product");
        }
    }

DynamicProxy.java

    public class DynamicProxy implements InvocationHandler {
        private Object proxyObject;

        DynamicProxy(Object proxyObject){
            this.proxyObject = proxyObject;
        }

        public Object createProxyInstance() {
            return Proxy.newProxyInstance(proxyObject.getClass().getClassLoader(),
                    proxyObject.getClass().getInterfaces(), this);
        }

        @Override
        public Object invoke(Object proxy, Method method, Object[] args)
                throws Throwable {
            return method.invoke(this.proxyObject, args);
        }
    }

测试jdk动态代理：

       public static void main(String[] args) throws NoSuchMethodException {
              // jdk 动态代理
              DynamicProxy factory = new DynamicProxy(new ProductServiceImpl());
              IProductService proxyTarget = (IProductService) factory
                      .createProxyInstance();
              proxyTarget.save();
       }

输出：
        `saved a product`

### cglib实现动态代理

cglib实现动态代理无需依赖接口，cglib底层依赖于asm字节码修改工具。

#### 使用到的jar包：
- asm-3.3.1.jar
- cglib-nodep-3.1.jar

PersonDao.java

    public class PersonDao {
        void save(){
            System.out.println("saved a person");
        }
    }

CglibProxy.java

    public class CglibProxy implements MethodInterceptor {
        private Enhancer enhancer = new Enhancer();

        public Object createProxy(Class clazz){
            enhancer.setSuperclass(clazz);
            enhancer.setCallback(this);
            return enhancer.create();
        }

        @Override
        public Object intercept(Object obj, Method method, Object[] args,
                                MethodProxy proxy) throws Throwable {
            System.out.println("前置处理");
            Object result = proxy.invokeSuper(obj, args);
            System.out.println("后置处理");
            return result;
        }
    }

测试cglib动态代理：

    public static void main(String[] args) {
            CglibProxy cglibProxy = new CglibProxy();
            PersonDao proxyDao =  (PersonDao)cglibProxy.createProxy(PersonDao.class);
            proxyDao.save();
    }

输出：
        `前置处理`
        `saved a person`
        `后置处理`

需要注意的是在使用jdk动态代理时，给DynamicProxy传入的构造参数必须是接口的实现类。CglibProxy代理类中intercept方法调用被代理对象的方法是*invokeSuper*。被代理的类不能被*final*修饰。

### 爬过的坑

直接使用asm和cglib的包会报错。

