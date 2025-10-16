# 🗑️ Nhận Diện Rác Thải với YOLOv8s và Tập Dữ Liệu TACO


## 1\. Giới thiệu

Dự án này tập trung vào việc áp dụng học sâu để giải quyết vấn đề rác thải. Chúng tôi đã huấn luyện mô hình **YOLOv8s** (You Only Look Once, phiên bản 8, kích thước nhỏ) để **nhận diện và phân loại** các loại rác khác nhau trong môi trường tự nhiên và đô thị.

  * **Mô hình:** YOLOv8s, được phát triển bởi Ultralytics.
  * **Tập Dữ liệu:** **TACO** (Trash Annotations in Context) – một tập dữ liệu mở rộng lớn về rác thải, được chú thích theo định dạng COCO.
  * **Mục tiêu:** Cung cấp một giải pháp nhận diện rác thải hiệu quả, có thể ứng dụng trong robot phân loại, drone giám sát môi trường, hoặc hệ thống thu gom tự động.

### 2 Tải về Mô hình

Các file trọng số `best.pt` và `last.pt` nằm trong thư mục:

-----

## 3\. Cài đặt và Yêu cầu

### 3.1 Yêu cầu Phần cứng

  * **GPU** (NVIDIA, khuyến nghị) với **CUDA 11.8+** và **cuDNN** để tăng tốc.
  * **CPU** (cần thiết cho inference trên các tác vụ nhỏ).

### 3.2 Cài đặt Phần mềm

1.  **Clone repository:**

    ```bash
    git clone https://github.com/hoangtuvungcao/YOLO8S-TACO.git
    cd YOLO8S-TACO
    ```

2.  **Cài đặt thư viện:**

    ```bash
    #cài đặt tối thiểu:
    pip install ultralytics
    ```

-----

## 4\. Sử dụng Mô hình (Dự đoán/Inference)

Sử dụng file cấu hình dữ liệu `taco.yaml` để đảm bảo mô hình nhận diện đúng tên lớp.

### 4.1 File Cấu hình (`taco.yaml`)

```yaml
# taco.yaml (Chỉ cần phần lớp học chính xác để Inference)
nc: [SỐ_LƯỢNG_LỚP]
names: ['plastic bag', 'plastic bottle', 'can', 'paper', ...] # Danh sách TÊN CÁC LỚP RÁC CHÍNH XÁC
```

### 4.2 Chạy Dự đoán

Sử dụng lệnh `yolo` với trọng số `best.pt` để đạt hiệu suất tốt nhất.

| Mục tiêu | Lệnh (Command) |
| :--- | :--- |
| **Ảnh Đơn** | `yolo predict model=best.pt data=taco.yaml source='path/to/image.jpg' save=True` |
| **Thư mục Ảnh** | `yolo predict model=best.pt data=taco.yaml source='path/to/image_folder/' save=True` |
| **Video** | `yolo predict model=best.pt data=taco.yaml source='path/to/video.mp4' conf=0.25 save=True` |
| **Webcam** | `yolo predict model=best.pt data=taco.yaml source=0` |

  * **Lưu ý:**
      * Thay `best.pt` bằng đường dẫn thực tế của bạn.
      * Tham số `conf=0.25` đặt ngưỡng tin cậy (confidence threshold).

-----

## 5\. Chuẩn bị Dữ liệu (Dành cho Tái Huấn luyện)

Phần này dành cho những ai muốn tái huấn luyện, fine-tune hoặc mở rộng tập dữ liệu.

### 5.1 Cấu trúc Dữ liệu

Tập dữ liệu TACO cần được chuyển đổi từ định dạng COCO gốc sang định dạng **YOLO** và được tổ chức như sau:

```
/data
    /images
        /train  (ảnh huấn luyện)
        /val    (ảnh kiểm tra)
    /labels
        /train  (.txt files, định dạng YOLO)
        /val    (.txt files, định dạng YOLO)
```


## 6\. Quá trình Huấn luyện


### 6.1 Tiếp tục Huấn luyện (Resume)

Sử dụng trọng số `last.pt` để tiếp tục quá trình training đã bị ngắt:

```bash
yolo task=detect mode=train model=runs/detect/yolov8s_taco_v1/weights/last.pt data=taco.yaml \
    epochs=200 imgsz=640 batch=16 \
    name=yolov8s_taco_v1_resume
```

-----


### 7 Đóng góp

Mọi đóng góp nhằm cải thiện độ chính xác, tốc độ hoặc tính năng của mô hình đều được chào đón\!

1.  Fork (phân nhánh) repository này.
2.  Tạo branch mới (`git checkout -b feature/taco-improvement`).
3.  Thực hiện thay đổi và commit.
4.  Mở một **Pull Request**.

### 7.1 Lời Cảm Ơn

  * Cảm ơn **Ultralytics** đã phát triển thư viện YOLOv8 mạnh mẽ.
  * Cảm ơn đội ngũ **TACO** đã cung cấp tập dữ liệu rác thải mở.
  * Cảm ơn cộng đồng đã hỗ trợ và sử dụng dự án.
