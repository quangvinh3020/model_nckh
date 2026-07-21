# NCKH - Mô hình segmentation YOLO

## 1. Tổng quan
Repository này lưu code xử lý dữ liệu, cấu hình huấn luyện và các checkpoint của 2 mô hình segmentation YOLO đã được train trên bộ dữ liệu 5 lớp.

Mục đích chính của repo là:
- up codebase mô hình lên GitHub để lưu trữ và chia sẻ
- ghi chú đầy đủ cấu hình train, checkpoint và kết quả
- làm tài liệu tham chiếu nhanh cho báo cáo hoặc chạy lại mô hình

## 2. Cấu trúc hiện có

```text
NCKH/
├── codebase.py
├── data.yaml
├── README.md
├── yolo11n-seg-vn-coco-1024-20260721T031243Z-1-001/
└── yolo26n-seg-vn-coco-1024-20260721T031249Z-1-001/
```

### Ý nghĩa các file chính
- `codebase.py`: script tiền xử lý/ghi nhãn tự động, dùng YOLO-Seg kết hợp SAM2 để refine mask và lưu label theo định dạng YOLO segmentation.
- `data.yaml`: khai báo đường dẫn dataset và danh sách lớp.
- `yolo11n-seg-vn-coco-1024/...`: thư mục kết quả huấn luyện của model `yolo11n-seg-vn-coco-1024`.
- `yolo26n-seg-vn-coco-1024/...`: thư mục kết quả huấn luyện của model `yolo26n-seg-vn-coco-1024`.

## 3. Dataset

- Đường dẫn dữ liệu: `/content/dataset_final`
- Split: `images/train`, `images/val`, `images/test`
- Số lớp: 5
- Danh sách lớp: `Motorbike`, `Person`, `Car`, `Bicycle`, `Bus`

## 4. Codebase.py làm gì

Script `codebase.py` dùng để hỗ trợ tạo dataset segmentation từ ảnh đầu vào.

Luồng xử lý chính:
1. Đọc ảnh từ `data/images`.
2. Chạy YOLO-Seg để phát hiện bbox và mask sơ bộ.
3. Dùng SAM2 để refine mask theo bbox.
4. Hỏi người dùng gán lại class ID nếu cần, và ghi nhớ mapping theo tên class.
5. Xuất nhãn sang `data/labels` theo định dạng YOLO segmentation.
6. Lưu ảnh preview có overlay mask vào `data/check`.

Thư viện được dùng trong script:
- `ultralytics`
- `torch`
- `opencv-python`
- `numpy`
- `Pillow`

### Chạy script

```bash
python codebase.py
```

### Thư mục mà script tạo ra
- `data/images`: ảnh đầu vào
- `data/labels`: nhãn YOLO segmentation
- `data/check`: ảnh preview có mask

## 5. Các mô hình đã huấn luyện

### 5.1 `yolo11n-seg-vn-coco-1024`
- Bài toán: segmentation
- Kích thước ảnh: 1024
- Epoch: 120
- Batch size: 4
- Seed: 1337
- `cls_remap`: true
- `cos_lr`: true
- `overlap_mask`: true

Thông số train chi tiết:
- `task`: segment
- `mode`: train
- `model`: `yolo11n-seg-vn-coco-1024/weights/last.pt`
- `data`: `/content/content/dataset_final/data.yaml`
- `project`: `/content/drive/MyDrive/CCTV_Dataset 16n`
- `name`: `yolo11n-seg-vn-coco-1024`
- `pretrained`: true
- `deterministic`: true
- `optimizer`: auto
- `device`: `0`
- `workers`: 2
- `patience`: 120
- `amp`: true
- `fraction`: 1.0
- `split`: val
- `imgsz`: 1024
- `batch`: 4
- `epochs`: 120
- `lr0`: 0.01
- `lrf`: 0.01
- `momentum`: 0.937
- `weight_decay`: 0.0005
- `warmup_epochs`: 3.0
- `warmup_momentum`: 0.8
- `warmup_bias_lr`: 0.0
- `box`: 7.5
- `cls`: 0.5
- `dfl`: 1.5
- `mosaic`: 1.0
- `mixup`: 0.1
- `copy_paste`: 0.4
- `close_mosaic`: 15
- `fliplr`: 0.5
- `scale`: 0.5
- `translate`: 0.1
- `hsv_h`: 0.015
- `hsv_s`: 0.7
- `hsv_v`: 0.4
- `mask_ratio`: 4
- `save_dir`: `/content/drive/MyDrive/CCTV_Dataset 16n/yolo11n-seg-vn-coco-1024`

