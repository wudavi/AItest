# 重点提醒：<br />
1.一定要进行初始化init操作，不然会无法进入Elva AI系统。<br />
2.<div>
    <table border="0">
      <tr>
        <th>方法</th>
        <th>showElva</th>
        <th>showConversation</th>
        <th>showFAQs</th>
        <th>showFAQSection</th>
        <th>showSingleFAQ</th>
      </tr>
      <tr>
        <td>作用</td>
        <td>启动机器人界面</td>
        <td>调用人工客服入口</td>
        <td>展示FAQ列表</td>
        <td>展示Section</td>
        <td>展示单条FAQ</td>
      </tr>
    </table>
</div>

# IOS SDK 接入说明：<br />
## 一、下载IOS SDK <br />
点击上一个页面右上角的“Clone or download”按钮下载IOS SDK，下载完成后解压文件。<br />
## 二、unity接口文件 <br />
interface下面的elvaIOS.cs。<br />
## 三、导入ElvaChatService 到项目<br />
把elvachatservice文件夹拷贝到plugins/iOS下，然后导入。<br />      
## 四、接口调用说明 <br />
### 1、SDK初始化<br/>
调用init函数：（必须在游戏开始阶段调用） <br />
public void init(string appKey,string domain,string appId){
elvaInit(appKey,domain,appId);
} 
> •	其中： <br />
App Key:app密钥，从Web管理系统获取。 <br />
domain:app域名，从Web管理系统获取。 <br />
AppId:app唯一标识，从Web管理系统获取。 <br />
注：这三个参数，请使用注册时的邮箱地址作为登录名登录 [Elva AI 后台](https://aihelp.net/elva)。在Settings菜单Applications页面查看。初次使用，请先登录[Elva AI 官网](http://aihelp.net/index.html)自助注册。

### 2、接口调用方法

1).	智能客服主界面启动，调用showElva方法，启动机器人界面 <br />
ElvaChatServiceSDKiOS.getInstance().showElva(string playerName,string playerUid,string serverId,string playerParseId,string showConversationFlag,Dictionary<string,object> config);  
> •	参数说明： <br />
playerName:游戏中玩家名称。  <br />
playerUid:玩家在游戏里的唯一标示id。  <br />
serverId:玩家所在的服务器编号。  <br />
playerParseId:空。  <br />
showConversationFlag(0或1):是否开启人工入口。此处为1时，将在机器人的聊天界面右上角，提供人工聊天的入口。如下图。 config:(可选)自定义ValueMap信息。可以在此处设置特定的Tag信息。 <br />

> •	参数示例:    <br />
Dictionary<string, object> dic = new Dictionary<string, object>();  <br />
dic.Add("dic1", "aaa");  <br />
dic.Add("dic2", "bbb");  <br />
List tags = new List();  <br />
//说明：hs-tags对应的值为List类型，此处传入自定义的Tag，需要在Web管理配置同名称的Tag才能生效。  <br />
tag.Add("paid");  <br />
tag.Add("server1");  <br />
dic.Add("hs-tags", tags);  <br />
ElvaChatServiceSDKiOS.getInstance().showElva("elvaTestName","12349303258",1, "","1",dic);  <br />

2).	展示单条FAQ，调用showSingleFAQ方法 <br />
showSingleFAQ(string faqId,Dictionary<string,object> config); <br />
> •	参数说明： <br />
faqId:FAQ的PublishID,可以在Elva AI 后台中，从FAQs菜单下找到指定FAQ，查看PublishID。 <br />
config:可选，自定义ValueMap信息。参照 1)智能客服主界面启动。 <br />
注：如果在web管理后台配置了FAQ的SelfServiceInterface，并且SDK配置了相关参数，将在显示FAQ的同时，右上角提供功能菜单，可以对相关的自助服务进行调用。
	
3).	展示相关部分FAQ，调用showFAQSection方法 <br />
showFAQSection(string sectionPublishId,Dictionary<string,object> config); <br />
> •	参数说明： <br />
sectionPublishId:FAQ Section 的PublishID（可以在Elva AI 后台 中，从FAQs菜单下[Section]菜单，查看PublishID） <br />
config:可选，自定义ValueMap信息。参照 1)智能客服主界面启动。 <br />

4).	展示FAQ列表，调用showFAQs方法 <br />
showFAQList(Dictionary<string,object> config) <br />
> •	参数说明： <br />
showFAQList(Dictionary<string,object> config) <br />
config:(可选)自定义ValueMap信息。参照 1)智能客服主界面启动。 <br />
5).	设置游戏名称信息，调用setName方法(建议游戏刚进入，调用Init之后就默认调用) <br />
setName(string gameName); <br />
> •	参数说明:
gameName:游戏名称，设置后将显示在SDK中相关界面标题栏。

7).	设置用户id信息，调用setUserId方法(使用自助服务必须调用，参见 2)展示单条FAQ) <br />
在showSingleFAQ之前调用：setUserId(string playerUid); <br />
> •	参数说明: <br />
playerUid:玩家唯一ID。 <br />

8.	设置服务器编号信息，调用setServerId方法(使用自助服务必须调用，参见 2)展示单条FAQ) 在showSingleFAQ之前调用：setServerId(string serverId); 
	•	参数说明: serverId:服务器ID。 
	9.	设置玩家名称信息，调用setUserName方法(建议游戏刚进入，调用Init之后就默认调用) setUserName(string userName); 
	•	参数说明: userName:玩家名称。 
	10.	直接进行vip_chat人工客服聊天，调用showConversation方法(必须确保9）设置玩家名称信息setUserName 已经调用) showConversation(string uid,string serverId,Dictionary<string,object> config); 
	•	参数说明: playerUid:玩家在游戏里的唯一标示id。 serverId:玩家所在的服务器编号。 config:可选，自定义ValueMap信息。参照 1)智能客服主界面启动。 
	11.	Elva AI 运营模块主界面启动，调用showElvaOP方法，启动运营模块界面 showElvaOP(string playerName, string playerUid, string serverId, string playerParseId, string showConversationFlag, Dictionary<string,object> config, int defaultTabIndex);
	•	参数说明： playerName:游戏中玩家名称。  playerUid:玩家在游戏里的唯一标示id。  serverId:玩家所在的服务器编号。  playerParseId:空。  showConversationFlag(0或1):是否开启人工入口。此处为1时，将在机器人的聊天界面右上角，提供人工聊天的入口。如下图。 config:自定义ValueMap信息。可以在此处设置特定的Tag信息。 defaultTabIndex:可选，设置默认打开的Tab页index（从0开始，如需默认打开Elva，可设置为999）。 

	•	参数示例:   Dictionary<string, object> dic = new Dictionary<string, object>();  dic.Add("dic1", "aaa");  dic.Add("dic2", "bbb");  List tags = new List();  //说明：hs-tags对应的值为List类型，此处传入自定义的Tag，需要在Web管理配置同名称的Tag才能生效。  tag.Add("paid");  tag.Add("server1");  dic.Add("hs-tags", tags);  ElvaChatServiceSDKAndroid.getInstance().showElvaOP("elvaTestName","12349303258",1, "","1",dic);  
	12.	设置语言，调用setSDKLanguage方法(Elva默认使用手机语言适配，如需修改，可在初始化之后调用，并在切换App语言后再次调用。) setSDKLanguage (String language); 
	•	参数说明: language:语言名称。如英语为en,简体中文为zh_CN。更多语言简称参见Elva后台，"设置"-->"语言"的Alias列。




