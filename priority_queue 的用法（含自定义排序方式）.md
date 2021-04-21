C++ | 

priority_queue 本质是一个堆。



1. 头文件是 #include<queue>



2. 关于 priority_queue 中元素的比较



　　模板申明带 3 个参数：priority_queue<Type, Container, Functional>，其中 Type 为数据类型，Container 为保存数据的容器，Functional 为元素比较方式。



　　Container 必须是用数组实现的容器，比如 vector,deque 等等，但不能用 list。STL 里面默认用的是 vector。



2.1 比较方式默认用 operator<，所以如果把后面 2 个参数缺省的话，优先队列就是大顶堆（降序），队头元素最大。**特别注意 pair 的比较函数**。



　　以下代码返回一个**降序**输出：



[![img](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

```
1 #include <iostream>
 2 #include <queue>
 3 using namespace std;
 4 int main(){
 5     priority_queue<int> q;
 6     for( int i= 0; i< 10; ++i ) q.push(i);
 7     while( !q.empty() ){
 8         cout<<q.top()<<endl;
 9         q.pop();
10     }
11     return 0;
12 }
```

 

[![img](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)



以下代代码返回 pair 的比较结果，先按照 **pair** 的 first 元素降序，first 元素相等时，再按照 second 元素降序：



[![img](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

```
1 #include<iostream>
 2 #include<vector>
 3 #include<queue>
 4 using namespace std;
 5 int main(){
 6     priority_queue<pair<int,int> > coll;
 7     pair<int,int> a(3,4);
 8     pair<int,int> b(3,5);
 9     pair<int,int> c(4,3);
10     coll.push(c);
11     coll.push(b);
12     coll.push(a);
13     while(!coll.empty())
14     {
15         cout<<coll.top().first<<"\t"<<coll.top().second<<endl;
16         coll.pop();
17     }
18     return 0;
19 }
```

 

[![img](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)



2.2 如果要用到小顶堆，则一般要把模板的 3 个参数都带进去。STL 里面定义了一个仿函数 greater<>，**基本类型可以用这个仿函数声明小顶堆**。



　　以下代码返回一个**升序**输出：



[![img](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

```
1 #include <iostream>
 2 #include <queue> 
 3 using namespace std;
 4 int main(){
 5     priority_queue<int, vector<int>, greater<int> > q;
 6     for( int i= 0; i< 10; ++i ) q.push(10-i);
 7     while( !q.empty() ){
 8         cout << q.top() << endl;
 9         q.pop();
10     }
11     return 0;
12 }
```

 

[![img](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)



以下代代码返回 pair 的比较结果，先按照 **pair** 的 first 元素升序，first 元素相等时，再按照 second 元素升序： 



[![img](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

```
1 #include<iostream>
 2 #include<vector>
 3 #include<queue>
 4 using namespace std;
 5 int main(){
 6     priority_queue<pair<int,int>,vector<pair<int,int> >,greater<pair<int,int> > > coll;
 7     pair<int,int> a(3,4);
 8     pair<int,int> b(3,5);
 9     pair<int,int> c(4,3);
10     coll.push(c);
11     coll.push(b);
12     coll.push(a);
13     while(!coll.empty())
14     {
15         cout<<coll.top().first<<"\t"<<coll.top().second<<endl;
16         coll.pop();
17     }
18     return 0;
19 }
```

 

[![img](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)



2.3 对于自定义类型，则必须重载 operator < 或者重写仿函数。



2.3.1 重载 operator < 的例子：返回 true 时，说明左边形参的优先级低于右边形参



[![img](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

```
1 #include <iostream>
 2 #include <queue> 
 3 using namespace std;
 4 struct Node{
 5     int x, y;
 6     Node(int a=0, int b=0):
 7         x(a),y(b){}
 8 };
 9 bool operator<(Node a, Node b){//返回true时，说明a的优先级低于b
10     //x值较大的Node优先级低（x小的Node排在队前）
11     //x相等时，y大的优先级低（y小的Node排在队前）
12     if( a.x== b.x ) return a.y> b.y;
13     return a.x> b.x; 
14 }
15 int main(){
16     priority_queue<Node> q;
17     for( int i= 0; i< 10; ++i )
18     q.push( Node( rand(), rand() ) );
19     while( !q.empty() ){
20         cout << q.top().x << ' ' << q.top().y << endl;
21         q.pop();
22     }
23     return 0;
24 }
```

 

[![img](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)



**自定义类型重载 operator < 后，声明对象时就可以只带一个模板参数**。



但此时不能像基本类型这样声明 priority_queue<Node,vector<Node>,greater<Node> >，原因是 greater<Node > 没有定义，如果想用这种方法定义则可以重载 operator >。



例子：返回的是小顶堆。但不怎么用，习惯是重载 operator<。



[![img](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

```
1 #include <iostream>
 2 #include <queue>
 3 using namespace std;
 4 struct Node{
 5     int x, y;
 6     Node( int a= 0, int b= 0 ):
 7         x(a), y(b) {}
 8 };
 9 bool operator>( Node a, Node b ){//返回true，a的优先级大于b
10     //x大的排在队前部；x相同时，y大的排在队前部
11     if( a.x== b.x ) return a.y> b.y;
12     return a.x> b.x; 
13 }
14 int main(){
15     priority_queue<Node,vector<Node>,greater<Node> > q;
16     for( int i= 0; i< 10; ++i )
17     q.push( Node( rand(), rand() ) );
18     while( !q.empty() ){
19         cout << q.top().x << ' ' << q.top().y << endl;
20         q.pop();
21     }
22     return 0;
23 }
```

 

[![img](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)



2.3.2 重写仿函数的例子（返回值排序与 2.3.1 相同，都是小顶堆。先按 x 升序，x 相等时，再按 y 升序）：



[![img](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

```
1 #include <iostream>
 2 #include <queue>
 3 using namespace std;
 4 struct Node{
 5     int x, y;
 6     Node( int a= 0, int b= 0 ):
 7         x(a), y(b) {}
 8 };
 9 struct cmp{
10     bool operator() ( Node a, Node b ){//默认是less函数
11         //返回true时，a的优先级低于b的优先级（a排在b的后面）
12         if( a.x== b.x ) return a.y> b.y;      
13         return a.x> b.x; }
14 };
15 int main(){
16     priority_queue<Node, vector<Node>, cmp> q;
17     for( int i= 0; i< 10; ++i )
18     q.push( Node( rand(), rand() ) );
19     while( !q.empty() ){
20         cout << q.top().x << ' ' << q.top().y << endl;
21         q.pop();
22     }
23     return 0;
24 }
```

[![img](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

全文完

本文由 [简悦 SimpRead](http://ksria.com/simpread) 优化，用以提升阅读体验

使用了 全新的简悦词法分析引擎 beta，[点击查看](http://ksria.com/simpread/docs/#/词法分析引擎)详细说明