---
layout: post
title: A brief surevy of deep learning based 3D object detection algorithm for autonomous driving scenarios !
---

## 1. Introduction:
  With the rise of Artificial Intelligence system, the race to convert conventional system into automated system is on rise too. One such domain is automated transportation system powered by autonomous vehicles. More than forty companies have joined the race to manufacture and deploy autonomous vehicles on road [1]. This surge was caused by the availability of relatively cheaper sensor LIDARS, rise of deep learning and availability of large public dataset.
    The complexity of task begins as 2D dimensional data is not sufficient enough to properly detect and localize object on ground plane. 3D data acquired by sensors such as LIDAR (Light Detection and Ranging), RGB-D sensors such as Microsoft Kinect, Intel RealSense etc. provides us ample amount of information about the surrounding objects, such as there scale, distance measurement and geometric information. Both the technologies to acquire 3D were lately used to construct model to perform the object detection and tracking in autonomous vehicles. Point clouds obtained from LIDAR are irregular continuous representation of geometrical information in 3d space. Depth images obtained from RGB-D sensor contains the three color channel and depth information corresponding to each cell, making it spatial data.  
     Deep learning has emerged as pioneer in field of computer vision, natural language processing and machine translation etc. Most of his success is related to 2d computer vision, with state-of-art models for object classification, localization, image translation etc. With so much success in 2d computer vision, deep learning has still a long way to go for 3d computer vision. The key challenge is to properly learn the geometrical structure of objects, moreover data format like point cloud has spare and unstructed nature, which makes it impossible to learn directly from them. During these year, research have worked a lot on creating models, which use multiple data stream both 2d and 3d to perform the task of object detection in autonomous vehicles. Several datasets are made publically available, such as The KITTI Vision Benchmark Suite [2], nuScenes [3], Ford AV [4], SemanticKITTI [5] boosting the research in the field of object detection in autonomous vehicle. A few survey papers have been published to cover the domain of 3d computer vision [6] [7] [8], but this will be the first survey that is specifically focused on how deep learning models are evolving in the field of 3d object detection in autonomous vehicles. The questions that we will be specifically focusing in this study are as follows.
     
 1. Why “Representation Matter” more in 3d computer vision then the deep model itself [9].
 2. Which Deep learning architecture performs better on task of 3d object detection in autonomous vehicle?
  
### 2. Point Cloud:
   Point cloud generally obtained from a 3d Scanners, is a set of data point representing the location each of point in 3d space. Point clouds contain raw representation of geometrical aspect of object in continuous space. There unstructured and sparse nature makes it hard to apply any deep learning on them directly [10]. Researchers have employed different schemes to convert raw point into a more suitable representation for deep learning models, specifically to be fed to a convolutional neural net for feature extraction [11] [12] [13] [14]. While most of the literature is focused on representing raw point cloud into a more structured form which is suitable for deep learning models; Qi et al. [10] in his pioneering work presented a novel deep learning architecture based on fully connected layers, that take raw point cloud as input. Inspired by this a lot of researches tend to explore such possibilities. In a recent effort, Shi et al. [15] proposed a complex deep learning architecture which uses the structured and point based method to extract spatial and geometrical feature to localize and detect object and hence achieved benchmark results on Kitti dataset [2]. We will future divide these studies based on what type of representation they choose for point cloud data.  
   
#### 2.1  Grid-Based:
   Research referred as grid based methods either converts point cloud to volumetric representation or to a 2d grid representation. Converting a point cloud into a volumetric representation or voxel cells causes a large amount of empty cells .By preprocessing a point cloud into a grid representation, it became quite suitable to be passed to any 2d image based deep network, but it losses important geometrical information about the objects.  

