一个好的object detection肯定要借助于avod和object detection api两个project，因为实现写的非常不错。
因为这次阅读主要是针对complex yolo实现的一次阅读，所以我们更多地要借鉴tensorflow object detection api
而这个project主要是提供修改tensorflow的方式方法。

1. anchor_generator 是一个纯虚类，只是一个interface，anchor_generators 文件夹里面是一个3D grid的
   生成，而我们需要的yolo其实是一个2D的这个方面其实要比AVOD简单，更接近于tensorflow research，grid方面
   我们还是使用tensorflow，matlab，opencv共有的坐标方式（batch，row，col，channel）这种“行列索引法”，
   本身要和“长宽索引”法在xy的坐标上有一个互换！具体实现请查看tensorflow object detection api。实际实现
   放在anchor_generators文件夹下面。

2. box_coder这个部分也就是encode和decode，最好还是借鉴tensorflow object detection api，因为我们complex
   yolo主要是2Dbox的coder。在decode的时候才用相对坐标还是绝对坐标这一点，我更倾向于使用绝对坐标，因为相对
   坐标，这时候就要注意原始anchor box的相对坐标要先计算好！实际的实现放在box_coders下面。

3. box_list 看完了 跟tensorflow research基本上没有区别。需要注意的是，左上角右上角的box表示法可以轻松的
   计算iou，中心长宽的表示法可以轻松的decode，两者各有所用。这是为什么需要box_list类的实现长成现在这个样子
   的根本原因。box_list_ops 看完了跟tensorflow research基本上没有区别。因为两个部分基本上所有的research
   都不太变化可以直接放在core里面，不需要继承。

4. losses.py，两者都是分别实现了定位误差和分类误差，complexyolo的话可以和avod相同的保留需要的部分同时加入
   confidence objectiveness的误差，也就是一个MSE。所有项目变化不大，照例直接放入core种无需继承

5. datasets/kitti/kitti_dataset.py，dataset预处理，我们可能需要模仿avod虽然这个和complex yolo有
   各种各样的差距，但是好在使用的有点云数据，更符合我们现在的使用方式：
   相反object detection api只有图像只能是说在encode tf_record（也许会）时候稍微借鉴一下。

6. avod/avod/core/bev_generators/bev_generator.py，avod/avod/core/bev_generators/bev_slices.py
   这两个文件的实现在我看来非常novel！我们的实现方式基本上就应该参考这个，唯一的问题就是，bev_generator作为一
   个纯虚类应该放在core种作为interface的形式存在，和bev_generators文件夹则应该最好是单独和core文件夹并列为
   一个实现的folder。

8. 
