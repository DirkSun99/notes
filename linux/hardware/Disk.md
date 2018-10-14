## 硬盘

功能：大容量存储器，存储视频、文本、音频等各种数据，成为现代电脑不可缺少的配件。

作用：由于计算机在工作时CPU、输入输出设备与存储器之间要大量地交换数据，因此，存储器的存取速度和容量，也是影响计算机运行速度的主要因素之一。特别是在服务器优化场景，硬盘的性能是决定网站性能的重要因素。

磁盘接口或类型：IDE, SCSI，SAS，STAT，SSD(电子的

性能：SSD(固态) > SAS > SATA

企业应用：
	
	1. 常规正式工作场景选SAS硬盘（转速是15000转/分，机械磁盘转数高的性能好）

	2. 不对外提供访问的服务器，如：线下的数据备份，可选SATA（7200-10000转/分）。SATA特点：容量大，价格便宜，速度慢。

	3. 高并发访问，小数据量，可以选择SSD。

	4. 企业应用一般会把 SATA 和 SSD 结合起来使用，热点存储，程序动态调度。