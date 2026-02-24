---
time: 10/02/2025
title: Diffusion Model trong bài toán Image Generation
description: Diffusion models là một lớp mô hình sinh dữ liệu (generative models) học cách biến nhiễu Gauss thành dữ liệu thực. Nếu ta dần dần thêm nhiễu lên dữ liệu thật cho đến khi nó trở thành nhiễu trắng (forward / noising process), thì học ngược lại (reverse / denoising process) là học một chuỗi phân phối để đi từ nhiễu về dữ liệu ban đầu. Khi mô hình học tốt, ta có thể bắt đầu từ nhiễu chuẩn và lặp lại quá trình ngược để sinh mẫu mới.
banner_url: 
tags: [deep-learning, computer-vision, image-generation]
is_highlight: false
is_published: false
---

## 1. Tóm tắt về Chuỗi Markov

### Ý tưởng chính

Một chuỗi Markov (Markov chain) là một mô hình toán học mô tả một quá trình ngẫu nhiên có trạng thái rời rạc, trong đó xác suất chuyển sang trạng thái tiếp theo chỉ phụ thuộc vào trạng thái hiện tại, không phụ thuộc vào toàn bộ quá khứ.

Nói cách khác: tương lai chỉ phụ thuộc vào hiện tại, chứ không phụ thuộc vào quá khứ.
Đây được gọi là tính chất Markov.

### Công thức

Cho tập các trạng thái hữu hạn hoặc đếm được $S = \{s_1, s_2, \ldots, s_n\}$.

Một quá trình ngẫu nhiên $\{X_t\}_{t=0}^{\infty}$ được gọi là chuỗi Markov nếu với mọi $t \geq 0$ và mọi trạng thái $s_0, s_1, \ldots, s_{t+1} \in S$, ta có:

$$ P(X_{t+1} = s_{t+1} | X_t = s_t, X_{t-1} = s_{t-1}, \ldots, X_0 = s_0) = P(X_{t+1} = s_{t+1} | X_t = s_t) $$

Công thức trên chính là cách viết toán học của định nghĩa "tương lai chỉ phụ thuộc vào hiện tại, không phụ thuộc vào quá khứ".
- Vế trái: xác suất đến trạng thái $s_{j}$ ở bước $t+1$, khi biết toàn bộ lịch sử từ bước 0 đến $t$.
- Vế phải: xác suất đến trạng thái $s_{j}$ ở bước $t+1$, chỉ cần biết trạng thái hiện tại $s_{i}$.
- Nếu 2 vế bằng nhau, tức là thông tin về các trạng thái trước đó $(X_{t-1}, \ldots, X_0)$ không mang thêm giá trị ngoài việc biết $X_{t}$. 
Đây chính là tính chất Markov.

### Thành phần của chuỗi Markov

1. **Trạng thái (States)**: Tập hợp các trạng thái mà hệ thống có thể ở trong đó.
2. **Xác suất chuyển trạng thái (Transition Probabilities)**: Ma trận xác suất chuyển trạng thái, trong đó mỗi phần tử $P_{ij}$ biểu thị xác suất chuyển từ trạng thái $s_i$ sang trạng thái $s_j$.

$$ P_{ij} = P(X_{t+1} = s_j | X_t = s_i) $$

với $P_{ij} \geq 0$ và $\sum_{j} P_{ij} = 1$ cho mọi $i$.

## 2. Diffusion Models

Diffusion models là một lớp mô hình sinh dữ liệu (generative models) học cách biến nhiễu Gauss thành dữ liệu thực.

Nếu ta dần dần thêm nhiễu lên dữ liệu thật cho đến khi nó trở thành nhiễu trắng (forward / noising process), thì học ngược lại (reverse / denoising process) là học một chuỗi phân phối để đi từ nhiễu về dữ liệu ban đầu.

Khi mô hình học tốt, ta có thể bắt đầu từ nhiễu chuẩn và lặp lại quá trình ngược để sinh mẫu mới.

