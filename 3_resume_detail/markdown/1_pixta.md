---
time:
title: Chặng đường mà tôi đã đi qua ở Pixta Vietnam
description: Bài viết chia sẻ về các kiến thức và kinh nghiệm tôi đã tích lũy được trong quá trình làm việc tại các dự án của Team AI tại Pixta Vietnam.
banner_url:
tags: [resume]
is_highlight: false
is_published: false
---

## 9. AI Headshot Image Generation (2025)

### 9.1. Mô tả dự án

### 9.2. Vai trò và trách nhiệm

Trong dự án AI Headshot, mình đảm nhận vai trò Project Manager kiêm Technical Leader.
Do đó, mình vừa đóng góp về mặt kỹ thuật chuyên môn vừa đóng góp về mặt quản lý dự án và đề xuất các chiến lược phát triển trở thành sản phẩm thương mại.

#### Về mặt quản lý

- **Lập kế hoạch và đề xuất chiến lược phát triển sản phẩm:**
- 

#### Về mặt kỹ thuật

- **Xây dựng cơ chế đánh giá chất lượng ảnh đầu ra tự động:**
    - ***Tiêu chí đánh giá độ tương đồng của định danh khuôn mặt (face ID) giữa ảnh gốc và ảnh AI-generated:*** Sử dụng mô hình ArcFace để trích xuất đặc trưng khuôn mặt và tính toán cosine similarity để đánh giá độ giống nhau (score càng cao càng là ảnh sinh ra càng giống với ảnh gốc, càng thấp càng khác biệt).
    - ***Tiêu chí đánh giá chất lượng hình ảnh tổng thể:*** Sử dụng mô hình NIMA (Neural Image Assessment) để đánh giá chất lượng hình ảnh dựa trên các yếu tố như độ sắc nét, độ sáng, bố cục (score càng cao là ảnh sinh ra càng đẹp, càng thấp là ảnh xấu).
    - ***Tiêu chí đánh giá độ đẹp tự nhiên của khuôn mặt (tránh các hiện tượng méo mó, dị dạng):*** Sử dụng mô hình TOPIQ-Face để đánh giá độ tự nhiên của khuôn mặt (score càng cao là ảnh sinh ra càng tự nhiên, càng thấp là ảnh có nhiều dị dạng).
    - ***Tiêu chí đánh giá độ liên quan giữa yêu cầu đầu vào và ảnh đầu ra:*** Sử dụng mô hình CLIP để tính toán độ liên quan giữa mô tả yêu cầu đầu vào (dạng text) và ảnh đầu ra (score càng cao là ảnh sinh ra càng liên quan đến yêu cầu, càng thấp là ảnh không liên quan).
    - ***Tỷ lệ ảnh "đạt chuẩn":*** được xác định dựa trên việc kết hợp các tiêu chí đánh giá trên. Ảnh đạt chuẩn là ảnh có điểm số vượt ngưỡng cho phép ở tất cả các tiêu chí.
    - ***Chọn ngưỡng điểm số trên từng tiêu chí:*** dựa vào thống kê điểm số từ bộ ảnh AI-generated mà mô hình pretrained gốc sinh ra (ở đây là FLUX.1-dev).
    - ***Đánh giá độ tương đồng với ảnh do con người chụp:*** sử dụng chỉ số FID (Fréchet Inception Distance) (càng thấp thì ảnh AI-generated càng giống ảnh thật, càng cao thì khác biệt).
    - ***Đánh giá độ đa dạng của ảnh AI-generated:*** sử dụng chỉ số LPIPS (Learned Perceptual Image Patch Similarity) (càng cao thì ảnh AI-generated càng đa dạng, càng thấp thì ảnh sinh ra càng giống nhau).

