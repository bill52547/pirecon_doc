# `base`模块
Base模块包括了最基本的类型定义,即[`NefBaseClass`](#_class_nefbaseclass_)基类及快速定义其子类的装饰器[`nef_class`]()

## `NefBaseClass` 类
`NefBaseClass`为本程序包中的推荐默认基类,在其实现中包括了对所有用于构建本程序包中类特征的定义和实现.
    其中包括,简易定义初始化函数(`__init__`),示例和字典(`dict`)之间的相互转化,类似字典的行为更新(`update`),及不可更改(`frozen`,`immutable`)特性

`NefBaseClass`的实例将会在[`nef_class`]()中.

方法名称 | 简述
---------- | -------------
`NefBaseClass.keys()`| 返回其所有属性的名称
`NefBaseClass.values()`| 返回当前实例的属性值
`NefBaseClass.items()`| 返回当前实例的所有键值对
`NefBaseClass.update()` | 更新当前实例,并输出新实例
`NefBaseClass.asdict()` | 将实例转化为字典
`NefBaseClass.from_dict()` | 将字典转化为实例
`NefBaseClass.astype()` | 转化实例为另一个类的实例
`NefBaseClass.attr_eq()` | 比较两个实例是否相等


### 字典化(dictionarilize)
任何一个`NefBaseClass`的子类均会具有类似字典的方法及与字典之间相互转化的能力:

`NefBaseClass.keys`:

: 返回当前类的属性名称为列表
```python
@classmethod
def keys(cls) -> list
```

`NefBaseClass.values`: 

: 返回当前实例的属性值,任何`NefBaseClass`的子类的实例均可使用`values`获取其值列表.

```python
def values(self) -> list 
```

`NefBaseClass.items`:

: 任何*NefBaseClass*的子类的实例均可使用*items*获取其键-值列表.*keys*, *values*和*items*共同实现了任何*NefBaseClass*的子类的实例表现的与字典相同.

```python
def items(self, recurse: bool = False) -> list
```
当`recurse`为`true`,则会将其属性转化为字典再输出,若该属性为`NefBaseClass`子类的实例,若为`false`, 则会将其直接输出

`NefBaseClass.update`:

: 更新当前实例的某个(某些)属性,并输出一个新实例.由于`NefBaseClass`的实例具有不可更改性,因此(通常来说)无法对其直接修改,如需要原有实例的基础上发生修改,则需重新
定义一个新实例.若目的是修改原实例,则需要覆盖原实例. 需要注意的是,`update`方法需要对属性名显性声明

```python
def update(self, **kwargs) --> 'NefBaseClass'
```
实例:
```python
b = a.update(key = val_new)
```

`NefBaseClass.asdict()`:

: 提供了从当前实例到字典的转化. 将当前实例的所有键值对转化为一个字典,即消除其类型信息. 
```python
def asdict(self, recurse=False) -> dict
```
当`recurse`为`true`,则会将其属性转化为字典再输出,若该属性为`NefBaseClass`子类的实例,若为`false`, 则会将其直接输出其属性为字典的value.

`NefBaseClass.from_dict()`:

: 提供了从字典到实例的转化. 将字典中所有符合当前类的属性名的value用来初始化实例,并忽略未知的value, 即消除其类型信息. 
```python
@classmethod
def from_dict(cls, dct: dict) -> object
```
需要注意的是,该操作会递归的尝试将字典中符合`from_dict`的结构结合当前类的属性类型转化为相应的实例.

### 不同类实例之间的转化和对比
任何一个`NefBaseClass`的子类的实例均可方便的转化为另一个子类的实例,并且会按照另一个子类的属性来保留相应数值.

`NefBaseClass.astype()`:

: 转化当前实例为另一个`NefBaseClass`的子类的实例.
```python
def astype(self, cls: type) -> object
```
其中`cls`为另一个子类

`NefBaseClass.attr_eq`:
: 比较两个`NefBaseClass`实例是否相同. 会比较两个实例是否具有相同类型以及其每个属性值是否相同.需要注意的是,`data`属性并不会被比较.

```python
def attr_eq(self, other) -> bool
```

## `nef_class` 方法