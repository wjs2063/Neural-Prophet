# Neural-Prophet
## how to use??

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

