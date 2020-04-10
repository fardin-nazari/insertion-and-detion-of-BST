# include <stdio.h>
# include <malloc.h>

struct node
{
	int data;
	struct node *leftchild;                       
	struct node *rightchild;
};
struct node*root;

void find(int item,struct node **ele,struct node **pos)
{
	struct node *p,*ptr;

	if(root==NULL)  
	{
		*pos=NULL;
		*ele=NULL;
		return;
	}
	if(item==root->data) 
	{
		*pos=root;
		*ele=NULL;
		return;
	}
	
	if(item<root->data)
		p=root->leftchild;
	else
		p=root->rightchild;
	ptr=root;

	while(p!=NULL)
	{
		if(item==p->data)
		{       *pos=p;
			*ele=ptr;
			return;
		}
		ptr= p;
		if(item<p->data)
			p=p->leftchild;
		else
			p=p->rightchild;
	 }
	 *pos=NULL;   
	 *ele=ptr;
}

void insert(int item)
{       struct node *tmp,*parent,*location;
	find(item,&parent,&location);
	if(location!=NULL)
	{
		printf("Item already present");
		return;
	}

	tmp=(struct node *)malloc(sizeof(struct node));
	tmp->data=item;
	tmp->leftchild=NULL;
	tmp->rightchild=NULL;

	if(parent==NULL)
		root=tmp;
	else
		if(item<parent->data)
			parent->leftchild=tmp;
		else
			parent->rightchild=tmp;
}


void situation_a(struct node *arg,struct node *pos )
{
	if(arg==NULL) 
		root=NULL;
	else
		if(pos==arg->leftchild)
			arg->leftchild=NULL;
		else
			arg->rightchild=NULL;
}

void situation_b(struct node *par,struct node *pos)
{
	struct node *child;

	
	if(pos->leftchild!=NULL) 
		child=pos->leftchild;
	else                
		child=pos->rightchild;

	if(par==NULL )   
		root=child;
	else
		if( pos==par->leftchild)   
			par->leftchild=child;
		else                  
			par->rightchild=child;
}

void situation_c(struct node *par,struct node *pos)
{
	struct node *p,*y,*suc,*z;            


	y=pos;
	p=pos->rightchild;
	while(p->leftchild!=NULL)
	{
		y=p;
		p=p->leftchild;
	}
	suc=p;
	z=y;

	if(suc->leftchild==NULL && suc->rightchild==NULL)
		situation_a(z,suc);
	else
		situation_b(z,suc);

	if(par==NULL) 
		root=suc;
	else
		if(pos==par->leftchild)
			par->leftchild=suc;
		else
			par->rightchild=suc;

	suc->leftchild=pos->leftchild;
	suc->rightchild=pos->rightchild;
}
int del(int item)
{
	struct node *parent,*location;
	if(root==NULL)
	{
		printf("Tree is empty");
		return 0;
	}

	find(item,&parent,&location);
	if(location==NULL)
	{
		printf("Item not present");
		return 0;
	}

	if(location->leftchild==NULL && location->rightchild==NULL)
		situation_a(parent,location);
	if(location->leftchild!=NULL && location->rightchild==NULL)
		situation_b(parent,location);
	if(location->leftchild==NULL && location->rightchild!=NULL)
		situation_b(parent,location);
	if(location->leftchild!=NULL && location->rightchild!=NULL)
		situation_c(parent,location);
	free(location);
}
void main()
{
	int select,Data;
	root=NULL;
	while(1)
	{
		printf("\n");
		printf("1.Insert\n");
		printf("2.Delete\n");
		printf("select either 1 or 2");
		scanf("%d",&select);

		switch(select)
		{
		 case 1:
			printf("Enter the number to be inserted : ");
			scanf("%d",&Data);
			insert(Data);
			break;
		 case 2:
			printf("Enter the number to be deleted : ");
			scanf("%d",&Data);
			del(Data);
			break;
		case 3:
            break;
		 default:
			printf("Wrong choice\n");
		}
	}
}
