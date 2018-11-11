# Bridges-and-Articulation
#include <bits/stdc++.h> 
#include <iostream>
#include <vector>
using namespace std;
int min(int a,int b){
    if(a>b){
        return b;
    }
    else{
        return a;
    }
}
int max(int a,int b){
    if(a>b){
        return a;
    }
    else{
        return b;
    }
}
struct edge{
    int lkey;
    int hkey;
};
struct vlist;
vector<int> mergearr(vector<int> A,vector<int> B){
    int n1=A.size();
    int n2=B.size();
    vector<int> C(n1+n2);
    int i=0;int j=0;int k=0;
    while(i<n1&&j<n2){
        if(A[i]<=B[j]){
            C[k]=A[i];k++;i++;
        }
        else{
            C[k]=B[j];k++;j++;
        }
    }
    if(i==n1){
        while(j<n2){
            C[k]=B[j];k++;j++;
        }
    }
    else{
        while(i<n1){
            C[k]=A[i];k++;i++;
        }
    }
    return C;
}
vector<int> mergesort(vector<int> Ar){
    int n=Ar.size();
    if(n==1){
        return Ar;
    }
    else{
        vector<int> A(n/2);
	    vector<int> B(n-(n/2));
	    for(int i=0;i<n/2;i++){
	        A[i]=Ar[i];
	    }
	    for(int i=n/2;i<n;i++){
	        B[i-n/2]=Ar[i];
	    } 
	    A=mergesort(A);
	    B=mergesort(B);
	    return mergearr(A,B);
    }
}
vector<edge> mergearre(vector<edge> A,vector<edge> B){
    int n1=A.size();
    int n2=B.size();
    vector<edge> C(n1+n2);
    int i=0;int j=0;int k=0;
    while(i<n1&&j<n2){
        if(A[i].lkey<B[j].lkey){
            C[k]=A[i];k++;i++;
        }
        else if(A[i].lkey==B[j].lkey&&A[i].hkey<=B[j].hkey){
            C[k]=A[i];k++;i++;
        }
        else{
            C[k]=B[j];k++;j++;
        }
    }
    if(i==n1){
        while(j<n2){
            C[k]=B[j];k++;j++;
        }
    }
    else{
        while(i<n1){
            C[k]=A[i];k++;i++;
        }
    }
    return C;
}
vector<edge> mergesorte(vector<edge> Ar){
    int n=Ar.size();
    if(n==1){
        return Ar;
    }
    else{
        vector<edge> A(n/2);
	    vector<edge> B(n-(n/2));
	    for(int i=0;i<n/2;i++){
	        A[i]=Ar[i];
	    }
	    for(int i=n/2;i<n;i++){
	        B[i-n/2]=Ar[i];
	    } 
	    A=mergesorte(A);
	    B=mergesorte(B);
	    return mergearre(A,B);
    }
}
struct vlist{
    vlist *p=NULL;
    int key=0;
    vlist *curr=NULL;
    int f=0;
    int color=0;
    int d=1000000;
    int low=1000000;
    vlist *parent=NULL;
    int nc=0;
    int e=0;
};
vector<int> pts;
vector<edge> edg;
int tim=0;
void DFS(vlist* arr,vlist* u){
    arr[u->key-1].color=1;
    tim++;
    arr[u->key-1].d=tim;
    //u->d=tim;
    arr[u->key-1].low=arr[u->key-1].d;
    //u->low=u->d;
    vlist* v=u->p;
    while(v!=NULL){

        if(arr[v->key-1].color==0){
            arr[v->key-1].parent=&arr[u->key-1];
            arr[u->key-1].nc=arr[u->key-1].nc+1;
            DFS(arr,arr+arr[v->key-1].key-1);
            if(arr[v->key-1].low==arr[v->key-1].d){
                edge edgins;
                edgins.lkey=min(v->key,u->key);
                edgins.hkey=max(v->key,u->key);
                edg.push_back(edgins);
            }
            arr[u->key-1].low=min(arr[u->key-1].low,arr[v->key-1].low);
            if(arr[v->key-1].low>=arr[u->key-1].d&&arr[u->key-1].d>1){
                arr[u->key-1].e=1;
            }
        }
        else if((arr[v->key-1].color==1)&&(&arr[v->key-1])!=arr[u->key-1].parent){
            arr[u->key-1].low=min(arr[u->key-1].low,arr[v->key-1].d);
        }
        v=v->p;
    }
    tim++;
    arr[u->key-1].color=2;
    if(arr[u->key-1].e==1){
        pts.push_back(u->key);
    }
    return;
}
int main(){
    int v;
    scanf("%d\n",&v);
    int e;
    scanf("%d\n",&e);
    vlist* arr=(vlist *) malloc(v*sizeof(vlist));
    for(int j=0 ;j<v;j++){
        arr[j].key=j+1;
    }
    for(int j=0 ;j<e;j++){
        int e1,e2;
        scanf("%d %d\n",&e1,&e2);
        e1=e1+1;
        e2=e2+1;
        if(arr[e1-1].f==0){
            arr[e1-1].p=(vlist *) malloc(sizeof(vlist));
            arr[e1-1].p->key=e2;
            arr[e1-1].f=1;
            arr[e1-1].curr=arr[e1-1].p;
        }
        else{
            arr[e1-1].curr->p=(vlist *) malloc(sizeof(vlist));
            arr[e1-1].curr->p->key=e2;
            arr[e1-1].curr=arr[e1-1].curr->p;
        }
        if(arr[e2-1].f==0){
            arr[e2-1].p=(vlist *) malloc(sizeof(vlist));
            arr[e2-1].p->key=e1;
            arr[e2-1].f=1;
            arr[e2-1].curr=arr[e2-1].p;
        }
        else{
            arr[e2-1].curr->p=(vlist *) malloc(sizeof(vlist));
            arr[e2-1].curr->p->key=e1;
            arr[e2-1].curr=arr[e2-1].curr->p;
        }
    }
    for(int i=0;i<v;i++){
        if (arr[i].color==0){
            tim=0;
            DFS(arr,arr+i);
            if(arr[i].nc>1){
            pts.push_back(arr[i].key);
            }
        }
    }
    int p=pts.size();
    if(p>0){
        vector<int> ptsf=mergesort(pts);
        int p=ptsf.size();
        for(int i=0;i<p;i++){
    	    printf("%d ",ptsf[i]-1);
    	}
    }
	else if(p==0){
	    printf("EMPTY");
	}
	printf("\n");
	p=edg.size();
	if(p>0){
    	vector<edge> edgf=mergesorte(edg);
        for(int i=0;i<p;i++){
    	    printf("%d %d\n",edgf[i].lkey-1,edgf[i].hkey-1);
    	}
	}
	return 0;
}
