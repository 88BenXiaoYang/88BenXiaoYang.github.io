---
title: Foundation构架秘籍-函数
date: 2020-09-14 19:20:51
tags:
---

# 数学运算(math)函数

## 算术运算函数

* rand()函数：产生随机数
* abs()函数/labs()函数：整数的绝对值
* fabs()/fabsf()/fabsl()函数：浮点数的绝对值
* floor()/floorf()/floorl()函数：向下取整
* ceil()/ceilf()/ceill()函数：向上取整
* round()/roundf()/roundl()函数：四舍五入
* sqrt()/sqrtf()/sqrtl()函数：求平方根
* fmax()/fmaxf()/fmaxl()函数：求最大值
* fmin()/fminf()/fminl()函数：求最小值
* hypot()/hypotf()/hypotl()函数：求直角三角形斜边的长度
* fmod()/fmodf()/fmodl()函数：求两数整除后的余数
* modf()/modff()/modfl()函数：浮点数分解为整数和小数
* frexp()/frexpf()/frexpl()函数：浮点数分解尾数和以二为底的指数

## 三角运算函数

* sin()/sinf()/sinl()/函数：求正弦值
* sinh()/sinhf()/sinhl()函数：求双曲正弦值
* cos()/cosf()/cosl()函数：求余弦值
* cosh()/coshf()/coshl()函数：求双曲余弦值
* tan()/tanf()/tanl()函数：求正切值
* tanh()/tanhf()/tanhl()函数：求双曲正切值

## 反三角运算

* asin()/asinf()/asinl()函数：求反正弦值
* asinh()/asinhf()/asinhl()函数：求反双曲正弦值
* acos()/acosf()/acosl()函数：求反余弦值
* acosh()/acoshf()/acoshl()函数：求反双曲余弦值
* atan()/atanf()/atanl()函数：求反正切值
* atan2()/atan2f()/atanl()函数：求坐标值的反正切值
* atanh()/atanhf()/atanhl()函数：求反双曲正切值

## 指数和对数运算

* pow()/powf()/powl函数：求n的m次方的值
* exp()/expf()/expl()函数：求e的x次方的值
* exp2()/exp2f()/exp2l()函数：2的x的次方的值
* log()/logf()/logl()函数：求以e为底的对数值
* log10()/log10f()/log10l()函数：求以10为底的对数值

# 数字对象(NSNumber)

## 整型对象

* numberWithShort:方法：创建短整型数字对象
* numberWithUnsignedShort:方法：创建无符号短整型数字对象
* numberWithInteger:方法：创建整型数字对象
* numberWithUnsignedInteger:方法：创建无符号整型数字对象
* numberWithInt:方法：创建整型数字对象
* numberWithUnsignedInt:方法：创建无符号整型数字对象
* numberWithLong:方法：创建并初始化长整型数字对象
* numberWithUnsignedLong:方法：创建并初始化无符号长整型数字对象
* numberWithLongLong:方法：创建并初始化长长整型数字对象
* numberWithUnsignedLongLong:方法：创建并初始化无符号长长整型对象
* initWithShort:方法：初始化短整型数字对象
* initWithUnsignedShort:方法：初始化无符号短整型数字对象
* initWithInteger:方法：初始化整型数字对象
* initWithUnsignedInteger:方法：初始化无符号整型数字对象
* initWithInt:方法：初始化整型数字对象
* initWithUnsignedInt:方法：初始化无符号整型数字对象
* initWithLong:方法：初始化长整型数字对象
* initWithUnsignedLong:方法：初始化无符号长整型数字对象
* initWithLongLong:方法：初始化长长整型数字对象
* initWithUnsignedLongLong:方法：初始化无符号长长整型数字对象
* shortValue:方法：取短整型数字对象的值
* unsignedShortValue:方法：取无符号短整型数字对象的值
* integerValue:方法：取整型数字对象的值
* unsignedIntegerValue:方法：取无符号整型数字对象的值
* intValue:方法：取整型数字对象的值
* unsignedIntValue:方法：取无符号整型数字对象的值
* longValue:方法：取长整型数字对象的值
* unsignedLongValue:方法：取无符号长整型数字对象的值
* longlongValue:方法：取长长整型数字对象的值
* unsignedLongLongValue:方法：取无符号长长整型数字对象的值

## 字符型对象

* numberWithChar:方法：创建并初始化字符型数字对象
* numberWithUnsignedChar:方法：创建并初始化无符号字符型数字对象
* initWithChar:方法：初始化字符型对象
* initWithUnsignedChar:方法：初始化无符号字符型对象
* charValue:方法：取字符型数字对象的值
* unsignedCharValue:方法：取无符号字符型数字对象的值

## 单精度型对象

* numberWithFloat:方法：创建并初始化单精度型数字对象
* initWithFloat:方法：初始化单精度型数字对象
* floatValue:方法：取单精度型数字对象的值

## 双精度型对象

* numberWithDouble:方法：创建并初始化一个双精度型数字对象
* initWithDouble:方法：初始化双精度型数字对象
* doubleValue:方法：取双精度型数字对象的值

