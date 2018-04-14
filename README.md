# People-Detection
Tensorflow Object Detection API for People Detection

Use Python 3.6 and Tensorflow 1.7

Source : [accoon_dataset](https://github.com/datitran/raccoon_dataset) 做點小修改

#轉換xml跟圖檔成 TFRecord 格式

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