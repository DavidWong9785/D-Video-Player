# D-Video-Player
A video player component base on Vue and iview

## Demo Address
  http://component.davidwong.cn/d-video-player

## Build Setup
* setup
<pre>
  npm install d-video-player --save
  npm install iview --save
</pre>

* import
  <pre>
    import Vue form 'vue'
    import 'iview/dist/styles/iview.css'
  </pre>
* use
  <pre>
    import DVideo from 'd-video-player'
    components: {DVideo},
    &lt;d-video
      :src="src"
      :type="type"
      :rate="rate"
      :options="options"
      @on-pause="pause"
      @on-start="start"
      @on-ended="end"
      @on-error="handleError"
      @on-loadeddata="loadeddata"
      @on-timeupdate="timeupdate"
      @on-rate-change="rateChange"
      &gt;&lt;/d-video&gt;
  </pre>
* params explain
  <pre>
    :src="video path"
    :type="video type -- tag source's type, for example, video/mp4, default value is video/mp4"
    :rate="rate, just a tag, it will take effect when you already set noRatePanel = false, also the rate menu can show"
    :options="params"
      options : {
        autoplay，   //default false
        muted，      //default false
        loop，       //default false
        noRatePanel，//default true, if you want show the rate menu, you can set it false
        preload，    //default auto
        poster       //default ''
      }
    @on-pause="pause callback"
    @on-start="start callback"
    @on-ended="end callback"
    @on-error="error callback"
    @on-timeupdate="timeupdate callback"
    @on-loadeddata="loadeddata callback"
    @on-rate-change="when you click the rate menu, it will return the current rate, 
        2 -- ld，1 -- sd（default），0 -- hd，you can set the video src as you needed"
  </pre>
* attention and skills
- every callback have two param
  <pre>
    @on-pause="pause($event, video)"
    @on-start="start($event, video)"
    @on-ended="end($event, video)"
    @on-error="handleError($event, errorMsg)"
    @on-loadeddata="loadeddata($event, video)"
    @on-timeupdate="timeupdate(currentTime, video)"
    @on-rate-change="rateChange(value, video)"
  </pre>
- you can get the video object on callback - 'on-loadeddata', also you can use this object operate the video, for example, play() or pause()
- but you can't use video object to set the video src directly
- if you need to set or change the video src, you can modify props src
- true：this.src = xxx，false：video.src = xxx;
- you must setup iview and import iview style
- if video'src change, it will not reset the progress bar, player would keep the current time and progress
- if you want reset the progress bar when change video'src, you can try 'video.currentTime = 0'
