---
layout: post
date: 2017-09-10
title: "Amazon WebService Settings"
categories: coding
author_name : archys
author_url : /author/archys
author_avatar: archys
show_avatar : true
feature_image: feature-water
show_related_posts: false
square_related: recommend-spain
---

# AWS settings


## 01_Key Pairs생성

- [AWS 접속](https://ap-northeast-2.console.aws.amazon.com/console/home?region=ap-northeast-2) 후 Region Seoul 체크
- EC2 Dashboard 접속 후 Launch Instance 클릭
	- 1. Ubuntu Server 선택
	- 2. t2.micro(Free tier eligible)선택 후 Launch
	- 3. Create a new key pair & 이름 설정 후 Download Key Pair
	- 다운받은 .pem파일을 ~/.ssh폴더에 넣기 (mv [KeyName].pem ~/.ssh
	- 4. Launch Instance
	- 5. Instance name 설정

- ~/.ssh폴더로 이동시킨 .pem파일은 해당 소유자만 읽을 수 있어야 햐기때문에 권한을 바꿔준다 `chmod 400 [KeyName].pem`


## 02_AWS Configure setting 하기

### 02-1_IAM접속 후 User 생성하기
- Users -> Add user
- Username: [ProjectName]-User
- Access type : Programmatic access Check
- Set permissions - Attach existing policies directly선택
- Policy name :
	- AmazonEC2FullAccess
	- AmazonRDSFullAccess
	- AmazonS3FullAccess
	- AWSElasticBeanstalkFullAccess 선택, CreateUser
- Access Key ID와 Secret access key는 User생성시에만 볼 수 있으므로 바로 **aws configure**를 실행하여 키 값들을 복사하여 입력 해 준다.


```
$ pip install awscli
$ aws configure --profile [projectname]
		AWS Access Key ID [****************]:
		AWS Secret Access Key [****************]:
		Default region name: ap-northeast-2
		Default output format: json
```



## 02_BOTO3


>IPYTHON 실행 후 다음을 입력

```
import boto3
session = boto3.Session(profile_name='eb_docker')
client.create_bucket(Bucket='eb-docker-bucket-archys', CreateBucketConfiguration={'LocationConstraint':'ap-northeast-2'})
```
- Bucket은 고유 name

- settings_deploy.json 파일 수정


> in terminal

```
export = DJANGO_SETTINGS_MODULE=config.settings.deploy

./manage.py collectstatic 으로 s3에 올라가는지 확인
```

RDS는 매 프로젝트마다 만들 필요는 없다.

```
eb init --profile eb_docker

export AWS_EB_PROFILE=eb_docker

export 해 줄 경우

export실행 후 eb init만 입력해도 된다
```


## eb log 보기

```
eb ssh

cd /var/log/

cat eb-activity.log
```

sudo docker exec a54f /root/.pyenv/versions/app/bin/python /srv/django_app/manage.py migrate --noinput --settings=config.settings.deploy


sudo docker ps

sudo docker ps -q

sudo docker ps --no-trunc -q -n


"default_superuser": {
"username": "archys"

password":"asdfasdf"
