#include <iostream>
#include <graphics.h>
#include <conio.h>
using namespace std;

struct node {
    int data;
    struct node *Pleft;
    struct node *Pright;
};

typedef struct node NODE;
typedef NODE* Tree;
void CreateCay(Tree &t)
{
	t = NULL; //cay rong
}
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

void drawNode(int x, int y, int key) {
    circle(x, y, 20);
    char buffer[5];
    itoa(key, buffer, 10);
    outtextxy(x - 5, y - 7, buffer);
}

void drawLine(int x1, int y1, int x2, int y2) {
    line(x1, y1, x2, y2);
}

void drawBST(Tree root, int x, int y, int xSpacing) {
    if (root == NULL)
        return;

    drawNode(x, y, root->data);

    if (root->Pleft != NULL) {
        drawLine(x, y + 20, x - xSpacing, y + 50);
        drawBST(root->Pleft, x - xSpacing, y + 50, xSpacing / 2);
    }

    if (root->Pright != NULL) {
        drawLine(x, y + 20, x + xSpacing, y + 50);
        drawBST(root->Pright, x + xSpacing, y + 50, xSpacing / 2);
    }
}

NODE* TimKiem(Tree t, int x) {
    if (t == NULL) {
        return NULL;
    } else {
        if (t->data > x) {
            return TimKiem(t->Pleft, x);
        } else if (t->data < x) {
            return TimKiem(t->Pright, x);
        } else {
            return t;
        }
    }
}

int TimMax(Tree t) {
    if (t->Pleft == NULL && t->Pright == NULL) {
        return t->data;
    }
    int MAX = t->data;
    if (t->Pleft != NULL) {
        int left = TimMax(t->Pleft);
        if (MAX < left) {
            MAX = left;
        }
    }
    if (t->Pright != NULL) {
        int right = TimMax(t->Pright);
        if (MAX < right) {
            MAX = right;
        }
    }
    return MAX;
}

void NodeTheMang(Tree &X, Tree &Y) {
    if (Y->Pleft != NULL) {
        NodeTheMang(X, Y->Pleft);
    } else {
        X->data = Y->data;
        X = Y;
        Y = Y->Pright;
    }
}

NODE* XoaNode(Tree &t, int del) 
{
    if (t == NULL) 
	{
        return t;
    } else {
        if (del < t->data) 
		{
            XoaNode(t->Pleft, del);
        } 
		else if (del > t->data)
		{
            XoaNode(t->Pright, del);
        } 
		else 
		{
            NODE *X = t;
            if (t->Pleft == NULL) 
			{
                t = t->Pright;
            } 
			else if (t->Pright == NULL) 
			{
                t = t->Pleft;
            } 
			else 
			{
                NodeTheMang(X, t->Pright);
            }
            delete X;
        }
    }
}

void Duyet_RNL(Tree t) {
    if (t != NULL) {
        Duyet_RNL(t->Pright);
        cout << t->data << " ";
        Duyet_RNL(t->Pleft);
    }
}


void Duyet_GTS(Tree t, int x, int y, int xSpacing) {
  if (t != NULL) {
    Duyet_GTS(t->Pleft, x - xSpacing, y + 50, xSpacing / 2);
    drawNode(x, y, t->data);
    delay(500);
    Duyet_GTS(t->Pright, x + xSpacing, y + 50, xSpacing / 2);
  }
}

void ThemNodeVaVe(Tree &t, int x, int initialx, int y = 50, int xSpacing = 200) {
  ThemNode(t, x);
  Duyet_GTS(t, initialx, y, xSpacing);
  cleardevice();
  //Duyet_GTS(t, getmaxx() / 2, 50, 200);
}
void XoaNodeVaVe(Tree &t, int x, int initialx, int y = 50, int xSpacing = 200) {
  XoaNode(t, x);
  Duyet_GTS(t, initialx, y, xSpacing);
  cleardevice();
}

void Menu(Tree &t) {
    NODE* X = NULL;
    int choice, x;

    do {
        cout << "\n\n\t\t ========== MENU ==========";
        cout << "\n1. Them du lieu cay";
        cout << "\n2. Tim kiem phan tu co trong cay";
        cout << "\n3. Tim phan tu MAX co trong cay";
        cout << "\n4. Xoa node bat ky co trong cay";
        cout << "\n5. Xuat du lieu cay";
        cout << "\n6. Ket thuc";
        cout << "\n\n\t\t ========== END ===========";
        cout << "\nNhap lua chon cua ban: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Nhap phan tu can them vao cay BST: ";
                cin >> x;
                ThemNodeVaVe(t, x, getmaxx() / 2);
                drawBST(t, getmaxx() / 2, 50, 200);
                break;
            case 2:
                cout << "Nhap phan tu can tim kiem: ";
                cin >> x;
                if (TimKiem(t, x) == NULL) {
                    cout << "\nPhan tu " << x << " khong ton tai trong cay BST!";
                } else {
                    cout << "\nPhan tu " << x << " co ton tai trong cay BST!";
                }
                system("pause");
                break;
            case 3:
                cout << "Phan tu MAX co trong cay BST la: " << TimMax(t);
                system("pause");
                break;
            case 4:
                cout << "Nhap gia tri can xoa: ";
                cin >> x;
                X = XoaNode(t, x);
                XoaNodeVaVe(t, x, getmaxx() / 2);
                drawBST(t, getmaxx() / 2, 50, 200);
                break;
            case 5:
                cout << "\n\t\tDuyet cay BST hien tai: ";
                Duyet_RNL(t);
                system("pause");
                break;
            case 6:
                cout << "\nThoat chuong trinh.";
                break;
            default:
                cout << "Lua chon khong hop le. Vui long nhap";
                break;
        }

    } while (choice != 6);
}

int main() {
    int gd = DETECT, gm;
    initgraph(&gd, &gm, "C:\\Turboc3\\BGI");
    Tree t;
    CreateCay(t);
    Menu(t);
    drawBST(t, getmaxx() / 2, 50, 200);
    system("pause");
    closegraph();
    return 0;
}
