# 预览问题

1、视频预览功能要怎么开发？   
SDK 初始化时，将默认创建 10 秒音视频数据缓存，开发者将设备采集到的音视频数据送入
ringbuffer 即可。SDK 库文件已实现数据的发送，开发者不需要再做对应开发。    
开发流程：   
(1)调用IPC_APP_Set_Media_Info设置音视频信息  
(2)调用TUYA_APP_Init_Ring_Buffer初始化ringbuffer  
(3)调用TUYA_APP_Put_Frame往SDK中输入音视频数据        

2、设备内存不足，怎么减小音视频数据缓存区大小？   
SDK 初始化时，将默认创建 10 秒音视频数据缓存，如果需要减小音视频数据缓存区大小，则可以通过修改tuya_ipc_ring_buffer_init中max_buffer_seconds参数进行修改，注意：max_buffer_seconds的值至少包含一个gop   

3、收到p2p的回调之后往SDK送流还是初始化后往SDK送流？     
建议在SDK初始化之后就将视频流持续送入SDK中，避免出现视频延时等体验不好现象  

4、SDK预览支持的视频格式是什么？   
目前支持的视频流格式为h264，暂不支持h265，后续将同时支持h265和h264  

5、SDK预览支持的音频格式有哪些？   
PCM，G711A，G711U建议使用G711U  

6、对讲功能中APP下发给设备端的音频数据是什么格式的？  
App 默认下发 G711U 格式音频，设备端如需以 PCM 格式播放，需要调用函数tuya_g711_decode 对下发的音频做转码处理   

7、视频流的分辨率、帧率，码率，帧间隔怎么设置？  
分辨率：若需要占满 App 屏幕，视频分辨率暂时仅支持 16 : 9  
帧率：建议20帧  
码率：建议主码流不大于1.5M，子码流不大于512K  
gop：建议设置成帧率的2倍即2s一个I帧  

8、预览视频出现卡顿、马赛克等图像质量问题如何排查？  
(1)排查编码器的编码信息是否与IPC_APP_Set_Media_Info中设置的视频信息一致  
(2)把送入SDK中的视频流数据保存到本地，用vlc播放是否异常，再用streameye进行分析视频帧有无异常  

9、声音出现卡顿如何排查？  
(1)排查编码器的编码信息是否与IPC_APP_Set_Media_Info中设置的音频信息一致  
(2)把送入SDK中的音频流数据保存到本地，用Audacity播放是否异常  

10、预览出图慢要怎么优化？  
(1)mqtt 上线回调函数：__IPC_APP_Get_Net_Status_cb 可以使用 STAT_MQTT_ONLINE
判定 mqtt 是否上线成功，成功后可往下执行剩余操作   
(2)wifi 连通外网后，可另起线程，进行 P2P 初始化，提高设备接入外网效率  
(3)在tuya_ipc_ring_buffer_init中实现强制出I帧的回调函数requestIframeCB  

11、构建加密通道失败是什么原因导致的?  
提示构建加密通道，可能是p2p没有初始化，网络有问题，超过最大同时在线人数等原因，根本原因是p2p没有打通，需要看日志具体问题需要具体分析  




