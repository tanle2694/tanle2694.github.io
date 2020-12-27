---
layout: post
comments: true
title:  "Process and translate speech with Azure Cognitive Speech Services"
title2:  "Process and translate speech with Azure Cognitive Speech Services"
date:   2020-11-26 00:22:00
permalink: 2020/09/26/azure-sp/
mathjax: true
img: /assets/introduce/aimldl.png
summary: 
---
Nguồn tại [đây](https://docs.microsoft.com/en-us/learn/modules/transcribe-speech-input-text/1-introduction) 


## Introduction 

Speech-to-Text là một khía cạnh của service Speech nhằm phiên âm một đoạn audio stream thành text. 
Ứng dụng của bạn có thể hiển thị các đoạn text này đến user hoặc thực hiện một hành động khi có một lệnh đầu vào.
Bạn có thể sử dụng service này hoặc với một SDK client library hoặc một REST API 

Với Speech service, bạn có thể:
- Mở ứng dụng của bạn với mobile, desktop, và web để cung cấp dịch vụ speech-to-text
- Dễ dàng phiên dịch từ nhiều ngôn ngữ thông qua REST inteface.
- Thực hiện hoạt động Text-to-Speech
- Thực hiện nhận diện các thực thể khi tích hợp với Language Understanding(LUIS)

## Learning objectives:
Trong module này, bạn sẽ :
- Học về khả năng của speech-to-text
- Khám phá dịch vụ Speech-to-Text để convert một file âm thanh thành text output 
- Khám phá khả năng convert âm thanh từ microphone thành text output

## Overview of speech-to-text 
Speech-to-text là một khía cạnh của Speech service trong Azure Cognitive Service, cung cấp khả năng phiên âm realtime các đoạn 
audio stream dựa trên học máy và trí tuệ nhân tạo. Các API của Speech service cho phép developer có thể thêm vào end-to-end, 
real-time phiên âm âm thanh cho các ứng dụng hoặc dịch vụ của họ.

Speech-to-text services được sử dụng thông qua platform-independent REST-base API hoặc Speech SDK. Các API này cho phép 
bạn tích hợp các service vòa bất cứ giải pháp nào yêu cầu speech translation. Cách bạn truy cập vào các services này sẽ thay 
đổi dựa trên liệu bạn sử dụng REST API hay Speech SDK. Bài thực hành này sẽ sử dụng Speech SDK

Speech service được thiết kế để thực hiện real-time speech-to-text cho các kịch bản như:
- Phiên dịch cho các buổi thuyết trình trực tiếp 
- Giao tiếp từ xa 
- Hỗ trợ khách hàng 
- Bussiness intelligence 
- Phụ đề các nội dung giải trí 
- Multilingual AI interactions

Speech-to-text service mặc định sử dụng Universal language model. Model này được train bởi data của Microsoft và được deploy 
trên cloud. Nó được tối ưu cho các kịch bản về đối thoại và chính tả. Khi sử dụng speech-to-text để nhận dạng và phiên dịch trong
một môi trường khác lạ, bạn có thể tạo và traing một model khác. Custom là hữu ích để giải quyết noise hoặc sử dụng các từ 
chuyên ngành.
