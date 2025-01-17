# Image Quilting
The objective of this project is to implement Texture synthesis and Texture transfer algorithms in Python [1]. The texture synthesis is the porcess of generating a large texture based on a prior texture, and the goal of texture transfer is to re-render `target` image based on `source` texture.

Texture_Synthesis
------------------
In this algorithm. at first, a patch from the texture is chosen randomly. Then, based on SSD template matching, we search for the most suitable patch that can be placed near the previous patch. Also, in order to merge these two patches with each other, *Minimum Error Boundary Cut* is used that can be implemented via dynamic programming. The image below shows the big picture of this algorithm [1]:

<p align="center">
<img width = "500" src="https://user-images.githubusercontent.com/46090276/208299546-2fba1c74-9eb4-4c71-8769-f13703b866bb.JPG" alt="algo">
</p>
  
**Note:** It worths mentioning that if we always choose the patch with minimum SSD, our synthesized texture will have a periodic structure which is not satisfactory. Hence, we try to pick the next patch randomly from a group of patches with the least SSD error. 

Here are some of my results:

Texture             |  Synthesized Texture
:-------------------------:|:-------------------------:
<img width = "150" src="https://user-images.githubusercontent.com/46090276/208292251-d2695d96-9445-43a0-98b4-c32994cba7f5.jpg" alt="texture">|<img width = "400" src="https://user-images.githubusercontent.com/46090276/208293053-091d69d9-2414-4bdc-86d3-6fe9cef30347.jpg" alt="synthesized_texture">|
<img width = "150" src="https://user-images.githubusercontent.com/46090276/208293285-5bddb3d9-17e6-4890-a050-7f67200b9e44.jpg" alt="texture">|<img width = "400" src="https://user-images.githubusercontent.com/46090276/208293239-efbca46f-dbaf-4334-8b4e-e8552b2c141d.jpg" alt="synthesized_texture">|
<img width = "150" src="https://user-images.githubusercontent.com/46090276/208294594-fa412fb9-dc4e-4cca-8d0f-201660d02298.jpg" alt="texture">|<img width = "400" src="https://user-images.githubusercontent.com/46090276/208294598-fdb63d33-74c5-45d0-bf9c-cec535fa4a96.jpg" alt="synthesized_texture">


Texture Transfer
---------------
In this section, we implemenet texture transfer algorithm which is based on texture synthesis algorithm, and we try to generate an image which has the texture of `source` image, and has the characteristics of `target` image. Unlike texture syntehsis algorithm, each patch is choosed based on two criteria:
1. The selected patch has small SSD error with its neighbor patch.
2. The selected patch has the similar luminance (intensity of pixels) to `target` image.
Therefore, we choose the next patch minimizing the error written below:
$$Error = (\alpha) SSD(patch_{prev}, source) + (1-\alpha) SSD(patch_{target}, source)$$
in which $patch_{prev}$ is the last patch of generated image, and $patch_{target}$ is the patch from `target` image that is in the same location of $patch_{prev}$.

In order to get a better result, I run this algorithm many times, and get average from all the generated images. To do so, I use `multiprocessing` in python in order to increase the speed of code. Some of my results are shown below.

Source             |  Target |  Result
:-------------------------:|:-------------------------:|:-------------------------:
<img width = "250" src="https://user-images.githubusercontent.com/46090276/208292251-d2695d96-9445-43a0-98b4-c32994cba7f5.jpg" alt="texture">  |  <img width = "250" src="https://user-images.githubusercontent.com/46090276/208292314-8842dee0-1c72-47d1-bed0-f02484134723.jpg" alt="source">   |   <img width = "250" src="https://user-images.githubusercontent.com/46090276/208292376-5b77ee8d-9ba3-43cb-8149-f8fc11964dd2.jpg" alt="res">|
<img width = "250" src="https://user-images.githubusercontent.com/46090276/208292422-23f1f8ce-8467-4618-9f40-9c8dd241d9d8.jpg" alt="texture">  |  <img width = "400" src="https://user-images.githubusercontent.com/46090276/208292432-6b470567-4edf-48ed-b1ca-1ceba6924aa2.jpg" alt="source">   |   <img width = "400" src="https://user-images.githubusercontent.com/46090276/208292458-19a9c677-c22e-4193-bc86-fb6f7c9d96b6.jpg" alt="res">|
<img width = "250" src="https://user-images.githubusercontent.com/46090276/208297070-c2622868-f124-4076-bcbc-22c8efbcb54d.jpg" alt="texture"> |  <img width = "250" src="https://user-images.githubusercontent.com/46090276/208297106-eb2b902c-17d6-4c25-890c-e38b61e8be0d.jpg" alt="source"> | <img width = "250" src="https://user-images.githubusercontent.com/46090276/208297160-90342d4c-00a4-4ef7-84d1-c68cc67c0fdd.jpg" alt="res">





References
--------------
1. A. A. Efros, and W. T. Freeman, “Image Quilting for Texture Synthesis and Transfer,” SIGGRAPH '01: Proceedings of the 28th annual conference on Computer graphics and interactive techniques, Aug. 2001
