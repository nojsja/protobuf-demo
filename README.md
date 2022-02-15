## js使用protobuf——支持web端交互使用

## ProtoBuf简介

Protocol Buffer的简称。Google旗下的一款平台无关，语言无关，可扩展的序列化结构数据格式，适合用于数据存储，作为不同应用、语言之间相互通信的数据交换格式,序列化后的数据为二进制数据（pb格式的数据），类比XML、JSON。

protobuf最先支持C++ C# Go JAVA Python PHP语言，最近发布的代码包又支持了JavaScript，今天就来谈下，js怎么使用protobuf。

官网地址 https://developers.google.com/protocol-buffers/

## 安装protobuf编译器

从github上下载编译器源码安装包，https://github.com/protocolbuffers/protobuf/releases

编译安装， 目前仅支持unix类型的系统。

Ubuntu 操作系統下：
```bash
$: sudo apt install protobuf-compiler
```

## 定义一个.proto文件

address.proto文件

```
message Address
{
    required string province  = 1;
    required string city = 2;
    required string county = 3;
}
```

## 编译生成访问类文件

运行下面的命令

```
protoc --js_out=import_style=commonjs,binary:. address.proto
```

会当前目录生成

```
address_pb.js
```

其中的--js_out的语法如下：

```
--js_out=[OPTIONS:]output_dir
```

如上面的例子中的option为 import_style=commonjs,binary， "."为生成文件的目录，这里为当前目录

注意：如果沒安裝 protoc 环境的话，可以去在线网站进行编译然后再保存到本地 `config/` 目录下即可。

## 打包为web可用的js文件

前置条件：需要安装npm。npm一般在安装nodejs的时候就会自动安装。

安装库文件的引用库

```
npm install -g require
```

安装打包成前端使用的js文件

```
npm install -g browserify
```

安装protobuf的库文件

```
npm install google-protobuf
```

打包js文件export.js

```
var address = require('./address_pb');
module.exports = {
    DataProto: address
}
```

编译生成可用js文件

```
browserify exports.js -o  address_main.js
```

## API

普通类型字段（required/optional）

get{FIELD}() // return field value

set{FIELD}(value) // set field value to value

clear{FIELD}(value) // clear filed value

数组类型字段操作（repeated）

add{FIELD}(value) // add one value to field

clear{FIELD}List() // clear filed

get{FIELD}List() // return array of field values

setInterestList(array)// set array

序列化/反序列化

serializeBinary() // 序列化

deserializeBinary(bin) // 反序列化（静态方法）

调试

toObject() // 打印数据

## 使用

```
<html>  
    <head>  
        <script type="text/javascript" src="./js/person_main.js"></script> 
    </head>
    <body>
        protobuf
    </body>
        <script type="text/javascript">
            var address1 = new proto.Address();
            address1.setProvince("北京");
            address1.setCity("北京");
            address1.setCounty("海淀");
            console.log(address1.toObject());
        </script>
</html>
```

[YuanKe](https://bobjin.com/blog/YuanKe) 2018年10月17日 2.1万 7