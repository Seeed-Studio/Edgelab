# 表计模型训练教程

训练表计数据集的模型是很简单的一件事情，可以查看[colab](https://colab.research.google.com/github/Seeed-Studio/Edgelab/blob/master/demo/meter.ipynb)运行示例，也可按照以下步骤进行：

1. 数据集准备
2. 修改配置文件
3. 模型训练
4. 模型推理测试
5. 模型导出
6. 使用自定义数据集

## 1. 数据集准备

 在数据集准备阶段，可以下载我们公开的表计数据集，可按照以下步骤进行：

1. 点击[下载](https://1drv.ms/u/s!AqG2uRmVUhlShtIhyd_7APHXEhpeXg?e=WwGx5m) 将数据集下载至本地文件夹。
2. 将下载的数据集解压，并记住这个文件路径，修改配置文件时会用。

## 2. 修改配置文件

打开[./configs/pfld/pfld_mv2n_112.py](configs/pfld/pfld_mv2n_112.py)文件，定位到需要修改的变量`data_root`,做出如下修改：

```python
data_root='解压后的数据集的绝对路径'
```

## 3. 模型训练

对于模型训练，这是非常简单的，你可以查看[开始使用](../get_started.md)文档,也可以使用以下命令进行训练。

```shell
python tools/train.py mmpose configs/pfld/pfld_mv2n_112.py
```

在训练完成后会在`./work_dir/pfld_mv2n_112/exp1/`文件夹下产生`latest.pth`模型权重文件。记住这个文件的路径，这个将在模型导出的时候使用。

## 4. 模型推理测试

在训练完成后我们可以测试训练后的模型效果，此时只需一行命令即可完成测试，如下：

```shell
python tools/test.py configs/pfld/pfld_mv2n_112.py ./work_dir/pfld_mv2n_112/exp1/latest.pth
```

此时会看到模型推理的效果。

## 5. 模型导出

模型导出onnx格式同样很简单，一行命令即可完成，如果需要导出其他格式，可点击[这里](./onnx2xxx.md)查看教程。

```shell
python tools/torch2onnx.py mmpose --config configs/pfld/pfld_mv2n_112.py --checkpoint ./work_dir/pfld_mv2n_112/exp
1/latest.pth --shape 112
```

### 参数解释

- `--config` :模型配置文件
- `--checkpoint` :模型训练后的权重文件
- `--shape` :模型输入数据的大小

## 6.使用自定义数据集

如果你想使用自定义数据集当然也可以，我们建议你使用[labelme](https://github.com/wkentaro/labelme)标注工具。如果你使用了我们的自动环境配置脚本，现在不需要你下载他或者安装它。现在你只需要激活你的虚拟环境和打开`labelme`即可，如下：

```shell
conda activate edgelab
labelme
```

此时你只需要标注你的数据集即可，同时数据集的结构需要按照我们提供数据数据的结构那样即可,如下：

```shell
data_root/
        |  
        ├──train/
        │     ├── images
        │     │   ├── xxx0.png
        │     │   └── xxx1.jpg
        │     ├── annotations
        │     │   ├── xxx0.json
        │     │   └── xxx1.json
        ├──val/
        │     ├── images
        │     │   ├── ***0.png
        │     │   └── ***1.jpg
        │     ├── annotations
        │     │   ├── ***0.json
        │     │   └── ***1.json
```

之后将配置文件的`data_root`的值修改为你的数据集根目录的绝对路径即可开始训练。
