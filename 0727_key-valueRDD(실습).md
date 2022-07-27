0727

# key-valueRDD(실습)

음식리뷰 평균구하기

만들기 실습



~~~python
from pyspark import SparkConf, SparkContext

# 로컬환경 # 앱 이름설정
conf = SparkConf().setMaster("local").setAppName("category-review-average")
sc = SparkContext(conf=conf)

 #데이터 폴더 경로
directory = "/Users/krc/Documents/Spark/data-engineering/01-spark/data"
# 데이터파일 
filename = "restaurant_reviews.csv" 

# RDD= lines #텍스트파일 불러오기
lines = sc.textFile(f"file:///{directory}/{filename}")
# RDD는 액션나오기까지 연산안하므로

#collect 연산 불러와준다
lines.collect()

~~~

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220727213727118.png" alt="image-20220727213727118" style="zoom:50%;" />

~~~python
# 헤더
# 'id,item,cateogry,reviews'
header = lines.first()

#필터
#헤더를 제외한 로우가져옴
filtered_lines = lines.filter(lambda row: row != header)

#출력
filtered_lines.collect()
~~~

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220727213822464.png" alt="image-20220727213822464" style="zoom:50%;" />

~~~python
# 파싱 function
# 하나의 로우를 받아옴 , 스플릿해서
# 카테고리는 필드의 2번째
# 리뷰는 필드의 3번째
# 스트링 값을 가져오는것이므로 정수표현을위해 int 감싸서 초기화 
# 파싱을 거친데이터를 key-value RDD로 만들것이기 때문에 2가지 값을 필터링해준다
# 튜플리턴
# 첫번째 값은 카테고리 두번째값은 리뷰

def parse(row):
    #'0,짜장면,중식,125'
    fields = row.split(",")
    category = fields[2]
    reviews = int(fields[3])
    return (category, reviews)
  
  
# key-value RDD 만들기
# RDD 이름 : categoryReviews
categoryReviews = filtered_lines.map(parse)


#출력 # 아이템,아이템의 리뷰개수
categoryReviews.collect()
~~~

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220727213955561.png" alt="image-20220727213955561" style="zoom:50%;" />

~~~python
# 평균을 구하기 위해서는 개수를 세야한다
# 카테고리 리뷰 카운트 생성

categoryReviewsCount = categoryReviews.mapValues(lambda x: (x, 1))


#출력
categoryReviewsCount.collect()
~~~

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220727214210244.png" alt="image-20220727214210244" style="zoom:50%;" />

~~~~python
#reduceByKey로 합산하기
reduced = categoryReviewsCount.reduceByKey(lambda x, y: (x[0] + y[0], x[1] + y[1]))

#합산 값
# 합산과 개수를 나누면 평균이 나오므로
reduced.collect()
~~~~

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220727214244926.png" alt="image-20220727214244926" style="zoom:50%;" />

~~~python
# 평균값
averages = reduced.mapValues(lambda x: x[0] / x[1])
averages.collect()
~~~

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220727214304720.png" alt="image-20220727214304720" style="zoom:50%;" />