---
title: 卷积神经网络的卷积
published: 2022-01-22 
tags: [opencv, AI, CNN]
category: Python
draft: false

---

- 测试图
![A](/img/CNN.png)

```python
import math
from PIL import Image
import matplotlib.pyplot as plt

img = Image.open("CNN.png") # 打开为图像对象
img = img.convert("1") # 转灰度图

raw = img.load() # 加载图像矩阵

pngV = [[raw[w, h] for w in range(img.size[1])] for h in range(img.size[0])] # 转为一个宽高相同的二维数组用来存放灰度值

kernel = [  #卷积核
    [0, 0, 0],
    [0, 1, 0],
    [1, 0, 1],
]

kernel1 = [ #无用的卷积核
    [1]
]


def conv(v, w, step=1, pad=0, fill_value=1, bias=0):#卷积过程，参数：灰度矩阵，权重（卷积核），步长，边缘填充，边缘填充的值，偏置
    v_row = len(v)
    v_col = len(v[0])
    if pad:
        for _ in range(pad):#上边缘填充
            v.insert(0, [fill_value for _ in range(v_col + pad * 2)])
        for i in range(pad, v_row + pad):#中间两侧边缘填充
            for _ in range(pad):
                v[i].insert(0, fill_value)
                v[i].append(fill_value)
        for _ in range(pad):#下边缘填充
            v.append([fill_value for _ in range(v_col + pad * 2)])
        v_row += pad * 2
        v_col += pad * 2

    w_row = len(w)
    w_col = len(w[0])
    res = []
    for x in range(w_row // 2, v_row - w_row // 2, step):#受卷积核宽高的影响需要计算起止位置
        line = []
        for y in range(w_col // 2, v_col - w_col // 2, step):
            temp = 0
            for i in range(w_row):
                for j in range(w_col):
                    t = v[x + i - w_row // 2][y + j - w_col // 2] * w[i][j] # 灰度 乘 权重
                    temp += t # 累加乘积
            temp += bias # 加偏置
            line.append(temp)
        res.append(line)
    return res

def relu(v): # 线性整流函数
    for i in range(len(v)):
        for j in range(len(v[i])):
            v[i][j]=v[i][j] if v[i][j] > 0 else 0
    return v

def max_pooling(v,size=2): # 最大池化
    v_row = math.ceil(len(v) / size)
    v_col = math.ceil(len(v[0]) / size)
    r = []
    for y in range(v_row):
        line = []
        for x in range(v_col):
            compare = []
            for j in range(y*size, y*size + size if y*size + size < len(v) else len(v)): # 防止边缘越界
                for i in range(x*size, x*size + size if x*size+size < len(v) else len(v[0])):
                    compare.append(v[j][i])
            line.append(max(compare))
        r.append(line)
    return r



if __name__ == '__main__':
    v = conv(pngV, kernel, step=9, pad=0, bias=-763)
    v = relu(v)
    v = max_pooling(v)
    plt.imshow(v)
    plt.show()
    plt.imsave("2.png", v)

```