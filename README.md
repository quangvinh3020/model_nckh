# NCKH - Ghi chú mô hình

## Tổng quan
Repo này hiện có 2 mô hình segmentation YOLO đã huấn luyện trên bộ dữ liệu với cấu hình `data.yaml`.

## Dataset
- Đường dẫn dữ liệu: `/content/dataset_final`
- Số lớp: 5
- Nhãn: `Motorbike`, `Person`, `Car`, `Bicycle`, `Bus`

## Các mô hình

### 1) `yolo11n-seg-vn-coco-1024`
- Loại bài toán: segmentation
- Kích thước ảnh: 1024
- Số epoch: 120
- Batch size: 4
- Seed: 1337
- Checkpoint chính: `yolo11n-seg-vn-coco-1024-20260721T031243Z-1-001/yolo11n-seg-vn-coco-1024/weights/best.pt`
- Checkpoint cuối: `yolo11n-seg-vn-coco-1024-20260721T031243Z-1-001/yolo11n-seg-vn-coco-1024/weights/last.pt`

Kết quả tốt nhất ghi nhận trong `results.csv`:
- mAP50 (Box): 0.81865 ở epoch 120
- mAP50 (Mask): 0.77435 ở epoch 109

### 2) `yolo26n-seg-vn-coco-1024`
- Loại bài toán: segmentation
- Kích thước ảnh: 1024
- Số epoch: 120
- Batch size: 4
- Seed: 1337
- Checkpoint chính: `yolo26n-seg-vn-coco-1024-20260721T031249Z-1-001/yolo26n-seg-vn-coco-1024/weights/best.pt`
- Checkpoint cuối: `yolo26n-seg-vn-coco-1024-20260721T031249Z-1-001/yolo26n-seg-vn-coco-1024/weights/last.pt`

Kết quả tốt nhất ghi nhận trong `results.csv`:
- mAP50 (Box): 0.80207 ở epoch 120
- mAP50 (Mask): 0.76736 ở epoch 120


