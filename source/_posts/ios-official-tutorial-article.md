---
title: "iOS Develpe Tutorials"
layout: post
date: 2015-11-08
tags:
- iOS
categories:
- software
---

# iOS Developer Library

## [NSFileManager](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSFileManager_Class/index.html#//apple_ref/doc/uid/TP40003660)

NSFileManager 用来管理文件,拷贝,复制,修改属性等等.NSFileManager 中的方法是线程安全的

## 音频&视频

### [Camera Programming Topics](https://developer.apple.com/library/ios/samplecode/PhotoPicker/Introduction/Intro.html#//apple_ref/doc/uid/DTS40010196-Intro-DontLinkElementID_2)

#### 拍照和录像(Camera Programming Topics for iOS)

- `UIImagePickerController`仅支持竖屏, 3.1之后你可以使用`cameraOverlayView`来替代系统的一些交互界面

- 如果拍照是必须的,那么记得在`Info.plist`中配置`UIRequiredDeviceCapabilities`键
- 使用`UIImagePickerController`的静态方法`isSourceTypeAvailable`来检测相机是否可用
- 实现Delegate

- 当以上可用的时候,设置`sourceType`为`UIImagePickerControllerSourceTypeCamera`
- `mediaTypes`可以配置为`kUTTypeImage`和`kUTTypeMovie`,然后使用`availableMediaTypesForSourceType`, 使用`allowsEditing`属性来开启或者编辑

#### 从相册中选择Item

- 配置`sourceType`为`UIImagePickerControllerSourceTypePhotoLibrary` 或者 `UIImagePickerControllerSourceTypeSavedPhotosAlbum`

## Data Management(数据管理)

### [时间和日期(Core Foundation)](https://developer.apple.com/library/ios/documentation/CoreFoundation/Conceptual/CFDatesAndTimes/CFDatesAndTimes.html#//apple_ref/doc/uid/10000125i)

五种时间表示方式:

```objective-c
// 2001.01.01 开始的秒数
CFAbsoluteTime
// 消逝的时间,秒为单位,double
CFTimeInterval
CFGregorianDate
CFGregorianUnits
CFDate
```

## [二进制编程(Core Foundation)](https://developer.apple.com/library/ios/documentation/CoreFoundation/Conceptual/CFBinaryData/CFBinaryData.html#//apple_ref/doc/uid/10000144-SW1)

### Binary Data

#### Creating Data Objects With Raw Bytes

```objective-c
CFDataCreate
CFDataCreateMutable
CFDataCreateWithBytesNoCopy
```

#### 访问和比较Bytes

```objective-c
// 三个CFData的基本函数:
// CFDataGetBytesPtr: 返回一个指向bytes的指针
// CFDataGetBytes: 将bytes放到一个buffer中
// CFDataGetLength: 返回bytes的数量

const UInt8 *myString = "Test string.";
CFDataRef myData =
        CFDataCreateWithBytesNoCopy(NULL, myString, strlen(myString), kCFAllocatorNull);
const UInt8 *ptr = CFDataGetBytePtr(myData);

//创建data1的子区间:data2
unsigned char aBuffer[20];
const UInt8 *strPtr = "ABCDEFG";
CFDataRef data1 =
        CFDataCreateWithBytesNoCopy(NULL, strPtr, strlen(strPtr), kCFAllocatorNull);
CFDataGetBytes(data1, CFRangeMake(2, 4), aBuffer);
CFDataRef data2 = CFDataCreate(NULL, aBuffer, 20);

//使用`CFEqual`进行一个byte-to-byte比较

//拷贝
CFDataCreateCopy //不变拷贝
CFDataCreateMutableCopy //可变拷贝
```

### Mutable Binary Data

#### 改变Bytes

