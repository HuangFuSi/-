# -
用函数实现如下二叉排序树算法1.插入新结点2.前序、中序、后序遍历二叉树3.中序遍历的非递归算法4.层次遍历二叉树5.在二叉树中查找给定关键字(函数返回值为成功1,失败0)输入格式 第一行：准备建树的结点个数n  第二行：输入n个整数，用空格分隔  第三行：输入待查找的关键字 第四行：输入待查找的关键字 第五行：输入待插入的关键字   输出格式 第一行：二叉树的先序遍历序列 第二行：二叉树的中序遍历序列 第三行：二叉树的后序遍历序列 第四行：查找结果 第五行：查找结果 第六行~第八行：插入新结点后的二叉树的先、中、序遍历序列 第九行：插入新结点后的二叉树的中序遍历序列(非递归算法) 第十行：插入新结点后的二叉树的层次遍历序列   


#include <iostream>
#include <algorithm>
#include "stdio.h"
#include <math.h>
#include "malloc.h"
#include <stack>
#define TRUE 1
#define FALSE 0
#define OK  1
#define ERROR  0
#define INFEASIBLE -1
#define OVERFLOW -2
#define STACK_INIT_SIZE 100 // 存储空间初始分配量
#define STACKINCREMENT 10 // 存储空间分配增量
using namespace std;
typedef int  Status;
int n,m;
typedef int  ElemType;
typedef struct BiTNode
{
    ElemType data;
    struct BiTNode *lchild,*rchild;//左右孩子指针
} BiTNode,*BiTree;
int Depth(BiTree T);
Status SearchBST(BiTree T,int key,BiTree f,BiTree *p);
Status InsertBST(BiTree *T,int key);
Status CreateBST(BiTree &T)
{
   T=NULL;
   int i,j;
   for(i=0;i<m;i++)
   {
       scanf("%d",&j);
       InsertBST(&T,j);
   }
    return OK;
}
Status PrintElement( ElemType e )    // 输出元素e的值
{
    printf("%d ", e );
    return OK;
}// PrintElement
Status SearchBST(BiTree T,int key,BiTree f,BiTree *p)
{
    if(!T)
    {
        *p=f;
        return FALSE;
    }
    else if(key==T->data)
    {
        *p=T;
        return TRUE;
    }
    else if(key<T->data)
    {
        return SearchBST(T->lchild,key,T,p);

    }
    else return SearchBST(T->rchild,key,T,p);
}



Status InsertBST(BiTree *T,int key)
{
    BiTree p,s;
    if(!SearchBST(*T,key,NULL,&p))
    {
        s=(BiTree)malloc(sizeof(BiTNode));
        s->data=key;
        s->lchild=s->rchild=NULL;
        if(!p)
            *T=s;
        else if(key<p->data)
            p->lchild=s;
        else
            p->rchild=s;
        return TRUE;
    }
    else
        return FALSE;
}
Status PreOrderTraverse(BiTree T,Status(*Visit)(ElemType e))
{
    if(T)
    {
        if(PrintElement(T->data))
            if(PreOrderTraverse(T->lchild,Visit))
                if(PreOrderTraverse(T->rchild,Visit))
                    return OK;
        return ERROR;
    }
    else return OK;

}
Status InOrderTraverse(BiTree T,Status(*Visit)(ElemType e))
{
    if(T)
    {
        if(InOrderTraverse(T->lchild,Visit))
            if(PrintElement(T->data))
                if(InOrderTraverse(T->rchild,Visit))
                    return OK;
        return ERROR;
    }
    else return OK;

}
Status PostOrderTraverse(BiTree T,Status(*Visit)(ElemType e))
{
    if(T)
    {

        if(PostOrderTraverse(T->lchild,Visit))
            if(PostOrderTraverse(T->rchild,Visit))
                if(Visit(T->data))
                    return OK;
        return ERROR;
    }
    else return OK;

}

Status FInOrderTraverse(BiTree T,Status(*Visit)(ElemType e))
{
    //SqStack S,q;
    //InitStack(S);
    stack<BiTree> S;
    BiTNode *p,*p2;
    p=T;
    //printf("A");
    ElemType e;
    while(p||!S.empty())
    {
        if(p)
        {
            S.push(p);
            p=p->lchild;
            //printf("GET\n");
        }
        else
        {
            //printf("DE\n");
            p=S.top();
            Visit(p->data);
            S.pop();
            //printf("OK%d ",e);
            p=p->rchild;
        }
    }
    return OK;
}
void PrintNodeAtlevel(BiTree T,int level)
{
    if(NULL==T||level<1)return;
    if(1==level){
    cout<<T->data<<" ";
    return ;
    }
    PrintNodeAtlevel(T->lchild,level-1);
    PrintNodeAtlevel(T->rchild,level-1);
}
int Depth(BiTree T)
{
    double i=0,j=0,k=2;
    while(i<m){
    i=pow(k,j)-1;
    j++;
    }
    return (int)(j+0.5);
}
void LevelTraverse(BiTree T)
{
    if(NULL==T)
    return;
    int depth=Depth(T);
    //printf("%d\n",depth);
    for(int i=1;i<=depth;i++)
    {
        PrintNodeAtlevel(T,i);
    }
}

int main()
{
int u,o;
   BiTree T,p;
   scanf("%d",&m);
   CreateBST(T);
   PreOrderTraverse(T,PrintElement);
   printf("\n");
   InOrderTraverse(T,PrintElement);
   printf("\n");
   PostOrderTraverse(T,PrintElement);
   printf("\n");
   scanf("%d",&u);
   printf("%d\n",SearchBST(T,u,NULL,&p));
   scanf("%d",&u);
   printf("%d\n",SearchBST(T,u,NULL,&p));
   scanf("%d",&u);
   InsertBST(&T,u);
   PreOrderTraverse(T,PrintElement);
   printf("\n");
   InOrderTraverse(T,PrintElement);
   printf("\n");
   PostOrderTraverse(T,PrintElement);
   printf("\n");
   FInOrderTraverse(T,PrintElement);
   printf("\n");
  LevelTraverse(T);
   printf("\n");
}
