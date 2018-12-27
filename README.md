# NodePlayer.js
Pure JavaScrip live stream player, 100% written in c/c++. :joy:

## 商用优化版
http://www.nodemedia.cn/products/node-media-player/
商用版现已支持http-flv

## 特性
- 仅支持WebSocket的传输协议
- 仅支持Flv封装
- 首屏启动低于500毫秒，延迟低于2秒
- 支持H.264/H.265 视频软解码
- 支持AAC/MP3/NellyMoser 音频软解码
- 支持iOS/Android原生浏览器或WebView控件
- 支持微信、QQ内部打开
- 视频渲染 WebGL
- 音频播放 WebAudio
- 全解码版js小于3M
- 支持平铺，等比缩放，拉伸填充的视频缩放模式

## 问题
解码性能低下，如何开启ffmpeg的SIMD  
优化思路：Emscripten不能编译.S文件，也不支持inline SIMD assembly。但是支持C API的 SSE1, SSE2, SSE3, SSSE3指令集。对h264addpx_template.c,h264chroma_template.c, h264dsp_template.c, h264idct_template.c, h264pred_template.c, h264qpel_template.c 进行SIMD移植。AAC,HEVC解码器也是相同原理。  
官方文档说编译ARM NEON指令集(#include <arm_neon.h>)代码还不支持，但也可能行。   
Intel有一个项目[ARM_NEON_2_x86_SSE](https://github.com/intel/ARM_NEON_2_x86_SSE) 是否可以考虑使用NEON的API进行移植开发，后期如果Emscripte开始支持NEON则可自动支持。  
emscripten/vector.h 中直接映射了SIMD.js的API，根据[这篇介绍](https://hacks.mozilla.org/2014/10/introducing-simd-js/) ,应该是同时支持X86平台的SSE和ARM平台的NEON指令集。那直接使用这个api来优化应该更好一点。

## 支持的服务端
[Node-Media-Server](https://github.com/illuspas/Node-Media-Server)

## 开发历程
 - [x] WebSocket网络传输
 - [x] FLV解析
 - [x] 音频解码与播放
 - [x] 视频解码与渲染
 - [ ] 兼容性测试
 - [ ] 性能优化
 
## 与flv.js比较
![img](https://github.com/illuspas/NodePlayer.js/blob/master/nodeplayerjs_flvjs.png)

## 微信内打开
![img](https://github.com/illuspas/NodePlayer.js/blob/master/wechat_desktop.jpg)
