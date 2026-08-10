使用Github Action将DockerHub镜像转存到阿里云私有仓库，供国内服务器使用，免费易用

视频教程：[https://www.bilibili.com/video/BV1Zn4y19743/](https://www.nodeseek.com/jump?to=https%3A%2F%2Fwww.bilibili.com%2Fvideo%2FBV1Zn4y19743%2F)  
项目地址： [https://github.com/tech-shrimp/docker\_image\_pusher](https://www.nodeseek.com/jump?to=https%3A%2F%2Fgithub.com%2Ftech-shrimp%2Fdocker_image_pusher)

使用方式  
配置阿里云  
登录阿里云容器镜像服务  
[https://cr.console.aliyun.com/](https://www.nodeseek.com/jump?to=https%3A%2F%2Fcr.console.aliyun.com%2F)  
启用个人实例，创建一个命名空间（ALIYUN\_NAME\_SPACE）

访问凭证–>获取环境变量  
用户名（ALIYUN\_REGISTRY\_USER)  
密码（ALIYUN\_REGISTRY\_PASSWORD)  
仓库地址（ALIYUN_REGISTRY）

Fork本项目  
项目地址： [https://github.com/tech-shrimp/docker\_image\_pusher](https://www.nodeseek.com/jump?to=https%3A%2F%2Fgithub.com%2Ftech-shrimp%2Fdocker_image_pusher)  
进入您自己的项目，点击Action，启用Github Action功能 配置环境变量，进入Settings->Secret and variables->Actions->New Repository secret 将上一步的 ALIYUN\_NAME\_SPACE，ALIYUN\_REGISTRY\_USER，ALIYUN\_REGISTRY\_PASSWORD，ALIYUN_REGISTRY 的值配置成环境变量

添加镜像  
打开images.txt文件，添加你想要的镜像，可以带tag，也可以不用（默认latest） 文件提交后自动进入Github Action构建

使用镜像  
回到阿里云，镜像仓库，点击任意镜像，可查看镜像状态。(可以改成公开，拉取镜像免登录)

在国内服务器pull镜像：

docker pull [registry.cn-hangzhou.aliyuncs.com/shrimp-images/alpine](https://www.nodeseek.com/jump?to=http%3A%2F%2Fregistry.cn-hangzhou.aliyuncs.com%2Fshrimp-images%2Falpine)

说明：  
[registry.cn-hangzhou.aliyuncs.com](https://www.nodeseek.com/jump?to=http%3A%2F%2Fregistry.cn-hangzhou.aliyuncs.com) 即 ALIYUN_REGISTRY  
shrimp-images 即 ALIYUN\_NAME\_SPACE  
alpine 即images.txt里面填的镜像