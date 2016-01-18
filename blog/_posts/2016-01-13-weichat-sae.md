---
layout: post
title: wechat_sae
author: ilsec
---

# 微信公众号开发教程

只介绍个人类型公众号的开发，权限限制的比较多，自定义菜单都无法创建。

## [新浪SAE注册](http://www.sinacloud.com)

这里就不介绍如何搭建SAE服务器了。要说明的一点是，一定要实名认证，不然PHP页面会嵌入新浪的js脚本。

若没实名认证，则所有的php页面都会插入js脚本，需要在php头插入如下代码屏蔽js脚本，才可以连接成功。

```
header('content-type: ');
```

## [微信公众号注册](https://mp.weixin.qq.com)

邮箱注册，微信绑定公众号。

## 微信token认证机制

在[开发]->[基本配置]->[填写服务器配置] 完成操作后，点击提交按钮，则会发送数据包

```
xxx.applinzi.com 101.226.62.82 1668 1 [18/Jan/2016:13:52:05 +0800] xxx 359 2 "GET /?signature=xxx&echostr=xxx×tamp=xxx&nonce=xxx HTTP/1.0" 200 19 "-" "Mozilla/4.0" 101.226.62.82.145xxx xxx
```

验证php代码如下

```
private function checkSignature()
{
      // you must define TOKEN by yourself
      if (!defined("TOKEN")) {
          throw new Exception('TOKEN is not defined!');
      }

      $signature = $_GET["signature"];
      $timestamp = $_GET["timestamp"];
      $nonce = $_GET["nonce"];

  $token = TOKEN;
  $tmpArr = array($token, $timestamp, $nonce);
      // use SORT_STRING rule
  sort($tmpArr, SORT_STRING);
  $tmpStr = implode( $tmpArr );
  $tmpStr = sha1( $tmpStr );

  if( $tmpStr == $signature ){
    return true;
  }else{
    return false;
  }
}
}
```

验证过程只有一次，就是在点击提交按钮的时候验证，验证完成后，微信服务器每次向新浪SAE服务器post数据的时候，都不会有**echostr**,所以需要加入如下验证.

```
if (isset($_GET["echostr"])) {
  $wechatObj->valid();
}else{
  $wechatObj->responseMsg();
}
```

完整代码如下：

```
<?php
/**
  * wechat php test
  */

//define your token
define("TOKEN", "weixin");
$wechatObj = new wechatCallbackapi();
if (isset($_GET["echostr"])) {
  $wechatObj->valid();
}else{
  $wechatObj->responseMsg();
}
//$wechatObj->valid();
class wechatCallbackapi
{
	public function valid()
    {
        $echoStr = $_GET["echostr"];

        //valid signature , option
        if($this->checkSignature()){
          header('content-type:text');
        	echo $echoStr;
        	exit;
        }
    }

    public function responseMsg()
    {
		//get post data, May be due to the different environments
		$postStr = $GLOBALS["HTTP_RAW_POST_DATA"];
    var_dump($postStr);
      	//extract post data
		if (!empty($postStr)){
                /* libxml_disable_entity_loader is to prevent XML eXternal Entity Injection,
                   the best way is to check the validity of xml by yourself */
                libxml_disable_entity_loader(true);
              	$postObj = simplexml_load_string($postStr, 'SimpleXMLElement', LIBXML_NOCDATA);
                $fromUsername = $postObj->FromUserName;
                $toUsername = $postObj->ToUserName;
                $keyword = trim($postObj->Content);
                $time = time();
                $textTpl = "<xml>
							<ToUserName><![CDATA[%s]]></ToUserName>
							<FromUserName><![CDATA[%s]]></FromUserName>
							<CreateTime>%s</CreateTime>
							<MsgType><![CDATA[%s]]></MsgType>
							<Content><![CDATA[%s]]></Content>
							<FuncFlag>0</FuncFlag>
							</xml>";
				if(!empty( $keyword ))
                {
              		$msgType = "text";
                	$contentStr = "Welcome to wechat world!";
                	$resultStr = sprintf($textTpl, $fromUsername, $toUsername, $time, $msgType, $contentStr);
                	echo $resultStr;
                }else{
                	echo "Input something...";
                }

        }else {
        	echo "";
        	exit;
        }
    }

	private function checkSignature()
	{
        // you must define TOKEN by yourself
        if (!defined("TOKEN")) {
            throw new Exception('TOKEN is not defined!');
        }

        $signature = $_GET["signature"];
        $timestamp = $_GET["timestamp"];
        $nonce = $_GET["nonce"];

		$token = TOKEN;
		$tmpArr = array($token, $timestamp, $nonce);
        // use SORT_STRING rule
		sort($tmpArr, SORT_STRING);
		$tmpStr = implode( $tmpArr );
		$tmpStr = sha1( $tmpStr );

		if( $tmpStr == $signature ){
			return true;
		}else{
			return false;
		}
	}
}

?>

```
