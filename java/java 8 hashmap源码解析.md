# java 8 hashmap源码解析

[java 8 hashmap源码详细解析](https://blog.csdn.net/carson_ho/article/details/79373134)

![java 1.8扩容](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWdjb252ZXJ0LmNzZG5pbWcuY24vYUhSMGNITTZMeTlwYldkamIyNTJaWEowTG1OelpHNXBiV2N1WTI0dllVaFNNR05FYjNaTU0xWjNZa2M1YUZwRE1YQmlWMFp1V2xoTmRXRnRiR2hpYms1dlpGTTFjR0o1T1RGalIzaDJXVmRTWm1GWE1XaGFNbFo2VEhwck1FNUVUVEpPVXpGb1RrUlpNMXB0VW1oWlZFNW9UVlJGZDAxNlZYZE1ia0oxV25vNWNHSlhSbTVhVlRGMldqTkplVXd5UmpGa1J6aDBZak5LY0ZwWE5UQk1NMDR3WTIxc2QwcFVaRVJoVnpGb1dqSldWMkZYVmpOTmFUaDVURE5qZGsxVVNUQk5RUQ?x-oss-process=image/format,png)

扩容后，会增多一位， h & (length - 1) 这时参与 & 运算的 h(hash)值也会新增参与运算的位。举例如下：

```
  0 0 1 0      h                              0 [1]  1  0    新的hash值 [1是新参与的] 
& 0 0 1 1      length - 1   ----> 扩容后    &  1  0  0  0
----------                                   -------------
  0 0 1 0   10进制: 2                          0  0  0  0        
```

新增参与运算的位 = 0， 则 & 运算后还是0， 即数组下标 = 原始位置

新增参与运算的位 = 1， 则 & 运算后还是1， 即数组下标 = 原始位置 + 扩展前的旧容量

![示例图1](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWdjb252ZXJ0LmNzZG5pbWcuY24vYUhSMGNITTZMeTlwYldkamIyNTJaWEowTG1OelpHNXBiV2N1WTI0dllVaFNNR05FYjNaTU0xWjNZa2M1YUZwRE1YQmlWMFp1V2xoTmRXRnRiR2hpYms1dlpGTTFjR0o1T1RGalIzaDJXVmRTWm1GWE1XaGFNbFo2VEhwck1FNUVUVEpPVXpCNVRrUlpNbHBxVm10WmFsRXpXbTFSTTA1cVp6Rk1ia0oxV25vNWNHSlhSbTVhVlRGMldqTkplVXd5UmpGa1J6aDBZak5LY0ZwWE5UQk1NMDR3WTIxc2QwcFVaRVJoVnpGb1dqSldWMkZYVmpOTmFUaDVURE5qZGsxVVNUQk5RUQ?x-oss-process=image/format,png)

数组转换示意图：

![数组转换示意图](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWdjb252ZXJ0LmNzZG5pbWcuY24vYUhSMGNITTZMeTlwYldkamIyNTJaWEowTG1OelpHNXBiV2N1WTI0dllVaFNNR05FYjNaTU0xWjNZa2M1YUZwRE1YQmlWMFp1V2xoTmRXRnRiR2hpYms1dlpGTTFjR0o1T1RGalIzaDJXVmRTWm1GWE1XaGFNbFo2VEhwck1FNUVUVEpPVXpGclRucG9hRnBVYTNkT2VteHFUVlJTYlUxcVNYbE1ia0oxV25vNWNHSlhSbTVhVlRGMldqTkplVXd5UmpGa1J6aDBZak5LY0ZwWE5UQk1NMDR3WTIxc2QwcFVaRVJoVnpGb1dqSldWMkZYVmpOTmFUaDVURE5qZGsxVVNUQk5RUQ?x-oss-process=image/format,png)