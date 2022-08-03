[TOC]



# Enum(枚举)

## 1.声明Enum(枚举)

用动物举一个简单的例子:

```c++
UENUM()
enum class EAnimal : uint8
{
	Cat,
	Dog,
	Elephant
};
```

enum class是C++11新增的枚举，只在枚举作用域内有效，使用时需要在之前加枚举类型名和两个冒号。可以通过继承的方式指定内存占用长度，不指定时默认是int，使用规则和C#或java/C#的枚举很像，有严格的类型检查，做位运算需要先转换为底层类型（可通过std::underlying_type转换）再进行位运算

## 2.使用TEnumRange 迭代UENUM(枚举)

为了实现枚举的迭代, 我们需要使用Unreal 的如下三个宏.

- [`ENUM_RANGE_BY_COUNT`]
- [`ENUM_RANGE_BY_FIRST_AND_LAST`]
- [`ENUM_RANGE_BY_VALUES`]

### 使用 `Count`

首先，在Enum(枚举)的末尾添加一个新成员。 名字可以自定义，一般采用`Count`。

通过标记UMETA(Hidden)` 将该值隐藏，这样编辑器中不会显示该成员。

最后添加宏`ENUM_RANGE_BY_COUNT` ，并传入枚举名称以及`Count`

```c++
UENUM()
enum class EAnimal : uint8
{
	Cat,
	Dog,
	Elephant,
	Count UMETA(Hidden)
};
ENUM_RANGE_BY_COUNT(EAnimal, EAnimal::Count);
```

### 使用  First and Last

如果不想在美剧中添加额外的值，那么可以使用宏`ENUM_RANGE_BY_FIRST_AND_LAST`,和上边一样，在枚举后添加宏，这里传入的是枚举的第一个成员和最后成员。

```c++
UENUM()
enum class EAnimal : uint8
{
	Cat,
	Dog,
	Elephant
};
ENUM_RANGE_BY_FIRST_AND_LAST(EAnimal, EAnimal::Cat, EMilkshake::Elephant);
```

### 使用  Values

当枚举成员的值非连续时，使用`ENUM_RANGE_BY_VALUES`宏，将枚举成员按顺序传入。

```c++
enum class EAnimal : uint8
{
	Cat = 2,
	Dog = 5,
	Elephant = 9
};

ENUM_RANGE_BY_VALUES(EAnimal, EAnimal::Cat, EAnimal::Dog, EAnimal::Elephant)
```

## 遍历枚举 Enum

无论使用的哪种 `ENUM_RANGE_BY... ` 宏，遍历方法都是一样的:

```c++
for (EAnimal Animal : TEnumRange<EAnimal>())
{

}
```



https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/builtin-types/enum#enumeration-types-as-bit-flags
如果希望枚举类型表示选项组合，请为这些选项定义枚举成员，以便单个选项成为位字段。 也就是说，这些枚举成员的关联值应该是 2 的幂。 然后，可以使用[按位逻辑运算符](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/bitwise-and-shift-operators#enumeration-logical-operators)或 `&` 分别合并选项或交叉组合选项。 若要指示枚举类型声明位字段，请对其应用 [Flags](https://docs.microsoft.com/zh-CN/dotnet/api/system.flagsattribute) 属性。 如下面的示例所示，还可以在枚举类型的定义中包含一些典型组合。

```c#
[Flags]
public enum Days
{
    None      = 0b_0000_0000,  // 0
    Monday    = 0b_0000_0001,  // 1
    Tuesday   = 0b_0000_0010,  // 2
    Wednesday = 0b_0000_0100,  // 4
    Thursday  = 0b_0000_1000,  // 8
    Friday    = 0b_0001_0000,  // 16
    Saturday  = 0b_0010_0000,  // 32
    Sunday    = 0b_0100_0000,  // 64
    Weekend   = Saturday | Sunday
}

public class FlagsEnumExample
{
    public static void Main()
    {
        Days meetingDays = Days.Monday | Days.Wednesday | Days.Friday;
        Console.WriteLine(meetingDays);
        // Output:
        // Monday, Wednesday, Friday

        Days workingFromHomeDays = Days.Thursday | Days.Friday;
        Console.WriteLine($"Join a meeting by phone on {meetingDays & workingFromHomeDays}");
        // Output:
        // Join a meeting by phone on Friday
    
        bool isMeetingOnTuesday = (meetingDays & Days.Tuesday) == Days.Tuesday;
        Console.WriteLine($"Is there a meeting on Tuesday: {isMeetingOnTuesday}");
        // Output:
        // Is there a meeting on Tuesday: False
    
        var a = (Days)37;
        Console.WriteLine(a);
        // Output:
        // Monday, Wednesday, Saturday
    }

}
```

