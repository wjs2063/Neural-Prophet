
# Neural-Prophet
### how to use??

Neural prophet 은 facebook 이 출간한 시계열 모델이다. 


### First
import library what you need
```
import warnings
!pip install neuralprophet[live]
from neuralprophet import NeuralProphet
from neuralprophet import set_random_seed 
set_random_seed(1337)
warnings.filterwarnings(action='ignore') 
```

### Second
사용할 데이터를 전처리 한다음 

다음과 같은 DataFrame 구조로 만들어 주어야함 


<img width="739" alt="스크린샷 2021-07-30 오전 12 39 50" src="https://user-images.githubusercontent.com/76778082/127522465-0b3be516-8b8f-478b-b83d-94a3e25ccd04.png">


위사진에선 y1~y11 은 regressor 를 추가하기위해 내가 임의로 추가한것임 ds와 y만 있어도 충분히 구동가능하다. 필요에맞게 늘리면됨


### Third 

model 정의 및 parameter 추가


```
model_23 = NeuralProphet(seasonality_mode='additive',yearly_seasonality=True,
                    weekly_seasonality=False,
                    daily_seasonality=True,
                    
                    changepoints_range=0.5,
                    ar_sparsity=0.5
                        
                    )
model_23.add_seasonality("first",period=24,fourier_order=24) 
model_23.add_seasonality("second",period=36,fourier_order=48)
model_23.add_seasonality("third",period=72,fourier_order=96)
#model_23.add_seasonality("fourth",period=24,fourier_order=20)
#model_23.add_seasonality("fifth",period=24*30.5,fourier_order=6) 
#model_23.add_future_regressor('y1',normalize=True)
model_23.add_future_regressor('y1')
model_23.add_future_regressor('y2')
model_23.add_future_regressor('y3')
model_23.add_future_regressor('y4')
model_23.add_future_regressor('y5')
model_23.add_future_regressor('y6')

model_23.add_future_regressor('y11')

historys=model_23.fit(final,freq='h',plot_live_loss=True,epochs=50)

future_23= model_23.make_future_dataframe(final, regressors_df=most_regressor_data, periods=1000)
predict = model_23.predict(df=future_23)

fig, ax = plt.subplots(figsize=(4, 2))
    model_23.plot(predict, xlabel="Hour", ylabel="Amounts", ax=ax)
    ax.set_title("Generation Amounts", fontsize=14, fontweight="bold")


```

해당코드를 설명하자면
seasonality mode 는 additive 와 multiplicative 둘중 하나를 선택하면된다.
그리고 데이터분석으로 yearly,weekly,daily seasonal 이 있는지 확인하고 해당 계절성을 적용해준다
change points 는 데이터에서 change 포인트를 지정해준다. default 값은 5이다 
ar_sparsity 0과1사이의 값을 주면되는데 0은 희소성을 주게되고 1은 정규화를 주지않는다
freq 알맞게 설정해주면된다. 나는 Hour 기준이였으므로 H 로설정했다.

그리고 regressor 추가할때 주의해야 할점

미래 값을 알고있을때만 future_regressor 가 적용가능하다 .
즉 자세히 설명하자면 내가 dataframe 을 ds y1 y2 y3 y 로 훈련시켰다면
y1,y2,y3 는 regressor 인것이다.

훈련시키고 바로예측한것이아니라 미래에 y1,y2,y3 값이 있어야지만 온전히 예측이 가능하다는것이다.
따라서 미래 y1,y2,y3 를 모른다면 적용시킬수없다.
꼭 예측하고싶다면 y1,y2,y3 를 회귀를 통해 단변량 예측한다음 y1,y2,y3  미래값을 작성한후 예측을하면된다.


make_future_dataframe 을 보면 regressor 데이터를 넣어주어야한다. 앞서말했듯이 미래값을 알고있어야 온전한 예측이 가능하다.
따라서 regressor 데이터 도넣어주어야하는데 그러한 구조또한 y1,y2,y3 만 가지고있는 dataframe 을 만들어주면 되게되는것이다 . 길이는 예측하고자하는 길이만큼~

그리고 예측하면 된다.

