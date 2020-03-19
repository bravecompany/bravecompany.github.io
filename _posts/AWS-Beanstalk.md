---
layout: post
title: 'AWS Elastic Beanstalk'
author: Cherang.kim
date: 2020-03-19 14:00
categories: [techCamp]
tags: [AWS]
comments: false
---

# AWS Elastic Beanstalk

## AWS Elastic Beanstalk
- 인스턴스(EC2) 및 OS 설치,
- 웹어플리케이션 소프트웨어 구성,
- 오토 스케일링 구성,
- 로드 밸런서 구성,
- 업데이트 배포 및 버전 관리,
- 모니터링 관리 설정

위의 항목들이 모두 프로비저닝 되어 있어 어플리케이션 설정,생성,배포,관리를 빠르게 지원해주는 PaaS



PaaS, laaS, SaaS, BaaS

![paas](./AWS_Beanstalk_Setting/paas.png)

1. On-Premise
	직접 인프라, 플랫폼, 어플리케이션을 운영하는 방식을 뜻한다.
2. IaaS (Infrastructure as a Service)
	서비스 제공자는 가상화 서버, OS, 스토리지 프로비저닝 을하여 사용자가 유형의 서버를 구축할 필요없이 어플리케이션을 올려서 사용할 수 있는 서비스
	ex) AWS EC2, S3
3. PaaS (Platform as a Service)
	IaaS가 제공하는것을 넘어서서 runtime과 web server까지 제공하여 사용자가 어플리케이션에만 집중할 수 있는 서비스
	ex) Azure, Google AppEngine, AWS Elastic Beanstalk
4. SaaS (Software as a Service)
	우리가 일반적으로 사용해왔던 Gmail과 같은 서비스로, 사용자는 어플리케이션 관리, 데이터에 신경쓰지 않고 단순히 제공되는 서비스만 이용하게 된다.
	ex) GMail, Google Docs
5. BaaS (Backend as a Service)
	앱개발을 위해 기존의 벡엔드 서버를 구현하지 않고 사용자는 API를 통해 앱에 필요한 서비스들(Oauth, DB, Location 등..)을 제공받는 서비스
	ex) Aws Amplify, Firebase




## 빈스톡의 구성

- 어플리케이션(Application) 과 환경(Environment)으로 구성
- 빈스톡은 어플리케이션을 만들고 하위에 1개 이상의 환경을 구성함   
- 어플리케이션
	- 인스턴스의 집합, 환경에 배포된 어플리케이션 버전의 관리        
	  - 어플리케이션의 재배포와 이전버전으로의 복원이 가능 ( S3에 로그들과 앱버전 기록 )
- 환경
	- EC 인스턴스, 로드밸런서, 오토스케일링그룹, 보안그룹의 집합체
	- '웹서버 환경'과 '워커 환경' 두가지 환경을 지원
	  - 웹서버 환경 : HTTP 기반의 API 서비스 및 웹어플리케이션 구성에 사용
	  - 워커 환경 : Amazon SQS를 이용한 메세지기반 백그라운드에서 처리하는 특수 애플리케이션을 구성할 때 사용







# 빈스톡 설치방법 및 사용법

1. AWS EB CLI (AWS EB Command Line Interface)를 이용하는 방법이 있음.
2. AWS 관리 콘솔(AWS Management Console)



## AWS CLI(Command Line Interface) 를 이용한 EB구성

1. AWSEB CLI 설치
	AWS EB CLI 설치 메뉴얼 - MacOS

- MacOS에는yum이나 apt대신 Homebrew 이용한다, 없다면 설치방법 구글링 후, brew 사전설치. 
	- $ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com. Homebrew/install/master/install)"
	- brew 최신버전으로 업데이트 - $ brew update
- brew를 이용한 awsebcli 설치
	- $ brew install awsebcli
	- 버전확인: $ eb --version -> EB CLI 3.2.2 (Python 3.4.3)
- 파이썬 설치
	- $ curl -O https://bootstrap.pypa.io/get-pip.py
	- $ python3 get-pip.py --user
- eb 최신버전으로 업그레이드
	- pip3 install awsebcli --upgrade --user
- eb 버전체크
	- $ eb --version


