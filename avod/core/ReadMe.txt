一个好的object detection肯定要借助于avod和object detection api两个project，因为实现写的非常不错。
因为这次阅读主要是针对complex yolo实现的一次阅读，所以我们更多地要借鉴tensorflow object detection api
而这个project主要是提供修改tensorflow的方式方法。

1. anchor_generator 是一个纯虚类，只是一个interface，anchor_generators 文件夹里面是一个3D grid的
   生成，而我们需要的yolo其实是一个2D的这个方面其实要比AVOD简单，更接近于tensorflow research，grid方面
   我们还是使用tensorflow，matlab，opencv共有的坐标方式（batch，row，col，channel）这种“行列索引法”，
   本身要和“长宽索引”法在xy的坐标上有一个互换！具体实现请查看tensorflow object detection api。

2. box_list 看完了 跟tensorflow research基本上没有区别。需要注意的是，左上角右上角的box表示法可以轻松的
   计算iou，中心长宽的表示法可以轻松的decode，两者各有所用。这是为什么需要box_list类的实现长成现在这个样子
   的根本原因

3. box_list_ops 看完了 跟tensorflow research基本上没有区别

4. losses.py，两者都是分别实现了定位误差和分类误差，complexyolo的话可以和avod相同的保留需要的部分同时加入
   confidence objectiveness的误差，也就是一个MSE。

5. 