- **Chuẩn bị dữ liệu huấn luyện:**
    - ***Tiền xử lý ảnh:*** xác định chủ thể chính trong ảnh, crop chân dung từ ảnh gốc.
    - ***Đánh giá chất lượng của ảnh huấn luyện:*** sử dụng một số tiêu chí như NIMA score, TOPIQ-Face score, kích thước ảnh ...
    - ***Nâng cao chất lượng ảnh huấn luyện:*** đối với các ảnh huấn luyện có chất lượng thấp, sử dụng mô hình SUPIR để nâng cao chất lượng ảnh.
    - ***Tăng cường dữ liệu (data augmentation):*** sử dụng GEMINI để tạo ra các biến thể khác nhau về cảm xúc (cười mỉm, cười to, mặt trung tính, giận dữ) của ảnh huấn luyện.
    - ***Tạo các cặp caption - image để huấn luyện mô hình text-to-image:*** sử dụng LLaVA để tạo caption mô tả ảnh dựa trên ảnh gốc.
    - ***Tạo mask phần chân dung:*** sử dụng Segment Anything Model (SAM) để tạo mask phần chân dung trong ảnh, giúp mô hình tập trung vào khu vực quan trọng khi huấn luyện.

- **Huấn luyện và dự đoán mô hình:**
    - ***Sử dụng các open-source pretrained model về text-to-image:*** Stable Diffusion 3.5, FLUX.1-dev, Qwen Image, FLUX.2-dev làm mô hình nền tảng.
    - ***Tinh chỉnh (fine-tune):*** các mô hình này sử dụng kỹ thuật LoRA (Low-Rank Adaptation).
    - ***Chuẩn bị bộ prompt:*** sử dụng ChatGPT để tạo ra khung prompt mẫu đa dạng, bổ sung thêm các thông tin về giới tính, độ tuổi, chủng tộc, đeo kính ... của người dùng vào prompt mẫu để tạo thành prompt đầu vào cho mô hình.

- **Tối ưu giải pháp:**
    - ***Tăng tốc tốc độ huấn luyện:*** đánh giá loss và metrics để điều chỉnh learning rate phù hợp (từ 1e-4 mặc định lên 1e-3)
    - ***Giảm overfit cho mô hình bằng cách giảm số lượng step huấn luyện (từ 4000 xuống 1000):*** sau khi thấy mô hình đã học cả trang phục và background từ ảnh huấn luyện thay vì chỉ mỗi khuôn mặt, chất lượng ảnh đầu ra mờ và nhiễu dần tăng theo số step huấn luyện.
    - ***Giảm overfit cho mô hình bằng cách giảm rank của LoRA (từ 32 xuống 4):*** sau khi thấy mô hình có dấu hiệu ghi nhớ ảnh huấn luyện thay vì học cách sinh ảnh mới.
    - ***Bổ sung thêm mask chân dung vào quá trình huấn luyện:*** giúp mô hình tập trung học các đặc trưng quan trọng của khuôn mặt, tránh bị phân tán bởi các chi tiết không liên quan trong ảnh.

### 9.3. Công nghệ sử dụng

- Ngôn ngữ lập trình và thư viện: Python, PyTorch, Hugging Face Diffusers, ai-toolkit.
- Mô hình hỗ trợ đánh giá chất lượng: ArcFace, NIMA, TOPIQ-Face, CLIP, FID, LPIPS.
- Mô hình hỗ trợ chuẩn bị dữ liệu: SUPIR, GEMINI, LLaVA, Segment Anything Model (SAM), ChatGPT.
- Mô hình image generation nền tảng: Stable Diffusion 3.5, FLUX.1-dev, Qwen Image, FLUX.2-dev, LoRA.
- Công cụ quản lý dữ án: ClearML, JIRA, GitHub.

## 8. Photo - Footage - Illustration Auto Review System (2023 - 2025)

### 8.1. Mô tả dự án

### 8.2. Vai trò và trách nhiệm

Trong dự án Auto Review, năm đầu 2023, mình tham gia dự án với vai trò Machine Learning Engineer, thời gian còn lại từ cuối 2023 đến 2025, mình đảm nhận vai trò AI Technical Leader.
Do đó, mình vừa đóng góp về mặt kỹ thuật chuyên môn vừa hỗ trợ các bạn thành viên khác trong dự án trong việc triển khai các công việc.

