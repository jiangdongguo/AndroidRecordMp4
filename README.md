# AndroidRecordMp4
**本地录制Mp4、抓拍jpg图片**
- 支持叠加中英文、时间水印；
- 支持前后置摄像头切换、分辨率切换;
- 支持前后置摄像头视频拍摄、JPG格式图片抓取
- 支持运动状态下相机自动对焦，同时支持手动对焦
- 支持自适应拍摄方向

**1. 添加依赖**   

(1) 在工程build.gradle中添加
```
allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
	}
```
  
(2) 在module的gradle中添加
```
dependencies {
    compile 'com.github.jiangdongguo:AndroidRecordMp4:v1.0.2'
}
```

**2. 使用方法**  

(1) 初始化引擎/释放资源 
 
```
 RecordMp4 mRecMp4 = RecordMp4.getRecordMp4Instance();
 mRecMp4.init(this);  // 上下文
 // 水印类型，包含三种：时间，文字，两者均包含
 mRecMp4.setOverlayType(RecordMp4.OverlayType.BOTH);
 mRecMp4.setOverlayContent("我爱你，中国！");

```

(2) 配置编码参数

```  
  EncoderParams mParams = new EncoderParams();
  mParams.setVideoPath(RecordMp4.ROOT_PATH+ File.separator + System.currentTimeMillis() + ".mp4");    // 视频文件路径
  mParams.setFrameWidth(CameraManager.PREVIEW_WIDTH);             // 分辨率
  mParams.setFrameHeight(CameraManager.PREVIEW_HEIGHT);
  mParams.setBitRateQuality(H264EncodeConsumer.Quality.MIDDLE);   // 视频编码码率
  mParams.setFrameRateDegree(H264EncodeConsumer.FrameRate._30fps);// 视频编码帧率
  mParams.setAudioBitrate(AACEncodeConsumer.DEFAULT_BIT_RATE);        // 音频比特率
  mParams.setAudioSampleRate(AACEncodeConsumer.DEFAULT_SAMPLE_RATE);  // 音频采样率
  mParams.setAudioChannelConfig(AACEncodeConsumer.CHANNEL_IN_MONO);// 单声道
  mParams.setAudioChannelCount(AACEncodeConsumer.CHANNEL_COUNT_MONO);       // 单声道通道数量
  mParams.setAudioFormat(AACEncodeConsumer.ENCODING_PCM_16BIT);       // 采样精度为16位
  mParams.setAudioSouce(AACEncodeConsumer.SOURCE_MIC);                // 音频源为MIC
  mRecMp4.setEncodeParams(mParams);
```
(3) 开始 /停止录制
 
```
 mRecMp4.startRecord();
 mRecMp4.stopRecord();
```  

(4) Camera渲染  

```  
public class MainActivity extends Activity implements SurfaceHolder.Callback{
    @Override
    public void surfaceCreated(SurfaceHolder surfaceHolder) {
        if(mRecMp4 != null){
            mRecMp4.startCamera(surfaceHolder);
        }
    }

    @Override
    public void surfaceChanged(SurfaceHolder surfaceHolder, int i, int i1, int i2) {

    }

    @Override
    public void surfaceDestroyed(SurfaceHolder surfaceHolder) {
        if(mRecMp4 != null){
            mRecMp4.stopCamera();
        }
    }
```  

(5) 摄像头控制  

```  
// 对焦
 mRecMp4.enableFocus(new CameraManager.OnCameraFocusResult() {
         @Override
         public void onFocusResult(boolean result) {
                   if(result){
                        showMsg("对焦成功");
                    }
               }
            });
  // 切换摄像头
  mRecMp4.switchCamera();
  
  // 修改默认分辨率
  mRecMp4.setPreviewSize(1280,720); 
  
  // 切换分辨率
  mRecMp4.setPreviewSize(1280,720);
  mRecMp4.restartCamera();
```  

(6) JPG图片抓拍
```
      mRecMp4.capturePicture(picPath, new SaveYuvImageTask.OnSaveYuvResultListener() {
          @Override
          public void onSaveResult(boolean result, String savePath) {
                  Log.i("MainActivity","抓拍结果："+result+"保存路径："+savePath);
               }
           }); 
```  

最后，不要忘记添加权限哈
 
<uses-permission android:name="android.permission.RECORD_AUDIO"/>
<uses-permission android:name="android.permission.CAMERA"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/> 


csdn博文地址：http://blog.csdn.net/andrexpert/article/details/72523408

License
-------

    Copyright 2017-2021 Jiangdongguo

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
