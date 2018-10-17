# D-Video-Player
基于Vue和iView的视频播放组件

## 快速上手
* 安装
<pre>
  npm install d-video-player --save
  npm install iview --save
</pre>

* 导入
  <pre>
    import Vue form 'vue'
    import 'iview/dist/styles/iview.css'
    import DVideo from 'd-video-player'
    Vue.use(DVideo);
  </pre>
* 在组件中使用
  <pre>
    &lt;d-video
      :src="src"
      :type="type"
      :rate="rate"
      :options="options"
      @on-pause="pause"
      @on-start="start"
      @on-ended="end"
      @on-loadeddata="loadeddata"
      @on-timeupdate="timeupdate"
      @on-rate-change="rateChange"
      &gt;&lt;/d-video&gt;
  </pre>
* 参数说明
  <pre>
    :src="视频路径"
    :type="视频类型，source标签里的type"
    :rate="码率，这一项是在设置了显示率切换menu之后才会有效"
    :options="参数"
      options : {
        autoplay，   //默认为false
        muted，      //默认为false
        loop，       //默认为false
        noRatePanel，//默认为false，是否显示码率切换menu
        preload，    //默认为'auto'
        poster       //默认为''
      }
    @on-pause="pause回调"
    @on-start="start回调"
    @on-ended="end回调"
    @on-timeupdate="timeupdate回调"
    @on-loadeddata="loadeddata回调"
    @on-rate-change="点击menu切换码率的回调，返回当前码率，是数字，
        2为流畅，1为高清（默认），0为超清，使用者可以根据回调值设置视频播放地址"
  </pre>
* 注意
- 每个回调的事件都会带上两个参数，一个是$event，一个是video对象
- 使用者可以利用video对象对视频进行操作，如play、pause
- 但不可以用video对象设置src，这是不允许的，单项数据流，
- 所以需要修改src就修改需要传入的src，正确：this.src = xxx，错误：video.src = xxx;
- 必须安装iview的依赖和导入iview的样式文件