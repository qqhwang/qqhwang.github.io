---
layout: post
title: 哈夫曼树C++实现编码与解码
tag: 2021
---

## 文本编码问题

利用最优二叉树(也称哈夫曼树)可以对文本进行编码. 例如一个文本内只出现了A,B,C,D,E,F,G,H八种符号, 并且各自出现的频数如下表:

| 字符 | A | B | C | D | E | F | G | H |
|----|----|----|----|----|----|----|----|----|
| 频数 | 5 | 29 | 23 | 3 | 11 | 14 | 7 | 8 |

每个字符的ASCII码占8位, 一个字节, 容易算出该文本占用字节数: 5+29+23+3+11+14+7+8 = 100.
可以按以下方法构造最优二叉树, 实现这些字符的重新编码.

![hafuman1.png](https://i.loli.net/2021/04/28/I3lnukZ9ir6vBtb.png)

![hafuman2.png](https://i.loli.net/2021/04/28/kuZ2ay1AVCqbmMi.png)

由上图, 可以得到字符新的编码:

|字符|A|B|C|D|E|F|G|H|
|----|----|----|----|----|----|----|----|----|
|频数|5|29|23|3|11|14|7|8|
|编码|0110|10|00|0111|010|110|1110|1111|

该文本占用的字节数为: (5*4+29*2+23*2+3*4+11*3+14*3+7*4+8*4)/8 = 33.875, 共34字节. 
如果文本文件中的字符序列为: ABCDFF……
则该文本文件的编码文件的二进制序列为: 011010000111110110……
反过来, 由此二进制序列, 结合上面的哈夫曼树, 可以解码得到对应的字符序列.
你的任务是选择合适的数据结构, 实现上面的算法过程, 输出字符的新编码.
要求: 写出问题分析、解决方案、参考程序和运行结果。测试数据请自己构造。

## 问题分析

哈夫曼树又称最优二叉树，是一种带权路径长度最短的二叉树。所谓树的带权路径长度，就是树中所有的叶结点的权值乘上其到根结点的路径长度（若根结点为0层，叶结点到根结点的路径长度为叶结点的层数）。树的路径长度是从树根到每一结点的路径长度之和，记为WPL=（W1L1+W2L2+W3L3+…+WnLn），N个权值Wi（i=1,2,…n）构成一棵有N个叶结点的二叉树，相应的叶结点的路径长度为Li（i=1,2,…n）

## 解决方案

### 1.数据结构的定义

哈夫曼树结构体包含如下内容：节点权重，当前节点的父节点，左右子树，节点信息。哈夫曼编码结构体包含如下内容：存编码的数组，编码数组的开始标志。

### 2.功能详细设计

构建哈夫曼树：使用一维数组，每个节点存储权重，父节点，左子树，右子树，以及数值的信息。遍历整个数组，根据每个节点的权重去找到最小以及第二小的节点，然后对各自的父节点，左右子树赋值，建立两个节点的联系将他们的联系填入该结构体数组中。

## 参考程序

```c++
#include<iostream>
#include<string>
#include<fstream> 
using namespace std;
char rd;
int total_char=0;
struct Htnode{
    int weight,parent,lchild,rchild;
    char c; 
};
struct Htcode{
    int bit[16],start;
};
int type(int a[]){
    string s;
    int sum = 0; 
    //cout<<"input:"<<endl;
    freopen("in.txt", "r", stdin);
    while(scanf("%c",&rd) == 1)
    {
        ++a[rd-'a'];
        ++total_char;
    }
    cout<<"input check:"<<endl; 
    for(int i = 0; i < 26; i++){
        if(a[i] != 0){
            char c = char(i+'a');
            cout<<"["<<i<<"]"<<c<<":"<<a[i]<<endl;
            sum++;  
        }
    }
    return sum;
} 
// 选取权值最小的两个结点
void selectMin(Htnode a[],int n, int &s1, int &s2)
{
     for (int i = 0; i < n; i++)
     {
         if (a[i].parent == -1 )// 初始化s1,s1的双亲为-1
         {
             s1 = i;
             break;
         }
     }
     for (int i = 0; i < n; i++)// s1为权值最小的下标
     {
         if (a[i].parent == -1 && a[s1].weight > a[i].weight )
             s1 = i;
     }
     for (int j = 0; j < n; j++)
     {
         if (a[j].parent == -1 && j != s1 )// 初始化s2,s2的双亲为-1
         {
             s2 = j;
             break;
         }
     }
     for (int j = 0; j < n; j++)// s2为另一个权值最小的结点
     {
         if (a[j].parent == -1 && a[s2].weight > a[j].weight && j != s1 )
             s2 = j;
     }
 }
//通过数组的方式构建哈夫曼树 
char b[26];
void HufmanTree(Htnode h[],int n, int a[]){
    int i,j,max1,max2,x1,x2;
    for(i=0;i<2*n;i++){
        h[i].weight=0;
        h[i].parent=-1;
        h[i].lchild=-1;
        h[i].rchild=-1;
        h[i].c='\0';
    }
    int mark=0;
    for(i=0;i<26;i++){
        if(a[i] != 0){
            h[mark].c = i + 'a';
            h[mark].weight = a[i]; 
            b[i] = mark;
            cout<<"[weight] for h["<<mark<<"] is "<<a[i]<<endl;
            mark=mark+1;
        }
    }
    for(i=0;i<n-1;i++)
    {
        selectMin(h, n+i, x1, x2); // 查找权值最小的2个根节点，下标为x1,x2
        // 将x1，x2合并，且x1和x2的双亲为n+i
        //根据每个节点信息, 将其信息存储到节点数组中
        h[x1].parent=n+i; h[x2].parent=n+i; 
        h[n+i].weight=h[x1].weight+h[x2].weight;
        h[n+i].lchild=x1;h[n+i].rchild=x2;
    }
    cout<<"[debug_result]"<<endl;
    for (int i=0;i<2*n-1;i++)
    {
        cout<<"["<<i<<"]"<<"W="<<h[i].weight<<" P="<<h[i].parent<<" L="<<h[i].lchild<<" R="<<h[i].rchild<<" C="<<h[i].c<<endl; 
    }
}
//哈夫曼编码
//根据叶子节点的位置, 将其path路径01数字填充到编码数组中
void  HuffmandeCode(Htnode h[], int n, int a[], Htcode hcode[]){
    HufmanTree(h, n, a);
    ofstream out;
    out.open("HuffmandeCode.txt", ios::out);
    int i,j;
    for(i=0;i<n;i++){
        j=0;
        int parent=h[i].parent;//记录当前节点的父亲 
        int c=i;
        while(c!=-1){//parent造成根节点不会被访问 
            hcode[i].bit[j++]=h[parent].lchild==c?0:1;//从叶子节点到根节点, 应该使用栈结构 
            c=parent;
            parent=h[parent].parent;
        }
        hcode[i].start=j-1;
    }
    for(i=0;i<n;i++){
        cout<<h[i].c<<":";
        for(j=hcode[i].start-1;j>=0;j--)
        {
            cout<<hcode[i].bit[j];//print codec on screen
        }
        cout<<endl;
    }
    freopen("in.txt", "r", stdin);
    while(scanf("%c",&rd) == 1)
    {
        for(j=hcode[b[rd-'a']].start-1;j>=0;j--)
        {
            out<<hcode[b[rd-'a']].bit[j];//output codec to file
        }
    }
    //out<<hcode[i].bit[j];
    out.close(); 
}
string load(){
    ifstream in("HuffmandeCode.txt");
    string str;
    char buffer[1024];
    if(!in.is_open()){
        cout<<"Failed to load"<<endl; 
        return NULL;
    } 
    cout << "Loading decodec file..." << endl;
    in.getline(buffer, 1024, ' ');
    return string(buffer);
}

//哈夫曼译码
void  HuffmanenCode(string s,int n,Htnode h[]){
    int i=0,j=0,lchild=2*n-2,rchild=2*n-2;
    while(s[i]!='\0'){
        if(s[i]=='0'){
        //出现的问题是,最初将lchild,rchild分开计算,导致在左右子树间相互变化出现
        //lchild,rchild不同同时表示同一个节点,最后想到lchild=rchild就能解决问题
            lchild=h[lchild].lchild;
            rchild=j=lchild;
        }
        if(s[i]=='1'){
            rchild=h[rchild].rchild;
            lchild=j=rchild;
        }
        if(h[lchild].lchild==-1 && h[rchild].rchild==-1){
            cout<<h[j].c;
            lchild=rchild=2*n-2;
            j=0;
        }
        i++;
    }
}
int main(){
    Htnode h[99999];
    Htcode hcode[26];
    int a[26]={0};
    string s;
    int n = type(a);
    cout<<"Total input type:"<<n<<endl;
    HuffmandeCode(h, n, a, hcode);
    HuffmanenCode(load(),n,h);
    return 0;
}
```

## 运行结果

为了便于查看文件内容，重新编码的文件并没有以二进制文件的形式保存，所以并不会减小文件大小。首先我们对题目中给出的一组数据进行测试：

![hafuman6.png](https://i.loli.net/2021/04/28/3eqnYXIj2aZPFdG.png)

![hafuman4.png](https://i.loli.net/2021/04/28/tyRDInvENCjAokV.png)

可以看出程序正确地统计了各个字符出现的次数，并且给出了一个编码表，虽然和题中的编码稍有区别，但总的效果是一样的。同时，在最后一行也成功地将编码解码之后的内容输出了。

之后测试了一段更复杂的文字，（在修正了一些bug之后）成功进行了编码和解码。好耶！

![hafuman5.png](https://i.loli.net/2021/04/28/ZmeUP8bXiJMABlD.png)

![hafuman3.png](https://i.loli.net/2021/04/28/CVGO31QFcTjbfmk.png)

关于修正bug：

1. 修复了节点数定义太少的问题；
2. 修复了存放编码的数组过小的问题；
3. 修复了当出现过的字母不连续时，不能正确将前几个节点初始化的问题；
4. 修复了解码时，不能正确找到存有字母的节点的问题；
5. 优化了检索权值最小的两个节点的方法；
6. 优化了读入文件数据的方式；

## 参考链接

[1] https://blog.csdn.net/qq_40845344/article/details/94490984

[2] https://blog.csdn.net/qq_31349683/article/details/112318492

最后更新于2021年4月28日
