STCostAggregation
=================


    ST（segment tree）确实是基于NLCA的改进版本，
    本文的算法思想是：
        基于图像分割，
        采用和NLCA同样的方法对每个分割求取子树，
        然后根据贪心算法，将各个分割对应的子树进行合并，
        算法核心是其复杂的合并流程。
    但是这个分割不是简单的图像分割，其还是利用了最小生成树（MST）的思想，
    对图像进行处理，在分割完毕的同时，每个分割的MST树结构也就出来了。
    然后将每个子树视为一个个节点，在节点的基础上继续做一个MST，
    因此作者号称ST是 分层MST，还是比较贴切的！

    算法是基于NLCA的，那么ST和NLCA比较起来好在哪里？
    作者给出的解释是，NLCA只在一张图上面做一个MST，
    并且edge的权重只是简单的灰度差的衍生值，这点不够科学，
    比如说，当遇到纹理丰富的区域时，这种区域会导致MST的构造出现错误
    ，其实想想看的确是这样，如果MST构造的不好，自然会导致视差值估计不准确。
    而ST考虑了一个分层的MST，有点“由粗到精”的意思在里面。
    
    ST是NLCA的扩展，是一种非传统的全局算法，
    与NLCA唯一的区别就在于ST在创建MST的时候，引入了一个判断条件，使其可以考虑到图像的区域信息。
    这点很新颖，说明作者阅读了大量的文章，并且组合能力惊人，组合了MST和基于图表示的图像分割方法。
    它的运行时间虽然比NLCA要大一些，但是相比较全局算法，速度已经很可以了。
    但是ST也有一些缺陷，首先，算法对图像区域信息的考虑并不是很严谨，
    对整幅图像用一个相同的判断条件进行分割，分割效果不会好到哪里去，
    并且基于实际数据实测，会发现视差图总是出现“白洞”，说明该处的视差值取最大，
    这也是区域信息引入的不好导致的。虽然在细节上略胜于NLCA，但是算法耗时也有所增加。
    上述原因也是本文引用率不佳的原因。



This project provides a reference implementation of the following paper:

* Xing Mei, Xun Sun, Weiming Dong, Haitao Wang and Xiaopeng Zhang. Segment-Tree based Cost Aggregation for Stereo Matching, in CVPR 2013.

Project URL: http://xing-mei.net/resource/page/segment-tree.html

The code is written by Yan Kong, please email comments and bug reports to kongyanwork@gmail.com.

How to Run:  
-------------------

* Use CMake to generate the project files.   
* Build the project.  
* Run "STCostAggre" or "STCostAggreTest" in the bin directory.  

Program Description:
-------------------

1.`STCostAggre` - the program which computes the disparity result for a given stereo pair. 

      STCostAggre leftImg rightImg dispImg [maxLevel] [scale] [sigma] [method]
         leftImg/rightImg: the file path of the left/right image
         dispImg: the output disparity file for the left image
         maxLevel: the maximum disparity level (default 60)
         scale: the scaling parameter for image display (default 4)
         sigma: the threshold parameter for the color distance (default 0.1)
         method: the two aggregation methods (0 for ST-1, 1 for ST-2)
  
2.`STCostAggreTest` - the program which tests `STCostAggre` on the Middlebury data sets. Place 
                     this program and `STCostAggre` in the same directory.


LICENSE
-------------------

Copyright (C) 2012-2013 by Yan Kong

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
