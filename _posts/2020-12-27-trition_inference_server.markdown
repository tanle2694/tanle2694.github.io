---
layout: post
title:  "Triton Inference Server: Deploy and Scaling AI application in production"
title2:  "Triton Inference Server: Deploy and Scaling AI application in production"
date:   2020-12-27 00:20:00
permalink: 2020/12/27/triton/
mathjax: true
summary: 
---

Trong bài viết này, mình sẽ giới thiệu đến mọi người *** *** *** - Một project open source để triển khai các ứng dụng 
AI trong môi trường production một cách nhanh chóng và hiệu quả.

## 1. Tại sao các dự án AI thường thất bại
Vào 20/06/2019, **Pactera Technologies NA Inc.**-một công ty ty công nghệ toàn cầu hàng đầu cộng tác với **Nimdzi Insights**-
một nhà tư vấn có ảnh hưởng đến ngành công nghiệp ngôn ngữ (language services industry) xuất bản một whitepaper tiết lộ 
rằng có 85% dự án AI đã thất bại trước khi được chuyển giao, vận dụng vào bussiness. 

Theo một khảo sát của Forrester-một công ty nghiên cứu thị trường hàng đầu thế giới. Vào năm 2017, có 51% doanh nghiệp 
được khảo sát tuyên bố rằng họ sử dụng AI để giải quyết các vấn đề của mình. Đến năm 2018, con số này tăng thêm 2% là 
53%. Tuy nhiên, đến giữa năm 2019 75% các dự án này đã mộ xanh cỏ. Một con số tương tự cũng được Gartner đưa ra là 
85% các dự án AI đều không đến được tay các nhà đầu tư.

Kịch bản này có vẻ giống với Blockchain. Vào năm 2017, sau khi mới ra trường mình cũng háo hức đi một vài summit để học 
hỏi các kiến thức. Keyword của năm đó là Blockchain, phần lớn các diễn giả ở hội trường chính đều nói về Blockchain. 
Mọi người say sưa vẽ lên một bức tranh tương lai đẹp đẽ về công nghệ này. Và những cái tên mình còn nhớ về các startup 
Blockchain thời đó giờ đã không còn hoạt động nữa.     

Cũng theo các survey, một số vấn đề dẫn đến sự thất bại có thể bao gồm:
- *Lack of MLOps*: AI model chỉ là một phần nhỏ của project, không phải tất cả. Một dự án AI thành công nên có 
MLOps. MLOps không chỉ là cách các các model AI được xây dựng mà còn là cách chúng được quản lý và hoạt động của cả hệ thống. 
- *Unrealistic expectations*: Con người mất hàng thập kỷ để biết cách xử lý và tích lũy kinh nghiệm về một vấn đề nào đó.
Nên kỳ vọng vào việc AI có thể vượt khả năng của con người có thể là còn quá sớm với những công nghệ hiện tại. 
- *High accuracy threshold for adoption*: Con người làm việc được phép sai sót còn AI thì không. Một số dự án có một 
tập để làm benchmark riêng, và sau khi đạt được một ngưỡng chính xác cao nhất nào đó(near-human) thì mới release ra 
sản phẩm. Startup sẽ có thể rơi vào tình trạng chưa đi đến chợ đã tiêu hết tiền :D.
- *Lack of focus on business goals*: Kịch bản của các AI project gắn với các startup thất bại thường do thiếu đi yếu tố 
về business. Họ quá tập trung vào việc sản phẩm này làm được gì mà quên mất sản phẩm này mang đến giá trị gì cho khách
hàng.   
- *Lack of quality data*: More data usually beats better algorithms(by Anand Rajaraman, Datawocky, 2008). Một model 
đạt độ chính xác cao và đạt được business objectives phụ thuộc vào kích thước và chất lượng của dataset. Chỉ sử dụng data 
từ public source thường không đảm bảo được chất lượng cần thiết. ?--



Đầu tiên, giới thiệu qua một số tính năng chính đáng chú ý của nó như sau của nó như sau:
- Multiple deep-learning framework: Không giống như  Tensorflow Serving chỉ hỗ trợ duy nhất các model AI được viết bằng 
Tensorflow, Triton server có thể support các model format phổ biến hiện nay như: TensorRT, Tensorflow Graphdef, Tensorflow SavedModel, 
ONNX, Pytorch Torchscript. 
- Concurrent model execution: Triton hỗ trợ bạn quản lý tài nguyên (GPU) để có thể run đồng thời một lúc nhiều model trên 
hệ thống bao gồm 1 GPU hoặc nhiều GPU. Với cách làm truyền thống thì  
 


Mọi người đã có thể biết đến việc triển khai API với Tensorflow Serving, tuy nhiên công cụ này chỉ có 
thể triển khai với các model AI viết bằng framework Tensorflow. Nhưng trong một số trường hợp researcher sử dụng Pytorch 
để training tạo ra model AI thì sao nhỉ? Chả có lẽ lại convert sang Tensorflow để 