##### 2.1.1 Volumetric or Voxel Based Method:
   Method based on voxelization of point cloud; discretize point cloud into equally spaced cells called as voxel cells. Due to the sparse nature of point cloud most the voxel cells are empty further it losses the information related to the boundaries of object. Engelcke et al. in his work [13], proposed a voting scheme to deal with zero values in the grid by proposing a voting weight similar to weight matrix of convnet. In the first step, point cloud is discretized into a sparse 3d grid and from each cell that contains non-zero number of points, features vector is extracted. A sparse convolutional neural layer with L1 sparsity penalty is proposed to encourage the data sparsity. This sparsity is further maintained using rectified linear unity, a linear hinge loss is emit a score of 1,-1 for positive and negative sample. The model suffers from information loss in terms of small objects like pedestrians. Voxelnet presented in [16], uses a region proposal network (RPN) for an end-to-end learning on point cloud. Similarly, the whole point cloud is divided into 3d equally spaced voxels, where points are grouped and randomly sampled inside each voxel cell. For each point inside voxel an offset is calculated from the local mean which is the centroid of all points in the cell, each point and its offset value are then transformed into feature space using a fully connected neural net. An element wise max pooling is performed to aggregate features. Both point wise features and aggregated features are concatenated together and fed to an fcnn and further to element wise max pool, which returns sparse non empty voxel tensors. This tensor are then fed to a middle 3d convolutional layers, which convert them to receptive field, which are fed to RPN network which down samples them and provide a probability map and a regression map. 
   In [17] authors proposed a class balanced grouping to solve the problem of class imbalance in autonomous vehicle dataset. The proposed method was designed according to nuScenes dataset [3], which was employed to evaluate the model. DS Sampling to improve the average density of rare classes in training split. 10 classes are further divided into groups for better training, as classes of similar shape, size and geometry can contribute to performance when trained jointly.  The grouping was done while keeping in mind that the number of instance of a specific class also matter in grouping. Further for model, raw point clouds are divided into voxels grid size of 0.1, 0.1, 0.2 m, which was decided manually. A sparse 3d convolutional network with skip connections was used, to build a network simpler to residual net. A region proposal network is used, with the same configuration and architecture as proposed in [16]. Point Pillars a technique proposed in [18], discretized the point cloud into equally spaced grid in x, y plane, which they referred as set of pillars of infinite height, every point inside pillars is distinguished by further five attributes taking account of other points in a pillar. The backbone detection network is similar to Voxel Net. And 3d boxes were regressed using a single shot detector.
   Kuang et al. [19] proposed multi scale feature aggregation network by using the approach of feature pyramid network [20], to build a bottom-up voxel feature aggregation network. The whole point cloud was divided into evenly spaced voxel cells and then the whole spaced is voxelized into multiple sizes. VFE was then applied for voxel feature encoding. Multi-scale tensors of feature were obtained, and are fused together. These features were then passed to region proposal network which is also inspired by feature pyramid network [20]. This bottom-up feature fusion greatly increases the model capacity and it showed a great improvement in detecting smaller objects. Yan et al. [21]  proposed an architecture inspired by Voxelnet , known as Second. An improved sparse convolution method for better inference and novel angle loss regression for better orientation regression of bounding box was proposed. They applied the same voxelization process as proposed in voxelnet. For different classes they divided the point cloud based on the ground truth distribution. Voxel Wise feature (VFE) extractor, was used as proposed in voxelnet [16]. They propose a sparse convolution layer based on general matrix multiplication method (GEMM) [22] based algorithm and rule generation algorithm which solve the main performance bottleneck. With the proposed implementation of sparse convolution they propose a so called Sparse Convolutional Middle extractor consisting sparse [23] and sub-manifold [24] convolution layer to convert sparse 3d voxels to 2d BEV representation. A single shot multi-box detector [25] was used for region proposal, with fixed sized anchors, considering the mean size for each class object. For the orientation, they proposed a Sine-Error loss for Angle regression.

