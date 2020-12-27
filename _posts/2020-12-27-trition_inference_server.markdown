---
layout: post
title:  "Triton Inference Server: Deploy and Scaling AI application in production "
title2:  "Triton Inference Server: Deploy and Scaling AI application"
date:   2020-12-27 00:20:00
permalink: 2020/12/27/triton/
mathjax: true
summary: 
---

Trong bài viết này, mình sẽ giới thiệu đến mọi người Triton Inference Server-Một project open source để triển khai các ứng dụng AI trong môi 
trường production. 

Đầu tiên, giới thiệu qua một số tính năng chính đáng chú ý của nó như sau của nó như sau:
- Multiple deep-learning framework: Không giống như  Tensorflow Serving chỉ hỗ trợ duy nhất các model AI được viết bằng 
Tensorflow, Triton server có thể support các model format phổ biến hiện nay như: TensorRT, Tensorflow Graphdef, Tensorflow SavedModel, 
ONNX, Pytorch Torchscript. 
- Concurrent model execution: Triton hỗ trợ bạn quản lý tài nguyên (GPU) để có thể run đồng thời một lúc nhiều model trên 
hệ thống bao gồm 1 GPU hoặc nhiều GPU. Với cách làm truyền thống thì  
 


Mọi người đã có thể biết đến việc triển khai API với Tensorflow Serving, tuy nhiên công cụ này chỉ có 
thể triển khai với các model AI viết bằng framework Tensorflow. Nhưng trong một số trường hợp researcher sử dụng Pytorch 
để training tạo ra model AI thì sao nhỉ? Chả có lẽ lại convert sang Tensorflow để 