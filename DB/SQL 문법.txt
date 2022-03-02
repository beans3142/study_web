#1.
SELECT productId, productName, CONCAT(price,'달러') AS price, productMainImg,
       Product.status, Product.updateAt,

       (CASE
            WHEN TIMESTAMPDIFF(MINUTE, Product.updateAt,NOW()) < 60 THEN CONCAT(TIMESTAMPDIFF(MINUTE, Product.updateAt,NOW()), '분 전')
            WHEN TIMESTAMPDIFF(HOUR,Product.updateAt, NOW()) < 24 THEN CONCAT(TIMESTAMPDIFF(HOUR, Product.updateAt,NOW()), '시간 전')
            WHEN TIMESTAMPDIFF(DAY,Product.updateAt, NOW()) < 24 THEN CONCAT(TIMESTAMPDIFF(DAY, Product.updateAt,NOW()), '일 전')
            END)


FROM Product
INNER JOIN User AS U ON U.userId=Product.userId
WHERE U.address LIKE '%오산동%'
ORDER BY Product.updateAt ASC;

-- 팁 :  SELECT A AS a, ... 에서 AS를 빼도 똑같이 작동한다.

-- CASE WHEN THEN ELSE : CASE 컬럼 WHEN 조건 THEN 조건이 참이라면 ~ ELSE 조건이 참이 아니라면 ~

    -- CASE WHEN, WHEN, WHEN 이런식으로도 사용 가능

-- TIMESTAMPDIFF : 시간의 차이를 알려주는 함수 (MINUTE,HOUR,DAY) =

-- CONCAT : CONCAT(A,B) A문자열과 B문자열을 합친다. A가 열인 경우 열의 모든 값에 B를 더해준다.

-- SELECT 와 FROM : SELECT (조회할 칼럼) FROM (조회할 테이블) *을 사용하면 모두 가져온다.

    -- SELECT A, B, C, D ...  이렇게 여러개의 값을 선택할 수 있다.

        -- SELECT A a, B b, C c 혹은 SELECT A AS a, B AS b, C AS c 이런 식으로 사용 할 수 도 있다.

-- WHERE : 테이블에서 조건을 만족하는 값을 조회

-- LIKE : 조회 조건이 명확하지 않을 경우 사용한다. 보통 %와 _와 같이 사용한다.

    -- %와 _은 다음과 같다.

    -- %의 경우
    -- 예를 들어 '%오산'으로 조회 하면 앞에 몇 글자가 오든 오산으로 끝나는 값을 조회하고 '오산%'로 하면 뒤에 무엇이 오든 오산으로 시작하는 값을 조회한다.
    -- 이를 응용해 '%오산%' 으로 조회를 할 경우 오산이 들어있는 모든 데이터를 조회한다.

    -- _의 경우
    -- %가 붙인 위치에 따라 오는 값들의 길이가 상관이 없었다면 _의 경우 _의 개수만큼 글자가 오는 것을 뜻한다.

    -- _와 %를 문자열로 사용하기 위해서는 앞에 \ 를 붙임으로써 해결할 수 있다.

    -- 그 외에도 AND나 OR 같은 것들을 이용해 보다 정밀한 조건을 만들 수 있다.


#2.
SELECT productId, productName,U.nickName as 이름,CONCAT(SUBSTRING_INDEX(SUBSTRING_INDEX(U.address, '시',1),' ',-1),'시') as 주소,U.profileImg,
       CONCAT(price,'원') as 가격, productDetail, Product.updateAt as '최종 수정 날짜', category,
       (SELECT count(*)
        FROM ViewCount
        GROUP BY productId
        HAVING productId=5) AS 조회수


FROM Product
INNER JOIN User U on Product.userId = U.userId
INNER JOIN Category C on Product.categoryId = C.categoryId
WHERE productId = 5;

#3.
SELECT productId, productName, CONCAT(price,'원'), productMainImg, updateAt
FROM Product
WHERE userId=1 AND status ='ACTIVE';

SELECT *
FROM User;

#4.
SELECT *
FROM Product
Where userId = 1 AND status = 'COMPLETED';