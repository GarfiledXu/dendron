---
id: 5ewz3du2u67wtgyyrzofdxp
title: Yuv
desc: ''
updated: 1706842599265
created: 1706791896144
---

##### 为什么是 4:2:0? 有没有4:1:1? 这个三分比值到底是什么?是否有四分比值?
1. 首先明确的是，三分比值，肯定不是 y:u:v / Y:Cb:Cr的比值
2. 引用 wiki 描述:
    
        Sampling systems and ratios
        The subsampling scheme is commonly expressed as a three-part ratio J:a:b (e.g. 4:2:2) or four parts, if alpha channel is present (e.g. 4:2:2:4), that describe the number of luminance and chrominance samples in a conceptual region that is J pixels wide and 2 pixels high. The parts are (in their respective order):

        J: horizontal sampling reference (width of the conceptual region). Usually, 4.
        a: number of chrominance samples (Cr, Cb) in the first row of J pixels.
        b: number of changes of chrominance samples (Cr, Cb) between first and second row of J pixels. Note that b has to be either zero or equal to a (except in rare irregular cases like 4:4:1 and 4:2:1, which do not follow this convention).
        Alpha: horizontal factor (relative to first digit). May be omitted if alpha component is not present, and is equal to J when present.
        This notation is not valid for all combinations and has exceptions, e.g. 4:1:0 (where the height of the region is not 2 pixels, but 4 pixels, so if 8 bits per component are used, the media would be 9 bits per pixel) and 4:2:1.

        The mapping examples given are only theoretical and for illustration. Also note that the diagram does not indicate any chroma filtering, which should be applied to avoid aliasing. To calculate required bandwidth factor relative to 4:4:4 (or 4:4:4:4), one needs to sum all the factors and divide the result by 12 (or 16, if alpha is present).

        抽样系统和比例
        子采样方案通常表示为三部分比率J : a : b（例如 4:2:2）或四部分（如果存在 alpha 通道）（例如 4:2:2:4），描述了J像素宽、2 像素高的概念区域中的亮度和色度样本。这些部分是（按各自的顺序）：

        J：水平采样参考（概念区域的宽度）。通常，4。
        a ：第一行J像素中的色度样本 ( Cr , Cb )数量。
        b ：第一行和第二行J像素之间色度样本（ Cr，Cb ）的变化数量。请注意，b必须为零或等于a（除了罕见的不规则情况，如 4:4:1 和 4:2:1，它们不遵循此约定）。
        Alpha：水平因子（相对于第一位数字）。如果 alpha 分量不存在，则可以省略；如果存在，则等于J。
        此表示法并非对所有组合都有效，并且有例外，例如 4:1:0（其中区域的高度不是 2 像素，而是 4 像素，因此如果每个组件使用 8 位，则媒体将为每 9 位像素）和 4:2:1。

        给出的映射示例仅是理论上的并用于说明。另请注意，该图未指示任何色度过滤，应应用色度过滤以避免混叠。要计算相对于 4:4:4（或 4:4:4:4）的所需带宽系数，需要将所有系数相加，并将结果除以 12（或 16，如果存在 alpha）。

##### 什么是采样?: 按规则从目标样本上获取数据
采集到RGB 原图像 => encode to YUV (对原图像以像素为单位，对像素的亮度，色度分别进行采样编码) => decode to RGB后图像
以像素为单位，目标图像为样本，在连续的数据范围内，按一定规则(频率或者方式)去获取离散的样本值/数据点
##### yuv色度采样的水平采样和垂直采样?
yuv的采样 目的就是从图像中获取数据，编码为yuv格式
y 的采样是百分百的? 图像的压缩和编码都是按人眼对图像的感知作为依据的，其中 色度 亮度，对亮度最为敏感，通常亮度数据的采样不能减少，否则图像画质将会受到较大的影响


##### 常见采样解释图
#### ref
[wiki: Chroma subsampling 只能说wili牛逼，专业了](https://en.wikipedia.org/wiki/Chroma_subsampling)
[csdn: 4:2:0到底是什么? very nice, 但是对于这个三分比值取值为0，解释的还是不够](https://blog.csdn.net/xueyushenzhou/article/details/40817949)
[Videography FAQ: What do 4:2:2 and 4:2:0 mean?](https://snapshot.canon-asia.com/article/eng/videography-faq-what-do-422-and-420-mean)
[](https://en.wikipedia.org/wiki/Chroma_subsampling)