Ngoài các thành viên team AI, dự án còn có sự tham gia của các thành viên Software Engineer và Infrastructure Engineer.
Mình đóng trò chơi cầu nối giữa team AI và các team khác để đảm bảo tiến độ và chất lượng dự án.

#### Về mặt kỹ thuật

- **Thấu hiểu và chuyển đổi từ các yêu cầu nghiệp vụ review ảnh thành các bài toán Machine Learning cụ thể.**

- **Chuẩn bị bộ dữ liệu đánh giá và kiểm thử:**
    - ***Bộ dữ liệu đánh giá:*** bao gồm các ảnh có lỗi và không lỗi, được sử dụng để phân tích đặc điểm các lỗi mà người dùng thường gặp khi tải ảnh lên hệ thống, từ đó cải thiện các mô hình.
    Các chỉ số đánh giá bao gồm precision, recall trên từng loại lỗi cụ thể.
    - ***Bộ dữ liệu kiểm thử:*** bao gồm các ảnh có lỗi và không lỗi, được xây dựng dựa trên phân phối dữ liệu thực tế, giúp mô phỏng được độ chính xác của hệ thống các mô hình khi triển khai thực tế.
    Các chỉ số đánh giá bao gồm precision, recall trên từng loại lỗi cụ thể.
    - ***Đối với dữ liệu video:*** sử dụng ffmpeg để resize sử dụng bản nhỏ của video nhằm tiết kiệm tài nguyên tính toán và trích xuất frame theo giây để đại diện cho nội dung video.
    - ***Đối với dữ liệu âm thành từ video:*** sử dụng ffmpeg để trích xuất âm thanh từ video.
    - Metadata: sử dụng các thông tin metadata đi kèm ảnh / video như tag, title, description để hỗ trợ các mô hình machine learning trong việc đưa ra dự đoán chính xác hơn.

- **Sử dụng các open-source pretrained model và các closed-source API:**
    - ***Bài toán phân lớp thương hiệu trong ảnh:*** sử dụng pretrained model BLIP-2, Qwen2-VL và API của Google Gemini để xác định xem ảnh có chứa thương hiệu hay không.
    - ***Bài toán phát hiện đối tượng trong ảnh:*** sử dụng pretrained model Detic để xác định một số đối tượng như ô tô, tàu thuyền, máy bay ... trong ảnh, trước khi xác định biển số trên các phương tiện.
    - ***Bài toán phát hiện ảnh / video trùng lặp:*** sử dụng perceptual hashing (pHash) để tính toán độ tương đồng giữa các ảnh / video, từ đó phát hiện các ảnh / video trùng lặp.
    - ***Bài toán phát hiện khuôn mặt trong ảnh:*** sử dụng pretrained model từ InsightFace và YoloFace để phát hiện khuôn mặt, bổ trợ lẫn nhau nhằm tăng độ chính xác.
    - ***Bài toán optical flow cho video:*** sử dụng pretrained model RAFT để tính toán optical flow giữa các frame liên tiếp trong video, từ đó phát hiện các hiện tượng như rung lắc, mờ chuyển động.

