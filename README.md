

# 前言




  在OSG中，osgUtil::Optimizer是一个非常重要的工具类，它提供了一系列优化场景图的方法，以提高渲染性能和效率。



 

# Demo




  ![请添加图片描述](https://img2024.cnblogs.com/blog/1971530/202411/1971530-20241128094328980-2038499750.gif)




  在笔者的pc上，优化前优化后渲染交互没啥区别，可能是使用的是一个没有分部件的STL大模型原型，内存32GB，以下为cpu和显卡：  ![在这里插入图片描述](https://img2024.cnblogs.com/blog/1971530/202411/1971530-20241128094328363-52070202.png)




  没优化前  ![在这里插入图片描述](https://img2024.cnblogs.com/blog/1971530/202411/1971530-20241128094328256-266716701.png)




  默认优化后，cpu占用率提升，可能是用于优化计算了：  ![在这里插入图片描述](https://img2024.cnblogs.com/blog/1971530/202411/1971530-20241128094328328-1813102543.png)




  开启所有优化选项，cpu占用率提升，gpu降低约1%\~%2  ![在这里插入图片描述](https://img2024.cnblogs.com/blog/1971530/202411/1971530-20241128094328301-221005750.png)



 

# osgUtil::Optimizer




## 功能概述




  osgUtil::Optimizer是一个强大的优化工具，它提供了多种优化策略，包括几何体合并、节点空间位置分组、相邻LOD节点合并等。以下是几个常用的优化功能：




* MERGE\_GEOMETRY：将多个几何体合并成一个，以减少渲染时的几何体数量，提高渲染效率。这一功能在处理大规模场景时尤为重要，可以显著减少渲染时间。
* SPATIALIZE\_GROUPS：根据节点的空间位置进行分组，便于后续进行裁剪和LOD（Level of Detail）划分。这有助于减少不必要的渲染，提升性能。
* COMBINE\_ADJACENT\_LODS：合并相邻的LOD节点，以简化场景图结构，提高渲染效率。
* 其他优化：osgUtil::Optimizer还提供了许多其他优化功能，如简化几何体、生成法线、生成Delaunay三角网等，以满足不同场景的需求。




## 使用方法




  使用osgUtil::Optimizer进行场景图优化的过程很简单。创建一个osgUtil::Optimizer对象，然后调用其optimize()方法，并传入要优化的场景图节点即可。




## 使用场景




* 大规模场景渲染：在处理大规模场景时，osgUtil::Optimizer可以通过合并几何体、优化节点结构等方式，显著提高渲染性能。
* 实时仿真：在实时仿真应用中，性能是至关重要的。osgUtil::Optimizer可以帮助开发者优化场景图，减少渲染时间，提高仿真效率。
* 虚拟现实：在虚拟现实应用中，场景复杂度和细节程度通常较高。osgUtil::Optimizer可以通过优化场景图结构，提高渲染效率，从而提升用户体验。
* 可视化：在可视化应用中，数据通常以三维图形的形式呈现。osgUtil::Optimizer可以帮助开发者优化场景图，提高渲染速度，使数据更加直观地呈现出来。



 

# :[Flowercloud 机场订阅加速](https://flowercloud6.com)osg::Optimizer使用步骤




## 步骤一：添加头文件




  ![在这里插入图片描述](https://img2024.cnblogs.com/blog/1971530/202411/1971530-20241128094328292-1036485453.png)





```
#include 

```



## 步骤二：创建实例




  ![在这里插入图片描述](https://img2024.cnblogs.com/blog/1971530/202411/1971530-20241128094328302-479604764.png)





```
// 步骤二：创建实例
osgUtil::Optimizer optimizer;

```



## 步骤三：优化场景（Node类型下的都可以）




  ![在这里插入图片描述](https://img2024.cnblogs.com/blog/1971530/202411/1971530-20241128094328256-1721301716.png)





```
// 步骤三：优化场景
// optimizer.optimize(pGroup.get());
optimizer.optimize(pGroup.get(), osgUtil::Optimizer::ALL_OPTIMIZATIONS);

```


 

# Demo源码





```
osg::ref_ptr OsgWidget::getOptimizerNode()
{
    // 其他demo的控件
    updateControlVisible(false);

    osg::ref_ptr pGroup = new osg::Group();

    // 加载支持stl格式插件
    osgDB::Registry::instance()->addFileExtensionAlias(".stl", "stl");

    // 加载模型
    {
        osg::ref_ptr pNode;
        QString filePath = "T:/CVN76.STL";

        pNode = osgDB::readNodeFile(filePath.toStdString());
        if(!pNode.get())
        {
            LOG << "Failed to openFile:" << filePath;
        }

        pGroup->addChild(pNode);
    }

#if 1
    // 优化场景
    {
        // 步骤一：添加头文件
//        #include 

        // 步骤二：创建实例
        osgUtil::Optimizer optimizer;

        // 步骤三：优化场景
//        optimizer.optimize(pGroup.get());
        optimizer.optimize(pGroup.get(), osgUtil::Optimizer::ALL_OPTIMIZATIONS);
    }
#endif

    return pGroup.get();
}

```


 

# 工程模板v1\.38\.0




  ![在这里插入图片描述](https://img2024.cnblogs.com/blog/1971530/202411/1971530-20241128094328328-763686508.png)