## 布尔型对象

* numberWithBool:方法：创建并初始化布尔型数字对象
* initWithBool:方法：初始化布尔类型对象
* boolValue:方法：取布尔型数字对象的值

## 数字对象通用方法

* isEqualToNumber:方法：比较两对象值是否相等
* compare:方法：比较值的大小

# 字符串对象(NSString)

## 创建及初始化

* string:方法：创建字符串
* stringWithstring:方法：用字符串来创建字符串
* stringWithCstring:方法：创建C字符串
* stringWithFormat:方法：创建NSLog()格式的字符串
* stringWithContentsOfFile:方法：将创建的字符串设置为指定文件的内容
* stringWithContentsOfURL:方法：将创建字符串设置为url的内容
* stringWithUTF8String:方法：将创建的字符串转换为UTF8字符串
* init:方法：初始化字符串
* initWithString:方法：用字符串来初始化字符串
* initWithCString:方法：初始化字符串
* initWithFormat:方法：用NSLog()格式初始化字符串
* initWithContentsOfFile:方法：将初始化的字符串设置为指定文件的内容
* initWithContentsOfURL:方法：将初始化的字符串设置为url的内容
* initWithUTF8String:方法：将初始化的字符串转换为UTF8字符串

## 判断和比较

* isEqualTostring:方法：比较字符串是否相等
* hasPrefix:方法：判断字符串是否以某个字符开始
* hasSuffix:方法：判断字符串是否以某个字符结束
* compare:方法：比较字符串的大小
* caseInsensitiveCompare:方法：不考虑大小写的比较大小

## 大小写转换

* uppercaseString:方法：小写字母转为大写字母
* lowercaseString:方法：大写字母转为小写字母
* capitalizedString:方法：将每个单词的首字母大写

## 截取

* substringToIndex:方法：从字符串的开头一直截取到指定的位置
* substringFromIndex:方法：从指定位置开始截取字符串直到结束
* substringWithRange:方法：根据指定范围返回子字符串
* characterAtIndex:方法：返回索引号所在字符串中的字符

## 转换类型

* doubleValue:/floatValue:方法：返回转换为浮点类型的值
* intValue:方法：返回转换为整型的值
* boolValue:方法：返回转换为布尔类型的值

## 对文件的处理

* stringByAppendingPathExtension:方法：为文件添加扩展名
* pathExtension:方法：获取文件扩展名
* stringByDeletingPathExtension:方法：删除扩展名
* writeToFile:方法：将字符串写入到文件
* writeToURL:方法：将字符串写入到url中
* stringByExpandingTildeInPath:方法：将“～”替换成系统的主目录
* stringByAbbreviatingWithTildeInPath:方法：将系统主目录替换为“～”
* lastPathComponent:方法：获取路径中的文件名
* stringByDeletingLastPathComponent:方法：获取路径中文件所在的位置
* stringByAppendingPathComponent:方法：组合位置和文件名
* isAbsolutePath:方法：判断绝对路径

## 其他

* length:方法：求字符串的长度
* stringByAppendingString:方法：字符串后面增加一个新字符串
* rangeOfString:方法：查找字符串中是否包含其他字符串
* stringByTrimmingCharactersInSet:方法：去除空格或回车

## 可变字符串 (NSMutableString)

* stringWithCapacity:方法：按照固定长度生成空字符串
* initWithCapacity:方法：初始化一个固定长度的字符串
* appendString:方法：在字符串的末尾附加另一个字符串
* appendFormat:方法：附加一个格式化字符串
* SetString:方法：将字符串设置为规定的内容
* insertString:方法：在指定位置插入字符串
* deleteCharactersInRange:方法：删除指定范围的字符串
* replaceCharactersInRange:方法：使用字符串代替指定范围的字符串
* replaceOccurrencesOfString:方法：替换 将字符串中的某个字全部替换成别一个字
* stringByReplacingOccurrencesOfString:方法；将字符串中的某个字全部替换成别一个字

# 数组对象(NSArray)

## 创建及初始化

* array:方法：创建数组
* arrayWithArray:方法：通过一个数组创建另一个数组
* arrayWithContentsOfFile:方法：创建数组将内容设置为指定文件内容
* arrayWithContentsOfURL:方法：创建数组将内容设置为url指定内容
* arrayWithObject:方法：创建具有一个元素的数组
* arrayWithObjects:方法：创建具有多个元素的数组
* init:方法：初始化数组
* initWithArray:方法：用数组初始化数组
* initWithContentsOfFile:方法：初始化数组将内容设置为指定文件内容
* initWithContentsOfURL:方法：初始化数组将内容设置为url指定内容
* initWithObjects:方法：初始化具有多个元素的数组

## 数组元素的操作