```objective-c
//CFMutableData的基本函数:
//CFDataGetMutableBytePtr, CFDataGetLength, CFDataSetLength,CFDataIncreaseLength

// Create some strings.
const UInt8 *myString = "string for data1";
const UInt8 *yourString = "string for data2";
  
// Declare buffers used later.
const UInt8 *data1Bytes, *data2Bytes;
   
// Create mutable data objects, data1 and data2.
CFMutableDataRef data1 = CFDataCreateMutable(NULL, 0);
CFMutableDataRef data2 = CFDataCreateMutable(NULL, 0);
CFDataAppendBytes(data1, myString, strlen(myString));
CFDataAppendBytes(data2, yourString, strlen(yourString));
    
// Get and print the data1 bytes.
data1Bytes = CFDataGetBytePtr(data1);
fprintf(stdout, "data1 before: \"%s\"\n", data1Bytes);
	 
// Get and print the data2 bytes.
data2Bytes = CFDataGetMutableBytePtr(data2);
fprintf(stdout, "data2 before: \"%s\"\n", data2Bytes);
	  
// Copy the bytes from data1 into the mutable bytes from data2.
CFDataGetBytes(data1,
				CFRangeMake(0, CFDataGetLength(data1)),
				data2Bytes);
									 
// Get and print the data2 bytes again.
fprintf(stdout, "data2 after: \"%s\"\n", CFDataGetBytePtr(data2));
```

#### 添加Bytes

```objective-c
CFMutableDataRef data1 = CFDataCreateMutable(NULL, 0);
CFDataAppendBytes(data1, "ABCD", strlen("ABCD"));

const UInt8 *aBuffer = CFDataGetBytePtr(data2);
CFDataAppendBytes(data1, aBuffer, CFDataGetLength(data2));
```

#### 替换Bytes

```objective-c
CFDataReplaceBytes
```

## [Preferences 编程](https://developer.apple.com/library/ios/documentation/CoreFoundation/Conceptual/CFPreferences/CFPreferences.html#//apple_ref/doc/uid/10000129i)

### Using the High-Level Preferences API

#### Saving a Simple Preference

```objective-c
CFStringRef textColorKey = CFSTR("defaultTextColor");
CFStringRef colorBLUE = CFSTR("BLUE");
 
// Set up the preference.
CFPreferencesSetAppValue(textColorKey, colorBLUE,
        kCFPreferencesCurrentApplication);
		  
// Write out the preference data.
CFPreferencesAppSynchronize(kCFPreferencesCurrentApplication);
```

#### Reading a Simple Preference.

```objective-c
CFStringRef textColorKey = CFSTR("defaultTextColor");
CFStringRef textColor;
 
// Read the preference.
textColor = (CFStringRef)CFPreferencesCopyAppValue(textColorKey,
       kCFPreferencesCurrentApplication);
// When finished with value, you must release it
// CFRelease(textColor);
```

#### Updating a Simple Preference

```objective-c
CFStringRef highScoreKey = CFSTR("High Score");
CFNumberRef tempScore;
int highScore;
 
 // Look for the preference.
 tempScore = (CFNumberRef)CFPreferencesCopyAppValue(highScoreKey,
         kCFPreferencesCurrentApplication);
		  
// If the preference exists, update it. If not, create it.
if (tempScore)
{
	// Numbers come out of preferences as CFNumber objects.
	if (!CFNumberGetValue(tempScore, kCFNumberIntType, &highScore)) {
			highScore = 0;
	}
	CFRelease(tempScore);

	printf("The old high score was %d.", highScore);
}
else
{
	// No previous value.
	printf("There is no old high score.");
	highScore = 0;
}

highScore += 5;

// Create the CFNumber to pass to the preference API.
tempScore = CFNumberCreate(NULL, kCFNumberIntType, &highScore);

// Set the preference value, or update it if it already exists.
CFPreferencesSetAppValue(highScoreKey, tempScore,
				kCFPreferencesCurrentApplication);

// Release the CFNumber.
CFRelease(tempScore);

// Write out the preferences.
CFPreferencesAppSynchronize(kCFPreferencesCurrentApplication);
```

## [Number and Value Programming](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/NumbersandValues/NumbersandValues.html#//apple_ref/doc/uid/10000038i)

对C语言原始数据类型的封装:`NSValue`以及子类`NSNumber`, `NSDecimalNumber`, `NSNull`

