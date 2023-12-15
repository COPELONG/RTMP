# RTMP



![image-20231105101742827](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105101742827.png)

![image-20231105101856506](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105101856506.png)

![image-20231105103719236](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105103719236.png)

![image-20231105103750733](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105103750733.png)

-----------

-------

--------

##  连接流程![image-20231105153252013](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105153252013.png)



![image-20231212215453444](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231212215453444.png)

### 握手

![image-20231212220312591](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231212220312591.png)

首先服务端与客户端需要通过3次交换报文完成握手,RTMP是由三个静态大小的块,而不是可变大小的块组成的,客户端与服务器发送相同的三个chunk,客户端发送c0,c1,c2,服务端发送s0,s1,s2。



### **建立网络连接：**

![image-20231105160333596](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105160333596.png)



### **RTMP建流&Play**

![image-20231212220740889](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231212220740889.png)

![image-20231105164058246](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105164058246.png)

## MESSAGE  CHUNK

 ![image-20231105170007377](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105170007377.png)

![image-20231212221203996](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231212221203996.png)

![image-20231212221136729](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231212221136729.png)

![image-20231105170606696](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105170606696.png)

需要缓冲音频数据

![image-20231212221812516](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231212221812516.png)

![image-20231105172345361](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105172345361.png)

![image-20231105172859172](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105172859172.png)

![image-20231105174604649](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105174604649.png)

Basici Header 是变长的，一般为一个字节

![image-20231105173938804](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105173938804.png)

![image-20231105174654012](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105174654012.png)

![image-20231105175008284](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105175008284.png)

![image-20231105184610050](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105184610050.png)