# Pytorch

## 历史 

2002 Torch

2011 Torch7(Y language)

2016 facebook pytorch0.1(C++实现的py接口)

2018 pytorch1.0 caffe2

主要DL框架：

tf, keras, mxnet, caffe, pytorch

现存版本：theano, torch, caffe

pytorch&tf现在十分nb

keras是基于tf和torch的封装

## 动态图&静态图

动态图：程序里新建一个东西就完成一步，如torch

静态图：搭好图之后就不能再动了，然后开始喂数据，运行时无法修改结构，如tf

python写tf时不好写...

tf2.0：动态图优先，支持静态图

深度学习库可以：

1. GPU加速
2. 自动求导
3. 常用网络层API