### Values

`NSValue` hode type: `int`, `float`, `char`, `pointers`, `structures`, `object ids`
`NSValue` 不可变

创建一个`NSValue`需要:

- 指向item的指针 
- 使用`@encode`编译指令来的到item的type

```objective-c
NSRange myRange = {4, 10};
NSValue *theValue = [NSValue valueWithBytes:&myRange objCType:@encode(NSRange)];

// C结构体
// assume ImaginaryNumber defined:
typedef struct {
    float real;
    float imaginary;
} ImaginaryNumber;
		 
		  
ImaginaryNumber miNumber;
miNumber.real = 1.1;
miNumber.imaginary = 1.41;
		   
NSValue *miValue = [NSValue valueWithBytes: &miNumber
                       withObjCType:@encode(ImaginaryNumber)];
									    
ImaginaryNumber miNumber2;
[miValue getValue:&miNumber2];

// 字符串
char *myCString = "This is a string.";
NSValue *theValue = [NSValue valueWithBytes:&myCString withObjCType:@encode(char **)];
```

### Numbers

`NSNumber` hode the type `char`, `short int`, `int`, `NSInteger`, `long int`, `long long int`, `float`, `double`, `BOOL`

```objective-c
NSInteger nine = 9;
float ten = 10.0;
 
NSNumber *nineFromInteger = [NSNumber alloc] initWithInteger:nine];
NSNumber *tenFromFloat = [NSNumber numberWithFloat:ten];

// 使用literals @
NSNumber *nineFromInteger = @9;
NSNumber *tenFromFloat = @10.0;
NSNumber *nineteenFromExpression = @(nine + ten);

// NSNumber 定义了一个 compare 方法:
NSComparisonResult comparison = [nineFromInteger compare:tenFromFloat];

// Float_MAX
NSNumber *bigNumber = @(FLT_MAX);
```

### 十进制

`NSDecimalNumber` 是 `NSNumber` 的一个不可变类

### NSNull

```objective-c
NSNull *nullValue = [NSNull null];
NSArray *arrayWithNull = @[nullValue];
NSLog(@"arrayWithNull: %@", arrayWithNull);
// Output: "arrayWithNull: (<null>)"

// 虽然 NSNull 语义上 == nil,但是它实际上却不等于nil
id aValue = [arrayWithNull objectAtIndex:0];
if (aValue == nil) {
	NSLog(@"equals nil");
}
else if (aValue == [NSNull null]) {
	NSLog(@"equals NSNull instance");
	if ([aValue isEqual:nil]) {
		NSLog(@"isEqual:nil");
	}
}
// Output: "equals NSNull instance"
```

## [Core Data Snippets](https://developer.apple.com/library/ios/documentation/DataManagement/Conceptual/CoreDataSnippets/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008285)

### 访问Core Data栈

```objective-c
// 得到一个Managed Object Context
NSPersistentStoreCoordinator *psc = <#Get the coordinator#>;
NSManagedObjectContext *newContext = [[NSManagedObjectContext alloc] init];
[newContext setPersistentStoreCoordinator:psc];

// 得到Model 和 Entities
NSManagedObjectModel *model = [psc managedObjectModel];
NSEntityDescription *entity = [[model entitiesByName] objectForKey:@"<#Entity name#>"];

// 添加一个Persistent Store
NSPersistentStoreCoordinator *psc = <#Get the coordinator#>;
NSURL *storeUrl = [NSURL fileURLWithPath:@"<#Path to store#>"];
NSString *storeType = <#Store type#>; // A store type, such as NSSQLiteStoreType
NSError *error;
if (![psc addPersistentStoreWithType:storeType configuration:nil
    URL:storeUrl options:nil error:&error]) {
	// Handle the error.
}
```

### 获得所管理的对象

