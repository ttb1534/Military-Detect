<hr>


## Before:



<p align=center>
<img src=".\imgs\logo.bmp" width="50%" alt="SIAS" href="https://sias.uestc.edu.cn/index.htm">
</p>
<p align=center>
<font face=宋体><b>
    本项目由电子科大深圳高研院-飞机识别课题组所有。<br/>
    欢迎交流指正！<br/></b></font>
<font face=TimesNewRoman><b>
    This project is base on UESTC-SIAS Aircraft Detect Research Group.<br/>
    Welcome to communicate with us and correct our mistakes!</b></font>
</p>

<hr>

# TensorRT优化

**NVIDIA TensorRT ™** 是基于Nvidia CUDA编程模型的SDK，主要用于高性能深度学习推理[^1]。具体优化结构如图1所示，TensorRT提供api和解析器来从所有主要的深度学习框架中导入经过训练的模型，经过权重与激活精度校准、层与张量融合、内核自动调整、动态张量显存和多流执行，然后生成可部署在各种环境的优化运行时引擎[^2]。


<font face=TimesNewRoman>NVIDIA TensorRT ™ is an SDK based on the Nvidia CUDA programming model for high-performance deep learning inference[^1]. The optimisation architecture is shown in Figure 1, where TensorRT provides api and parsers to import trained models from all major deep learning frameworks, undergo weight and activation accuracy calibration, layer and tensor fusion, kernel auto-tuning The TensorRT architecture is shown in Figure 1[^2].</font>


<p align=center>
<img src="./imgs/trt-info.png" width = "70%"> <br/>
<b><font color=#999AAA size=3 face="仿宋">
图1.TensorRT优化原理<br>Figure 1. TensorRT optimization principle
</font></b>
</p>

在我们的项目中，为了使yolo训练的模型进一步轻量化，我们将模型送入TensorRT中优化产生Engine引擎，然后再应用在GPU推理中，优化流程如图2所示。在最后的GPU推理阶段，优化后的引擎被反序列化解析，当推理请求发出时，输入数据从CPU复制到GPU，推理完成后再以异步方式返回结果至CPU。

<font face=TimesNewRoman>In our project, to further lighten the yolo-trained model, we send the model into TensorRT to optimise it to produce the Engine engine, which is then applied to GPU inference, with the optimisation flow shown in Figure 2. In the final GPU inference stage, the optimised engine is deserialised and parsed, and when an inference request is made, the input data is copied from the CPU to the GPU and the result is returned to the CPU asynchronously when inference is complete.</font>


<p align=center>
<img src="./imgs/architecture.png" width = "60%"> <br/>
<b><font color=#999AAA size=3 face="仿宋">
图2.TensorRT处理模型流程图<br>Figure 2. Flow chart of the TensorRT processing model
</font></b>
</p>

# 实验结果

我们的实验基于Military Aircraft Detection Dataset，该数据集包含36种，5062张军用飞机图片，包含中、美、俄、欧等国家热门机型[^3]。最终经过TensorRT加速后的模型虽然会损失一定的精度，但检测速度得到了极大的提升。采用FP16精度对模型优化后的对比效果如图3所示：

<font face=TimesNewRoman>Our experiments are based on the Military Aircraft Detection Dataset, which contains 36 species and 5062 images of military aircraft, including popular aircraft types from China, the US, Russia and Europe [^3]. The final model after TensorRT acceleration is greatly improved in detection speed, although some accuracy is lost. The comparative results of the model optimisation using FP16 accuracy are shown in Figure 3.</font>

<p align=center>
<img src="./imgs/res1.png" width = "80%"> <br/>
<b><font color=#999AAA size=3 face="仿宋">
图3.未使用TensorRT优化（左）与使用TensorRT优化（右）检测对比<br>Figure 3. Comparison of detection without TensorRT optimization (left) and with TensorRT optimization (right)
</font></b>
</p>

经过TensorRT加速后的视频如下所示：

The video after TensorRT acceleration is shown below.

<p align=center><a href="http://www.youtube.com/watch?feature=player_embedded&v=avn9G6E_H7Q" target="_blank">
<img src="http://img.youtube.com/vi/avn9G6E_H7Q/0.jpg" alt="IMAGE" width="640" height="480" border="10" />
</a>
</p>







# 参考文献

[^1]:Nvidia.NVIDIA TensorRT 可编程推理加速器. [EB/OL].https://developer.nvidia.cn/zh-cn/tensorrt. 2021
[^2]: Houman Abbasian et al.Speeding Up Deep Learning Inference UsingTensorRT. [EB/OL].https://developer.nvidia.com/blog/speeding-up-deep-learning-inference-using-tensorrt. 2020.
[^3]: https://www.kaggle.com/a2015003713/militaryaircraftdetectiondataset

