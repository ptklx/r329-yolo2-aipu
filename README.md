# r329-yolo2-aipu

目前由于这个目标检测资料残缺不全，走过很多坑才把yolo2部署上去，想起过往都是泪。让我一样一样的尝试，试出来的简直是体力活。
问题是maixpy3 链接网络不是很稳定，模型加载nn.load(self.m, opt=self.options) 释放有问题，就得reboot，所以也就放弃maixpy3sdk调试，对输入宽高不等支持也是一个坑


在极术这提供了两个周易量化sdk版本，最新的其实已经是v2.2了，github上model-zoo是v2.2的，他们和v2.0（也就是sipeed的）不通用支持也不一样。
在申请下载一定要提供官方邮件，不然不给下呀，不问我为什么知道的，你可为极术小姐姐。

虽然，说例子有提供yolo，ssd但都不能很好的用，有参考价值的是周易AIPU部署mobilenet_v2_ssd填坑记
主要是没整个流程例子，有些就得自己摸索，黑盒一个

主要是卡在量化，sipeed只支持v2.0，
模型选择，尝试了许多，发现算子不支持，或者就是各种问题
最后看到这个
https://github.com/dianjixz/v831_yolo
几下改了，训练自己的数据就通了

量化配置cfg，v2.2不用配置生成min max文件方便多了，但蛋疼地方是下面不支持
哪v2.0的min/max 如何生成啦

#https://github.com/matrix-2020/quantize_script/tree/6c1c5fbab5e97d5760be772d7310f1594a92c210
也就最后自己添加region，和nms 的奇葩
最后参考model-zoo官方删除的v2.0版本，配置cfg
（私心也许是花公司资源搞出的，也就不忙公开代码）