```objective-c
NSManagedObjectContext *context = <#Get the context#>;

NSFetchRequest *fetchRequest = [[NSFetchRequest alloc] init];
NSEntityDescription *entity = [NSEntityDescription entityForName:@"<#Entity name#>" inManagedObjectContext:context];
[fetchRequest setEntity:entity];

NSError *error;
NSArray *fetchedObjects = [context executeFetchRequest:fetchRequest error:&error];
if (fetchedObjects == nil) {
	// Handle the error.
}

// 根据顺序Fetch
NSSortDescriptor *sortDescriptor = [[NSSortDescriptor alloc] initWithKey:@"<#Sort key#>"
    ascending:YES];
NSArray *sortDescriptors = @[sortDescriptor];
[fetchRequest setSortDescriptors:sortDescriptors];

// 根据断言
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"<#Predicate string#>",
    <#Predicate arguments#>];
[fetchRequest setPredicate:predicate];
```

### Fetch 特定属性的值

```objective-c
// Fetch 唯一的值
NSFetchRequest *request = [[NSFetchRequest alloc] init];
[request setReturnsDistinctResults:YES];

// Fetch 满足某个函数(最大,最小...)的Attribute值
// Create an expression for the key path.
NSExpression *keyPathExpression = [NSExpression expressionForKeyPath:@"<#Key-path for the property#>"];
 
 // Create an expression to represent the function you want to apply
 NSExpression *expression = [NSExpression expressionForFunction:@"<#Function name#>"
     arguments:@[keyPathExpression]];
// Create an expression description using the minExpression and returning a date.
NSExpressionDescription *expressionDescription = [[NSExpressionDescription alloc] init];
// The name is the key that will be used in the dictionary for the return value.
[expressionDescription setName:@"<#Dictionary key#>"];
[expressionDescription setExpression:expression];
[expressionDescription setExpressionResultType:<#Result type#>]; // For example, NSDateAttributeType
// Set the request 的 properties to fetch just the property represented by the expressions.
[request setPropertiesToFetch:@[expressionDescription]];
```

### 创建与删除Managed Objects

```objective-c
// 创建一个Managed Object
NSManagedObjectContext *context = <#Get the context#>;
<#Managed Object Class#> *newObject = [NSEntityDescription
    insertNewObjectForEntityForName:@"<#Entity name#>"
	    inManagedObjectContext:context];

// 保存
NSManagedObjectContext *context = <#Get the context#>;
NSError *error;
if (![context save:&error]) {
    // Handle the error.
}

// 删除
NSManagedObject *aManagedObject = <#Get the managed object#>;
NSManagedObjectContext *context = [aManagedObject managedObjectContext];
[context deleteObject:aManagedObject];
NSError *error;
if (![context save:&error]) {
    // Handle the error.
}
```

## [Timer 编程指南](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Timers/Timers.html#//apple_ref/doc/uid/10000061i)

### Timer

`Timers`和`NSRunLoop`一块儿工作, A timer is not a real-time mechanism, 大概有50-100毫秒误差

除了`Timers`,你还可以选择`performSelector:withObject:afterDelay:`可以在一段时间后,直接在另外一个对象上调用方法. `performSelectorOnMainThread:withObject:waitUntilDone:` 在一个线程上调用方法, 使用`cancelPreviousPerformRequestsWithTarget:`来取消,

```objective-c

```


## [Property List 编程指南](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/PropertyLists/Introduction/Introduction.html#//apple_ref/doc/uid/10000048i)

Property List 将数据组织成named values 以及 lists of values

## [Collections 编程指南](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Collections/Collections.html#//apple_ref/doc/uid/10000034i)

这个就是`Collections`:

![Collections](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Collections/Art/collections_intro_2x.png)

