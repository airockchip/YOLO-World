# RKNN optimization for exporting model

## Source
Base on https://github.com/AILab-CVC/YOLO-World with commit id as b4fd87838d7f53adc0dbf5844313b92d9e3124c7


## What different
With inference result values unchanged, the following optimizations were applied:
- A sigmoid operator is added to the confidence branch of the model output.(Modify `deploy/easydeploy/model/model.py`)

- The model input is augmented with "texts", which is the output of the clip_text model and can be customized to the number of categories to be detected.(Modify `deploy/export_onnx.py`)

- The LpNormalization in the model input is removed, as the clip_text model already has a corresponding normalization operation.(Modify `yolo_world/models/dense_heads/yolo_world_head.py`)


## Export ONNX model

After meeting the installation environment requirements in the original `Yolo_World` repository for exporting the model, execute the following command to export the model:


``` sh
PYTHONPATH=./ python deploy/export_onnx.py path/to/config path/to/weights --custom-text path/to/customtexts --opset 11  --model-only

# path/to/config refers to the model configuration file.
# path/to/weights refers to the model weight file.
# custom-text refers to the detection category text.
# After execution, an ONNX model will be generated. If the original model is yolo_word_v2s.pth, the generated ONNX model will be yolo_word_v2s.onnx. 
```

## Convert to RKNN model, Python demo, C demo

Please refer to https://github.com/airockchip/rknn_model_zoo.
