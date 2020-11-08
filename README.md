# forth-day-
类模板的知识点
#include <iostream>
#include <string>

using namespace std;

//类模板
/*template<class T1,class T2, ...... ,class t(n)>
	class 对象名{
	T1 a1;
	T2 a2;
	.
	.
	.
	};
*/
template<class T>
class persion1{
public:
	T name;
	persion1(T name)
	{
		this->name=name;
	}

	void print()
	{
		cout<<name<<endl;
	}
};
void test1()
{
	persion1<int> a(1);//使用时要给出明确的T的数据类型
	a.print();
	persion1<string> b("fhjakfh");
	b.print();
}

/*类模板与函数模板的区别*/
//1.类模板没有自动类型推导的使用方式
//2.类模板在模板函数列表中可以有默认参数

template<class nametype,class agetype=int>
class persion{
	public:
		nametype name;
		agetype age;
		persion(nametype name,agetype age)
		{
			this->name=name;
			this->age=age;
		}
		void show()
		{
			cout<<name<<'\n'<<age<<endl;
		}
		void error()
		{
			return 0;
		}
};

void test2()
{
	persion<string,int> a("afhauh",12);//使用时要给出明确的T的数据类型,如persion a("afhauh",12)无法编译
	a.show();
	persion<string> b("jfdlafj",23);//类模板在模板函数列表中可以有默认参数
	b.show();

}
/*类模板中成员函数创建时机*/
//普通类中的成员函数一开始就可以创建
//类模板中的成员函数在调用时才创建

class persion2{
public:
	int a;
	persion2(int a)
	{
		this->a=a;
	}
	void show()
	{
		cout<<a<<endl;
		cout<<"person2 show"<<endl;
	}
/*	void error()
		{
			return 0;
		}*/
};


void test3()
{
	persion<string,int> a("afhauh",12);/*类模板persion中有void error(){	{return 0;	}};而不报错；而普通模板有这个函数会报错*/
	a.show();
	persion2 b(23);
	b.show();

}

/*类模板与继承*/
//当子模板继承的父类是一个类模板时，子类在声明的时候，要指定出父类中T的类型
//如果不指定，编译器无法给子类分配内存
//如果想灵活指定父类中T的类型，子类也需变为类模板

template<class T>
class base{
public:
	T a;
};

//class son:public base//是错误的
//正确的为(不灵活)
class son:public base<int>
{
	
};

//如果想灵活指定父类中T的类型，子类也需变为类模板

template<class T1,class T>
class son1:public base<T>
{
public:
	T1 b;
};

void test4()
{
	son s;
	s.a=5;
	cout<<s.a <<endl;
	son1<int,int> s1;
	s1.a=5;
	s1.b =6;
	cout<<s1.a<<'\n'<<s1.b<<endl;
}

/*类模板与友元*/
//全局函数类内实现--直接在类内声明友元即可
//全局函数类外实现--需要提前让编译器知道全局函数的存在

//提前让编译器知道persion类存在
template<class T1,class T2>
class persion3;

//类外实现
template<class T1,class T2>
void print1(persion3<T1,T2> p)
{
	cout<<"类外实现"<<p.name<<'\n'<<p.age<<endl;
}

template<class T1,class T2>

class persion3{
	//全局函数类内实现
	friend void print(persion3<T1,T2> p)
	{
		cout<<p.name<<'\n'<<p.age<<endl;
	}
	//全局函数类外实现声明
	//加空模板参数列表？？？？VS中需要
	//全局函数类外实现--需要提前让编译器知道全局函数的存在
	friend void print1(persion3<T1,T2> p);
public:
	persion3(T1 name,T2 age)
	{
		this->name=name;
		this->age=age;
	}
private:
	T1 name;
	T2 age;
};



void test5()
{
	//全局函数在类内实现测试
	persion3<string,int> p("sda",42);
	print(p);
	persion3<string,int> d("sgfsda",42);
	print1(d);
}
int main()
{

//	test1();
//	test2();
//	test3();
//	test4();
	test5();
	int i;
	cin>>i;
	return 0;
}
