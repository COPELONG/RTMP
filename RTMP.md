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

![image-20240405110140964](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240405110140964.png)



![image-20240612181206287](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240612181206287.png)

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

fmt 的类型即Chunk Type(块消息类型) 也决定了Message Header 的字节长度：

 0 类型的块消息头占 11 个字节长度、

 1 类型的块消息头占用 7 个字节长度、

 2 类型的块消息头占用 3 个字节长度

 3 类型的块没有消息头



![image-20231105173938804](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105173938804.png)

**上图的Header Length 所占的字节是 Basic Header + Message Header 的字节总和。**





![image-20231105174654012](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105174654012.png)

上图的 Type ID == Message Type ID。 当其表示为1，2，3，5 和 6 来作为协议控制消息，并在 chunk Stream ID 为 2 的块流中发送，必 须 用 0 作 为 Strean ID。

![image-20240612205043111](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240612205043111.png)

 Message Type ID 分别表示协议控制消息--1 2 3 5 6、用户控制消息--4、其他消息类型



![image-20231105175008284](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105175008284.png)

![image-20231105184610050](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231105184610050.png)





![image-20240612164833243](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240612164833243.png)





# Librtmp



![image-20240612210239691](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240612210239691.png)

![image-20240612210443403](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240612210443403.png)

## ffmpeg推流

```c++
	/*
	AVFormatContext, ...........
		avformat_open_input(......)
		avformat_find_streaminfo(.....)

		avformat_alloc_output_context2(.....)
		avformat_new_stream(...)
		avcodec_parameters_copy(....)


		avio_open(..)
		avformat_write_header(...)

		while(1){
			av_read_frame(...)//AVPacket
			av_interleaved_write_frame(...)

		}
	*/
        /* copy packet */
        // 3.3 更新packet中的pts和dts
        // 关于AVStream.time_base(容器中的time_base)的说明：
        // 输入：输入流中含有time_base，在avformat_find_stream_info()中可取到每个流中的time_base
        // 输出：avformat_write_header()会根据输出的封装格式确定每个流的time_base并写入文件中
        // AVPacket.pts和AVPacket.dts的单位是AVStream.time_base，不同的封装格式AVStream.time_base不同
        // 所以输出文件中，每个packet需要根据输出封装格式重新计算pts和dts
        av_packet_rescale_ts(&pkt, in_stream->time_base, out_stream->time_base);
```



## librtmp收流

![image-20240613115420432](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240613115420432.png)



```c++
#include <librtmp/rtmp.h>

int main() {
    RTMP *rtmp = RTMP_Alloc();
    RTMP_Init(rtmp);
    
    if (!RTMP_SetupURL(rtmp, "rtmp://example.com/live/stream")) {
        RTMP_Free(rtmp);
        return -1;
    }
    
    // 设置缓冲区大小为 3000 毫秒（3 秒）
    RTMP_SetBufferMS(rtmp, 3000); //用于设置 RTMP（实时消息传输协议） 流的缓冲区大小。
    
    if (!RTMP_Connect(rtmp, NULL)) { //与指定的 RTMP URL 建立网络连接。执行必要的握手过程以初始化 RTMP 会话。
        RTMP_Free(rtmp);
        return -1;
    }
    
    if (!RTMP_ConnectStream(rtmp, 0)) { //连接到指定的 RTMP 流。
        RTMP_Close(rtmp);
        RTMP_Free(rtmp);
        return -1;
    }
    
    // 进行数据传输或接收操作
	while (nRead = RTMP_Read(rtmp, buf, bufsize)) {
		fwrite(buf, 1, nRead, fp);
	}
    
    // 关闭连接并释放资源
    RTMP_Close(rtmp);
    RTMP_Free(rtmp);
    
    return 0;
}

```



## 解析 Flv 文件并用 librtmp 发送

![image-20240613174733442](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240613174733442.png)

RTMP_Write(rtmp, pFileBuf, 11 + datalength + 4)  缓冲区buf存储FLV每一个Tag

RTMP_SendPacket(rtmp, packet, 0) 则解析FLV文件中的每一个字段，然后打包成一个数据包：

```c++
//不是音视频Tag
if (type != 0x08 && type != 0x09) {
			//jump over non_audio and non_video frame£¬
			//jump over next previousTagSizen at the same time
			fseek(fp, datalength + 4, SEEK_CUR); // 4：previoustagsize
			continue;
		}

if (fread(packet->m_body, 1, datalength, fp) != datalength) {
			int nerror = 1;
			continue;
		}
		
		packet->m_headerType = RTMP_PACKET_SIZE_LARGE;
		packet->m_nTimeStamp = timestamp;
		packet->m_packetType = type;
		packet->m_nBodySize = datalength;

RTMP_SendPacket(rtmp, packet, 0) 
```

