Checkpoint:
- `yolo11n-seg-vn-coco-1024-20260721T031243Z-1-001/yolo11n-seg-vn-coco-1024/weights/best.pt`
- `yolo11n-seg-vn-coco-1024-20260721T031243Z-1-001/yolo11n-seg-vn-coco-1024/weights/last.pt`

Kết quả tốt nhất trong `results.csv`:
- mAP50 (Box): 0.81865 ở epoch 120
- mAP50 (Mask): 0.77435 ở epoch 109

### 5.2 `yolo26n-seg-vn-coco-1024`
- Bài toán: segmentation
- Kích thước ảnh: 1024
- Epoch: 120
- Batch size: 4
- Seed: 1337
- `cls_remap`: true
- `cos_lr`: true
- `overlap_mask`: true

Thông số train chi tiết:
- `task`: segment
- `mode`: train
- `model`: `yolo26n-seg-vn-coco-1024/weights/last.pt`
- `data`: `/content/content/dataset_final/data.yaml`
- `project`: `/content/drive/MyDrive/CCTV_Dataset 26n and model`
- `name`: `yolo26n-seg-vn-coco-1024`
- `pretrained`: true
- `deterministic`: true
- `optimizer`: auto
- `device`: `0`
- `workers`: 2
- `patience`: 120
- `amp`: true
- `fraction`: 1.0
- `split`: val
- `imgsz`: 1024
- `batch`: 4
- `epochs`: 120
- `lr0`: 0.01
- `lrf`: 0.01
- `momentum`: 0.937
- `weight_decay`: 0.0005
- `warmup_epochs`: 3.0
- `warmup_momentum`: 0.8
- `warmup_bias_lr`: 0.0
- `box`: 7.5
- `cls`: 0.5
- `dfl`: 1.5
- `mosaic`: 1.0
- `mixup`: 0.1
- `copy_paste`: 0.4
- `close_mosaic`: 15
- `fliplr`: 0.5
- `scale`: 0.5
- `translate`: 0.1
- `hsv_h`: 0.015
- `hsv_s`: 0.7
- `hsv_v`: 0.4
- `mask_ratio`: 4
- `save_dir`: `/content/drive/MyDrive/CCTV_Dataset 26n and model/yolo26n-seg-vn-coco-1024`

Checkpoint:
- `yolo26n-seg-vn-coco-1024-20260721T031249Z-1-001/yolo26n-seg-vn-coco-1024/weights/best.pt`
- `yolo26n-seg-vn-coco-1024-20260721T031249Z-1-001/yolo26n-seg-vn-coco-1024/weights/last.pt`

Kết quả tốt nhất trong `results.csv`:
- mAP50 (Box): 0.80207 ở epoch 120
- mAP50 (Mask): 0.76736 ở epoch 120

## 6. So sánh nhanh

- `yolo11n-seg-vn-coco-1024` cho kết quả tốt hơn một chút so với `yolo26n-seg-vn-coco-1024` trên cả Box và Mask.
- Hai mô hình dùng cùng cấu hình train chính: `imgsz=1024`, `batch=4`, `epochs=120`, `seed=1337`.

## 7. Phụ lục thông số toàn bộ (raw)

Thông số dưới đây là bản đầy đủ 100% trích từ file `args.yaml` của từng mô hình.

### 7.1 Full args - yolo11n-seg-vn-coco-1024