* containsObject:方法：判断数组中是否包含某个元素
* count:方法：计算元素个数
* firstObjectCommonWithArray:方法：获取首元素
* lastObject:方法：获取最后一个元素
* objectAtIndex:方法：获取在某个位置的数组元素
* objectAtIndexs:方法：获取数组元素
* arrayByAddingObject:方法：在数组末尾添加元素
* arrayByAddingObjectsFromArray:方法：在数组的末尾添加另一个数组
* subarrayWithRange:方法：数组的一部分创建数组
* isEqualToArray:方法：比较数组是否相等
* indexOfObject:方法：返回元素所在的位置
* indexOfObjectIdenticalTo:方法：返回元素所在的位置
* componentsJoinedByString:方法：数组转换为字符串
* componentsSeparatedByString:方法：字符串转换为数组
* sortedArrayHint:方法：数组转换为数据对象
* writeToFile:方法：将数组中的内容写入到文件
* writeToUrl:方法：将数组中的内容写入到url
* objectEnumerator:方法：数组元素从前向后访问
* reverseObjectEnumerator:方法：数组元素从后向前访问
* pathsMatchingExtensions:方法：查看某文件夹下的东西
* sortedArrayUsingFunction:方法：实现数组元素的简单排序

## 可变数组 (NSMutableArray)

* arrayWithCapacity:方法：创建一个具有固定长度的可变数组
* initWithCapacity:方法：初始化一个具有固定长度的可变数组
* addObject:方法：添加数组元素
* addObjectsFromArray:方法：用数组创建可变数组
* removeObject:方法：删除指定的元素
* removeAllObjects:方法：删除可变数组中的所有元素
* removeLastObject:方法：删除可变数组中的最后一个元素
* removeObjectAtIndex:方法：删除指定位置的元素
* removeObjectsAtIndex:方法：删除可变数组中的元素
* removeObjectsInRange:方法：删除某个范围内的可变数组中的元素
* removeObjectsInArray:方法：删除与另一个数组相同的元素
* replaceObjectAtIndex:方法：替换可变数组中某一位置的元素
* replaceObjectsAtIndexes:方法：替换可变数组中的多个元素
* replaceObjectsInRange:方法：替换某一范围的数组元素
* insertObject:方法：在某一位置插入数组元素
* insertObjects:方法：在某一位置或范围插入另一数组元素
* exchangeObjectAtIndex:方法：交换两个元素
* setArray:方法：设置可变数组中内容

# 字典对象(NSDictionary)

## 创建及初始化

* dictionary:方法：创建字典
* dictionaryWithContentsOfFile:方法：将创建的字典内容设置为指定文件内容
* dictionaryWithContentsOfURL:方法：将创建的字典内容设置为指定url内容
* dictionaryWithDictionary:方法：用字典创建字典
* dictionaryWithObject:方法：创建具有一个键-值的字典
* dictionaryWithObjects:方法：创建具有多个键-值的字典
* dictionaryWithObjectsAndKeys:方法：创建具有多个键-值的字典
* init:方法：初始化字典
* initWithContentsOfFile:方法：将初始化的字典内容设置为指定文件内容
* initWithContentsOfURL:方法：将初始化的字典内容设置为指定url内容
* initWithDictionary:方法：用字典初始化字典
* initWithObjects:方法：初始化具有多个键-值的字典
* initWithObjectsAndKeys:方法：初始化具有多个键-值的字典

## 访问键-值

* objectForKey:方法：返回键的值
* allKeys:方法：返回所有的键
* allValue:方法：返回所有的值
* allKeysForObject:方法：返回值所对应的键
* keyEnumerator:方法：将字典中所有的键放到NSEnumerator对象中
* objectEnumerator:方法：将字典中所有的值放到一个NSEnumerator对象中

## 文件的处理

* fileCreationDate:方法：文件创建日期
* fileModificationDate:方法：文件修改的日期
* fileSize:方法：文件的大小
* fileExtensionHidden:方法：扩展名是否隐藏
* fileType:方法：文件的类型
* fileGroupOwnerAccountID:方法：文件所属组标识
* fileGroupOwnerAccountName:方法：文件所属组名
* fileHFSCreatorCode:方法：文件分层系统创建者编码
* fileHFSTypeCode:方法：文件分层系统类型编码
* fileIsAppendOnly:方法：文件是否只读
* fileIsImmutable:方法：文件是否可变
* fileOwnerAccountID:方法：文件所属人标识
* fileOwnerAccountName:方法：文件所属人
* filePosixPermissions:方法：权限
* fileSystemFileNumber:方法：文件系统的文件编号
* fileSystemNumber:方法：文件系统编号
* writeToFile:方法：字典内容写入文件中
* writeToURL:方法：字典内容写入url中

## 其他

* count:方法：字典键-值个数
* isEqualToDictionary:方法：判断字典是否相等

## 可变字典

* dictionaryWithCapacity:方法：创建固定长度的可变字典
* initWithCapacity:方法：初始化固定长度的可变字典
* setObject:方法：设置键-值
* setDictionary:方法：用字典设置可变字典中的内容
* removeAllObjects:方法：删除所有的内容
* removeObjectForKey:方法：删除键所对应的值
* removeObjectsForKeys:方法：删除多个键所有的值
* addEntriesFromDictionary:方法：将字典中的键-值添加到可变字典中

# 集合(NSSet)

## 创建以初始化

