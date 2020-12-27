---
layout: post
title:  "EfficientDet: Deep dive into the structure"
title2:  "EfficientDet: Deep dive into the structure"
date:   2020-09-26 00:20:00
permalink: 2020/09/26/efficientdet/
mathjax: true
summary: In this post, I 
---

In this post, I do a deep dive into the structure of Efficientdet for object detection, focusing on the model's motivation, design, and architecture. 


Recently, the Google brain team published their EfficientDet model for object detection and segmentation with the goal of building a scalable framework that can be easily applied to other use cases in object detection.The paper concludes that EfficientDet outperforms similar-sized models on benchmark datasets. 


In this blog post, we explore the rationale behind EfficientDet’s creation and how it works.



## Object detection

Before exploring EfficientDet, let's find out about the detection problem. 


In many real-world applications,we want to decrease model size and increase accuracy because of resource constraints. Given real-world resource constraints, model efficiency becomes increasingly important for object detection.


There have been many previous works aiming to develop more efficient detector architectures, such as one-stage detectors, anchor-free detectors or compressing existing models. They usually tend to achieve better efficiency by sacrifice accuracy. Moreover, most previous works only focus on a specific or a small range of resource requirements, but the variety of real-world applications, from mobile devices to datacenter, often demand different resource constraints.


A natural question: Is it possible to build a scalable detection architecture with both higher accuracy and better efficiency across a wide spectrum of resource constraints(from 3B to 300B FLOPS)? 
Paper  aims to tackle this problem by studying various design choices of detector architectures. Based on the one-stage detector paradigm. We examine the design choices for backbone, feature fusion and class/box network with 2 main challenges:


- **Challenge 1**: efficient multi-scale feature fusion: FPN has been widely used for multiscale feature fusion. While fusing different input features, most previous works simply sum them up without distinction. However, since these different input features are at different resolutions, they usually contribute to fused output features unequally. To address this issue, authors propose a highly effective FPN with weight input features called BiFPN.


- **Challenge 2**: model scaling - While previous works mainly rely on bigger backbone network or larger input image sizes for higher accuracy,. Scaling up feature network and box/class prediction network is also critical when taking into account both accuracy and efficiency. Paper proposes a compound scaling method for object detectors which jointly scales up the resolution/depth/width for all backbone, feature network, box/class prediction network.

## BiFPN
### FPN
Recognition objects at vastly different scales is a fundamental challenge in computer vision.

The simplest way can be used is to use  a pyramid of the same image at different scale to detect
objects like the diagram below

<div class="imgcap">
    <div> 
        <img src="/assets/efficientdet/pyramid_image.png" align="center">
        <div class="thecap"> Fig 1: Pyramid image (Source: <a href = "https://arxiv.org/pdf/1612.03144.pdf">Feature Pyramid Networks</a>) <br></div>
    </div>
</div>

The question: why use pyramid image can resolve the challenges about the multi-scale object?

We will see how it works through an example about face detection with [MTCNN](https://arxiv.org/ftp/arxiv/papers/1604/1604.02878.pdf)

In this model, we want to create an **image pyramid**, in order to detect faces at all different sizes.

<div class="imgcap">
    <div> 
        <img src="/assets/efficientdet/mtcnn_image_pyramid.png" align="center">
        <div class="thecap"> Fig 2: MTCNN Pyramid image (Source: <a href = "https://arxiv.org/ftp/arxiv/papers/1604/1604.02878.pdf">Face detection image pyramid</a>) <br></div>               
    </div>
</div>

For each scaled copy, we have a 12x12 kernel that will go through every part of the image, scanning for faces.
The way this kernel go through the image is the same way convolution operation work.
This portion of the image is passed to MTCNN model, which returns the coordinates of a bounding box if it notices a face.

  
<div style="text-align:center;">
    <iframe width="600" height = "400" src="https://www.youtube.com/embed/w4tigQn-7Jw" frameborder="0" allowfullscreen></iframe>
    <div class="thecap">Video: Kernel find bigger faces in smaller-scaled image</div>
</div>

After passing in the image, we convert face bounding box from the scaled image to coordinates at the original image.

<div class="imgcap">
    <div> 
        <img src="/assets/efficientdet/unscaled_image.png" align="center">
        <div class="thecap"> Fig 3: Convert bounding box to the sizes of the original image (Source: <a href = "https://towardsdatascience.com/how-does-a-face-detection-program-work-using-neural-networks-17896df8e6ff">How does a face detection program work</a>) <br></div>               
    </div>
</div>


However, there is a problem. Processing muli-scale images is time consuming and memory demand is too high to be trained 
end to end simultaneously. Hence, it often used in inference to push accuracy as high as possible, in particular for competitions
where speed is not a concern.

Alternatively, we use a pyramid of features for object detection. This in-network feature hierarchy use feature maps at 
different spartial resolution and often used on deep convnet 

<div class="imgcap">
    <div> 
        <img src="/assets/efficientdet/pyramid_feature_hierarchy.png" align="center">
        <div class="thecap"> Fig 4: Pyramid feature hierarchy (Source: <a href = "https://arxiv.org/pdf/1612.03144.pdf">Feature Pyramid network for object detection</a>) <br></div>               
    </div>
</div>


The [Single Short Detector](https://arxiv.org/pdf/1612.03144.pdf) (SSD) is one of the first models use Convenet features 
pyramid hierarchy. SSD will reuse the mult-scale feature maps at different layers computed in forward pass. Thus, the cost
of computation for multi-scale will be decrease compare with image pyramid. 

<div class="imgcap">
    <div> 
        <img src="/assets/efficientdet/ssd.png" align="center">
        <div class="thecap"> Fig 5: SSD model architecture (Source: <a href = "https://arxiv.org/pdf/1512.02325.pdf">SSD paper</a>) <br></div>               
    </div>
</div>

But, an problem appear. The model use a deepl convnet for feature extract and the spartial resolution decrease lead to 
increase the semantic value of feature map. The low-level structures is not effective for accurate object detection. 
This is the reason performs much worse for small objects.

 
<div class="imgcap">
    <div> 
        <img src="/assets/efficientdet/resolution_semantic.png" align="center">
        <div class="thecap"> Fig 6: Relative between resolution and semantic value on feature maps <br></div>               
    </div>
</div>

After that, the FPN(Feature pyramid network) is proposed. FPN provides a top-down pathway and skip connection 
to construct the higher resolution layers from the semantic rich layers. This way helps improve the accurate for object detector.


<div class="imgcap">
    <div> 
        <img src="/assets/efficientdet/FPN.png" align="center">
        <div class="thecap"> Fig 6: FPN network (Source: <a href = "https://arxiv.org/pdf/1612.03144.pdf">Feature Pyramid network for object detection</a>)<br></div>               
    </div>
</div>


The next 

### Problem formulation
Multi-scale feature fusion aims to aggregate features at different resolutions. Formally,
given a list of multi-scale features $ \vec{P}^{in} = (P_{l1}^{in}, P_{l2}^{in}, ...) $


$$ a_2 $$



