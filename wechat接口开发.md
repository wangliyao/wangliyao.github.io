##微信公众号开发流程
###1.接入微信，与微信服务器确认是微信开发者进行的请求，成为开发者成功

开发者通过检验signature对请求进行校验（下面有校验方式）。若确认此次GET请求来自微信服务器，请原样返回echostr参数内容，则接入生效，成为开发者成功，否则接入失败。加密/校验流程如下：

1）将token、timestamp、nonce三个参数进行字典序排序

2）将三个参数字符串拼接成一个字符串进行sha1加密

3）开发者获得加密后的字符串可与signature对比，标识该请求来源于微信

示例代码：
------------------------------------
	def auth_wechat
    	if check_signature?(params[:signature], params[:timestamp], params[:nonce])
      	return render text: params[:echostr]
    	end
  	end
  	
  	def check_signature?(signature, timestamp, nonce)
    	Digest::SHA1.hexdigest([timestamp, nonce, @@token].sort.join) == signature
  	end
------------------------------------

###2.获取access_token

https请求方式: GET

https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=APPID&secret=APPSECRET

返回值：{"access_token":"ACCESS_TOKEN","expires_in":7200}

参数说明：

access_token : 凭证

expires_in : 凭证有效时间

###3.此处只涉及连接问题
 其他问题可参考微信开发手册
#### 微信开发gem
[微信支付gem:wx_pay](https://github.com/jasl/wx_pay)

[微信开发常用gem:weixin_rails_middleware](https://github.com/lanrion/weixin_rails_middleware):这个gem运行命令可直接生成相应的基础功能（包括连接微信接口）

[微信高级功能api实现gem:weixin_authorize](https://github.com/lanrion/weixin_authorize)