* set:方法：创建集合
* setWithArray:方法：用数组创建集合
* setWithObject:方法：创建具有一个元素的集合
* setWithObjects:方法：创建具有多个元素的集合
* setWithSet:方法：集合创建集合
* init:方法：初始化集合
* initWithArray:方法：用数组初始化集合
* initWithObjects:方法：初始化具有多个元素的集合
* initWithSet:方法：集合初始化集合

## 访问元素

* objectEnumerator:方法：将所有集合中的元素放到NSEnumerator对象中
* allObjects:方法：返回集合中所有的元素
* anyObject:方法：返回任意一个元素
* count:方法：返回元素个数

## 判断比较

* containsObject:方法：判断集合中是否包含某个元素
* member:方法：判断集合中是否包含某个元素并返回
* isSubsetOfSet:方法：判断一个集合是否是一个集合的子集
* intersectsSet:方法：判断交集
* isEqualToSet:方法：判断集合是否相等

## 可变集合

* setWithCapacity:方法：创建具有固定长度的可变集合
* initWithCapacity:方法：初始化具有固定长度的可变集合
* setSet:方法：通过集合设置可变集合的内容
* addObject:方法：添加元素
* addObjectsFromArray:方法：添加数组中的元素
* removeAllObjects:方法：删除所有元素
* removeObject:方法：删除指定的元素
* unionSet:方法：添加集合元素
* minusSet:方法：去除另一个集合中的元素
* intersectSet:方法：做交集

# 文件(NSFileManager、NSFileHandle)

## defaultManger:方法：创建文件管理器

## 文件与目录的操作

* createFileAtPath:方法：创建文件
* copyItemAtPath:方法：复制文件
* moveItemAtPath:方法：移动文件
* removeItemFileAtPath:方法：删除文件
* attributesOfItemAtPath:方法：获取文件的属性
* setAttributes:方法：更改属性
* currentDirectoryPath:方法：获取当前的目录
* changeCurrentDirectoryPath:方法：更改目录
* createDirectoryAtPath:方法：创建目录

## 获取文件和目录信息

* contentsAtPath:方法：获取文件中的信息
* enumeratorAtPath:方法：枚举目录
* contentsOfDirectoryAtPath:方法：列举目录

## 判断文件

* fileExistsAtPath:方法：判断文件是否存在
* isReadableFile:方法：判断是否能进行读取操作
* isWritableFileAtPath:方法：判断是否能进行写入操作
* isDeletableFileAtPath:方法：判断是否可删除
* isExecutableFileAtPath:方法：判断是否可以执行
* contentsEqualAtPath:方法：判断是否相等

## 文件读取

* init:方法：初始化文件读写对象
* fileHandleForReadingAtPath:方法：读取时打开文件
* fileHandleForWritingAtPath:方法：写入时打开文件
* fileHandleForUpdatingAtPath:方法：更新时打开文件
* writeData:方法：数据写入文件
* readDataToEndOfFile:方法：读取数据
* readDataOfLength:方法：读取固定大小的内容
* offsetInFile:方法：获取当前偏移量
* seekToFileOffset:方法：设置当前的偏移量
* seekToEndOfFile:方法：将偏移量定位到文件尾
* truncateFileAtOffset:方法：设置字节
* availableData:方法：返回可用数据
* closeFile:方法：关闭文件

## 目录工具函数

* NSUserName()函数：返回登录名
* NSFullUserName()函数：返回完整用户名
* NSHomeDirectory()函数：返回路径
* NSHomeDirectoryForUser()函数：返回用户的主目录
* NSTemporaryDirectory()函数：返回临时文件的路径目录

# 时间和日历(NSDate、NSDateFormatter、NSCalendarDate、NSCalendar、NSTimeZone、NSTimer)

## 时间的创建及初始化

* date:方法：创建时间
* dateWithString:方法：用字符串创建时间
* dateWithNaturalLanguageString:方法：用字符串创建时间
* dateWithTimeInterval:方法：用时间间隔创建时间
* dateWithTimeIntervalSince1970:方法：用时间间隔创建时间
* dateWithTimeIntervalSinceNow:方法：用时间间隔创建时间
* dateWithTimeIntervalSinceReferenceDate:方法：用时间间隔创建时间
* init:方法：初始化时间
* initWithString:方法：用字符串初始化时间
* initWithTimeInterval:方法：用时间间隔初始化时间
* initWithTimeIntervalSince1970:方法：用时间间隔初始化时间
* initWithTimeIntervalSinceNow:方法：用时间间隔初始化时间
* initWithTimeIntervalSinceReferenceDate:方法：用时间间隔初始化时间

## 时间的比较

* isEqualToDate:方法：比较是否相等
* compare:方法：比较时间
* earlierDate:方法：比较哪个时间早
* laterDate:方法：比较哪个时间晚

## 获取时间

* dateByAddingTimeInterval:方法：获取经过时间间隔后的时间
* distantPast:方法：获取过去的时间
* distantFuture:方法：获取将来的时间
* timeIntervalSinceDate:方法：获取两时间的差值
* timeIntervalSinceNow:方法：获取两时间的差值
* timeIntervalSince1970:方法：获取两时间的差值
* timeIntervalSinceReferenceDate:方法：获取两时间的差值

