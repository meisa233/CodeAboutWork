### BitMap介绍
这里的BitMap指的是把数据存放在一个以bit为单位的数据结构里。 <br />
每位都只有0和1两个值。为0的时候，证明值不存在，为1的时候说明存在。<br />
>
举例来说：
>
```
[0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0]
```

这是24位，也就是24bit, 同时8bit为1个字节。这里的空间也就是3个字节。<br />

这个时候假如我们要存放2 4 6 8 9 10 17 19 21这些数字到我们的BitMap里，我们用把对应的位设置为1就可以了。<br />

[0 0 0 1 0 1 0 1 0 0 0 0 0 0 1 1 1 0 1 0 1 0 1 0]

这样我们只用3个字节就存放了 9 * sizeof(int)大小的数字。在64位编译器里一般一个int类型是32bit也就是4个字节。我们存放这么多数字，连一个int的空间都不到。<br />

### BitMap实现

其实BitMap实现不是很难，只是平时可能用位运算不是特别多。所以不熟悉。<br />

主要的操作有三个，除了初始化之外，就是set() 和 get() 还有 del()方法，这三个，一个是把index置为1，一个是得到index位的0 or 1，最后的是把index位置位0.<br />
>
实现1：<br />
```
//
//  Header.h
//  BloomFilter
//
//  Created by Alps on 15/3/19.
//  Copyright (c) 2015年 chen. All rights reserved.
//
//这个是 BitMap.h文件。

class BitMap{
public:
    BitMap(){
        bitmap = NULL;
        size = 0;
    }
    BitMap(int size){ // contractor, init the bitmap
        bitmap = NULL;
        bitmap = new char[size];
        if (bitmap == NULL) {
            printf("ErroR In BitMap Constractor!\n");
        }else{
            memset(bitmap, 0x0, size * sizeof(char));
            this->size = size;
        }
    }


    /*
     * set the index bit to 1;
     */
    int bitmapSet(int index){
        int addr = index/8;
        int addroffset = index%8;
        unsigned char temp = 0x1 << addroffset;
        if (addr > (size+1)) {
            return 0;
        }else{
            bitmap[addr] |= temp;
            return 1;
        }
    }

    /*
     * return if the index in bitmap is 1;
     */
    int bitmapGet(int index){
        int addr = index/8;
        int addroffset = index%8;
        unsigned char temp = 0x1 << addroffset;
        if (addr > (size + 1)) {
            return 0;
        }else{
            return (bitmap[addr] & temp) > 0 ? 1 : 0;
        }
    }

    /*
     * del the index from 1 to 0
     */
    int bitmapDel(int index){
        if (bitmapGet(index) == 0) {
            return 0;
        }
        int addr = index/8;
        int addroffset = index%8;
        unsigned char temp = 0x1 << addroffset;
        if (addr > (size + 1)) {
            return 0;
        }else{
            bitmap[addr] ^= temp;
            return 1;
        }
    }

private:
    char *bitmap;
    int size;
};
```
>
>
>
实现2：<br />
```C++
#include <iostream>
#include <vector>

using namespace std;

class BitMap
{
public:
	//它是一种“整型”类型，里面保存的是一个整数，就像int, long那样。这种整数用来记录一个大小(size)。size_t的全称应该是size type，就是说“一种用来记录大小的数据类型”。
	/*
	它是一种“整型”类型，里面保存的是一个整数，就像int, long那样。这种整数用来记录一个大小(size)。size_t的全称应该是size type，就是说“一种用来记录大小的数据类型”。

	通常我们用sizeof(XXX)操作，这个操作所得到的结果就是size_t类型。

	因为size_t类型的数据其实是保存了一个整数，所以它也可以做加减乘除，也可以转化为int并赋值给int类型的变量。

	类似的还有wchar_t, ptrdiff_t。

	wchar_t就是wide char type，“一种用来记录一个宽字符的数据类型”。

	ptrdiff_t就是pointer difference type，“一种用来记录两个指针之间的距离的数据类型”。

	通常，size_t和ptrdiff_t都是用typedef来实现的。你可能在某个头文件里面找到类似的语句：

	typedef unsigned int size_t;

	而wchar_t则稍有不同。在一些旧的编译器中，wchar_t也可能是用typedef来实现，但是新的标准中wchar_t已经是C/C++语言的关键字，wchar_t类型的地位已经和char, int的地位等同了。

	在标准C/C++的语法中，只有int float char bool等基本的数据类型，至于size_t,或size_type都是以后的编程人员为了方便记忆所定义的一些便于理解的由基本数据类型的变体类型。


	示例程序：！！！！！！！！！！

	int i;//定义一个int类型的变量i

	size_t size=sizeof(i);//用sizeof操作得到变量i的类型的大小

	//这是一个size_t类型的值

	//可以用来对一个size_t类型的变量做初始化

	i=(int)size;//size_t类型的值可以转化为int类型的值

	char c='a';//c保存了字符a，占一个字节

	wchar_t wc=L'a';//wc保存了宽字符a，占两个字节

	//注意'a'表示字符a，L'a'表示宽字符a

	int arr[]={1,2,3,4,5};//定义一个数组

	int *p1=&arr[0];//取得数组中元素的地址，赋值给指针

	int *p2=&arr[3];

	ptrdiff_t diff=p2-p1;//指针的减法可以计算两个指针之间相隔的元素个数

	//所得结果是一个ptrdiff_t类型

	i=(int)diff;//ptrdiff_t类型的值可以转化为int类型的值
	---------------------
	作者：Elen005
	来源：CSDN
	原文：https://blog.csdn.net/elen005/article/details/79516136
	版权声明：本文为博主原创文章，转载请附上博文链接！
	*/
	BitMap(size_t num)
	{
		_v.resize((num >> 5) + 1);
	}

	void Set(size_t num) //set 1
	{

		size_t index = num >> 5;
		size_t pos = num % 32;
		//_v[index] |= (1 << pos);
		_v[index] = (1 << pos);
	}

	void ReSet(size_t num) //set 0
	{
		size_t index = num >> 5;
		size_t pos = num % 32;
		_v[index] &= ~(1 << pos);
	}

	bool HasExisted(size_t num)//check whether it exists
	{
		size_t index = num >> 5;
		size_t pos = num % 32;
		bool flag = false;
		if (_v[index] & (1 << pos))
			flag = true;
		return flag;

	}

private:
	vector<size_t> _v;
};


void TestBitMap()
{
	BitMap bm((size_t)-1);//store all unsigned int  获取size_t的最大的可能值：https://stackoverflow.com/questions/19234248/what-is-this-code-doing-size-t-1
	bm.Set(1);
	bm.Set(111);
	bm.Set(222);
	bm.Set(3333);
	bm.Set(7777);
	bm.Set(9999);
	bm.Set(666666);
	bm.ReSet(1);
	bm.ReSet(666666);

	cout << bm.HasExisted(1) << endl;
	cout << bm.HasExisted(111) << endl;
	cout << bm.HasExisted(222) << endl;
	cout << bm.HasExisted(3333) << endl;
	cout << bm.HasExisted(7777) << endl;
	cout << bm.HasExisted(9999) << endl;
	cout << bm.HasExisted(666666) << endl;
}

int main()
{
	TestBitMap();
	return 0;
}
```
