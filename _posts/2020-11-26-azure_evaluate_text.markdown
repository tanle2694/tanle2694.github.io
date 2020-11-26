---
layout: post
comments: true
title:  "Đánh giá văn bản với Azure Cognitive Language Services"
title2:  "Evaluate text with Azure Cognitive Language Services"
date:   2020-11-26 00:22:00
permalink: 2020/11/26/azure-evaluate-text/
mathjax: true
img: /assets/introduce/aimldl.png
summary: Những năm gần đây dfadfadfadf
---

Nguồn được dịch lại tại [đây](https://docs.microsoft.com/en-us/learn/modules/classify-and-moderate-text-with-azure-content-moderator/1-introduction)


## Mục tiêu 
Trong module này chúng ta sẽ học về:
- **Text content moderation** là gì?
- Các tính năng chính của Azure Content Moderator cho bài toán Text moderation
- Test text moderation với API test console
  
## Tổng quan về text moderation

Khi sử dụng content moderation, với một nội dung ta có thể block, approve hoặc review nó dựa trên các chính sách(policies)
hoặc ngưỡng(thresholds). Bạn có thể dùng trợ lý máy móc để tăng cường kiểm soát nội dung mà các partners, nhân viên, hoặc 
người tiêu dùng viết ra. Các môi trường có thể kiểm duyệt bao gồm:
- Chat rooms
- Discussion boards 
- Chatbots 
- E-commerce catalogs 
- Documents 
 
Kết quả trả lại từ Text moderation API bao gồm các thông tin:
- Một list các từ không mong muốn tìm thấy trong đoạn text.
- Kiểu của các từ không mong muốn được tìm lấy là gì.
- Thông tin nhận dạng các nhân được tìm thấy trong đoạn text(Possible personally identifiable information)

## Profanity 
Khi ta đưa text vào API, các từ tục tĩu trong text được định danh và trả lại trong một JSON response. Các từ này được 
trả lại với tên là *Term* trong JSON response, cùng với giá trị index là vị trí của từ trong đoạn text.

Ta có thể sử dụng một danh sách các thuật ngữ với API này. Trong trường hợp nếu một từ ngữ không tốt được định danh trong 
text, một *Listid* cũng được trả lại để định danh từ tùy chỉnh cụ thể. 

<div class="codeHeader" id="code-try-0" data-bi-name="code-header">
    <span class="language">
         Json Example
    </span>
    
</div>
			
<pre tabindex="0" class="has-inner-focus">
    <code class="lang-json" data-author-content="&quot;Terms&quot;: [
    {
        &quot;Index&quot;: 118,
        &quot;OriginalIndex&quot;: 118,
        &quot;ListId&quot;: 0,
        &quot;Term&quot;: &quot;crap&quot;
    }
    "><span><span class="hljs-string">"Terms"</span>: [
    {
        <span class="hljs-attr">"Index"</span>: <span class="hljs-number">118</span>,
        <span class="hljs-attr">"OriginalIndex"</span>: <span class="hljs-number">118</span>,
        <span class="hljs-attr">"ListId"</span>: <span class="hljs-number">0</span>,
        <span class="hljs-attr">"Term"</span>: <span class="hljs-string">"crap"</span>
    }
    </span>
    </code>
</pre>

## Classification 
Tính năng này của API có thể phân loại text vào một số categories dựa trên một số đặc điểm:
- *Category 1*: Rõ ràng là khiêu dâm hoặc nội dung người lớn.
- *Category 2*: Sexual suggestiveness(ví dụ như phụ nữ mặc đồ bơi)
- *Category 3*: Nội dung xúc phạm 

<div class="codeHeader" id="code-try-0" data-bi-name="code-header">
    <span class="language">
         Ví dụ
    </span>
    
</div>
			
<pre tabindex="0" class="has-inner-focus"><code class="lang-json" data-author-content="&quot;Classification&quot;: {
    &quot;ReviewRecommended&quot;: true,
    &quot;Category1&quot;: {
        &quot;Score&quot;: 0.99756889843889822
        },
    &quot;Category2&quot;: {
        &quot;Score&quot;: 0.12747249007225037
        },
    &quot;Category3&quot;: {
        &quot;Score&quot;: 0.98799997568130493
    }
}
"><span><span class="hljs-string">"Classification"</span>: {
    <span class="hljs-attr">"ReviewRecommended"</span>: <span class="hljs-literal">true</span>,
    <span class="hljs-attr">"Category1"</span>: {
        <span class="hljs-attr">"Score"</span>: <span class="hljs-number">0.99756889843889822</span>
        },
    <span class="hljs-attr">"Category2"</span>: {
        <span class="hljs-attr">"Score"</span>: <span class="hljs-number">0.12747249007225037</span>
        },
    <span class="hljs-attr">"Category3"</span>: {
        <span class="hljs-attr">"Score"</span>: <span class="hljs-number">0.98799997568130493</span>
    }
}
</span></code></pre>

## Personally identifiable information(PII)

Định danh thông tin(PPI) có một tầm quan trọng to lớn trong một số ứng dụng. Tính năng này của API có thể giúp bạn 
phát hiện nếu có bất kỳ một thông tin nào trong text được coi là PII. Các thông tin chính mà có thể được phát hiện bao gồm:
- Địa chỉ Email
- Địa chỉ mail US
- Địa chỉ IP 
- Số điện thoại US 
- Số điện thoại UK 
- Số an sinh xã hội(Social Security)

Nếu có thông tin PII được tìm thấy, JSON response sẽ bao gồm các thông tin liên quan bao gồm text và thứ tự trong đoạn text
ban đầu. 

