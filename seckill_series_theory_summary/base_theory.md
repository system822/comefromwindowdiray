##5/6/2020 8:52:34 PM 
###The light of mind， light in silence， in the busy。
**心灵的灯，在寂静中光明，在热闹中熄灭。**
###<center>base_theory</center>
	BASE
		. BA	Basic Availability	基本业务可用性（支持分区失败）
		. S		Soft state	柔性转台	（允许短时间内不同步）
		. E		Eventual consistency	最终一致性（最终一致性是一致的，不是实时一致）

	原子性 A 和持久性 D 必须保障
		为了可用性，性能和降级服务的需要，只有降低一致性C 与隔离性I