## 时间和字符串的相互转换

* init:方法：初始化用于时间转换的对象
* setDateFormat:方法：设置格式
* initWithDateFormat:方法：初始化用于时间转换的对象

## 日历时间的创建及初始化

* calendarDate:方法：创建日历时间
* dateWithYear:方法：创建日历时间并设置内容
* dateWithString:方法：创建日历时间并设置内容及格式
* init:方法：初始化日历时间
* initWithYear:方法：初始化日历时间并设置内容
* initWithString:方法：初始化日历时间并设置内容及格式

## 获取日历时间信息

* dayOfWeek:方法：获取天数
* dayOfMonth:方法：获取天数
* dayOfYear:方法：获取天数
* hourOfDay:方法：获取时间
* minuteOfHour:方法：获取时间
* secondOfMinute:方法：获取时间
* monthOfYear:方法：获取时间
* yearOfCommonEra:方法：获取年
* dayOfCommonEra:方法：获取天数
* calendarFormat:方法：获取日历的格式
* timeZone:方法：获取时区
* dateByAddingYears:方法：获取日期时间

## 设置日历时间

* setCalendarFormat:方法：设置日历的格式
* setTimeZone:方法：设置时区

## 日历的使用

* currentCalendar:方法：创建日历
* autoupdatingCurrentCalendar:方法：获取日历
* initWithCalendarIdentifier:方法：初始化日历
* local:方法：获取区域
* firstWeekday:方法：获取每周的第一天
* minimumDaysInFirstWeek:方法：获取天数
* calendarIdentifier:方法：获取日历
* setFirstWeekday:方法：设置每周的第一天
* setMinimumDaysInFirstWeek:方法：设置天数

## 时区的使用

* timeZoneWithName:方法：用已知时区创建时区
* timeZoneWithAbbreviation:方法：用已知时区创建时区
* timeZoneForSecondsFromGMT:方法：用偏移创建时区
* initWithName:方法：用已知时区初始化时区
* systemTimeZone:方法：获取系统的时区
* localTimeZone:方法：获取本地时区
* knownTimeZoneNames:方法：返回所有时区
* name:方法：获取名称
* abbreviation:方法：获取缩写
* secondsFromGMT:方法：获取秒数

## 定时器的使用

* timerWithTimeInterval:方法：创建定时器
* initWithFireDate:方法：初始化定时器
* setFireDate:方法：设置时间
* fireDate:方法：返回时间
* invalidate:方法：使定时器无效
* isValid:方法：判断是否有效

# 进程、线程、锁(NSProcessInfo、NSThread、NSLock)

## 使用进程

* processInfo:方法：创建进程
* init:方法：初始化进程
* processName:方法：获取进程的名称
* environment:方法：获取变量/值
* globallyUniqueString:方法：生成字符串
* operatingSystem:方法：获取操作系统信息
* operatingSystemName:方法：获取操作系统的名称
* operatingSystemVersionString:方法：获取操作系统的版本信息
* processIdentifier:方法：获取进程的标识符
* arguments:方法：获取进程的参数
* hostName:方法：获取主机名称
* setProcessName:方法：设置进程的名称
* processorCount:方法：获取CPU的数目

## 线程的创建及初始化

* detachNewThreadSelector:方法：创建线程
* init:方法：初始化线程
* initWithTarget:方法：初始化线程

## 执行线程

* start:方法：开启线程
* cancel:方法：取消线程
* exit:方法：结束线程

## 获取与设置线程

* currentThread:方法：获取当前线程
* threadPriority:方法：获取属性值
* setThreadPriority:方法：设置属性值
* name:方法：获取名称
* setName:方法：设置名称
* stackSize:方法：获取堆栈
* setStackSize:方法：设置堆栈

## 判断线程

* isMultiThreaded:方法：判断线程是否为主线程
* isExecuting:方法：判断线程是否在执行
* isCancelled:方法：判断线程是否取消
* isFinished:方法：判断线程是否结束

## 使用互斥锁

* lock:方法：调用锁
* unlock:方法：关闭锁
* trylock:方法：锁定锁
* lockBeforeDate:方法：在一定时间内获取锁

## 使用递归锁

* lock:方法：调用锁
* unlock:方法：关闭锁
* tryLock:方法：获取锁
* lockBeforeDate:方法：在一定时间内获取锁

## 使用条件锁

* initWithCondition:方法：初始化条件锁
* condition:方法：获取条件
* lockWhenCondition:方法：在条件允许下调用锁
* unlockWithCondition:方法：在条件允许下关闭锁
* tryLockWhenCondition:方法：在条件允许下获取锁
* lockWhenCondition:方法：在条件和时间允许下获取锁
* tryLock:方法：获取锁
* lockBeforeDate:方法：在一定时间下获取锁
* lock:方法：调用锁
* unlock:方法：关闭锁

# 数据对象及归档(NSData、NSKeyedArchiver)

## 数据对象的创建及初始化

