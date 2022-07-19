0718

# 데이터 파이프라인

> 분석의 기반이 되는 파이프라인 흐름
>
> Kafka 실습으로 온프레미스와 클라우드환경에서 데이터수집 차이 비교



## 파이프라인

한 데이터 처리 단계의 출력ㄷ이 다음단계의 입력으로 이어지는 형태로 연결된 구조를 가리킨다.

요구사항수집 & 데이터선정	---------->		데이터 전처리 저장 --------->  데이터 시각화 분석	

 

## Data Pipline Architecture

Stream queue 

EMR : Hadoop과 관련된 echo system 관리서비스



![image-20220718140748911](/Users/krc/Library/Application Support/typora-user-images/image-20220718140748911.png)

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220718144659946.png" alt="image-20220718144659946" style="zoom:50%;" />





## 데이터 파이프라인 구성방안

1. 요구사항에 빠른대응
2. 에러가 없어야한다. (지속가능함)
3. 시스템적으로 발생하는 문제에 대해서 유연한 scability
4. **<u>Scale up / Scale Out</u>** 이 자유로워야한다
5. 이벤트성데이터 부하에도 처리가 가능해야한다
6. 데이터 쌓이는 공간에 문제가 없어야 한다.
7. 수집데이터 (저장데이터)의 유연성
8. 쉬운 분석데이터 Format



# AWS





## EC2, S3, RDS , API Gateway, CloudWatch



### ec2 : amazon Elastic Computer Cloud

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220718154710827.png" alt="image-20220718154710827" style="zoom:50%;" />



### S3

키: 버킷+ **버전**

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220718155019545.png" alt="image-20220718155019545" style="zoom:50%;" />



### RDS : amazon Relational Database Service

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220718155242257.png" alt="image-20220718155242257" style="zoom:50%;" />





### API Gateway

프록시 역할 아키텍쳐

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220718155339083.png" alt="image-20220718155339083" style="zoom:50%;" />





### CloudWatch

One service one monitoring 모니터링 툴

![image-20220718155416757](/Users/krc/Library/Application Support/typora-user-images/image-20220718155416757.png)





# Kafka

스트림큐의 대표적인 서비스 

보통 파일사이즈 1GB 이상 초과되면 데이터 삭제



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220718162940977.png" alt="image-20220718162940977" style="zoom:50%;" />



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220718163213904.png" alt="image-20220718163213904" style="zoom:50%;" />



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220718163350032.png" alt="image-20220718163350032" style="zoom:50%;" />