```yaml
task: segment
mode: train
model: /content/drive/MyDrive/CCTV_Dataset 16n/yolo11n-seg-vn-coco-1024/weights/last.pt
data: /content/content/dataset_final/data.yaml
epochs: 120
time: null
patience: 120
batch: 4
imgsz: 1024
save: true
save_period: -1
cache: false
device: '0'
workers: 2
project: /content/drive/MyDrive/CCTV_Dataset 16n
name: yolo11n-seg-vn-coco-1024
exist_ok: false
pretrained: true
cls_remap: true
optimizer: auto
verbose: true
seed: 1337
deterministic: true
single_cls: false
rect: false
cos_lr: true
close_mosaic: 15
resume: /content/drive/MyDrive/CCTV_Dataset 16n/yolo11n-seg-vn-coco-1024/weights/last.pt
amp: true
fraction: 1.0
profile: false
freeze: null
multi_scale: 0.0
compile: false
overlap_mask: true
mask_ratio: 4
dropout: 0.0
val: true
split: val
save_json: false
conf: null
iou: 0.7
max_det: 300
quantize: null
dnn: false
plots: true
end2end: null
source: null
vid_stride: 1
stream_buffer: false
visualize: false
augment: false
agnostic_nms: false
classes: null
retina_masks: false
embed: null
show: false
save_frames: false
save_txt: false
save_conf: false
save_crop: false
show_labels: true
show_conf: true
show_boxes: true
line_width: null
format: torchscript
keras: false
optimize: false
dynamic: false
simplify: true
opset: null
workspace: null
nms: false
lr0: 0.01
lrf: 0.01
momentum: 0.937
weight_decay: 0.0005
warmup_epochs: 3.0
warmup_momentum: 0.8
warmup_bias_lr: 0.0
distill_model: null
dis: 6.0
box: 7.5
cls: 0.5
cls_pw: 0.0
dfl: 1.5
pose: 12.0
kobj: 1.0
rle: 1.0
angle: 1.0
nbs: 64
hsv_h: 0.015
hsv_s: 0.7
hsv_v: 0.4
degrees: 0.0
translate: 0.1
scale: 0.5
shear: 0.0
perspective: 0.0
flipud: 0.0
fliplr: 0.5
bgr: 0.0
mosaic: 1.0
mixup: 0.1
cutmix: 0.0
copy_paste: 0.4
copy_paste_mode: flip
auto_augment: randaugment
erasing: 0.4
cfg: null
tracker: tracktrack.yaml
save_dir: /content/drive/MyDrive/CCTV_Dataset 16n/yolo11n-seg-vn-coco-1024
```

### 7.2 Full args - yolo26n-seg-vn-coco-1024

```yaml
task: segment
mode: train
model: /content/drive/MyDrive/CCTV_Dataset 26n and model/yolo26n-seg-vn-coco-1024/weights/last.pt
data: /content/content/dataset_final/data.yaml
epochs: 120
time: null
patience: 120
batch: 4
imgsz: 1024
save: true
save_period: -1
cache: false
device: '0'
workers: 2
project: /content/drive/MyDrive/CCTV_Dataset 26n and model
name: yolo26n-seg-vn-coco-1024
exist_ok: false
pretrained: true
cls_remap: true
optimizer: auto
verbose: true
seed: 1337
deterministic: true
single_cls: false
rect: false
cos_lr: true
close_mosaic: 15
resume: /content/drive/MyDrive/CCTV_Dataset 26n and model/yolo26n-seg-vn-coco-1024/weights/last.pt
amp: true
fraction: 1.0
profile: false
freeze: null
multi_scale: 0.0
compile: false
overlap_mask: true
mask_ratio: 4
dropout: 0.0
val: true
split: val
save_json: false
conf: null
iou: 0.7
max_det: 300
quantize: null
dnn: false
plots: true
end2end: null
source: null
vid_stride: 1
stream_buffer: false
visualize: false
augment: false
agnostic_nms: false
classes: null
retina_masks: false
embed: null
show: false
save_frames: false
save_txt: false
save_conf: false
save_crop: false
show_labels: true
show_conf: true
show_boxes: true
line_width: null
format: torchscript
keras: false
optimize: false
dynamic: false
simplify: true
opset: null
workspace: null
nms: false
lr0: 0.01
lrf: 0.01
momentum: 0.937
weight_decay: 0.0005
warmup_epochs: 3.0
warmup_momentum: 0.8
warmup_bias_lr: 0.0
distill_model: null
dis: 6.0
box: 7.5
cls: 0.5
cls_pw: 0.0
dfl: 1.5
pose: 12.0
kobj: 1.0
rle: 1.0
angle: 1.0
nbs: 64
hsv_h: 0.015
hsv_s: 0.7
hsv_v: 0.4
degrees: 0.0
translate: 0.1
scale: 0.5
shear: 0.0
perspective: 0.0
flipud: 0.0
fliplr: 0.5
bgr: 0.0
mosaic: 1.0
mixup: 0.1
cutmix: 0.0
copy_paste: 0.4
copy_paste_mode: flip
auto_augment: randaugment
erasing: 0.4
cfg: null
tracker: tracktrack.yaml
save_dir: /content/drive/MyDrive/CCTV_Dataset 26n and model/yolo26n-seg-vn-coco-1024
```



