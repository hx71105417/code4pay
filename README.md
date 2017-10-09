# code4pay
类似支付宝、微信等二维码支付方案实现，半离线模式。

## 基础算法
totp。

## 流程
1.客户端从服务器获得token以（https传输）。
2.生成付款码时，获取token以及当前时间戳，totp(token+UnixTimestamp),转换为byte[]并截取指定长度后转换为int变量otp。
3.id 根据后面付款码组成不同为不同值。
4.使用otp对id 进行异或（根据自己需要选择其他混淆算法，混淆因子必须为otp） 得到enId,code = enid 拼上otp。
5.服务器获取付款码后，反向解码，拿到otp，id。
6.通过id找到token，在当前时间戳前后n个周期里运算otp[],只要otp包含在otp[]，及验证通过，然后通过cardId进行付款。
7.token通过一定策略进行更新，如限定使用次数、有效期等。
## 付款码组成
18位数字
前两位付款用途，支付宝28等

方案1：后16位为数据段，实际可用math.log(math.pow(17,10)-1,2)=53比特位。
6位otp密码占用math.log(math.pow(7,10)-1,2)=20比特位。
假设cardId 占5bits ，则userid数 最大为Math.pow(2,28),2.7亿，好像有点少.

方案2：采用tsp方案，为每一个支付渠道（usrid+cardid）生成一个token。即10位token（经过混淆）+6位otp。
这种方案比较简单，每次添加卡片时即申请一个token。最大可用token数为math.pow(11,10)-1，100亿张。

混淆算法：设otp在[0,n]中，code=id*n+otp，code/n得到id，code%n得到otp，otp为6位，n为math.pow(7,10)+1的情况下，id最大值约为100亿。


## 实现
todo。。。