* data:方法：创建数据对象
* dataWithBytes:方法：用已有数据创建数据对象
* dataWithContentsOfFile:方法：将数据对象内容设置为指定文件内容
* dataWithContentsOfURL:方法：将创建对象的内容设置为url指定的内容
* dataWithData:方法：用已有数据对象创建新的数据对象
* init:方法：初始化数据对象
* initWithBytes:方法：用已有数据初始化数据对象
* initWithContentsOfFile:方法：对数据对象初始化并将其设置为指定文件内容
* initWithContentsOfURL:方法：对数据对象初始化并将其设置为指定url内容
* initWithData:方法：用已有数据对象初始化新的数据对象

## 数据对象的使用

* bytes:方法：将数据对象转换为字符
* dataUsingEncoding:方法：将字符串转换为数据对象
* length:方法：计算数据对象的长度
* isEqualToData:方法：判断两个数据对象是否相等
* subdataWithRange:方法：截取数据对象
* writeToFile:方法：写入文件
* writeToURL:方法：写入url

## 可变数据对象

* dataWithCapacity:方法：创建一个具有固定空间大小的可变数据对象
* dataWithLength:方法：创建具有固定长度的可变数据对象
* initWithCapacity:方法：初始化具有固定空间大小的可变数据对象
* initWithLength:方法：初始化具有固定长度的可变数据对象
* setData:方法：设置内容
* setLength:方法：设置长度
* appendBytes:方法：添加数据
* appendData:方法：添加数据对象
* mutableBytes:方法：可变数据对象转化为字符
* replaceBytesInRange:方法：替换
* resetBytesInRange:方法：删除

## 归档

* archiveRootObject:方法：数据归档
* unarchiveObjectWithFile:方法：取消归档
* encodeBool:方法：对布尔类型的数据编码
* decodeBoolForKey:方法：对布尔类型数据解码
* encodeInt:方法：对整型数据编码
* decodeIntForKey:方法：对整型数据解码
* encodeFloat:/encodeDouble:方法：对浮点型数据编码
* decodeFloatForKey:/decodeDoubleForKey:方法：对浮点型数据解码
* encodeObject:方法：对对象进行编码
* decodeObjectForKey:方法：对对象进行解码
* archivedDataWithRootObject:方法：将其他类型的数据进行转换
* initForWritingWithMutableData:方法：初始化
* initForReadingWithData:方法：初始化

# 窗口和颜色(NSWindow、NSColor)

## 获取窗口信息

* aspectRatio:方法：获取窗口的纵横比
* orderedIndex:方法：获取索引
* title:方法：获取窗口的标题
* miniwindowTitle:方法：获取窗口最小化后的标题
* frame:方法：获取窗口的位置和大小
* minSize:方法：获取窗口的最小尺寸
* maxSize:方法：获取窗口的最大尺寸
* miniwindowImage:方法：获取窗口最小化后的图片
* backgroundColor:方法：获取窗口的背景颜色
* childWindows:方法：获取子窗口
* alphaValue:方法：获取窗口的透明度值

## 设置窗口

* setAspectRatio:方法：设置窗口的纵横比
* setIsVisible:方法：设置窗口是否可见
* setTitle:方法：设置窗口标题
* setMiniwindowTitle:方法：设置窗口最小化后的标题
* setIsMiniaturized:方法：设置窗口是否最小化
* setIsZoomed:方法：设置窗口是否最大化
* setFrame:方法：设置窗口的位置和大小
* setFrameOrigin:方法：设置窗口的位置
* setFrameTopLeftPoint:方法：设置窗口的位置
* setMinSize:方法：设置窗口的最小尺寸
* setMaxSize:方法：设置窗口的最大尺寸
* setMiniwindowImage:方法：设置窗口最小化后的图片
* setBackgroundColor:方法：设置窗口的背景颜色
* setCanHide:方法：设置窗口是否可以隐藏
* setAlphaValue:方法：设置窗口的透明度值
* disableFlushWindow:方法：将窗口设置为禁用的
* setHasShadow:方法：设置窗口的阴影

## 判断窗口

* isVisible:方法：判断窗口是否可见
* isMiniaturized:方法：判断窗口是否最小化
* isZoomed:方法：判断窗口是否最大化
* canHide:方法：判断窗口是否可以隐藏
* hasShadow:方法：判断窗口是否有阴影
* isMiniaturizable:方法：判断窗口是否有最小化按钮
* hasTitleBar:方法：判断窗口是否有工具栏

## 创建自定义颜色对象

* colorWithCalibratedRed:方法：用标准RGB分量创建颜色对象
* colorWithCalibratedWhite:方法：用标准灰度分量创建颜色对象
* colorWithCalibratedHue:方法：用标准HSB分量创建颜色对象
* colorWithDeviceCyan:方法：用设备CMYB分量创建颜色对象
* colorWithDeviceRed:方法：用设备RGB分量创建颜色对象
* colorWithDeviceWhite:方法：用设备灰度分量创建颜色对象
* colorWithDeviceHue:方法：用设备HSB分量创建颜色对象
* colorWithPatternImage:方法：用图像创建颜色对象

