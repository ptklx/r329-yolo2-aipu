# r329-yolo2-aipu

目前由于这个目标检测资料残缺不全，走过很多坑才把yolo2部署上去，想起过往都是泪。让我一样一样的尝试，试出来的简直是体力活。
问题是maixpy3 链接网络不是很稳定，模型加载nn.load(self.m, opt=self.options) 释放有问题，就得reboot，所以也就放弃maixpy3sdk调试，对输入宽高不等支持也是一个坑


在极术这https://aijishu.com/a/1060000000215443 提供了两个周易量化sdk版本，最新的其实已经是v2.2了，github上model-zoo是v2.2的，他们和v2.0（也就是sipeed的）不通用支持也不一样。
在申请下载一定要提供官方邮件，不然不给下呀，不问我为什么知道的，你可加极术小姐姐问问。

虽然，说例子有提供yolo，ssd但都不能很好的用，有参考价值的是《周易AIPU部署mobilenet_v2_ssd填坑记 》https://aijishu.com/a/1060000000224602
主要是没整个流程例子，有些就得自己摸索，黑盒一个

主要是卡在量化，sipeed只支持v2.0，
模型选择，尝试了许多，发现算子不支持，或者就是各种问题
最后看到这个
https://github.com/dianjixz/v831_yolo
自己改了下，训练自己的数据就通了

量化配置cfg，v2.2不用配置生成min max文件方便多了，但蛋疼地方是sipeed哪个镜像不支持
哪v2.0的min/max 如何生成啦

#https://github.com/matrix-2020/quantize_script/tree/6c1c5fbab5e97d5760be772d7310f1594a92c210
也就最后自己添加region，和nms 的奇葩
 ####### name yolo_416 MAX         ....        min ####
    # 'yolo_416_region_0': array([0.]), 
    # 'yolo_416_region': {'coords': 13},       #min 'yolo_416_region': {'coords': -2.7752295},
    # 'yolo_416_region_1': array([0.]), 
    # 'yolo_416_region_2': array([0.]), 
    # 'yolo_416_region_3': array([0.]), 
    # 'yolo_416_region_4': array([0.]), 

    # 'yolo_416_nms_0': array([0.]), 
    # 'yolo_416_nms_1': array([0.]),
    # 'yolo_416_nms_2': array([0.]), 
    # 'yolo_416_nms_3': array([0.])
 可以看到大多数是0

最后参考model-zoo官方删除的v2.0版本，配置cfg，input也是坑
还得整合个r329-Tina-jishu,全志的直接配置aipu，再改下sys_partition.fex 大小，很好编译过，
但，就怕但了，我用的是sipeed开发板不是哪个evb5，也就不想改端口配置了https://r329.docs.aw-ol.com/r329_evb5/
那就上docker https://github.com/sipeed/R329-Tina-jishu ， 最后搞成一个固件，哪yolo2的目标检测部署也就完成，发现效果比k210好，也许是模型更大，性能更强了，哈哈哈哈哈

（夹杂点私心，也许是花公司资源搞出的，也就不忙公开代码）

