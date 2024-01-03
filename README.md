#include <iostream>
#include <graphics.h>
#include <conio.h>
#include <fstream>
#include <sstream>

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
		if(t->data >= x)
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
        drawLine(x, y + 20, x - xSpacing , y + 30);
        drawBST(root->Pleft, x - xSpacing , y + 50, xSpacing / 2);
    }

    if (root->Pright != NULL) {
        drawLine(x, y + 20, x + xSpacing, y + 30);
        drawBST(root->Pright, x + xSpacing , y + 50, xSpacing / 2);
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
void XoaTatCaNode(Tree &t) {
    if (t == NULL) {
        return;
    }

    XoaTatCaNode(t->Pleft);
    XoaTatCaNode(t->Pright);

    delete t;
    t = NULL;
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
  drawBST(t, getmaxx() / 2, 50, 200);
}


//Ham Nhap file
void NhapTuFile(Tree &t) {
    char fileName[50];

    cout << "Nhap ten file text: ";
    cin >> fileName;

    ifstream file(fileName);

    if (!file.is_open()) {
        cout << "Khong mo duoc file!" << endl;
        return;
    }

    int x;
    while (file >> x) {
        ThemNode(t, x);
    }

    file.close();
    cout << "Da nhap du lieu tu file thanh cong!" << endl;
    drawBST(t, getmaxx() / 2, 50, 200);
    system("pause");
}

void Menu(Tree &t) {
    NODE* X = NULL;
    int choice, x;
	
    do {
    	system("cls");
        cout << "\n\n\t\t ========== MENU ==========";
        cout << "\n1. Them du lieu cay";
        cout << "\n2. Tim kiem phan tu co trong cay";
        cout << "\n3. Tim phan tu MAX co trong cay";
        cout << "\n4. Xoa node bat ky co trong cay";
        cout << "\n5. Xuat du lieu cay";
        cout << "\n6. Nhap du lieu co san trong file text";
        cout << "\n7. Reset tat ca cac node trong cay";
        cout << "\n8. Ket thuc";
        cout << "\n\n\t\t ========== END ===========";
        cout << "\nNhap lua chon cua ban: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Nhap phan tu can them vao cay BST: ";
                cin >> x;
                ThemNodeVaVe(t, x, getmaxx() / 2);
                drawBST(t, getmaxx() / 2, 50, 200);
                cout << "Phan tu " << x << " da duoc them vao cay BST!";
                system("pause");
                break;
            case 2:
                cout << "Nhap phan tu can tim kiem: ";
                cin >> x;
                if (TimKiem(t, x) == NULL) {
                    cout << "Phan tu " << x << " khong ton tai trong cay BST!";
                } else {
                    cout << "Phan tu " << x << " co ton tai trong cay BST!";
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
                if (TimKiem(t, x) == NULL) {
                    cout << "Phan tu " << x << " khong ton tai trong cay BST!";
                } else {
                    cout << "Phan tu " << x << " da xoa trong cay BST!";
                    X = XoaNode(t, x);
                	XoaNodeVaVe(t, x, getmaxx() / 2);
                	drawBST(t, getmaxx() / 2, 50, 200);
                }
				system("pause");
                break;
            case 5:
                cout << "\n\t\tDuyet cay BST hien tai: ";
                Duyet_RNL(t);
                system("pause");
                break;
        	case 6:
        		NhapTuFile(t);
        		//NhapTuFile(t, "INPUT.txt");
        		//ThemNodeVaVe(t, x, getmaxx() / 2);
                drawBST(t, getmaxx() / 2, 50, 200);
                //Duyet_RNL(t);
                //system("pause");
                break;
			case 7:
    			XoaTatCaNode(t);
    			cleardevice();
    			cout << "\nTat ca cac node da duoc xoa!";
    			system("pause");
    			break;
    	 	case 8:
                cout << "\nThoat chuong trinh.";
                break;
            default:
                cout << "Lua chon khong hop le. Vui long nhap";
                break;
        }

    } while (choice != 8);
}

int main() {
    int gd = DETECT, gm ;
     // Cung c?p du?ng d?n d?n trình di?u khi?n d? h?a
	 initwindow(1920, 1080);
      //G?i hàm initgraph() v?i du?ng d?n du?c ch? d?nh b?i bi?n pathtodriver
  	//initgraph(&gd, &gm, "");
    Tree t;
    CreateCay(t);
    //NhapTuFile(t, "INPUT.txt");
    Menu(t);
    drawBST(t, getmaxx() / 2, 50, 200);
    system("pause");
    closegraph();
    return 0;
}