## 创建颜色对象

* redColor:方法：用红色创建颜色对象
* greenColor:方法：用绿色创建颜色对象
* blueColor:方法：用蓝色创建颜色对象
* cyanColor:方法：用青色创建颜色对象
* magentaColor:方法：用紫红色创建颜色对象
* yellowColor:方法：用黄色创建颜色对象
* blackColor:方法：用黑色创建颜色对象
* brownColor:方法：用棕色创建颜色对象
* darkGrayColor:方法：用深灰色创建颜色对象
* grayColor:方法：用灰色创建颜色对象
* lightGrayColor:方法：用浅灰色创建颜色对象
* orangeColor:方法：用橙色创建颜色对象
* purpleColor:方法：用紫色创建颜色对象
* whiteColor:方法：用白色创建颜色对象

## 获取颜色分量

* redComponent:方法：获取红色的分量
* greenComponent:方法：获取绿色的分量
* blueComponent:方法：获取蓝色的分量
* cyanComponent:方法：获取青色的分量
* magentaComponent:方法：获取紫红色分量
* yellowComponent:方法：获取黄色分量
* blackComponent:方法：获取黑色的分量
* whiteComponent:方法：获取白色的分量
* alphaComponent:方法：获取透明度分量
* hueComponent:方法：获取色调的分量
* saturationComponent:方法：获取饱和度的分量
* brightnessComponent:方法：获取亮度分量
* patternImage:方法：获取图像信息

# 自定义视图(NSView)

## initWithFrame:方法：初始化自定义视图

## 获取与设置自定义视图信息

* frame:方法：获取自定义视图的框架
* setFrame:方法：设置自定义视图框架
* frameRotation:方法：获取自定义视图的旋转度数
* setFrameRotation:方法：设置自定义视图旋转度数
* setFrameOrigin:方法：设置自定义视图的位置
* setFrameSize:方法：设置自定义视图的大小
* bounds:方法：获取自定义视图框架
* setBounds:方法：设置自定义视图框架
* boundsRotation:方法：获取自定义视图的旋转度数
* setBoundsRotation:方法：设置自定义视图旋转的度数
* setBoundsOrigin:方法：设置视图的位置
* setBoundsSize:方法：设置视图的大小
* subviews:方法：获取子视图
* setPostsFrameChangedNotifications:方法：设置是否接收视图的变化
* setPostsBoundsChangedNotifications:方法：设置是否接收视图的变化
* printJobTitle:方法：获取输出标题

## drawRect:方法：绘图

## 判断自定义视图

* postsFrameChangedNotifications:方法：判断是否接收视图变换的消息
* postsBoundsChangedNotifications:方法：判断是否接收视图变换的消息
* isFlipped:方法：判断视图是否翻转
* isRotatedFromBase:方法：判断视图是否旋转
* isRotatedOrScaledFromBase:方法：判断视图是否旋转或缩放
* canDraw:方法：判断视图是否绘制
* isOpaque:方法：判断视图是否不透明

## 操作自定义视图

* addSubview:方法：添加视图
* removeFromSuperview:方法：删除视图
* replaceSubview:方法：替换视图

# 文本框和文本视图(NSTextField、NSTextView)

## 获取与设置文本框信息

* stringValue:方法：获取文本框的字符串
* setStringValue:方法：设置文本框中的字符串
* backgroundColor:方法：获取文本框的背景颜色
* setBackgroundColor:方法：设置文本框的背景颜色
* textColor:方法：获取字符串的颜色
* setTextColor:方法：设置字符串的颜色
* setImportsGraphics:方法：设置是否可以将图像拖到文本框
* setEditable:方法：设置文本框是否可以编译
* bezelStyle:方法：获取文本框边框的风格
* setBezelStyle:方法：设置文本框边框的风格
* setBezeled:方法：设置文本框是否接受bezeled边框
* setBordered:方法：设置文本框是否接受黑边框

## 判断文本框信息

* importsGraphics:方法：判断是否可以将图像拖到文本框
* isEditable:方法：判断文本框是否可以编辑
* isBezeled:方法：判断文本框是否接受了bezeled边框
* isBordered:方法：判断文本框是否接受了黑边框
* acceptsFirstResponder:方法：判断文本框是否可以编辑

## initWithFrame:方法：创建并初始化文本视图

## 获取与设置文本视图信息

* backgroundColor:方法：获取文本视图的颜色
* setBackgroundColor:方法：设置文本视图的背景颜色
* setImportsGraphics:方法：设置文件是否可以导入到文本视图
* setAcceptsGlyphInfo:方法：设置文本视图是否接受字形信息
* setAlignment:方法：设置文本视图内容的对齐方式
* insertionPointColor:方法：获取插入点的颜色
* setInsertionPointColor:方法：设置插入点的颜色
* setAllowsUndo:方法：设置文本视图是否可以撤销
* selectedTextAttributes:方法：获取用于指示选择的属性
* setSelectedTextAttributes:方法：设置文本视图用于指示选择的属性
* textContainer:方法：获取文本框的文本容器
* acceptableDragTypes:方法：获取文本视图的数据类型
* markedTextAttributes:方法：获取绘制标记的文本属性
* setMarkedTextAttributes:方法：设置绘制标记的文本属性
* setSmartInsertDeleteEnabled:方法：设置选择字符串周围的空间
* markedRange:方法：获取被标记文本的范围
* selectedRange:方法：获取选中文本的范围
* setSelectedRange:方法：设置文本的选中范围
* typingAttributes:方法：获取新文本的属性
* setTypingAttributes:方法：设置新文本的属性