- **Chuẩn bị bộ dữ liệu và huấn luyện mô hình:**
    - ***Bài toán phân lớp ảnh không phù hợp (NSFW):*** kết hợp giữa các bộ dữ liệu công khai trên internet và bộ dữ liệu nội bộ của Pixta để huấn luyện mô hình image classification dựa trên kiến trúc Vision Transformer (ViT).
    - ***Bài toán phát hiện thẻ tín dụng và tiền mặt trong ảnh:*** sử dụng kỹ thuật image processing để cắt ghép hình ảnh thẻ tín dụng và tiền mặt vào các ảnh nền khác nhau, từ đó tạo bộ dữ liệu tổng hợp để huấn luyện mô hình object detection dựa trên kiến trúc YOLOv7.
    - ***Bài toán phát hiện khuôn mặt trong ảnh:*** sử dụng và tiền xử lý bộ dữ liệu độ phân giải cao nội bộ của Pixta kết hợp với bộ dữ liệu công khai WIDERFACE theo chiến lược SNIPER và AutoFocus để huấn luyện mô hình face detection dựa trên kiến trúc RetinaFace.
    - ***Bài toán phân lớp ảnh bị cắt xén (trimming):*** sử dụng kỹ thuật image processing để tạo bộ dữ liệu ảnh bị cắt xén từ các ảnh gốc, từ đó huấn luyện mô hình image classification dựa trên kiến trúc Vision Transformer (ViT).
    - ***Bài toán phân lớp ảnh bị xoay (orientation):*** sử dụng kỹ thuật image processing để tạo bộ dữ liệu ảnh bị xoay từ các ảnh gốc, từ đó huấn luyện mô hình image classification dựa trên kiến trúc Vision Transformer (ViT).
    - ***Bài toán phân lớp ảnh AI-generated:*** sử dụng bộ dữ liệu nội bộ của Pixta kết hợp với bộ dữ liệu công khai được crawl từ các website chia sẻ ảnh AI-generated để huấn luyện mô hình image classification dựa trên kiến trúc Vision Transformer (ViT).
    - ***Bài toán chấm điểm chất lượng ảnh tổng thể:*** sử dụng bộ dữ liệu nội bộ của Pixta với các metadata và openCLIP vector được lưu trữ ở vector database Milvus để đánh giá điểm chất lượng ảnh bằng mô hình logistic regression.

- **Triển khai mô hình và tối ưu hiệu suất:**
    - ***Đóng gói các mô hình:*** các mô hình được triển khai dưới dạng API sử dụng TritonAPI (đối với mô hình deep learning) và FastAPI (đối với mô hình còn lại), và được đóng gói trong container Docker để dễ dàng triển khai.
    - ***Quy trình CI/CD:*** sử dụng GitHub Actions và AWS ECR để tự động hóa quy trình build, test và deploy các mô hình lên môi trường production.
    - ***Quản lý và giám sát mô hình:*** sử dụng ClearML để quản lý phiên bản mô hình và lưu trữ logs, Prometheus và Grafana để giám sát hiệu suất và tài nguyên sử dụng của các mô hình, AWS EKS để triển khai và quản lý hạ tầng, Sentry để theo dõi lỗi và sự cố trong quá trình vận hành.
    - ***Tối ưu thời gian chạy hệ thống:*** theo dõi số lượng request, lập lịch và cài đặt autoscaling trên AWS EKS bằng KEDA để đảm bảo hệ thống có thể xử lý tải cao mà không bị quá tải hoặc lãng phí tài nguyên khi tải thấp dựa vào các metrics từ HA Proxy.
    - ***Xây dựng pipeline vận hành tất cả các mô hình:*** xây dựng pipeline nhận dữ liệu từ AWS SQS và AWS S3, sau đó tuần tự gọi các mô hình để xử lý ảnh / video, cuối cùng lưu kết quả vào cơ sở dữ liệu PostgreSQL.

### 8.3. Công nghệ sử dụng

- Ngôn ngữ lập trình và thư viện: Python, PyTorch, Hugging Face, FastAPI, TritonAPI, FFmpeg.
- Mô hình open-source pretrained và closed-source API: BLIP-2, Qwen2-VL, Google Gemini, Detic, InsightFace, YoloFace, RAFT, Vision Transformer (ViT), YOLOv7, RetinaFace.
- Dữ liệu: bộ dữ liệu nội bộ của Pixta, WIDERFACE, hình ảnh từ Google/Bing search.
- Công cụ quản lý dự án và mô hình: ClearML, JIRA, GitHub.
- Hạ tầng triển khai: Docker, AWS ECR, AWS EKS, Prometheus, Grafana, Sentry, PostgreSQL, AWS S3, AWS SQS, KEDA, HA Proxy.