2. AWS 접속정보 및 권한 설정
- CLI 접속을 위해 AWS 콘솔에서 계정생성 및 권한 부여 ( IAM )
- IAM에서 생성된 엑세스 키와 토큰 ( aws_access_key_id, aws_secret_access_key)를 사용해서 접속권한 설정
	- 빈스톡 어플리케이션 초기화(생성): $ eb init
	- ID / KEY 등록
- profile 확인 : $ cat ~/.aws/config
```
[profile user01]
aws_access_key_id = USER01_ACCESS_KEY_ID
aws_secret_access_key = USER_01_AWS_SECRET_ACCESS_KEY

[profile user02]
aws_access_key_id = USER02_ACCESS_KEY_ID
aws_secret_access_key = USER_02_AWS_SECRET_ACCESS_KEY
```
- 어플리케이션이 생성되었는지 콘솔에서 확인
- 접속권한은 용도에 따라 여러개로 구성이 가능하고, eb 명령어 전체에 --profile 프로파일명 옵션을 이용해서 사용이 가능함.
	- my-profile에 만들어진 eb목록을 불러옴 : $ eb list --profile my-profile


3. 어플리케이션 생성 및 배포
- $ mkdir HelloWorld
- $ cd HelloWorld
- $ eb init -p PHP
- $ echo "Hello World" > index.html
- $ eb create dev-env
- $ eb open


## AWS(AWS Management Console) 관리콘솔을 이용한 EB구성

1. 어플리케이션 생성

![paas](./AWS_Beanstalk_Setting/image.png)
![paas](./AWS_Beanstalk_Setting/image-1.png)

2. 환경 생성

![paas](./AWS_Beanstalk_Setting/image-2.png)
![paas](./AWS_Beanstalk_Setting/image-3.png)
![paas](./AWS_Beanstalk_Setting/image-4.png)

- 추가옵션 구성으로 디폴트 옵션을 아래와 같이 설정할수 있다.
  아래의 이미지 2개는 구성을 단일 인스턴스 와 고가용성을 선택하였을 시의 기본설정 값이다.
  ( 용량과 로드밸런서, 배포 설정만 상이함 )
![paas](./AWS_Beanstalk_Setting/image-5.png)
![paas](./AWS_Beanstalk_Setting/image-6.png)
![paas](./AWS_Beanstalk_Setting/image-7.png)
![paas](./AWS_Beanstalk_Setting/image-8.png)

- 아래 이미지는 환경유형이 단일인스턴스 사용으로  선택되었을때이며 
  로드밸런싱 수행 선택 시, 사용할 인스컨스 개수와 가용영역, 조정트리거 등일 활성화 된다.
![paas](./AWS_Beanstalk_Setting/image-9.png)

- 로드밸런서는 구성사전설정을  고사용성으로 설정 시, 기능이 활성화 된다.
  ( API 서버 구축 시, 단일 인스턴스 사용으로 별도 로드밸런서 설정 )
![paas](./AWS_Beanstalk_Setting/image-10.png)

- 어플리케이션이 업데이트 될때 생기는 딜레이와 오류가 발생했을때에 대응하여 다음의 배포 방식이 있다
  Beanstalk에서는 아래의 배포방식 중 1~3 정책 지원.
```
1. All at Once
   단일 및 다중 인스턴스 업데이트시 한꺼번에 업데이트를 진행한다.
   다운타임이 생기므로 사용자가 없는 시간에 업데이트를 해야하는 단점이 있다.
2. Rolling
   사용자가 지정한 업데이트율에 따라 일부 인스턴스가 먼저 업데이트 된 후,업데이트가 끝난후에 나머지 인스턴스 업데이트가 실행된다.
   부분적인 업데이트로 진행되므로 서비스 중지되지 않는다.
   하지만 업데이트 중간에 사용자들이 서로 다른 버전의 어플리케이션을 노출된다.
3. Rolling with additional batch
   Rolling과 유사하게 배포율을 설정하는데, 초기 업데이트가 기존의 인스턴스를 활용하지 않고 
   추가 인스턴스를 생성하여 테스팅 후 나머지 인스턴스를 업데이트하여 추가된 인스턴스와 합쳐지게 된다.
4. Immutable
   기존의 인스턴스를 업데이트하지 않고 완전히 새로운 인스턴스를 생성한다.
5. Blue/GreenI
   mmutable과 새로운 인스턴스를 생성한다는 점에선 유사.
   아예 새로운 환경을 만들어 직접 기존 환경과 URL 스와이핑을 통해 수동으로 업데이트해준다.
   따라서, 이전 환경과 업데이트된 환경 두개가 실행중이고 이 덕분에 빠른 롤백 및 서비스중지가 없는 배포가 가능하다.
```
![paas](./AWS_Beanstalk_Setting/image-11.png)
![paas](./AWS_Beanstalk_Setting/image-12.png)
![paas](./AWS_Beanstalk_Setting/image-13.png)
![paas](./AWS_Beanstalk_Setting/image-14.png)
![paas](./AWS_Beanstalk_Setting/image-15.png)
![paas](./AWS_Beanstalk_Setting/image-16.png)
![paas](./AWS_Beanstalk_Setting/image-17.png)