## 判断文本视图的信息

* importsGraphics:方法：判断文件是否可以导入到文本视图
* acceptsGlyphInfo:方法：判断文本视图是否接受字形信息
* allowsUndo:方法：判断文本视图是否启用撤销
* smartInsertDeleteEnabled:方法：判断选择字符串周围的空间

# 图像、图像视图(NSImage、NSImageView)

## 加载图像

* imageNamed:方法：加载Supporting Files文件夹中的图片
* initWithContentsOfURL:方法：加载URL中的图像
* initWithContentsOfFile:方法：加载文件中的图片
* initWithSize:方法：加载图像的大小

## 获取与设置图像信息

* size:方法：获取图像的大小
* setSize:方法：设置图像的大小
* setFlipped:方法：设置图像是否倒立
* cacheMode:方法：获取图像的缓存模式
* setCacheMode:方法：设置图像的缓存模式
* backgroundColor:方法：获取图像的背景色
* setBackgroundColor:方法：设置图像的背景色
* name:方法：获取图像的名称
* setName:方法：设置图像的名称
* setTemplate:方法：设置图像是否表示一个模板图像
* imageTypes:方法：获取图像类型
* imageUnfilteredTypes:方法：获取图像类型
* imageFileTypes:方法：获取文件类型
* imageUnfilteredFileTypes:方法：获取文件类型
* imagePasteboardTypes:方法：获取粘贴板类型
* imageUnfilteredPasteboardTypes:方法：获取粘贴板类型
* representations:方法：获取图像表示

## 判断图像信息

* isFlipped:方法：判断图像是否倒立
* prefersColorMatch:方法：判断图像颜色匹配
* isTemplate:方法：判断图像是否为模板图像

## 获取与设置图像视图

* image:方法：获取显示图像的信息
* setImage:方法：设置显示的图像
* imageAlignment:方法：获取图像的对齐方式
* setImageAlignment:方法：设置图像的对齐方式
* imageFrameStyle:方法：获取框架的风格
* setImageFrameStyle:方法：设置框架的风格
* imageScaling:方法：获取图像缩放的方式
* setImageScaling:方法：设置图像缩放方式
* isEditable:方法：判断图像视图是否可以编辑
* setEditable:方法：设置图像视图是否编辑
* allowsCutCopyPaste:方法：判断图像是否可复制、粘贴等操作
* setAllowsCutCopyPaste:方法：设置图像是否可复制、粘贴等操作
* animates:方法：判断图像视图是否播放动画
* setAnimates:方法：设置图像视图是否播放动画

# 表视图(NSTableView)

## 获取表视图信息

* rowHeight:方法：获取表视图的行高
* headerView:方法：获取NSTableHeaderView对象
* intercellSpacing:方法：获取表单元之间的间距
* numberOfColumns:方法：获取表视图中的列数
* numberOfRows:方法：获取表视图的行数
* numberOfSelectedColumns:方法：获取选中的列数
* numberOfSelectedRows:方法：获取选择的行数
* rowSizeStyle:方法：获取行风格

## 设置表视图的信息

* setRowHeight:方法：设置表视图的行高
* setIntercellSpacing:方法：设置表单元之间的间距
* setRowSizeStyle:方法：设置行风格
* setAllowsColumnReordering:方法：设置用户是否可以重新排列列标题
* setAllowsColumnResizing:方法：设置是否可以调整列标题
* setAllowsColumnSelection:方法：设置是否可以选择一整列
* setAllowsTypeSelect:方法：设置是否可以通过按键字符进行选择
* setAllowsMultipleSelection:方法：设置是否允许选择多行或多列

## 判断表视图信息

* allowsColumnReordering:方法：判断用户是否可以重新排列列标题
* allowsColumnResizing:方法：判断是否可以调整列标题
* allowsColumnSelection:方法：判断是否可以选择一整列
* allowsEmptySelection:方法：判断是否允许有0个行或列被选中
* allowsTypeSelect:方法：判断是否可以通过按键字符进行选择
* allowsMultipleSelection:方法：判断是否允许选择多行或多列

# 常见控件(NSButton、NSDatePicker、NSProgressIndicator、NSComboBox)

## 按钮控件(NSButton)

* title:方法：获取按钮的标题
* setTitle:方法：设置按钮的标题
* image:方法：获取按钮的图像
* setImage:方法：设置按钮的图像
* isTransparent:方法：判断按钮是否透明
* setTransparent:方法：设置按钮是否透明
* showsBorderOnlyWhileMouseInside:方法：判断边框的显示