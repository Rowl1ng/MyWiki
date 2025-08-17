# Presentation

在twitter上看到有人说：research is just glorified content creation。感觉尤其适用于presentation这一块，因为presentation本质上就是建立和听众的链接，把晦涩的研究一滴一滴注入进听众的脑袋里。内容本身是一部分，内容怎么表达也很关键。

- 讲话本身：
    - 语速：适中
    - 语调 （intonation）：可以和一句话的重点结合起来。比如需要强调的地方语速变慢、语调升高。（这样看来和演员的台词训练类似，要有抑扬顿挫，一切只为更清晰地传达信息）
- 感情：
    - 自信：觉得自己的工作没有那么厉害？可能是imposter syndrome。既然已经要说给大家听，那这10分钟里我就是唯一的专家！
    - 热情： 对自己的研究有没有passion从语调就可以判断出来。如果presenter有热情的话，听众也更容易代入情境。
- 展示：
    - 数学符号怎么展示更清晰？颜色？
    - 一页slide承载的内容有限，要去繁存简。
    - 每句话最好能和slide上的图形元素呼应。
    - 页内：复杂流程可以借助动画展示。
    - 跨页：transition里的平滑化也可以用来做动画。
    - 右上角标上主讲人名字和页码，方便结束后观众提问题。
* Slides
  * Assisted by animations 
  * Less formulas
  * 在powerpoint里做3D模型
  * 在注释里写scripts
  * 标点符号前不能有空格
  * Customize a slide master。
  * 先把pdf转svg，再导入powerpoint，最后在powerpoint内转成object，就是单独的小图啦！
  * ICCV slides：5min slides的感觉更接近广告，引诱人去看你的paper，所以不用具体到每一步做了什么。
  *   * 做presentation slides介绍自己的研究：写script整理思路

  * Transformer workshop Day1：线下听和看都很累，主讲人语速有些快，投影也看不大清；Day2: 结合pytorch code讲得非常细致：scaling factor is used to smooth softmax, how to select the value?
    * Miika Aittala: Elucidating the Design Space of Diffusion-Based Generative Models用1D场景解释Diffusion model随时间t增加noise后x的变化，动画做的很intuitive，包括x的分布范围、代表每一步梯度的箭头，可能是用matplotlib的animation功能画的？
    *   * AD的talk：Diffusion Models，非常intuitive，从内而外展开洋葱一般，手写草图的示意图更friendly、更容易接受
  
资源：
- 布白PPT：PPT 视觉化终极指南
* 🧑🏻‍🏫PPT教程：动画+对齐；用powerpoint的平滑transition来做动画; 新技能：在powerpoint里画高斯曲线
  
# Poster

软件可以是Omnigraffle、Powerpoint。

一般大印大小是A0 (841 x 1189 mm or 33.1 x 46.8 inches) 
* Poster：
  * 尝试转到powerpoint，但是放缩group会改变元素布局，Lan建议用pdf导出图再粘贴
  * 尝试在overleaf里编辑poster，A0大小。画2D slice of SDF。
