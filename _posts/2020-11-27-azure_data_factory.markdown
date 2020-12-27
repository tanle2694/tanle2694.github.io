---
layout: post
comments: true
title:  "Azure Data Factory"
title2:  "Azure Data Factory"
date:   2020-11-27 00:22:00
permalink: 2020/11/27/azure_data_factory/
mathjax: true
img: /assets/introduce/aimldl.png
summary: 
---


## Azure Data Factory

Trong thế giới của big data, các data mà không có tổ chức thường được lưu trong các hệ thống lưu trữ có cấu trúc
không có cấu trúc, hoặc các hệ thống lưu trữ khác. Tuy nhiên bản thân raw data không thể cung cấp insight có ý nghĩa để 
phục vụ cho phân tích, data scientists, hoặc đưa ra các quyết định cho business.

Big data yêu cầu một service mà có thể dàn dựng và vận hành các quá trình để có thể chuyển hóa số lượng lớn raw data thành 
các hiểu biết hữu ích cho hoạt động kinh doanh. Azure Data Factory là một dịch vụ quản lý trên cloud được xây dựng cho các 
quá trình phức tạp extract-transform-load(ETL), extract-load-transform(ELT) và tích hợp dữ liệu.

Ví dụ, tưởng tượng rằng một công ty về game thu thập petabytes game log. Và công ty muốn phân tích các logs này để đạt được 
một insight về sở thích người dùng, thông tin game, marketing campaign, 