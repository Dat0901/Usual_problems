Doc của PaddleOCR
![Screenshot from 2025-05-12 11-25-25](https://github.com/user-attachments/assets/cf0483f4-f83e-4d02-8bf2-d38c785e8777)
Không được cập nhật từ 2022 và chỉ dừng ở PP-OCRv3
Check doc bằng tiếng Trung (tiếng Anh không được cập nhật và dừng ở v3) trong src sẽ có PP-OCRv4

**1. Check lúc export từ .pdparams sang pdiparams của Recognition gặp 
*WARNING: The shape of model params not matched with loaded params***
+ Check xem đã tải cùng version file .yml và file Training_model
+ Check trong file .yml đã dùng đúng dictionary với Training_model (có nhiều dict kiểu en_dict.txt, ppocr_dict.txt, ...) 

**2. Check lúc export từ .pdparams sang pdiparams gặp 
*WARNING: The pretrained params not in model***
![Screenshot from 2025-05-06 16-04-50](https://github.com/user-attachments/assets/6043b634-2f3b-47f6-a1cf-659766fedee3)
![Screenshot from 2025-05-06 10-07-53](https://github.com/user-attachments/assets/62c799d2-831c-45ec-8794-b66edda0edac)
+ Check xem đã tải cùng version file .yml và file Training_model 

**3.Khi infer bằng .pdparams thì model hoạt động tốt, những khi export sang .pdiparams hoặc .onnx rồi infer thì model lại không trả về kết quả như lúc infer bằng .pdparams**  
+ Check xem đã tải cùng version file .yml và file Training_model 
+ Lúc export có thể lúc truyền model của mình vào thì có thể không nhận. Chuyển trực tiếp pretrain_model trong .yml từ default thành cái pdparams của mình.
![[Pasted image 20250512113629.png]]

+ Khi test bằng .pdiparams hoặc .onnx ví dụ det bằng: 
```
python3 tools/infer/predict_det.py \
--det_model_dir="/home/datdq/1WorkSpace/PaddleOCR/output/205_final_output_best_acc_infer" \
--image_dir="/home/datdq/1WorkSpace/lp_dataset/data_test_lp/test_img" \
```
Thì model không trả về kết quả được như lúc test .pdparams do thiếu các bước preprocess, truyền thêm vào thì kết quả bằng lúc test .pdparams.

![[Pasted image 20250512110456.png]]
Tương tự như model rec cũng cần truyền thêm preprocess step vào
![[Pasted image 20250512111333.png]]
Det + Rec:

**Chú ý** khi truyền 
**det_limit_type=min** (do LP bé nên cần scale up, *check operators.py*) 
thì cần truyền thêm 
**det_limit_side_len={tùy model input check d2s_train_image_shape trong file .yml để đặt**}, không sẽ mặc định là 736, khiến lệch với model input size -> kết quả det kém

**4. Với những ảnh lp dài, có độ cao < 25 pixels detection bị loạn hoặc không bắt được, trả về box linh tinh hoặc không trả về box.**
![[camera2_9bc9cd90-b161-4aee-abde-359099a6feb7.jpg]]
Những ảnh này thì thay vì cho vào det -> rec, thì cho thẳng vào rec luôn.
Các ảnh height >25 det bình thường

**5. Chạy recognition của pdiparams hoặc onnx gặp 
 *InvalidArgumentError: Broadcast dimension mismatch. Operands could not be broadcast together with the shape of X and the shape of Y* **

![[Pasted image 20250512114245.png]]
Truyền thêm các preprocess vào như lỗi ở 3.