##### 2.1.2 Projection Based Method:  
   Methods which convert point cloud to 2d grid representation, converts point cloud to either a Bird’s Eye View representation or a Front Eye View representation [11] [14] or use a coordinate system to convert 3d point cloud into a 2d plane representation. Li et al. used projection function to convert point cloud into a 2d point map, analogous to a cylindrical image [21]. These point maps are then first down-sampled and then up-sampled using multiple convolutional layers, further last layer outputs two map with similar dimension as the input map , for point segmentation and bounding box regression.
   In [11], authors proposed a deep learning network called BirdNet. In their effort, they encoded point cloud xyz coordinates as three channels of image with height, intensity and depth information. Further a normalization map is applied for density normalization. This 2d bird’s eye view representation was then fed to Fast-RCNN [22] a state of the art 2d object detection network. In an improvement to BirdNet, Barrera et al. [23] replaced hand crafted post processing with an end-to-end learning framework and named it as BirdNet+. They replaced the Fast-RCNN [22] VGG-16 [24] feature extractor with a RESNET-50 [25] for better accuracy and to deal with small objects like pedestrian and cyclist, they use Feature Pyramid RESNET module. RPN is used to generate proposals, whose output is fed to two fully connected Neural Nets, which are further split into three heads, for class prediction, bounding box and yaw angle prediction. In [26] authors converted point cloud into BEV representation, by representing points as 2d spatial image thus preserving height information. Final input is a combination of the 3D occupancy tensor and the 2D reflectance image. Two separate networks where train, the first network extracts general feature and latter one outputs two separate map, a score map for object probability of objects and a geometry map for shape and size.
    In an effort to learn from multiple representation and multi stream, Chen et al. [14] converted point cloud into BEV and front view representation. For BEV representation, the whole point cloud is discretized into 2d grid with resolution of 0.1m with values height, intensity and density responding to each cell. The whole point cloud is divided into M slices, doing so they obtained M height maps, corresponding to the maximum height in the cell, similarly intensity for each cell the maximum reflectance value and density is the number of points in cell. The Front view representation is obtained by projecting point cloud to a cylinder plane, thus obtaining dense front view maps as proposed in [12]. The whole fusion based model will be discussed in the respective section. Simon et al. [27] proposed an extended version of YOLOv2 [28], which is a state-of-the-art 2d object detector and referred it as Complex-YOLO. They first convert the 3d point cloud into BEV representation as proposed by MV3D [14]. YOLOv2 [28] based feature extractor is used to extract feature maps, while the region proposal network (RPN ) is modified to Euler region proposal network (E-RPN ) which has a complex angel argument for the orientations of points within grid cell. Thus an orientation angle is learnt for each object. The loss function of yolov2 is edited to contain the regression loss of the arguments of orientation angle. 
### 2.2 Point Based:
   Point based methods are usually referred as models which use PointNet [10] to learn point-wise feature from raw point cloud. Point net was the first to extract meaningful results by feeding raw point to a fully connected network. Figure 1 shows the architecture of PointNet. Qi et al. in his work, argued about the unordered nature of point, there invariance to permutation and rigid transformations. The core of the architecture lies in a simple symmetric function; max pooling and multi-layer perceptron for point-wise feature aggregation. An improvement was further proposed by the same author to capture local structure, which is called PointNet++ [34]. This uses set abstraction layer, which consists of sampling layer, grouping layer and a PointNet, to group and encode local features into fewer global abstract features.