- 업로드 및 배포 기능으로 어플리케이션 소스코드를 압축하여 업로드 및 바로 배포가 가능하다
![paas](./AWS_Beanstalk_Setting/image-18.png)

- 배포된 어플리케이션 소스들이 S3에 저장되어 있어 업로드된 버전이력을 확인 할수 있으며 해당 소스로 소스배포가 가능하다
![paas](./AWS_Beanstalk_Setting/image-19.png)
![paas](./AWS_Beanstalk_Setting/image-20.png)

3. 서비스를 위한 Beanstalk 추가 설정 

- Beanstalk 구성으로 추가된 리소스 확인
![paas](./AWS_Beanstalk_Setting/image-21.png)

- 추가된 EC2 확인
![paas](./AWS_Beanstalk_Setting/image-22.png)

- 추가된 EC2의 보안그룹에 inbound rules 설정
![paas](./AWS_Beanstalk_Setting/image-23.png)
![paas](./AWS_Beanstalk_Setting/image-24.png)

- ACM을 통해서 인증서 등록
  - 아마존 인증서 발급
  - 외부 인증서 등록
![paas](./AWS_Beanstalk_Setting/image-25.png)

- 로드밸런서에서 사용될 대상 그룹 생성하여 EC2 등록
![paas](./AWS_Beanstalk_Setting/image-26.png)

- 로드밸런서 생성
![paas](./AWS_Beanstalk_Setting/image-27.png)

- Application Load Balancer로 생성 ( 각 유형을 공부가 필요 TT )
![paas](./AWS_Beanstalk_Setting/image-28.png)
![paas](./AWS_Beanstalk_Setting/image-29.png)
![paas](./AWS_Beanstalk_Setting/image-30.png)
![paas](./AWS_Beanstalk_Setting/image-31.png)
![paas](./AWS_Beanstalk_Setting/image-32.png)

4. CodeCommit 로 소스 관리

- Beanstalk에 사용될 어플리케이션 소스 리포지토리 생성
![paas](./AWS_Beanstalk_Setting/image-33.png)

- 실제 배포할 버전을 새로운 브랜치로 생성 - release
![paas](./AWS_Beanstalk_Setting/image-34.png)

5. CodePipeline로 자동 배포

![paas](./AWS_Beanstalk_Setting/image-35.png)
![paas](./AWS_Beanstalk_Setting/image-36.png)

- 소스공급자를 CodeCommit로 브랜치 명을 배포를 위한 생성한 release로 설정
![paas](./AWS_Beanstalk_Setting/image-37.png)

- 빌드는 해당되지 않으므로 빌드 스테이지 건너뛰기
![paas](./AWS_Beanstalk_Setting/image-38.png)

- 공급자를 Beanstalk의 Cherang-test 환경으로 설정
![paas](./AWS_Beanstalk_Setting/image-39.png)

- 생성 완료
![paas](./AWS_Beanstalk_Setting/image-40.png)

- 배포 ( CodeCommit의 release 브랜치로 poll이 실행되면 CloudWatch가 인지하고 자동배포가 실행된다 )
![paas](./AWS_Beanstalk_Setting/image-41.png)




![aws-lifecycle](./AWS_Beanstalk_Setting/aws-lifecycle.png){: width="100px" height="70px"}