MATLAB定点设计器扩展，依赖MATLAB Fixed-Point Designer和[埃博拉酱的MATLAB扩展](https://ww2.mathworks.cn/matlabcentral/fileexchange/96344-matlab-extension)

本项目的发布版本号遵循[语义化版本](https://semver.org/lang/zh-CN/)规范。开发者认为这是一个优秀的规范，并向每一位开发者推荐遵守此规范。
# 目录
为避免命名冲突，本包所有函数均在命名空间FixedPoint下，使用前需import：
```MATLAB
import FixedPoint.*
```
- [Logical2UInteger](#Logical2UInteger) 将逻辑数组拼合为无符号整数
- [UInteger2Logical](#UInteger2Logical) 将无符号整数展开为逻辑数组
# Logical2UInteger
将逻辑数组拼合为无符号整数
```MATLAB
load('+FixedPoint\Logical.mat');
%将256×8 logical每行合并成一个uint8数值，变成256×1 uint8
FixedPoint.Logical2UInteger(Logical,2);
%合并成2个uint4类型（依赖MATLAB定点设计器fi类型）
FixedPoint.Logical2UInteger(Logical,2,UIntegerBits=4);
```
本函数将逻辑数组沿指定维度拼合成无符号整数，低位在前高位在后。例如[1 1 1 1 0 0 0 0;0 0 0 0 1 1 1 1]可以沿第2维拼接成一个uint8列向量[15;240]，或者uint4矩阵[15 0;0 15]。基本数据类型uint8, uint16, uint32, uint64以外的整数类型，将使用embedded.fi类实现
## 输入参数
LogicalArray logical，必需，要拼合的逻辑数组

Dimension(1,1)uint8，可选，拼合维度。默认采用LogicalArray的最长维度

UIntegerBits(1,1)uint8=size(LogicalArray,Dimension)，名称值，整数位数。如果size(LogicalArray,Dimension)不能被UIntegerBits整除，将补0至能整除为止。
## 返回值
UIntegerArray，拼合成的数值数组。如果UIntegerBits为8、16、32或64，将对应返回uint8, uint16, uint32, uint64类型数组。除此之外的值，将返回embedded.fi类型数组，其WordLength和SumWordLength属性值均等于UIntegerBits。数组的尺寸，在Dimension维度以外和LogicalArray相等，Dimension维度长度等于ceil(size(LogicalArray,Dimension)/UIntegerBits)
# UInteger2Logical
将无符号整数展开为逻辑数组
```MATLAB
FixedPoint.UInteger2Logical((0:255)');
```
本函数将无符号整数沿指定维度展开成逻辑数组，低位在前高位在后。例如uint8列向量[15;240]可以沿第2维展开成[1 1 1 1 0 0 0 0;0 0 0 0 1 1 1 1]。支持embedded.fi类型展开。
## 输入参数
UIntegerArray，必需，要展开的无符号整数组

Dimension，可选，展开维度。默认采用最短维度。
## 返回值
LogicalArray logical，展开的逻辑数组。在Dimension以外维度尺寸和UIntegerArray相同，在Dimension维度尺寸等于size(UIntegerArray,Dimension)乘上数据类型的位数。如uint8类型为8位，numerictype(false,9,0)类型为9位