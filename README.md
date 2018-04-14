# People-Detection
Tensorflow Object Detection API for People Detection

Use Python 3.6 and Tensorflow 1.7

Source : [accoon_dataset](https://github.com/datitran/raccoon_dataset) 做點小修改

# 轉換xml跟圖檔成 TFRecord 格式

在此使用[labelimg](https://github.com/tzutalin/labelImg)進行圖片標註，將標註完的XML跟JPG分別放在：

```cmd
raccoon_dataset\annotations
```

和

```cmd
raccoon_dataset\images
```

運行生成csv

```cmd
python xml_to_csv.py
```

在生成TFRecord之前，先修正Label Map保持格式一致

```cmd
# TO-DO replace this with label map
def class_text_to_int(row_label):
    if row_label == 'male':
        return 1
    if row_label == 'female':
        return 2
    else:
        None
```

運行生成TFRecord

```cmd
python generate_tfrecord.py
```

# 開始訓練

先建立一個label_map.pbtxt，內容為你的Label Map

```cmd
item {
  id: 1
  name: 'male'
}

item {
  id: 2
  name: 'female'
}
```

設定模型參數以及配置文件，打開raccoon_dataset\data\faster_rcnn_inception_v2_coco.config，修正文件位置

這裡使用[COCO-pretrained SSD with MobileNet model](https://blog.csdn.net/weixin_37667519/article/details/download.tensorflow.org/models/object_detection/ssd_mobilenet_v1_coco_2017_11_17.tar.gz)為預訓練模型

```cmd
fine_tune_checkpoint: "G:\\DL\\faster_rcnn_resnet101_coco_11_06_2017\\model.ckpt"
...
input_path: "G:\\DL\\test\\raccoon_dataset\\data\\train.record"
...
label_map_path: "G:\\DL\\test\\raccoon_dataset\\label_map.pbtxt"
...
input_path: "G:\\DL\\test\\raccoon_dataset\\data\\eval.record"
...
label_map_path: "G:\\DL\\test\\raccoon_dataset\\label_map.pbtxt"
```

接下來用TensorFlow Object Detection API開始訓練，使用models\research\object_detection\train.py

```cmd
python train.py \
--logtostderr \
--pipeline_config_path={你的faster_rcnn_inception_v2_coco.config位置} \
--train_dir={訓練模型導出位置} \
```

Windows 環境需要注意路徑反斜線