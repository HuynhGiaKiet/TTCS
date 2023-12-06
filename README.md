#include<iostream>
using namespace std;
struct node 
{
	int data;// du lieu ma node se luu tru
	struct node *Pleft; //Node lien ket ben trai cua cay <=> cay con trai
	struct node *Pright; //Node lien ket ben phai cua cay <=> cay con phai
};
typedef struct node NODE; //Thay cum struct node == NODE
typedef NODE* Tree; //Thay con tro NODE* == Tree

//Khoi tao cay
void CreateCay(Tree &t)
{
	t = NULL; //cay rong
}

//Ham them 
void ThemNode(Tree &t, int x)
{
	//neu cay rong
	if(t == NULL)
	{
		NODE *p = new NODE; //Khoi tao 1 cai node de them vao cay
		p->data = x; //Them du lieu x vao data
		p->Pleft = NULL;
		p->Pright = NULL;
		t = p;//Node p chinh la node goc <=> x chinh la goc
	}
	else //Cay co ton tai phan tu
	{
		//Neu phan tu them vao ma nho hon node goc => them vao ben trai
		if(t->data > x)
		{
			ThemNode(t->Pleft,x);// duyet qua trai de them phan tu x
		}
		else if(t->data < x)
		{
			ThemNode(t->Pright,x);// duyet qua phai dem them phan tu x
		}
	}
}	

//Ham xuat cay nhi phan theo RNL (giam dan)
void Duyet_RNL(Tree t)
{
	if(t !=NULL)
	{
		Duyet_RNL(t->Pright);//duyet qua phai
		cout << t->data << " "; //Xuat du lieu trong node
		Duyet_RNL(t->Pleft); //duyet qua trai			
	}
}
//Ham nhap du lieu
void Menu(Tree &t)
{
	int choice,x;
	do
	{
		system("cls");//chon menu xong thi xoa cai menu de no in ra xau :)))
		cout <<"\n\n\t\t ========== MENU ==========";
		cout <<"\n1. Them du lieu cay";
		cout <<"\n2. Xuat du lieu cay";
		cout <<"\n3. Ket thuc";
		cout <<"\n\n\t\t ==========================";
		
		
		cout <<"\nNhap lua chon cua ban: ";
		cin >> choice;
		
		switch(choice)
		{
			case 1:
				
				cout <<"Nhap phan tu can them vao cay BST: ";
				cin >> x;
				ThemNode(t,x);
				break;
			case 2:
				cout <<"\n\t\tDuyet cay BST hien tai: ";
				Duyet_RNL(t); 
				system("pause");
			case 3:
    			cout << "\nThoat chuong trinh.";
                break;
            default:
                cout << "Lua chon khong hop le. Vui long nhap";
                break;
		}
	} while (choice != 3);
}
int main()
{
	//Khai bao cau truc
	Tree t; //khoi tao 
	CreateCay(t);
	Menu(t);
	system("pause");
	return 0;
}
