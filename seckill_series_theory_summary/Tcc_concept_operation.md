##5/6/2020 9:50:18 PM 
###Some countries in the world is more important， it is human conscience
**世界上还有些国家更重要的，那便是人类的良心**
###<center>Tcc concept operation_flow</center>
####1. Tcc 概念
	Try: 尝试执行业务
		完成所有业务检查(一致性)
		预留必须业务资源(准隔离性)

	Confirm:确认执行业务
		真正执行业务
		不作任何业务检查
		只使用Try阶段预留的业务资源
		Confirm操作要满足幂等性

	Cancel: 取消执行业务
		释放Try阶段预留的业务资源
		Cancel操作要满足幂等性

####2. Tcc 操作流程
<img style="height:400px;width:500px" src="/img/Tcc_operation_flow.png" />

	1. 一个完整的业务活动由一个主业务服务与若干从业务服务组成
	2. 主业务服务负责发起并完成整个业务活动
	3. 从业务服务提供TCC型业务操作
	4. 业务活动管理器控制业务活动的一致性，它登记业务活动中的操作， 并在业务活动提交时确认所有的TCC型操作的confirm操作，在业务活动取消时调用所有TCC型操作的cancel操作

>注意 confirm cancel 是由业务活动管理器操作的

####3. 适用范围
	1. 强隔离性、严格一致性要求的业务活动
	2. 适用于执行时间较短的业务（比如处理账户、收费等业务）

	


