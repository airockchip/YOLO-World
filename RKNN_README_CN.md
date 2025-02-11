# 导出 RKNPU2 适配模型说明

## Source

​本仓库基于 https://github.com/AILab-CVC/YOLO-World  仓库的 b4fd87838d7f53adc0dbf5844313b92d9e3124c7 commit 进行修改,验证.



## 模型差异

在不影响输出结果, 不需要重新训练模型的条件下, 有以下改动:

- 模型输出中置信度分支增加sigmoid算子(修改 `deploy/easydeploy/model/model.py`)

- 模型输入增加texts, 其为clip_text模型的输出，可自定义检测的类别文本数量(修改 `deploy/export_onnx.py`)

- 模型输入中消除LpNormalization, 因clip_text模型中已存在对应归一化操作(修改 `yolo_world/models/dense_heads/yolo_world_head.py`)


## 导出onnx模型

在满足`Yolo_World`原仓库中导出模型的安装环境后，执行以下语句导出模型

``` sh
PYTHONPATH=./ python deploy/export_onnx.py path/to/config path/to/weights --custom-text path/to/customtexts --opset 11  --model-only

# path/to/config 为模型配置文件
# path/to/weights 为模型权重文件
# custom-text 为检测的类别文本
# 执行完毕后，会生成 ONNX 模型. 假如原始模型为 yolo_word_v2s.pth，则生成 yolo_word_v2s.onnx 模型。
```

## 转RKNN模型、Python demo、C demo

请参考 https://github.com/airockchip/rknn_model_zoo