### Arrays: 有序集合
```objective-c
// 可变数组基本方法
NSMutableArray *array = [NSMutableArray array];
[array addObject:[NSColor blackColor]];
[array insertObject:[NSColor redColor] atIndex:0];
[array insertObject:[NSColor blueColor] atIndex:1];
[array addObject:[NSColor whiteColor]];
[array removeObjectsInRange:(NSMakeRange(1, 2))];

// 检查object是否在一个array中
// indexOfObject: 使用 `isEqual`, indexOfObjectIdenticalTo: 使用 指针
NSUInteger index;
index = [yesArray indexOfObject:yes2];
index = [yesArray indexOfObjectIdenticalTo:yes2];

// 使用`NSSortDescriptor`排序
NSSortDescriptor *lastDescriptor =
[[NSSortDescriptor alloc] initWithKey:last
	ascending:YES
	selector:@selector(localizedCaseInsensitiveCompare:)];
NSSortDescriptor *firstDescriptor =
[[NSSortDescriptor alloc] initWithKey:first
	ascending:YES
	selector:@selector(localizedCaseInsensitiveCompare:)];

NSArray *descriptors = [NSArray arrayWithObjects:lastDescriptor, firstDescriptor, nil];
sortedArray = [array sortedArrayUsingDescriptors:descriptors];

// 使用function来sort,不够灵活
NSInteger lastNameFirstNameSort(id person1, id person2, void *reverse)
{
    NSString *name1 = [person1 valueForKey:last];
	NSString *name2 = [person2 valueForKey:last];
		 
	NSComparisonResult comparison = [name1 localizedCaseInsensitiveCompare:name2];
	if (comparison == NSOrderedSame) {
			name1 = [person1 valueForKey:first];
			name2 = [person2 valueForKey:first];
			comparison = [name1 localizedCaseInsensitiveCompare:name2];
	}

	if (*(BOOL *)reverse == YES) {
			return 0 - comparison;
	}
	return comparison;
}

BOOL reverseSort = YES;
sortedArray = [array sortedArrayUsingFunction:lastNameFirstNameSort
context:&reverseSort];

// 使用`Blocks`来`sort`
NSArray *sortedArray = [array sortedArrayUsingComparator: ^(id obj1, id obj2) {
 
      if ([obj1 integerValue] > [obj2 integerValue]) {
			  return (NSComparisonResult)NSOrderedDescending;
	  }

	  if ([obj1 integerValue] < [obj2 integerValue]) {
			  return (NSComparisonResult)NSOrderedAscending;
	  }
	  return (NSComparisonResult)NSOrderedSame;
}];

// 过滤数组-使用谓词
MutableArray *array =
    [NSMutableArray arrayWithObjects:@"Bill", @"Ben", @"Chris", @"Melissa", nil];
	 
NSPredicate *bPredicate =
	[NSPredicate predicateWithFormat:@"SELF beginswith[c] 'b'"];
NSArray *beginWithB =
	[array filteredArrayUsingPredicate:bPredicate];
// beginWithB contains { @"Bill", @"Ben" }.

NSPredicate *sPredicate =
[NSPredicate predicateWithFormat:@"SELF contains[c] 's'"];
[array filterUsingPredicate:sPredicate];
// array now contains { @"Chris", @"Melissa" }

// Pointer Array -- 
NSPointerArray *ptrArray=[NSPointerArray pointerArrayWithOptions: options];
```

NSPointerArray:

![NSPointerArray](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Collections/Art/PointerArray_2x.png)

### Dictionaries: 键值对集合

键可以是任何实现了`NSCoping`,`hash`,`isEqual`方法的对象

```objective-c
NSMutableDictionary *dict = [NSMutableDictionary dictionaryWithObjectsAndKeys:@"Jo", first, @"Smith", last, nil];

// 排序:keysSortedByValueUsingSelector:
// 排序:keysSortedByValueUsingComparator:

// NSMapTable -- 大致意思是:当你想要更高级的内存管理选项或者存储特殊的指针的时候(如Weak指针),可以考虑使用这个
```

NSMapTable:

![NSMapTable](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Collections/Art/MapTable_2x.png)

### Sets: 无序对象集合

```objective-c
// NSSet: 不可变
// 交集,并集,减集,异或...

// NSMutableSet: mutable

// NSCountedSet: 是NSMutableSet的子类,可以添加多个相同的对象,实际上应该称为`bag`

// NSHashTable: 可以存储Weak References的东东
```

NSHashTable:

![NSHashTable](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Collections/Art/HashTable_2x.png)

### Index Sets: 将Indexes排序成一个Array

