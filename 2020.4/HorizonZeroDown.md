# Horizon Zero Dawn Volumetric Cloudscapes

2015年siggraph分享。之后的大表哥2的云依赖于此文章。一眨眼，居然已经是5年前的游戏了，当年ps4万起来惊为天人的游戏，没想到现在要开始分析了实现了，letsgo搞起来



最初他们尝试了houdini中制作云的形状然后烘焙成billboard的方式。光照信息使用球鞋函数在houdini中一起制作，但是问题是这样不够灵活，且需要制作不同的角度的球鞋信息，过于麻烦

于是尝试基于噪声动态生成。



1. To explain it better I have broken it down into 4 sections: Modeling, Lighting, Rendering and Optimization.

2. modeling

   1. 思路刚开始用fbm，perlin noise。但是perlin noise的问题是在wispy的部分还可以，但是么有那种突出的感觉，像菜花那种的部分不太行，所以换了一个noise函数。worley
   2. 使用worley offset perlin noise，这样保持perlin的连续性，同时dilate the noise
   3. 使用2*  3d texture 1* 2dtexture来保存noise，few textures、low resolution
   4. So to sum up our modeling approach…

   we follow the standard ray-march/ sampler framework

   but we build the clouds with two levels of detail

   a low frequency cloud base shape

   and high frequency detail and distortion 

   Our noises are custom and made from Perlin, Worley and Curl noise

   We use a set of presets for each cloud type to control density over height and cloud coverage

   These are driven by our weather simulation or by custom textures for use with cutscenes

   and it is all animated in a given wind direction.

3. lighting部分
   
   1. 

今日工作内容：

1.阅读Real-Time_Volumetric_Cloudscapes_of_Horizon_Zero_Dawn，实现modeling & shading部分。 doing

明日工作内容：

继续1



完成了体积云raymarch和着色的部分，达到和地平线ppt接近效果。目前问题是需要采样数比较多。今天准备参展寒霜引擎体积渲染的解决方案，用taa的来降低采样点。这个做完了开始做预计算大气散射。



1.配置win环境。编译ekwai。1d
2.熟悉编辑器。制作测试素材。2d
3.调研unity hdrp体积光、物理天空盒部分代码。1d



体积云生成的密度、湿度信息，用taa和clipaabb优化了一下，raymarch的sample数会降低掉16，效果还可以。