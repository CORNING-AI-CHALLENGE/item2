# Corning AI challenge _ ITEM 2 

### 🏴‍☠️ 생성형 AI를 이용한 inverse problem 해결 방안 제안


<br/>

💡 Purpose of project:

Inverse problems are mathematical problems where the goal is to find the underlying cause or source of a given effect. Inverse problems are commonly encountered in various fields such as physics, engineering, and image processing. In the field of design, inverse problems arise when the goal is to generate a design based on a given set of specifications or requirements. Traditional methods for solving inverse problems in design often require extensive computations and are prone to errors. To address this issue, we’d like to solve inverse problems for optimized design pattern recommendation using generative AI methods.


<br/>

🔑 Objectives:

The primary objective of this project is to investigate the use of generative AI methods for solving inverse problems for finding optimized design pattern. The following are the specific objectives of the project.


  * Be able to recommend 2-dimensional (or 3-dimensional) pattern satisfying a given target. (It can be anything related to pattern optimization. For example, find 2-dimensional electrode pattern satisfying a given electric field)


  * Recommended patterns should be listed in order of performance/process convenience (For example, feasibility of process can be judged by the continuity of the patterns or the large spacing between patterns.)


  * Final output should include the explanation of how the generative models work and why they make certain decisions.


<br/>

📖 참고 내용 (아래는 단지 예시이며, 다른 주제를 선정하여도 상관 없습니다) 

✔️ Electric field generated by 2-d electrode pattern

![image](https://github.com/CORNING-AI-CHALLENGE/item2/assets/146830948/196055c2-f172-436c-a766-71fb4bdf20ca)


(위의 식에서 계산 편의상 A=1이라 가정)
```python
import numpy as np
import numba as nb

@nb.jit(nopython=True, parallel=True)
def cal_field(input_pattern):
    dimx, dimy = input_pattern.shape 
    field_x = np.zeros((dimx,dimy))
    field_y = np.zeros((dimx,dimy))
    field_z = np.zeros((dimx,dimy))
    for i in range(dimx):
        for j in range(dimy):
            tmp_x = 0.
            tmp_y = 0.
            tmp_z = 0.
            for ii in range(dimx):
                for jj in range(dimy):
                    tmp_x += input_pattern[ii,jj]*(i-ii)/(((i-ii)**2+(j-jj)**2+1**2)**(3/2))
                    tmp_y += input_pattern[ii,jj]*(j-jj)/(((i-ii)**2+(j-jj)**2+1**2)**(3/2))
                    tmp_z += input_pattern[ii,jj]*(1.)/(((i-ii)**2+(j-jj)**2+1**2)**(3/2))
            field_x[i,j] = tmp_x
            field_y[i,j] = tmp_y
            field_z[i,j] = tmp_z
    return field_x,field_y,field_z
```


✔️ Autoencoder를 이용한 electrode pattern 예측 base code
RGB code 예시로

✔️ 예측 결과 예시 

![image](https://github.com/CORNING-AI-CHALLENGE/item2/assets/146830948/3c069f00-74e4-492e-a99d-6adb782b49cd)





