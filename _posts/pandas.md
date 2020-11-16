groupby

 > 분할 : 지정된 키 값 기준으로 데이터프레임을 나눔

 > 적용 : 집계, 변환, 필터링 같은 함수 계산

 > 결합 : 연산 결과를 결과배열에 병합

 > df.groupby([분할 칼럼 기준])[적용 기준 칼럼).집계함수() 형태로 사용

     ex) df.groupby(["age"])["height"].mean() 

           -> age에 따른 height변수의 평균 값

- 마지막 집계함수 자리에 aggregate 함수 사용 가능

 > df.groupby(["age"])["height"].aggregate(["mean", "sum"])

           -> age에 따른 height 변수의 평균 값과 합계


DataFrame으로 행열, 나타낸는 법

https://rfriend.tistory.com/282?category=675917

쟝고 가상환경 키는법
https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/development_environment

Groupby  연산기초

https://jjangjjong.tistory.com/7

DataFrame 병합
https://nittaku.tistory.com/121

Pandas 그래프
https://blog.naver.com/PostView.nhn?blogId=okkam76&logNo=221328995340&parentCategoryNo=&categoryNo=27&viewDate=&isShowPopularPosts=false&from=postView


https://3months.tistory.com/292
https://stackoverrun.com/ko/q/10588810
https://flik.tistory.com/59




from pandas import Series, DataFrame
import pandas as pd
import numpy as np

from parse import load_dataframes
from IPython.display import display

def main():
    #data = load_dataframes()
    #df = pd.DataFrame(data["reviews"], columns=['user', 'score'], index=['store'])
    
    #print(df)
    #ddf = data["reviews"]
    
    #print(ddf)
    #grouped1 = df['score'].groupby(df['store'])
    #grouped = ddf['score'].groupby(ddf['store'])
    #for name, group in grouped :
     #   print(name)
      #  print(group)
   # df.groupby(df['store'], df['user'])['cost'].sum()
   # print( df.groupby(df['store'])['score'].sum())
    #tips_multi = df.groupby([df['store']])
    #rtips = tips_multi.astype(df.SparseDtype("int", np.nan))
    #rtips = tips_multi.aggregate(np.sum)
    #print(rtips)

    #df_drop_row = df.dropna(axis=0)
    #user = df['user']
    #user_drop  = user.drop_duplicates()
    #store = df['store']
    # print(len(store))
    #store_drop = store.drop_duplicates()
    # print(len(store_drop))

    # print(user_drop)
    # print(store_drop.tolist())
 
    #raw_store = df.dropna(axis=0)['store'].values
    #raw_user = df.dropna(axis=0)['user'].values
    
    # print(raw_user.sort())
    #user_score = raw_user.tolist()
   # print(user_score)

    #ndf = df.set_index('store')
    #display(ndf)

    #store_user = pd.DataFrame(data["reviews"], columns=['store', 'user'])
    #df_drop_row = store_user.dropna(axis=0)
    # print(len(df_drop_row))
    
    data = load_dataframes()
    df = pd.DataFrame(data['reviews'], columns=['user', 'store', 'score'])
    print(df)
    df_dop_row = df.dropna(axis=0)
    user = df_dop_row['user']
    user_drop = user.drop_duplicates()
    store = df_dop_row['store']
    store_drop = store.drop_duplicates()
    #print(store_drop)
    #print(user_drop)
    # print(df_dop_row)

    raw_user = df_dop_row['user'].values
    raw_store = df_dop_row['store'].values
    raw_score = df_dop_row['score'].values

    #print(store_drop.tolist())
    #print(user_drop.sort_values().tolist())

    store_dict = {}
    for now_store in store_drop.tolist():
        store_dict[now_store] = {}

    # store_dict = { 'store_1':{}, 'store_2':{}, ...}


    for i in range(len(df_dop_row)):
        store_dict[raw_store[i]][raw_user[i]] = raw_score[i]
    # store_dict = { 'store_1':{'user_1':2, 'user_2':4}, 'store_2':{}, …}
    
    matrix_df = pd.DataFrame(store_dict)
    #print(matrix_df)
    
    sdf = matrix_df.astype(pd.SparseDtype("float", np.nan))
    print(sdf.head())
    print(sdf.dtypes)
    print(sdf.sparse.density)
   # for now_store in store_drop:
    #    temp = []
     #   for now_user in user_drop:
      #      if now_user in store_dict[now_store]:
       #         temp.append(store_dict[now_store][now_user])
        #    else:
         #       temp.append(0)

      #  matrix_df[now_store] = temp
    
    #sparse_matrix = matrix_df.to_sparse(0)
    #print(sparse_matrix)

# 950331
if __name__ == "__main__":
    main()

//2020.09.11
open api에서 개 정보를 모아서 csv를 만드는 것은 성공했다.(아직 추가하거나 뺄거는 있지만 일단은)

그런데 보호소 정보를 따로 db로 모으고 싶었는데, 
보호소 검색이 너무 귀찮다.

보호소 상세정보를 얻으려면 보호소id, 보호소 name이 필수고
보호소 id, name을 얻으려면 시도코드, 시군구코드가 필수다
또 유기견 정보를 찾으면서 보니까 보호소정보가 안맞아서 유기견 데이터는 있는데
화면에서 보이지 않는거같은 정보도 있었다.

하나씩 읽어서 보호소아이디, 이름을 얻었다
그런데 다 얻고 보니, doc파일에는 이름이나 아이디 둘 중하나를 입력하면 결과를 얻을 수있다고한다.
홈페이지에도 그렇게 설명을 해줬으면 얼마나 좋을까.......

그런데 너무 많이 요청했다고 오늘은 더이상 정보를 읽을 수가 없다.
그래서 csv로 저장한 값을 읽어보려고 하는데

data = pd.read_csv("C:/git/bigdata/open/sido.csv", header=None)

->

UnicodeDecodeError: 'utf-8' codec can't decode byte 0xbc in position 0: invalid start byte
인코딩에러가 난다.\(ㅇㅁㅇ)/
이러지마세요..... 흑흑 면접도 준비하고, 코테도 준비해야한다구요

이유는 파일을 쓸때 인코딩을 utf-8로 하지 않아서 였다.
write할때 utf-8로 인코딩하는것을 잊지말자