![Index_Sets](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Collections/Art/IndexSet_2x.png)

```objective-c
NSIndexSet
NSMutableIndexSet

//迭代
NSUInteger index=[anIndexSet firstIndex];
while(index != NSNotFound)
{

	NSLog(@" %@",[anArray objectAtIndex:index]);
	index=[anIndexSet indexGreaterThanIndex: index];
}

//Index Sets和Blocks
NSIndexSet *lessThan20=[someArray indexesOfObjectsPassingTest:^(id obj, NSUInteger index, BOOL *stop){
	if ([obj isLessThan:[NSNumber numberWithInt:20]]){
		return YES;
	}
	return NO;
}];
```

### Index Paths: 通过Nested Array来排序一个Path

Index Paths: Storing a Path Through Nested Arrays

一个假想的公司:

![Nested_array](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Collections/Art/NestedArrays_2x.png)

```objective-c
// 像上面那样: Bill T 可以表示为:0.0.1.1.2

NSUInteger arrayLength = 5;
NSUInteger integerArray[] = {0,0,1,1,2};
NSIndexPath *aPath = [[NSIndexPath alloc] initWithIndexes:integerArray length:arrayLength];
```

### 拷贝Collections

Shallow copies and deep copies

![Shallow_copy_deep_copy](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Collections/Art/CopyingCollections_2x.png)

```objective-c
// 浅复制
NSArray *shallowCopyArray = [someArray copyWithZone:nil];
NSDictionary *shallowCopyDict = [[NSDictionary alloc] initWithDictionary:someDictionary copyItems:NO];

// 深复制
// one-level-deep copy, 每个对象将会收到`copyWithZone`的消息,你的对象必须实现`NSCopying`protocol
NSArray *deepCopyArray=[[NSArray alloc] initWithArray:someArray copyItems:YES];
// a true deep copy,比如有一个array of arrays
NSArray* trueDeepCopyArray = [NSKeyedUnarchiver unarchiveObjectWithData:
          [NSKeyedArchiver archivedDataWithRootObject:oldArray]];

// Copying and Mutability -- 
// `copyWithZone`: 维系表面层次不可变性
// `initWithArray:copyItems:` with NO:
// `initWithArray:copyItems`: with YES:
// Archiving and unarchiving the collection leaves the mutability of all levels as it was before.
```

### 迭代

```objective-c
// 快速迭代:
for (NSString *element in someArray) {
     NSLog(@"element: %@", element);
}
NSString *key;
for (key in someDictionary){
	NSLog(@"Key: %@, Value %@", key, [someDictionary objectForKey: key]);
}

// Block-Based 迭代:
NSArray *anArray = [NSArray arrayWithObjects:@"A", @"B", @"D", @"M", nil];
NSString *string = @"c";
[anArray enumerateObjectsUsingBlock:^(id obj, NSUInteger index, BOOL *stop){
	if ([obj localizedCaseInsensitiveCompare:string] == NSOrderedSame) {
		 NSLog(@"Object Found: %@ at index: %i",obj, index);
		 *stop = YES;
	 }
 }];

// 使用 Enumerator
NSMutableDictionary *myMutableDictionary = <#Get a mutable dictionary#> ;
NSMutableArray *keysToDeleteArray =
[NSMutableArray arrayWithCapacity:[myMutableDictionary count]];
NSString *aKey;
NSEnumerator *keyEnumerator = [myMutableDictionary keyEnumerator];
while (aKey = [keyEnumerator nextObject])
{
	if ( /* test criteria for key or value */ ) {
		[keysToDeleteArray addObject:aKey];
	}
}
[myMutableDictionary removeObjectsForKeys:keysToDeleteArray];
```

### 指针函数选项

```objective-c
NSPointerFunctionsOptions collectionOptions = NSPointerFunctionsObjectPersonality | NSPointerFunctionsZeroingWeakMemory;
NSPointerFunctionsOptions collectionOptions = NSPointerFunctionsOpaquePersonality | NSPointerFunctionsOpaqueMemory;
```
