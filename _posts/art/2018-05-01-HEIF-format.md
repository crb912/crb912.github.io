---
published: true
title: HEIF文件格式解析
layout: post
author: Eureka912
category: art
hide: ture
---  
  
* 目录
{:toc}
  
## 1. Overview 概述
**High Efficiency Image File Format** (**HEIF**, 发音为：heef)，由 [Moving Picture Experts Group](https://en.wikipedia.org/wiki/Moving_Picture_Experts_Group) ( MPEG，即动态图像专家组) 于2013年开发，它基于ISOBMFF标准。HEIF是一个容器的图片格式，它可以包含图片和图片序列（一个文件可以包含不止一个图片）。当前的编码格式有：`HEVC` 和`H.264/MPEG-4 AVC` 两种，并且未来可能有新的编码格式加入。  

**问题一：HEIF和heic有什么关联呢？**  

**答**：  
*.heic* 只是一个HEIF文件格式的一种扩展名，言外之意是：HEIF不仅有 *.heic* 这种扩展名，还有其它的，比如说：*.heif* 和 *.avci*，它们都是属于HEIF文件格式。当然，常见的只有 *.heif* 和 *.heic* 这两种，而 *.avci* 很少见。  
在苹果的实现中，单个图片采用 .heic 文件扩展名，并且默认指定了它是HEVC编码，它默认使用的都是 HEVC 的编码格式。当然，苹果未来会对 `H.264/MPEG-4 AVC` 编码的 *.avci* 文件进行支持，以及 *.heif* 文件。 这里有苹果官方幻灯片版的一份 PDF文档，附上[下载链接](https://devstreaming-cdn.apple.com/videos/wwdc/2017/513fzgbviu23l/513/513_high_efficiency_image_file_format.pdf?dl=1)。

两种编码格式如下所示:  

![image](http://ww1.sinaimg.cn/large/ce2e9210ly1fpv17och1tj20bs04bq2t.jpg)  
*Figure 1：* 引用自[网络](https://blog.csdn.net/olsQ93038o99S/article/details/78238090)，侵删。
---

## 2. 数字图片的相关概念:
### 2.1 Raster and vector images 位图和矢量图
#### 矢量图(vector image)：
![image](http://ww1.sinaimg.cn/large/ce2e9210ly1fpv1g0ec4tj205x05ka9v.jpg)  
 --line width=1; line from 0,0 to 20,20;
 *Figure 2：* 引用自[heikkila](https://dspace.cc.tut.fi/dpub/bitstream/handle/123456789/24147/heikkila.pdf)，p13.
 
它们都是通过数学公式计算获得的，如上图。由轮廓的方向、曲率、粗细以及轮廓和填充的颜色等数学模型决定的图形叫矢量图，图形可以无限放大，不变色、不模糊。
### 位图(raster image ,bitmap image):
![image](http://ww1.sinaimg.cn/large/ce2e9210ly1fpv1jyr960j205o05jjr5.jpg)  
  -- 5 * 5 的对角矩阵    *Figure 3：* 引用自[heikkila](https://dspace.cc.tut.fi/dpub/bitstream/handle/123456789/24147/heikkila.pdf)，p13.  
  
位图是由称作像素（图片元素）的单个点组成的图像，上图可以用 5 * 5 的对角矩阵去存储，空白记为0，黑色块记为1。HEIF格式只支持位图。
### 2.2 色深(bit depth)
描述每个像素填充信息的参数称为色深，bit depth通常有1 bit ~ 64 bit。特别的,对于只有1bit的.   由0或1组成的双色,通常是黑白的灰度图像。  
Bit depth 最常见的是 24 bit ，红蓝绿各占8位。  
              □□□□□□□□  □□□□□□□□  □□□□□□□□  
 换句话说,对于一个bit depth为24的图片,它的每个像素有2^24种可能。 
### 2.3 元数据(Metadata)
存储描述图片的参数，比如性质，内容，标题，地理位置，版权等，为方便数据库和文件系统调用。
Exif（Exchangeable image file format）是被广泛接受的标准。

### 2.4 当前存在的位图格式(Current raster image formats)
旧的文件格式有：Gif,PNG,TIFF,JPEG  
较新的格式包括：WebP和BPG,还有HEIF  

**GIF**：
1987年引入，前身是基于RLE编码的图片格式VIDTEX.
1989年，GIF更新到支持动画和透明。开始采用有损压缩方式：色块从24位调色板中生成了256种颜色，采用LZW编码（它有第三方专利）
GIF不是一个储存图片的很好选择，因为对于每个色块而言，它可选的颜色太过有限。 

**PNG**：
1995提出，PNG作为GIF的替代,意图是创建一个免专利费的图片格式。
1996第一个说明文档发布。它提供更好的压缩，更真的颜色支持，可选的alpha通道透明度。
2004年发布了IOS标准，png成了互联网上最受欢迎的有损图片格式。  

**TIFF**：Tagged Image File Format  

**JPEG**：Joint Photographic Experts Group
1992年第一个JPEG标准，1994年成为ISO标准  
JPEG2000在2000年成为一个国际标准，目的是取代92年的JPEG标准。 
JPEG2000需要更高的计算资源，且不向后兼容，到2016年仍有许多浏览器和图片应用缺少对它的支持。

**WebP**：
Google在2010年引入。

---

## 3. ISOBMFF标准 (=ISO Base Media File Format)
HEIF格式正是基于该标准设计而成，该标准用于：存储基于时间轴或者无时间的媒体数据，其前身是苹果的QuilckTime File Format(.mov)格式。
它的设计理念----盒子(容器)结构：
		

 - 定义了盒子结构(box-structured)文件，意味这数据被分配在这些盒子中。
 - 允许盒子中包含其他盒子。
 - 每个盒子的头部包含着盒子类型的信息，和这个盒子的大小。
 - 这样做使之成为可能：读程序可以跳过不必要的信息和不支持的盒子
 - 使用**4个ASCII码**作为不同的入口，标示盒子的类型。这些4字节通常称为 FourCC or 4CC。

注：大多数人使用术语*容器*，而*盒子*, 此处仅个人习惯。

---

## 4.HEIF的高级结构和底层实现
![image](http://ww1.sinaimg.cn/large/ce2e9210ly1fpv1t9v1lqj20ba0k50t5.jpg)   
*Figure 4：* 引用自[heikkila](https://dspace.cc.tut.fi/dpub/bitstream/handle/123456789/24147/heikkila.pdf)，p26.  

如上图，从最外层看，它有四层盒子类型的结构，分别是：

 - **ftyp**
 - **meta**
 - **moov**
 - **mdat**
 
**关于上述4层结构的功能**，如下：



**ftyp：**  
它总是 HEIF文件的开头，它以brand参数描述了文件的类型和兼容性。
![image](http://ww1.sinaimg.cn/large/ce2e9210ly1fpv1xrdojlj20kh066q45.jpg)   

*Figure 5：* 引用自[Apple](https://developer.apple.com/videos/play/wwdc2017/503/).  

ftpy的底层实现，例如：
```
ftyp (size=0x18, majorbrand = 'heic', minorversion = 0, compatiblebrands = mif1, heic)
```


**meta**：
它存储了单张图片和衍生图片的元数据。  


**仅单张image的图片**：  
一个单图片hevc编码且有Exif的文件，可能的结构如下：
```
MetaBox 'meta':  (container)
    HandlerBox 'hdlr':  'pict'
    PrimaryItemBox 'pitm': item_ID=1;
    ItemInfoBox 'iinf': entry_count=2
      1) 'infe': item_ID=1, item_type='hvc1';
      2) 'infe': item_ID=2, item_type='Exif'
  
    ItemLocationBox 'iloc': item_count=2
        item_ID=1, extent_count=1, extent_offset=X, extent_length=Y;
        item_ID=2, extent_count=1, extent_offset=P, extent_length=Q;
    ItemReferenceBox 'iref':
        referenceType='cdsc', from_item_ID=2, ref_count=1, to_item_ID=1;
    ItemPropertiesBox 'iprp':
        ItemPropertyContainerBox 'ipco':
        'hvcC'
        'ispe'
    ItemPropertyAssociation 'ipma': entry_count=1
        1) item_ID=1, association_count=2
            essential=1, property_index=1;
            essential=0, property_index=2;
    MediaDataBox 'mdat' or 'idat':
        HEVC Image (at file offset X, with length Y)
        HEVC Image (at file offset X, with length Y)
```
 *Code 1：* 引用自[ISO_IEC_23008-12_2017](http://standards.iso.org/ittf/PubliclyAvailableStandards/c066067_ISO_IEC_23008-12_2017.zip)，p76.

**衍生的图片**:  
HEIF一个灵活的特性就是支持派生图像（derived images）。 所谓的派生图像有两种方式 
1. 一种方式恒等派生(identity derivation), 通过为图像分配转换属性来创建。比方说，既有一个图片，也有一个翻转版本的图片提供给该用户。  
2. 另一种是通过来自 image items的图片网格( image grid )+ 图像布局( image overlay)来派生。一个图片网格，允许其图块旁边邻接着其它图块。

![image](http://ww1.sinaimg.cn/large/ce2e9210ly1fpv1z47ctqj20b70ist98.jpg)

*Figure 6：* 引用自[heikkila](https://dspace.cc.tut.fi/dpub/bitstream/handle/123456789/24147/heikkila.pdf)，p28.

**moov**：
如果一个图片文件包含了图片序列，则存储在这里面。它包含了很多Track boxed,每个盒子代表一个图片序列。

**mdat**：
无论是单张图片还是图片序列，其图片数据都存储在mdat盒子。一个文件里面可以包含多个mdat盒子。示例如下：

 - Exif元数据
 - thumbnail 	缩略图
 - HEVC 		编码后的图片

 ![image](http://ww1.sinaimg.cn/large/ce2e9210ly1fpv23kocjtj20mi0e6jt9.jpg)
 
 *Figure 7：* 引用自[Apple](https://developer.apple.com/videos/play/wwdc2017/503/). 
 
 ## 5. 苹果的.heic图片文件
 
 苹果的heic格式文件，就是衍生方式实现的典范。**苹果并不是直接把一张图片item存储在一个盒子中，而是它把图片按网格(grid)拆分成图块(tiles)，按图块去分别存储在每个mdat盒子中**。

下面给出一个示例，这个主图片的数据是从三个预先编码的图片item中衍生出来的。这个4个图块有同样的宽度和高度。
```
FileTypeBox 'ftyp': major‑brand='heic', compatible‑brands='heic'
MetaBox 'meta':  (container)
HandlerBox  'hdlr': 'pict'
PrimaryItemBox 'pitm': item_ID=1;
ItemInfoBox 'iinf': entry_count=4 
1) 'infe': item_ID=1, item_type='hvc1';
2) 'infe': item_ID=2, item_type='hvc1';
3) 'infe': item_ID=3, item_type='hvc1';
4) 'infe': item_ID=4, item_type='hvc1';
ItemLocationBox 'iloc':  item_count=4
item_ID=1, extent_count=1, extent_offset=P0, extent_length=Q0;
item_ID=2, extent_count=1, extent_offset=P1, extent_length=Q1;
item_ID=3, extent_count=1, extent_offset=P2, extent_length=Q2;
item_ID=4, extent_count=1, extent_offset=P3, extent_length=Q3;
ItemReferenceBox:
referenceType='base', from_item_ID=1, reference_count=3,
to_item_ID=2,
to_item_ID=3,
to_item_ID=4;
ItemPropertiesBox 'iprp':
ItemPropertyContainerBox 'ipco':
'hvcC'
'ispe'
ItemPropertyAssociation 'ipma': entry_count=4
1) item_ID=1, association_count=2,
essential=1, property_index=1;
essential=0, property_index=2;
2) item_ID=2, association_count=2,
essential=1, property_index=1;
essential=0, property_index=2;
3) item_ID=3, association_count=2,
essential=1, property_index=1;
essential=0, property_index=2;
4) item_ID=4, association_count=2,
essential=1, property_index=1;
essential=0, property_index=2;
MediaDataBox:
HEVC Image (at file offset P0, with length Q0)
HEVC Image (at file offset P1, with length Q1)
HEVC Image (at file offset P2, with length Q2)
HEVC Image (at file offset P3, with length Q3)
```
 
*Code 2：* 引用自[ISO_IEC_23008-12_2017](http://standards.iso.org/ittf/PubliclyAvailableStandards/c066067_ISO_IEC_23008-12_2017.zip)，p77.  
 


实际上对于苹果的.heic文件的图片，图片一般由512*512这种的图块(tiles)构成。然后按照 从左到右，从上到下 的顺序，依次将图块存入HEIF图像序列。假设如果这个图片如果刚好不是图块(tiles)的偶数倍，那么图片的右边和底部将用黑色填充。苹果也建议用这些帧的整数倍去构成图片。对于一个iphone7上典型的4032×3024分辨率图片，如下图所示，苹果将这个图片以这种方式存储：  
	
 - 把这个图片分成8 \*6 的块，行8，列6。(我测试的时候好像发现了也有7*5的图片)
 - 其余的部分用黑色的块去填充。
 
 ![image](http://ww1.sinaimg.cn/large/ce2e9210ly1fpv2a163i8j20e8080dmn.jpg)

 *Figure 7：* 引用自 *liuziangexit@github*. 

**问题二：为什么上面说“其余的部分”？**

**答**：  
因为对于这个4032 \* 3024的 heif 图片，它的每个图块大小是512\*512，而 4032 * 3024 不是512*512的整数倍。
请参考下面的计算：
```
	 	4032 * 3024 =  12192768  
	 	
		512*512*46 = 12058624          46个网格，不足以容纳全部像素。
		512*512*48 = 12582912          48个网格，足够，但需要填充。  
```
**问题三：苹果为什么要以图块的方式存储？**

**答：**  

苹果的官方幻灯片提供了解释：

![image](http://ww1.sinaimg.cn/large/ce2e9210gy1fqh6yo8vv8j20uy0gy3zo.jpg)

*Figure 8：* 引用自 *Apple*

我的理解是：
1. 方便多线程可以并行处理，这样更快，因为每个图块可以单独解码。
2. 降低内存开销。单张的静态图片对手机的HEVC解码器而言太大，所以把它分成图块处理可以更好。
3. 便于裁剪。

 


## 6. HEIF文件的读写
HEIF文件的写入和解析，已经由诺基亚的技术人员已经实现了（C++）。  感谢这些聪明+勤奋的工程师们！  链接如下：

https://github.com/nokiatech/heif  

**原理**：HEIF文件的读写，本质上是对盒子的处理。在更低的层次上说，就是如何去处理ISOBMFF标准设计的BitStream 类。规则是：   

 1. 每个盒子至少有代码对它保存和加载（序列化和反序列化）
 2. 被处理的盒子中，也可能包含了其它盒子
 ![image](http://ww1.sinaimg.cn/large/ce2e9210ly1fpv2irxvxej20jk09mmxu.jpg)

*Figure 9：* 引用自[heikkila](https://dspace.cc.tut.fi/dpub/bitstream/handle/123456789/24147/heikkila.pdf)，p41. 

## 7. 参考资料推荐：
1. 如果你想深入学习HEIF的设计理念，一定不要错过这份关于HEIF的ISO标准文档：[ISO_IEC_23008-12_2017](http://standards.iso.org/ittf/PubliclyAvailableStandards/c066067_ISO_IEC_23008-12_2017.zip)  
2. 这篇博文，给了我学习的思路，感谢这么棒的作者：http://cheeky4n6monkey.blogspot.com/2017/10/monkey-takes-heic.html
3. 当然，我最喜欢的是下面的这篇文档：**Lasse Heikkila's HEIF implementation thesis** ，虽然只有60页，但十分清晰而有力，帮助我快速理解了HEIF，我的这篇文章的思路也基于此。  链接：
https://dspace.cc.tut.fi/dpub/bitstream/handle/123456789/24147/heikkila.pd
4. **如果你想自己去提取heif文件中的图片**，一定会使用到诺基亚的库。我已经基于实现了heic图片转其它格式，如：jpeg/png/tiff，如果你想要源码可以私信我。  
诺基亚的库(C++)：https://github.com/nokiatech/heif  
一个值得借鉴的代码(heif2jpeg): https://github.com/monostream/tifig  
如果你不会使用这个Nokia 技术人员用C++写的HEIF库，请在下方留言，我有空的时候可以补个教程。

5. ImageMagick (*Github*)仓库下的讨论，给了我莫大的启发~ 分享：https://github.com/ImageMagick/ImageMagick/issues/507

## 8. 最后：
不当之处，欢迎指正。  
***Suggestions for improvements are highly welcome!***
