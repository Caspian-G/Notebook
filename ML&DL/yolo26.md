#### dependency

```
pip install torch torchvision torchaudio
pip install ultralytics
```

#### setting.json

C:\Users\\...\AppData\Roaming\Ultralytics\settings.json

#### 示例代码

```
from ultralytics import YOLO
model = YOLO('yolo26n.pt')
source = 'cat.jpg'
model.predict(source, save=True)
```

###### 模型版本

yolo26n.pt (nano)

yolo26s.pt (small)

yolo26m.pt (medium)

yolo26l.pt (large)

yolo26x.pt (extra large)
