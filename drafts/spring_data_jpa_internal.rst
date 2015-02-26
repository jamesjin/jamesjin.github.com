Spring Data JPA 工作原理
==============================


#. RepositoryConfigurationSource#getCandidates

扫描指定包，找出所有 RepositoryInterface

#. RepositoryConfigurationExtension#getRepositoryConfigurations

#. RepositoryBeanDefinitionBuilder#build
挨个构建 BeanDefinitionBuilder, 所有类型都为 RepositoryConfigurationExtension#getRepositoryFactoryClassName, 默认值为 JpaRepositoryFactoryBean. 这个类内部会创建 JpaRepositoryFactory, 再由 JpaRepositoryFactory 创建真正的 Repository. JpaRepositoryFactory 创建的代理有一组 advice 组成，分别是 预先配置好的 RepositoryProxyPostProcessor 们生成的 advice，主要是事务

#. 真正的执行者 QueryExecutorMethodInterceptor



.. author:: default
.. categories:: none
.. tags:: none
.. comments::
