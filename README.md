# Horovod Multi Node Multi GPUS Ready BERT

[This is  a new update of the origin (Google's) BERT implementation ](https://github.com/lambdal/bert)


[Horovod环境搭建指南](https://zhuanlan.zhihu.com/p/351693076)


[个人关于分布式机器学习的总结](https://zhuanlan.zhihu.com/p/365662727)
- TensorFlow
- Pytorch
- Horovod
- kubeflow

==具体细节先参照原文链接==


改进点：
- 原先代码是TF1.x，迁移到TF2.x(测试环境 TensorFlow 2.3.1)
- 原先代码是Single Node Multi GPUs,现在是 Multi Node Multi GPUs

## TF代码迁移
```
tf_upgrade_v2 --intree bert/ --outtree bert-upgraded/
```

迁移后生成的report.txt显示有部分代码迁移失败
```
ERROR: Using member tf.flags in deprecated module tf.flags. tf.flags and tf.app.flags have been removed, please use the argparse or absl modules if you need command line parsing.
```
是tf.flags这个包已经被tf2.x遗弃了，要替换成其他API。此处是举个例子，上传code已经替换好。

## 其他问题
- 有个bug，create_pretraining_data与run_pretraining的时候需要max_seq_length、max_predictions_per_seq统一，不然会运行失败。
- batch_size 按照实际情况适当调整，不然会OOM
- no_hvd_pretrain.sh   正常情况下的预训练
- hvd_pretrain.sh Horovod多机多卡情况下的分布式训练

## 深坑警告
要注意下TF和cuda的版本。cuda 10.1+tf1.x的情况下，回发现GPU只占用了99M(利用率低)，升级成2.x后发现正常使用了。