Ví dụ:
<pre tabindex="0" class="has-inner-focus"><code class="lang-json" data-author-content="&quot;PII&quot;: {
    &quot;Email&quot;: [{
        &quot;Detected&quot;: &quot;abcdef@abcd.com&quot;,
        &quot;SubType&quot;: &quot;Regular&quot;,
        &quot;Text&quot;: &quot;abcdef@abcd.com&quot;,
        &quot;Index&quot;: 32
        }],
    &quot;IPA&quot;: [{
        &quot;SubType&quot;: &quot;IPV4&quot;,
        &quot;Text&quot;: &quot;255.255.255.255&quot;,
        &quot;Index&quot;: 72
        }],
    &quot;Phone&quot;: [{
        &quot;CountryCode&quot;: &quot;US&quot;,
        &quot;Text&quot;: &quot;5557789887&quot;,
        &quot;Index&quot;: 56
        }, {
        &quot;CountryCode&quot;: &quot;UK&quot;,
        &quot;Text&quot;: &quot;+44 123 456 7890&quot;,
        &quot;Index&quot;: 208
        }],
    &quot;Address&quot;: [{
        &quot;Text&quot;: &quot;1 Microsoft Way, Redmond, WA 98052&quot;,
        &quot;Index&quot;: 89
        }],
    &quot;SSN&quot;: [{
        &quot;Text&quot;: &quot;999-99-9999&quot;,
        &quot;Index&quot;: 267
        }]
    }
"><span><span class="hljs-string">"PII"</span>: {
    <span class="hljs-attr">"Email"</span>: [{
        <span class="hljs-attr">"Detected"</span>: <span class="hljs-string">"abcdef@abcd.com"</span>,
        <span class="hljs-attr">"SubType"</span>: <span class="hljs-string">"Regular"</span>,
        <span class="hljs-attr">"Text"</span>: <span class="hljs-string">"abcdef@abcd.com"</span>,
        <span class="hljs-attr">"Index"</span>: <span class="hljs-number">32</span>
        }],
    <span class="hljs-attr">"IPA"</span>: [{
        <span class="hljs-attr">"SubType"</span>: <span class="hljs-string">"IPV4"</span>,
        <span class="hljs-attr">"Text"</span>: <span class="hljs-string">"255.255.255.255"</span>,
        <span class="hljs-attr">"Index"</span>: <span class="hljs-number">72</span>
        }],
    <span class="hljs-attr">"Phone"</span>: [{
        <span class="hljs-attr">"CountryCode"</span>: <span class="hljs-string">"US"</span>,
        <span class="hljs-attr">"Text"</span>: <span class="hljs-string">"5557789887"</span>,
        <span class="hljs-attr">"Index"</span>: <span class="hljs-number">56</span>
        }, {
        <span class="hljs-attr">"CountryCode"</span>: <span class="hljs-string">"UK"</span>,
        <span class="hljs-attr">"Text"</span>: <span class="hljs-string">"+44 123 456 7890"</span>,
        <span class="hljs-attr">"Index"</span>: <span class="hljs-number">208</span>
        }],
    <span class="hljs-attr">"Address"</span>: [{
        <span class="hljs-attr">"Text"</span>: <span class="hljs-string">"1 Microsoft Way, Redmond, WA 98052"</span>,
        <span class="hljs-attr">"Index"</span>: <span class="hljs-number">89</span>
        }],
    <span class="hljs-attr">"SSN"</span>: [{
        <span class="hljs-attr">"Text"</span>: <span class="hljs-string">"999-99-9999"</span>,
        <span class="hljs-attr">"Index"</span>: <span class="hljs-number">267</span>
        }]
    }
</span></code></pre>

## Create and subscribe to a Content Moderator resource

### Create and subscribe to a Content Moderator resource
1. Đăng nhập vào [Azure portal](https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize?client_id=c44b4083-3bb0-49c1-b47d-974e53cbdf3c&response_type=code%20id_token&scope=https%3A%2F%2Fmanagement.core.windows.net%2F%2Fuser_impersonation%20openid%20email%20profile&state=OpenIdConnect.AuthenticationProperties%3DOP8ctPPZyTW4Lo--cV91bwin5gxNnjf7hTzuOInEHMjd9Gstt_AmItVcvNrKJABY8lzNFUG-UtrBmclbZ7b3MMxHXajSPkDdXW3Yn3aXbO1egw3soPvgdktFbmR8rDQZdyBFZAaEnzuu1ENoR3XcnEJHFYzc-394N8VynYkNz0A5aghCfGwwhpEaHwqfNbVRv9i6SVWjO-FSvX8qOmD75LA6k4eC7gAlO0_5MqiT-x7ig0vWjDl8cw6baZFuSH6xRH5zny2zUpeibxaDWbSQpMObfUv4CpL87G5_QDj94lL5mby4JEuuIuxhjNkkRh1otJ6mLNiwL3IecQdwNrTcyw6NiDLcGRFtS4dl1G-s8G3rA_8DVNFVBo-6k2EzVeCx&response_mode=form_post&nonce=637420158773489121.NDdkMDRjZWUtOTM0YS00ZGQwLWFlZmQtNTgyOWI0ODM1YWU3OTE4MjQ2OTEtN2Q4Zi00NjVhLWE2OWQtNjBiZWEyYWJjMWI2&redirect_uri=https%3A%2F%2Fportal.azure.com%2Fsignin%2Findex%2F&site_id=501430&client-request-id=c6e11265-8f84-414c-b550-acb90bac7695&x-client-SKU=ID_NET45&x-client-ver=5.3.0.0)
2. 