# hello-world
参考官方文档学习
> hello world
```c
#define TRUE 1
#define FALSE 0
#define OK 1
#define ERROR 0
#define INFEASIBLE -1
#define OVERFLOW -2
typedef int status;
#define MAXSIZE 100
#define ADD 10
#include <stdio.h>
#include <stdlib.h>

typedef struct{
	int *base;
	int length;
	int listsize;
}sqlist;//顺序表的存储结构
status listinsert(sqlist *L,int ip,int e)
{
	if((ip<1)||(ip>L->length+1))return ERROR;
	if(L->length==MAXSIZE){
	L->base=(int *)realloc(L->base,(L->listsize+ADD)*sizeof(int));
	L->listsize+=ADD;
	}
	for(int i=L->length-1;i>=ip-1;i--){
		L->base[i+1]=L->base[i];
	}
	L->base[ip-1]=e;
	L->length++;
	return OK;
}

status initlist(sqlist *L,int length){
	L->base=(int *)malloc(sizeof(int)*MAXSIZE);
	if(!L->base)return OVERFLOW;
	L->length=0;
	for (int i=1;i<=length;i++){
		int e;
		scanf("%d",&e);
		listinsert(L,i,e);
	}
	return OK;
}
status destorylist(sqlist *L){
	if(L->base)free(L->base);
	else return ERROR;
	L->length=0;
	return OK;
}
status clearlist(sqlist *L){
	L->length=0;
	return OK;
}
status listempty(sqlist L){
	if(L.length==0)return TRUE;
	else return FALSE;
}
status listlength(sqlist L){
	return L.length;
}
status getelem(sqlist L,int i,int *e){
	if((i<1)||(i>L.length))return ERROR;
	*e=L.base[i-1];
	return OK;
}
status locatelem(sqlist L,int e){
	for(int i=0;i<L.length;i++){
		if(e==L.base[i])return i+1;
	}
	return 0;
}
status listdelete(sqlist *L,int i,int *e){
	if((i<1)||(i>L->length))return ERROR;
	*e=L->base[i-1];
	for(i;i<L->length;i++){
		L->base[i-1]=L->base[i];
	}
	L->length--;
	return OK;
}
status printlist(sqlist L){
	for(int i=0;i<L.length;i++){
		if(i!=L.length-1)printf("%d ",L.base[i]);
		else printf("%d\n",L.base[i]);
	}
	return OK;
}
void qsort(sqlist *L, int begin, int end){
    if(begin > end)
        return;
    int tmp = L->base[begin];
    int i = begin; 
    int j = end;
    while(i != j){
        while(L->base[j] >= tmp && j > i)
            j--;
        while(L->base[i] <= tmp && j > i)
            i++;
        if(j > i){
            int t = L->base[i];
            L->base[i] = L->base[j];
            L->base[j] = t;
        }
    }
    L->base[begin] = L->base[i];
    L->base[i] = tmp;
    qsort(L, begin, i-1);
    qsort(L, i+1, end);
}
void mergelist(sqlist *la,sqlist *lb,sqlist *lc){
	lc->listsize=lc->length=la->length+lb->length;
	lc->base=(int *)realloc(lc->base,lc->listsize*sizeof(int));
	if(!lc->base)exit(OVERFLOW);
	int *pa=la->base,*palast=la->base+la->length-1;
	int *pb=lb->base,*pblast=lb->base+lb->length-1;
	int *pc=lc->base;
	while(pa<=palast && pb<=pblast){
		if(*pa<=*pb) *pc++=*pa++;
		else *pc++=*pb++;
	}
	while(pa<=palast)*pc++=*pa++;
	while(pb<=pblast)*pc++=*pb++;
}
int main(){
	int m,n,t;
	int *e=&t;
	sqlist la,*LA=&la,lb,*LB=&lb,lc,*LC=&lc;
	printf("la个数：");
	scanf("%d",&m);
	printf("la元素:");
	initlist(LA,m);
	printf("lb个数:");
	scanf("%d",&n);
	printf("lb元素:");
	initlist(LB,n);
	printf("请输入插入la的位置和元素（la有%d个元素）",la.length);
	scanf("%d%d",&m,&n);
	listinsert(LA,m,n);
	printf("请输入插入lb的位置和元素（lb有%d个元素）",lb.length);
	scanf("%d%d",&m,&n);
	listinsert(LB,m,n);
	printf("查找la第几个元素（la有%d个元素）",la.length);
	scanf("%d",&n); 
	getelem(la,n,e);
	printf("是%d\n",*e);
	printf("查找lb第几个元素（lb有%d个元素）",lb.length);
	scanf("%d",&m); 
	getelem(lb,m,e);
	printf("是%d\n",*e);
	printf("输入你想查找la的元素");
	scanf("%d",&n); 
	if(locatelem(la,n))printf("在%d处\n",locatelem(la,n));
	else printf("找不到\n");
	printf("输入你想查找lb的元素");
	scanf("%d",&m); 
	if(locatelem(lb,m))printf("在%d处\n",locatelem(lb,m));
	else printf("找不到\n");
	qsort(LA,0,(la.length-1));
	qsort(LB,0,(lb.length-1));
	printf("合并排序：");
	initlist(LC,0);
	mergelist(LA,LB,LC);
	printlist(lc);
	
}
---
你好
