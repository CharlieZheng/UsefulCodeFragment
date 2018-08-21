# a&b 和 a&~b

### 举例：

|a=3                                       |           b=1                               |          a&b=1 != 0                           |
|:---|:---:|---:|
|a=2                                       |           b=1                               |          a&b=0 == 0                          |
|a=(11110000)_2                    |           b=(00001111)_2          |           a&b=0 == 0                          |
|a=(11110000)_2                    |           b=(11001100)_2          |           a&b=192 != 0                       |


现存在一个“彗星”对象

它有若干状态

int A（表示是否正在太阳系中飞行）=0000 0001

int B=0000 0010

int C=0000 0100

int D=0000 1000

然后用一个标识Big表示所有状态的true false情况

当Big=0000 0011时，则表示此彗星正在太阳系中飞行且也正处于B状态中，而不处于C和D状态中

1. add，当要增加状态A时

    Big |= A

2. exists，Big中是否有A状态

    return Big & A

3. remove，去除状态A

    Big = Big & ~A

### 写在最后
当然如果某个属性有多个状态则不能用这种表示法

# 在二进制数某个位置插入0

```
@Test
public void test3() {
    final int index = 80;
    long mData = Long.MAX_VALUE - 79;
    String originalMData = Long.toBinaryString(mData);
    final long mask = (1L << index) - 1;
    final long before = mData & mask;
    final long after = ((mData & ~mask)) << 1;
    mData = before | after;
    System.out.print(String.format("%-8s%80s\n%-8s%80s\n%-8s%80s\n%-8s%80s\n%-8s%80s\n%-8s%80s\n", "mask: ", Long.toBinaryString(mask), "~mask: ", Long.toBinaryString(~mask), "before: ", Long.toBinaryString(before), "after: ", Long.toBinaryString(after), "mData1: ", originalMData, "mData2: ", Long.toBinaryString(mData)));
}
```

打印情况（从右边数起第 (index=80) % 64 = 16 的位置插入了一个0）：

```
mask:                                                                   1111111111111111
~mask:                  1111111111111111111111111111111111111111111111110000000000000000
before:                                                                 1111111110110000
after:                  1111111111111111111111111111111111111111111111100000000000000000
mData1:                  111111111111111111111111111111111111111111111111111111110110000
mData2:                 1111111111111111111111111111111111111111111111101111111110110000
```

