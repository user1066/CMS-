# CMS-
自用

【主要功能在每一个播放器内核中都相同】
播放器列表
播放器配置
M3U8资源缓存
IP请求限制
空资源地址提示
JSON对接
解析线路切换
解析自动切换
视频/图片广告
暂停广告
跑马灯公告
播放器LOGO
时间显示
电量显示
标题显示
字幕功能
弹幕功能
选集列表
下一集
自动下一集
播放记录
加载画面
画中画
锁屏
视频旋转
移动端自动横屏
资源地址加密
注意问题
1.不兼容IE浏览器

2.php版本推荐7.4  支持7.1~7.4

3.框架引入不支持同时引入多个播放器

json对接教程
1.json接口只支持get请求类型，具体配置方法后台有写自行查看

2.json配置默认回源设置了.m3u8,.mp4意味着资源地址中包含这两个字符串就不会进行解析而是直接播放

3.如果说有一些资源地址中携带.m3u8但是也需要解析的话可以直接指定播放组，指定播放组后只要配有json就一定会走解析，指定方法在接口地址中加上from=播放组，例如https://域名.com/player/index.php?code=art&from=qq&url=

4.如果是在接口地址里传参form指定播放组代码匹配解析随便，但如果是根据特征码自动匹配尽量吧特征码填长一点，比如腾讯视频资源，不要直接填qq，你不能保证只有腾讯视频网址里有qq也许其他网址也有被匹配错了导致播放失败，所以应该填v.qq.com

指定默认json教程
有人可能不喜欢使用播放器的线路切换功能，那么你可以在接口地址中加上from=json编号，json编号就是json接口的序列，从0开始，那么第一条json就0第二条1第三条2…

例如:

https://域名.com/player/index.php?code=art&key=2&url=

意思是使用json配置中的第三条json解析资源
第二种方式的传参代码
template\使用模板\html\vod\play.html找到{include file=”public/foot”}在上面加上传参代码

<script>
    let iframeObj = $('iframe')[2];
    iframeObj.addEventListener('load', () => {
        iframeObj.contentWindow.postMessage({
            "id":"{$obj.vod_id}",
            "name":"{$obj.vod_name}-{$obj['vod_play_list'][$param['sid']]['urls'][$param['nid']]['name']}",
            "group":"{$GLOBALS['_COOKIE']['group_name']}",
            "next":"{$obj.player_info.url_next}",
            "sid":"{$param.sid}",
            "nid":"{$param.nid}",
            "api":"http://127.0.0.1/index.php",
            "dmId":""
        }, "*");
    })
</script>
参数说明（url传参方式同样是这些参数）
id：影片id

name：影片名字

group：用户组名称【不需广告功能可填空】

next：下一集地址【不需下一集该功能可填空】

sid：片源【不需选集该功能可填空】

nid：集数【不需选集该功能可填空】

api：选集内容获取api【不需选集该功能可填空】

dmId：自定义弹幕id【可以根据影片id+集数设置id】

2.将选集列表api上传到使用播放器的网站中，直接跟目录解压即可。

3.打开苹果cms后台添加播放器即可

播放器代码
MacPlayer.Html = '<iframe border="0" src="http://d.com/player/index.php?code=qw&url='+MacPlayer.PlayUrl+'" width="100%" height="100%" marginWidth="0" frameSpacing="0" marginHeight="0" frameBorder="0" scrolling="no" vspale="0" noResize></iframe>';
MacPlayer.Show();
播放器接口
https://json.suxun.site/player/artplayer.php?iurl=

其中域名要改成自己的，code参数wq要改成自己的

注意问题
1.后台默认开启了IP变动检测，如果登录不了后台打开application/config.php找到loginip_check将true改为false
2.如果第二种对接方法一直显示参数加载等待中代表无法使用这种方法传参，在接口种加入if=1参数切换到url传参
3.选集列表api上传步骤视频种未操作，这个上传到苹果cms根目录解压即可