![alt text](https://github.com/UsamaHasan/usamahasan.github.io/blob/master/content/pointnet.jpg)
Figure 1. PointNet Architecture.
	Shi et al. [35] presented PointRCNN, a region proposal network based on PointNet architecture. A two stage detector, first based on a PointNet encoder-decoder model for bin based 3d box generation, and a second stage canonical 3d bounding box refinement  


### 2.3 Combined:
   The above presented methods use point cloud by either projecting them into a grid based representation or instead they learn directly from raw point cloud. In a recent effort two new literature were presented, which used both volumetric grid representation and PointNet based element-wise feature extraction to detect object in a Lidar scene [29] [30].  Shi et al. in [29]  constructed a Point-Voxel RCNN (PV-RCNN), in which voxel based operation efficiently encodes multi scale feature representation and generate high quality 3d proposals. While point based set abstraction operation preserve the location in receptive field. They propose a voxel set abstraction (VSE) module, which first select keypoints through furtherest point sampling (FPS), then from each keypoint, in the 3d CNN feature volume neighboring non empty voxels are obtained and are transformed by a PointNet [10] block. Which is further extended by extracting raw keypoints and keypoints obtained from BEV map obtained from sparse 3d convolutional layer are also concatenated. Further Roi-grid pooling via set abstraction is proposed to further aggregate keypoint features with 3d box proposals. 
   Authors in [30] used both voxel based method and point based method for better computational efficiency. First raw points are normalized into low resolution voxel, which are then aggregated with neighboring points through voxel based convolution and are then devoxelized. Similarly raw point clouds are passed to Multi-Layer Perceptron (MLP), to extract individual point features. Yang et al. [31] proposed a two staged sparse to dense model, which uses predefined spherical anchors with variable radius separate for each class to generate proposals. Point Net ++ [32] is used to first extract semantic features from point cloud and with these scores, Intersection over union is calculated against the predefined spherical anchors with proposed PointsIOU method. Non-maximum suppression (NMS) is applied to refine proposals. Points within the proposal regions plus the semantic point feature from PointNet++ [37] are fed to a PointNet to further generate proposals for bounding box regression. These proposals are then first voxelized with 35 points randomly sampled for each voxel and VFE layer, proposed in Voxelnet [16], is used to extract features, this whole unit is referred as PointsPool layer.
![alt text](https://github.com/UsamaHasan/usamahasan.github.io/blob/master/content/pvrcnn.png)
Figure 2. PVRCNN Architecture
	
	
## 3. Multi-Fusion:
   A great deal of literature has covered model that use multiple streams of data, to predict 3d bounding box [14] [33] [34]. As autonomous vehicles are equipped with multi sensor and cameras, researchers have constructed models that took advantage of rich spatial feature of 2d images and geometric feature from deep sensing sensor like LIDAR. Researchers have also argued that, due to the highly sparse nature of 3d point cloud they sometime lack to capture enough data to represent a proper geometry of object. Vora et al. [35] presented a scenario where point cloud representation of a pedestrian, was not sufficient enough to be labeled as pedestrian, which more or less applies to all small objects in autonomous vehicle scenario. Authors in [36] proposed a deep continuous fusion model for multi sensor fusion. Two streams of data are fused together, camera stream and Lidar data as BEV representation.
   A continuous fusion layer is proposed which first gather k nearest lidar points, and fuses them with the corresponding pixel 2d feature camera map using multi-layer perceptron. They used four fusion layers to fuse between the two streams of data. Before this fusion, RESNET-18 [25] model is used as backbone feature extraction model. .The most notable work in this sub field is MV3D [14], for which we have already discussed the point cloud representation. Further an RPN is feed with BEV representation. Prior anchor are used for regression. Multi view ROI pooling is proposed to obtained feature map of same length from multi view representation input. Deep fusion is performed to hierarchical fuse the feature from different representation. Ku et al. [33], in his work first converted point cloud data into BEV representation according to the method proposed by MV3D [14]. Two feature extractors are used for the separate streams of data. An encoder decoder model is proposed for this task. Encoder is a modified version VGG-16 [24], which down samples both the input streams to 8 times. The decoding is done on the principle of feature pyramid network (FPN) [20]. These features where then passed to RPN and a second stage detector. In the RPN stage, regression is done on the ground truth and the prior box based on anchors, which are placed after fixed distance and their dimensionality is determined by clustering the training examples. A network in network [37] approach is used to reduce dimensionality. Further feature were fused together and were feed to a fully connected neural net. A four corner encoding is proposed for bounding box regression. Qi el al. in his later work [38], presented 3d object detector based on frustum of point cloud obtained from 2d object detector. Frustum Proposal were generated by first incorporating 2d detectors on RGB data to generate object proposals, then lifting this 2d space using depth information into a 3d space called frustum. Points inside this frustum are collected and called frustum point cloud. Further they used PointNet++ [32] for instance segmentation. Enhancing this approach, Wang el al. [39] generated multiple frustums by sliding across the frustum axis. These frustums generated by sliding are then first aggregated on point level by passing them to a PointNet [10]. These outputs of frustums are then concatenated together and then passed to fully connected convolutional network, which contains convolutional block and de-convolutional block for end-to-end estimation of amodel 3d bounding box. 
   In a complete different approach authors in [40], used High Definition (HD) maps to build priors and showed HD maps can come quite handy when combined with lidar point cloud data in decision making for perception system. They further propose a prediction module for HD maps from a single online lidar sweep as maps are not available everywhere. The Lidar data is represented as BEV which further is incorporated with HD maps as ground priors. For this purpose a binary occupancy map is computed for pixel wise classification. This representation is based to the main model, which consist of a backbone and a header network. The backbone network contain dense convolutional layer for feature extraction, the header network contains convolutional layer for pixel wise dense detection, which emits confidence score and geometric information.

   
## 4 Bibliography

[1]  "RESEARCH BRIEFS," 4 March 2020. [Online]. Available: https://www.cbinsights.com/research/autonomous-driverless-vehicles-corporations-list/.

[2]  P. L. R. U. Andreas Geiger, "Are we ready for Autonomous Driving? The KITTI Vision Benchmark Suite," in Conference on Computer Vision and Pattern Recognition (CVPR), 2012. 

[3]  V. B. A. H. L. S. V. E. L. Q. X. A. K. Y. P. G. B. O. B. Holger Caesar, "nuScenes: A multimodal dataset for autonomous driving," arXiv preprint arXiv:1903.11027, 2019. 

[4]  A. V. G. P. W. W. H. K. J. M. Siddharth Agarwal, Ford Multi-AV Seasonal Dataset, arXiv, 2020. 

[5]  M. G. A. M. J. Q. S. B. C. S. J. G. J. Behley, "SemanticKITTI: A Dataset for Semantic Scene Understanding of LiDAR Sequences," in Proc. of the IEEE/CVF International Conf.~on Computer Vision (ICCV), 2019. 

[6]  H. W. Q. H. H. L. L. L. M. B. Yulan Guo, "Deep Learning for 3D Point Clouds: A Survey," arXiv preprint arXiv:1912.12033, 2019. 

[7]  A. S. A. S. K. C. R. D. G. G. a. D. Eman Ahmed, "A survey on Deep Learning Advances on Different 3D Data," arXiv:1808.01462 , 2019. 

[8]  E. C. S. N. I. K. Anastasia Ioannidou, "Deep Learning Advances in Computer Vision with 3D Data: A Survey," ACM Computing Surveys, vol. 50, no. 2, p. 38, 2017. 

[9]  W.-L. C. D. G. B. H. M. C. a. K. Q. W. Yan Wang, "Pseudo-LiDAR from Visual Depth Estimation: Bridging the Gap in 3D Object Detection for Autonomous Driving," 
in CVPR , 2019. 

[10]  H. S. K. M. L. J. G. Charles R. Qi, "PointNet: Deep Learning on Point Sets for 3D Classification and Segmentation," in Conference on Computer Vision and Pattern Recognition (CVPR), 2017. 

[11]  C. G. F. M. M. A. d. l. E. Jorge Beltran, "BirdNet: a 3D Object Detection Framework," in 21st International Conference on Intelligent Transportation Systems (ITSC), Maui, HI, USA, 2018. 

[12]  T. Z. T. X. Bo Li, "Vehicle Detection from 3D Lidar Using Fully Convolutional Network," in Computer Vision and Pattern Recognition (CVPR), 2016. 

[13]  D. R. D. Z. W. C. H. T. I. P. Martin Engelcke, "Vote3Deep: Fast object detection in 3D point clouds using efficient convolutional neural networks," in IEEE International Conference on Robotics and Automation (ICRA), Singapore, 2017. 

[14]  H. M. J. W. B. L. T. X. Xiaozhi Chen, "Multi-View 3D Object Detection Network for Autonomous Driving," in IEEE Conference on Computer Vision and Pattern Recognition (CVPR), Honolulu, 2017. 

[15]  C. G. L. J. Z. W. J. S. X. W. H. L. Shaoshuai Shi, "PV-RCNN: Point-Voxel Feature Set Abstraction for 3D Object Detection," in IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2020. 

[16]  O. T. Yin Zhou, "VoxelNet: End-to-End Learning for Point Cloud Based 3D Object Detection," in CVF Conference on Computer Vision and Pattern Recognition, Salt Lake City, 2018. 

[17]  Z. J. X. Z. Z. L. a. G. Y. Benjin Zhu, Class-balanced Grouping and Sampling for Point Cloud 3D Object Detection, arXiv:1908.09492, 2019. 

[18]  S. V. H. C. L. Z. J. Y. Alex H. Lang, "PointPillars: Fast Encoders for Object Detection from Point Clouds," in Conference on Computer Vision and Pattern Recognition (CVPR), 2019. 

[19]  B. ,. J. A. ,. M. Z. Z. Z. Hongwu Kuang, "Voxel-FPN: multi-scale voxel feature aggregation," Sensor, 2020. 

[20]  P. D. R. G. K. H. B. H. S. B. Tsung-Yi Lin, "Feature Pyramid Networks for Object Detection," in IEEE Conference on Computer Vision and Pattern Recognition (CVPR), Honolulu, 2017. 

[21]  T. Z. T. X. Bo Li, "Vehicle Detection from 3D Lidar Using Fully Convolutional Network," in Robotics: Science and Systems, 2016. 

[22]  R. Girshick, "Fast R-CNN," in ICCV '15: Proceedings of the 2015 IEEE International Conference on Computer Vision (ICCV), 2015. 

[23]  C. G. J. B. F. G. Alejandro Barrera, "BirdNet+: End-to-End 3D Object Detection in LiDAR Bird’s Eye View," in IEEE International Conference on Intelligent Transportation Systems (ITSC2020), 2020. 

[24]  A. Z. K. Simonyan, "Very Deep Convolutional Networks for Large-Scale Image Recognition," in International Conference on Learning Representations, 2015. 

[25]  X. Z. S. R. J. S. Kaiming He, "Deep Residual Learning for Image Recognition," in IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2016. 

[26]  W. L. R. U. Bin Yang, "PIXOR: Real-time 3D Object Detection from Point Clouds," in CVF Conference on Computer Vision and Pattern Recognition (CVPR), 2018. 

[27]  S. M. K. A. H.-M. G. Martin Simon, "Complex-YOLO: An Euler-Region-Proposal for Real-Time 3D Object Detection on Point Clouds," in ECCV 2018 Workshops, Munich, 2018. 

[28]  J. a. F. A. Redmon, "YOLO9000: Better, Faster, Stronger," arXiv preprint arXiv:1612.08242, 2016. 

[29]  C. G. L. J. Shaoshuai Shi, "PV-RCNN: Point-Voxel Feature Set Abstraction for 3D Object Detection," in IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2020. 

[30]  H. T. Y. L. S. H. Zhijian Liu, "Point-Voxel CNN for Efficient 3D Deep Learning," in Advances in Neural Information Processing Systems, 2019. 

[31]  Y. S. S. L. X. S. J. J. Zetong Yang, "STD: Sparse-to-Dense 3D Object Detector for Point Cloud," in The IEEE International Conference on Computer Vision (ICCV), 2019. 

[32]  L. Y. H. S. L. J. G. Charles R. Qi, "PointNet++: Deep Hierarchical Feature Learning on Point Sets in a Metric Space," in Conference on Neural Information 
Processing Systems (NIPS), 2017. 

[33]  J. a. M. M. a. L. J. a. H. A. a. W. S. Ku, "Joint 3D Proposal Generation and Object Detection from View," IROS, 2018. 

[34]  B. Y. S. W. R. U. Ming Liang, "Deep Continuous Fusion for Multi-sensor 3D Object Detection," in ECCV , 2018. 

[35] A. H. L. B. H. O. B. Sourabh Vora, "PointPainting: Sequential Fusion for 3D Object Detection," ArXiv, vol. abs/1911.10150, 2019. 

[36]  B. Y. S. W. R. U. Ming Liang, "Deep Continuous Fusion for Multi-sensor 3D Object Detection," in ECCV, Munich, 2018. 

[37]  Q. C. S. Y. Min Lin, "Network In Network," CoRR, vol. abs/1312.4400, 2014. 

[38]  W. L. C. W. H. S. L. J. G. Charles R. Qi, "Frustum PointNets for 3D Object Detection from RGB-D Data," in Conference on Computer Vision and Pattern Recognition (CVPR) , 2018. 

[39] K. J. Zhixin Wang, "Frustum ConvNet: Sliding Frustums to Aggregate Local Point-Wise Features for Amodal," in IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS), 2019. 

[40] M. L. R. U. Bin Yang, "HDNET: Exploiting HD Maps for 3D Object Detection," in CoRL , 2018. 

[41] Y. M. B. L. Yan Yan, "SECOND: Sparsely Embedded Convolutional Detection," Sensors , vol. 18, 2018. 

[42] A. A. D. G. Aravind Vasudevan, "Parallel Multi Channel convolution using General Matrix Multiplication," in IEEE 28th International Conference on Application-specific Systems, Architectures and Processors (ASAP), Seattle, 2017 . 

[43] L. v. d. M. Benjamin Graham, "Submanifold Sparse Convolutional Networks," in arXiv:1706.01307v1, 2017. 

[44] B. Graham, "Sparse 3D convolutional neural networks," in BMVC, 2015. 

[45] D. A. D. E. C. S. S. R. C.-Y. F. A. C. B. Wei Liu, "SSD: Single Shot MultiBox Detector," in ECCV , 2015. 

  
