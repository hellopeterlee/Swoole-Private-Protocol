### 通过cli方式执行php SwoolePrivateDemo.php开启服务端

### 另开一窗口执行php SwooleClient.php 可看到效果

### 具体业务逻辑可根据需求自行设计和更改

#### 通信协议为长连接，字节流，包含消息头和消息体两部分

| 名称            | 长度 | 说明                                                         |
| --------------- | ---- | ------------------------------------------------------------ |
| 消息头(header)  | 42   | 详见消息头定义表                                             |
| 消息体(content) | 变长 | 对于请求参数，是接口的入参，json格式，例如：{"code":"demo","startDate":"20170825"}，其中code为接口名，必传。对于响应消息，是返回的数据集合，json格式：例如{"data":[],"errCode":0,"code":"demo"} ，其中errCode错误码0（成功） -1（失败），data为返回的记录数组。 |

#### 消息头定义表

| 名称                                      | 长度 | 说明                                                         |
| ----------------------------------------- | ---- | ------------------------------------------------------------ |
| msg_split（消息分隔符）                   | 1    | 固定传0(无符号字符，以下相同)                                |
| msg_length ( 消息长度 )                   | 4    | 整个消息（包含消息头和消息体）的实际长度 (无符号大端字节序)  |
| msg_type ( 消息类别 )                     | 1    | 1-请求密钥消息，2-返回密钥消息，3-心跳请求消息，4-心跳应答消息，5-拉取请求消息，6-拉取应答消息，7-推送请求消息，8-推送应答消息 |
| uuid ( 请求者ID )                         | 32   | 客户端请求ID                                                 |
| version ( 版本号 )                        | 1    | 固定传1                                                      |
| cipher（包体加密标识）                    | 1    | 0-不加密 1-加密；如果需要加密，请先请求密钥                  |
| replyCipher（服务端响应包体是否需要加密） | 1    | 0-不需要加密 1-需要加密                                      |
| compress（包体压缩标识）                  | 1    | 0-未压缩 1-压缩；先压缩后加密/先解密后解压缩                 |
