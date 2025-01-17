
几种不同方案的兼容性表：


[https://open\-icc.dahuatech.com/\#/videoDoc/integration\_web](https://github.com)


推荐使用：**WSPlayer无插件播放器**


wsplayer播放器demo下载地址：


[https://open\-icc.dahuatech.com/\#/download?currentTab\=4](https://github.com)


![](https://img2024.cnblogs.com/blog/237138/202501/237138-20250103131757194-2049390372.png)


 下载压缩包里面有一个对接文档，按对接文档的说明，因为跨域的问题，因此需要弄后台接口代理代理大华接口服务。


![](https://img2024.cnblogs.com/blog/237138/202501/237138-20250103131922429-1722471090.png)


 下面是API文档的网页地址：


[https://open\-icc.dahuatech.com/\#/home?url\=%3Fnav%3Dwiki%2Fevo\-oauth%2FuserPass.html\&version\=enterprisebase/5\.0\.16\&blank\=true](https://github.com):[蓝猫加速器官网](https://lanmaovqn.com/)


 关于后台接口的转发代理，不用自己写，大华给了java示例代码(c\+\+的也有)，可以直接调用：


[https://open\-icc.dahuatech.com/\#/home?url\=%3Fnav%3Dwiki%2Fcommon%2Fquickstart.html%23%E9%89%B4%E6%9D%83%E8%AE%A4%E8%AF%81\&version\=enterprisebase/5\.0\.16\&blank\=true](https://github.com)


![](https://img2024.cnblogs.com/blog/237138/202501/237138-20250103132231994-921965948.png)


将示例java代码移植到项目后台中去，然后配置大华平台服务的ip、端口、key等信息即可。


 只有调用部分是自己写的，代码如下：




```
package org.jeecg.modules.pollute.controller;


import com.alibaba.fastjson.JSONObject;
import com.dahuatech.icc.demo.brm.device.DeviceDemo;
import com.dahuatech.icc.demo.video.ptzControl.PtzControlDemo;
import com.dahuatech.icc.demo.video.realTimePreview.RealTimePreviewDemo;
import com.dahuatech.icc.demo.video.videoReplay.VideoReplayDemo;
import com.dahuatech.icc.model.brm.device.ChannelPageRequest;
import com.dahuatech.icc.model.brm.device.ChannelPageResponse;
import com.dahuatech.icc.model.brm.device.DeviceTreeRequest;
import com.dahuatech.icc.model.brm.device.DeviceTreeResponse;
import com.dahuatech.icc.model.video.ptzControl.OperateCameraRequest;
import com.dahuatech.icc.model.video.ptzControl.OperateCameraResponse;
import com.dahuatech.icc.model.video.ptzControl.OperateDirectRequest;
import com.dahuatech.icc.model.video.ptzControl.OperateDirectResponse;
import com.dahuatech.icc.model.video.realTimePreview.RtspUrlRequest;
import com.dahuatech.icc.model.video.realTimePreview.RtspUrlResponse;
import com.dahuatech.icc.model.video.videoReplay.PlayBackByTimeResponse;
import com.dahuatech.icc.model.video.videoReplay.PlaybackByTimeRequest;
import com.dahuatech.icc.model.video.videoReplay.RegularVideoRecordRequest;
import com.dahuatech.icc.model.video.videoReplay.RegularVideoRecordResponse;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import javax.servlet.http.HttpServletRequest;
import java.util.Arrays;

/**
 * 鉴权的获取公钥和认证这2个接口不用显示调用，会自动执行。无需关心token有效时间，保活等
 *
 * @Author dengjie
 * @since 2020-01-20
 */
@Slf4j
@Api(tags="大华视频接口")
@RestController
@RequestMapping("/pollute/dahuatech")
public class DahuatechController
{
    @Autowired
    private HttpServletRequest request;

    @ApiOperation(value="设备树查询", notes="设备树查询")
    @RequestMapping(value = "/getDeviceTree", method = RequestMethod.POST)
    public DeviceTreeResponse getDeviceTree(@RequestBody DeviceTreeRequest obj)
    {
        return DeviceDemo.getDeviceTree(obj);
    }
    
    @ApiOperation(value="设备通道分页查询", notes="设备通道分页查询")
    @RequestMapping(value = "/getChannelPage", method = RequestMethod.POST)
    public ChannelPageResponse getChannelPage(@RequestBody ChannelPageRequest obj)
    {
        return DeviceDemo.getChannelPage(obj);
    }

    @ApiOperation(value="实时-获取视频流", notes="实时-获取视频流")
    @RequestMapping(value = "/StartVideo", method = RequestMethod.POST)
    public RtspUrlResponse StartVideo(@RequestBody RtspUrlRequest obj)
    {
        return RealTimePreviewDemo.getRtspUrl(obj);
    }

    @ApiOperation(value="回放-获取录像文件", notes="回放-获取录像文件")
    @RequestMapping(value = "/QueryRecords", method = RequestMethod.POST)
    public RegularVideoRecordResponse QueryRecords(@RequestBody RegularVideoRecordRequest obj)
    {
        return VideoReplayDemo.getRegularVideoRecords(obj);
    }

    @ApiOperation(value="回放-获取视频流", notes="回放-获取视频流")
    @RequestMapping(value = "/StartPlaybackByTime", method = RequestMethod.POST)
    public PlayBackByTimeResponse StartPlaybackByTime(@RequestBody PlaybackByTimeRequest obj)
    {
        return VideoReplayDemo.getPlaybackByTimeRtspUrl(obj);
    }

    @ApiOperation(value="云台-方向控制", notes="云台-方向控制")
    @RequestMapping(value = "/OperateDirect", method = RequestMethod.POST)
    public OperateDirectResponse OperateDirect(@RequestBody OperateDirectRequest obj)
    {
        return PtzControlDemo.operateDirect(obj);
    }

    @ApiOperation(value="云台-镜头控制", notes="云台-镜头控制")
    @RequestMapping(value = "/OperateCamera", method = RequestMethod.POST)
    public OperateCameraResponse OperateCamera(@RequestBody OperateCameraRequest obj)
    {
        return PtzControlDemo.operateCamera(obj);
    }
}
```


总结一下和海康平台的区别：


海康：需要插件、不需要后台服务代理


大华：不需要插件，需要后台服务代理


 


最后申请**clientId和****clientSecret**凭证的方法和步骤详见下面页面说明：


[https://open\-icc.dahuatech.com/\#/home?url\=%3Fnav%3Dwiki%2Fcommon%2Fquickstart.html%23%E5%87%86%E5%A4%87ICC%E7%8E%AF%E5%A2%83\&version\=enterprisebase/5\.0\.16\&blank\=true](https://github.com)


