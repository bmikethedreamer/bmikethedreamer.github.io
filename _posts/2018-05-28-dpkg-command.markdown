---
layout: post
title:  "[리눅스] 패키지를 설치할 때는 dpkg"
date:   2018-05-23 22:11:56 +0900
categories: jekyll update
---
오늘은 리눅스에서 apt-get-get을 사용하지 않고 파일을 설치하는 방법을 알아보도록 하겠습니다.

우분투는 확장자가 deb인 데비안 패키지를 사용합니다. 

## 기본 패키지 관리 명령 dpkg
우분투의 기본 패키지 관리 명령어는 dpkg 입니다. dpkg [옵션] [명령] 형식으로 입력합니다. 자주 사용하는 하위 명령어는 아래와 같습니다.
* -i: 패키지를 설치하거나 최신 버전으로 업그레이드
* -r: 설정 파일은 그대로 두고 패키지를 삭제
* -P: 패키지와 함께 설정 파일까지 모두 삭제
* -C: 패키지가 제대로 설치되었는지 확인
* -s: 패키지 상태 정보를 출력
* -L: 패키지에 들어 있는 파일과 경로 보여줌
* -l [패턴]: 패턴과 일치하는 패키지 보여줌

요즘 저는 엘라스틱 스택을 배우고 있기 때문에 관련 패키지 중 Elasticsearch의 패키지를 wget으로 다운로드 받아 dpkg로 설치해보도록 하겠습니다. (해당 부분은 점차 다루도록 하겠습니다.)

> 참조: [엘라스틱 스택](https://www.elastic.co/kr/)

저는 elasticsearch의 6.2.2 버전을 다운로드 받아 설치하겠습니다. 먼저 아래의 명령어로 파일을 다운로드 받습니다.

    # wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.2.deb
    # wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.2.deb.sha512
      
저는 저의 홈 디렉터리에 받았습니다. 이제 dpkg 명령어를 사용하여 설치하도록 하겠습니다.

	# sudo shasum -a 512 -c elasticsearch-6.2.2.deb.sha512
	# sudo dpkg -i elasticsearch-6.2.2.deb

> 설치 자바 1.8 이상 버전이 깔려있어야 합니다.  
> 참조: [우분투 자바 설치 방법](http://blog.dskim.xyz/devops/2017/04/18/devops-ubuntu16.04-jdk-1.8.html)  
> 참조: [Install Elasticsearch with Debian Package](https://www.elastic.co/guide/en/elasticsearch/reference/6.2/deb.html#deb) 

실행 시키기 위하여 아래 명령어를 입력합니다.

	# sudo /bin/systemctl daemon-reload
	# sudo /bin/systemctl enable elasticsearch.service
	# sudo systemctl start elasticsearch.service
	
실제 elasticsearch가 실행되었는지 아래와 같은 명령어로 확인합니다.

	# curl -X GET "localhost:9200/" 

아래와 같은 응답이 오면 성공적으로 패키지를 설치한 것입니다.

	{
	  "name" : "6JG1zJx",
	  "cluster_name" : "elasticsearch",
	  "cluster_uuid" : "qntrvDh3RGKfRo2DcdLInA",
	  "version" : {
		    "number" : "6.2.2",
		    "build_hash" : "10b1edd",
		    "build_date" : "2018-02-16T19:01:30.685723Z",
		    "build_snapshot" : false,
		    "lucene_version" : "7.2.1",
		    "minimum_wire_compatibility_version" : "5.6.0",
		    "minimum_index_compatibility_version" : "5.0.0"
	  },
	  "tagline" : "You Know, for Search"
	}

## 출처
리눅스 서버를 다루는 기술 p. 124
