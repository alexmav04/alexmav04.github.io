---
title: "用linked list做稀疏矩陣加法"
layout: single
author_profile: true
comments: true
categories:
  - computer
tags:
  - data structure and algorithm
  - C++
sidebar:
  nav: "sidepost"
---

關於linked list這裡有很好的講解：
* [Linked List: Intro(簡介)](http://alrightchiu.github.io/SecondRound/linked-list-introjian-jie.html)
* [Linked List: 新增資料、刪除資料、反轉](http://alrightchiu.github.io/SecondRound/linked-list-xin-zeng-zi-liao-shan-chu-zi-liao-fan-zhuan.html)

有很清楚的示意圖輔助觀念的養成，光看這兩篇便省了不少需要花在理解上的時間，也很適合收藏做為日後複習觀看。

雖然再次寫下關於linked list的介紹文有助於自身觀念釐清，但這次先著重在應用上。

此篇文章的誕生是因為課堂作業需求，剛好再次熟悉並釐清了很多在linked list上的觀念誤區。

---
## 為何而做
本次要使用linked list在稀疏矩陣（sparse matrix）的加法上。

稀疏矩陣是其元素大部分為零的矩陣。為什麼特別指定稀疏矩陣？我們一般要存矩陣第一個優先會想到array型態，然而今天遇上稀疏矩陣，既然大部分元素為0，事實上我們便可以不用做到「每一個元素都要參與運算」這件事。

用linked list儲存那些少數且必須的資料，需要的時候直接由linked list遍歷即可。

## 初步設計
提前規劃，寫起來會更加簡潔有效率。

這裡的input我們設計成：
先輸入矩陣大小m,n，再來輸入矩陣元素。
前面數字輸入column的位置，後面數字是那個位置的值，並且用0判斷元素輸入結束。

舉例來說：

$$\begin{pmatrix}
1 & 6 & 0 & 85\\
5 & 0 & 0 & 0\\
0 & 0 & 2 & 0
\end{pmatrix}$$

這個矩陣
input:
先輸入矩陣大小
```
> 3 4
```

而後輸入元素：
```
> 1 1 2 6 4 85 0
> 1 5 0
> 3 2 0
> 0
```

### linked list的設計
linked list用比較不精確的白話文說，就是建造一個物件，他有一些自身的性質外，我們會再多存一兩個性質，去指向這個物件的前一個或後一個資料。因為有了這個性質，很巧妙地彼此之間有了連結（link），把每個物件都像是火車一樣都串連起來了。這部分可以講的東西有很多，上方或下方已提供很不錯的資料，在此不再贅述。

這次只專注在設計我需要的linked list，和使用它。

* 節點（node）

節點是主體，沒了節點再怎麼談連接都只是空談。
此處的node我要存四個資料：值（value）、所在列（row_pos）、所在行（column_pos）以及下個node的位置。

並且設置兩個創造節點的方式：一個為無指定值，一個為有指定值。


```cpp
class llNode {
private:
  int value;
  int row_pos;
  int column_pos;
  llNode *next;

public:
  llNode() : next(NULL){};
  llNode(int a) : value(a), next(NULL){};

  friend class LinkedList;
};
```

* linked list

創建好基本元素Node後，可以來弄linked list了。
這裡我需要的功能有新增node、列印已存在node、列印矩陣、矩陣相加。
（若是不太確定需求，也可寫完函式確定型態後再回頭新增）

其中要新增一個起頭的node，在這裡我們將它命名為 `first`。
```cpp
class LinkedList {
private:
  llNode *first;

public:
  LinkedList() : first(NULL){};
  void PrintList();
  void addNode(int newrow_pos, int newcolumn_pos, int newvalue);
  void printMatrix(int m, int n);
  LinkedList matrixAdd(LinkedList A, LinkedList B);
};
```

### 功能設計
就是寫function。

* addNode

這是最基本的功能，linked list就是由節點所組成的，因此我們需要有能夠新增節點的功能。

新增節點所需資料：行位置、列位置、值。在這程式中必須準備好這三項才能申請新節點（公務人員口吻XD）。

```cpp
void LinkedList::addNode(int newrow_pos, int newcolumn_pos, int newvalue) {
//content
}
```
接下來我需要三個node:
1. 存我要存進去的node（`newNode`）
2. 有資料傳進來必須暫時要有地方放（`temp`）
3. temp有時會存放先前資料，需要多個空間放新的node（`tempNew`）
      
```cpp
  llNode *newNode = new llNode(newvalue);
  llNode *temp = new llNode();
  llNode *tempNew = new llNode();
```
最基本的需求弄好了，接下來就是要放入資料和做連接。
首先，若是在程式中沒有用過linked list的狀況下，它的開頭應該是空的，也就是要放入的第一個資料必須放去`first`中。

若是已經有了資料，也不需要一一巡覽，只需要確認我們確實到達linked list尾端，並把東西加入這個list中。因此

```cpp
  if (first == NULL) {
    //是空的就直接加入
    temp->value = newvalue;
    temp->row_pos = newrow_pos;
    temp->column_pos = newcolumn_pos;
    temp->next = NULL;

    first = temp;
  } else {
    //若不是空的就先到達尾端再加入
    temp = first;
    while (temp->next != NULL) {
      temp = temp->next;
    }
    tempNew->value = newvalue;
    tempNew->row_pos = newrow_pos;
    tempNew->column_pos = newcolumn_pos;
    tempNew->next = NULL;
    temp->next = tempNew;
  }
```

再來要進入重頭戲了。
* matrixAdd

這個function很明顯就是關於矩陣加法的功能了。

在`matrixAdd`中我需要兩個linked list當變數（也就是兩個矩陣），並利用linked list的特性去做巡覽和相加。

```cpp
LinkedList LinkedList::matrixAdd(LinkedList A, LinkedList B) {
//content
}
```
首先要設置新的linked list `C` 去放置我們相加處理過後的元素。
在巡覽前我們必須要先設定好開頭，電腦才會知道我們要從linked list的哪裡開始。

```cpp
  LinkedList C;
  llNode *nodeA = A.first;
  llNode *nodeB = B.first;
```
接下來要設立一個迴圈。這次我使用的方式是先判斷巡覽到的node是否都有東西，進而再去判斷要怎麼透過辨識row和column的位置去做相加。

如果其中一個矩陣已經巡覽完畢沒東西的話，那很簡單，那個位置鐵定就是另一個矩陣巡覽到的元素，直接放進去linked list`C`中

```cpp
  while (nodeA != NULL && nodeB != NULL) {
  //content
  }
  
  while (nodeA != NULL) {
    C.addNode(nodeA->row_pos, nodeA->column_pos, nodeA->value);
    nodeA = nodeA->next;　//要記得巡覽後指向下一個
  }
  while (nodeB != NULL) {
    C.addNode(nodeB->row_pos, nodeB->column_pos, nodeB->value);
    nodeB = nodeB->next;
  }

```
接著我們專注在第一個while迴圈中。
在此迴圈中要做什麼事情呢？這邊我的方式是先比對row的位置一不一樣，接著再比對column的位置。

```
  while (nodeA != NULL && nodeB != NULL) {
    if (nodeA row 的位置 = nodeB row 的位置) {
      if (nodeA column 的位置 = nodeB column 的位置) {
        指定sum = 巡覽到的 nodeA + nodeB;
        加入C中
        nodeA指向下一個
        nodeB指向下一個
      } else { 
      //也就是row一樣但column位置不同，比較column位置大小
        if (nodeA column 的位置較小) {
          存nodeA 的資料進去C
          nodeA指向下一個
        } else {
          存nodeB 的資料進去C
          nodeB指向下一個
        }
      }
    } else { //兩個row的位置就已經不同的狀況
      //先比較row位置的大小
      if (nodeA row 的位置較小) {
        存nodeA 的資料進去
        nodeA指向下一個
      } else {
        存nodeB 的資料進去C
        nodeB指向下一個
      }
    }
  }
```
這裡應該可以感覺得出來，當初在創建linked list的元素順序很重要，如果亂了這裡的設置應該會失敗。（但是我也懶得寫通用狀況）

用程式碼實現差不多是這樣：

```cpp
  while (nodeA != NULL && nodeB != NULL) {
    if (nodeA->row_pos == nodeB->row_pos) {
      if (nodeA->column_pos == nodeB->column_pos) {
        int sum = nodeA->value + nodeB->value;
        C.addNode(nodeA->row_pos, nodeA->column_pos, sum);
        nodeA = nodeA->next;
        nodeB = nodeB->next;
      } else {
        if (nodeA->column_pos < nodeB->column_pos) {
          C.addNode(nodeA->row_pos, nodeA->column_pos, nodeA->value);
          nodeA = nodeA->next;
        } else {
          C.addNode(nodeB->row_pos, nodeB->column_pos, nodeB->value);
          nodeB = nodeB->next;
        }
      }
    } else {
      if (nodeA->row_pos < nodeB->row_pos) {
        C.addNode(nodeA->row_pos, nodeA->column_pos, nodeA->value);
        nodeA = nodeA->next;
      } else {
        C.addNode(nodeB->row_pos, nodeB->column_pos, nodeB->value);
        nodeB = nodeB->next;
      }
    }
  }
```
用手算矩陣加法很簡單，程式也是如此。
下篇將會介紹如何使用linked list實現稀疏矩陣的乘法。

## 完整程式碼
因為自己非本科系（資訊工程相關領域之類的），所以相當了解即使老師提點了仍不能好好地寫出一個完整程式的痛苦XD

通常來講大概是搞不清楚架構吧，到能夠好好地自己獨立寫完一個小小的專案也是因為看了海量的程式，就連以前工作也是很靠網路上的資源，其中不乏有熱心人士無私地直接把整組程式拿出來給大家看。

當初會架設這個部落格有部份也是為了回饋社會的，所以希望有需求的可以盡量拿走改成自己的，反正也不是多高級的東西XD

當然，最好的狀況是，你能告訴我有哪裡能夠改進的，我將會非常感激！

```cpp
#include <iostream>
using namespace std;

class llNode {
private:
  int value;
  int row_pos;
  int column_pos;
  llNode *next;

public:
  llNode() : next(NULL){};
  llNode(int a) : value(a), next(NULL){};

  friend class LinkedList;
};

class LinkedList {
private:
  llNode *first;

public:
  LinkedList() : first(NULL){};
  void PrintList();
  void addNode(int newrow_pos, int newcolumn_pos, int newvalue);
  void printMatrix(int m, int n);
  LinkedList matrixAdd(LinkedList A, LinkedList B);
};

int main() {
  int m, n, pos, value;
  int operation;
  LinkedList list1, list2, list3;

  cout << "Enter the number of row and column:" << endl;
  cin >> m >> n;

  //為了專注在資料結構上，有很多可能會有的例外狀況我直接忽略
  //例如輸入錯誤等等的
  cout << "Enter the element of matrix A:" << endl;
  for (int i = 0; i < m; i++) {
    while (cin >> pos && pos != 0 && pos <= n) {
      cin >> value;
      pos = pos - 1;
      list1.addNode(i, pos, value);
    }
  }

  cout << "Enter the element of matrix B:" << endl;
  for (int j = 0; j < m; j++) {
    while (cin >> pos && pos != 0 && pos <= n) {
      cin >> value;
      pos = pos - 1;
      list2.addNode(j, pos, value);
    }
  }

  list3 = list3.matrixAdd(list1, list2);

  cout << endl;
  cout << "The matrix A:" << endl;
  list1.printMatrix(m, n);
  cout << endl;
  cout << "The matrix B:" << endl;
  list2.printMatrix(m, n);
  cout << endl;
  cout << "The matrix A+B:" << endl;
  list3.printMatrix(m, n);
  cout << endl;
}

void LinkedList::addNode(int newrow_pos, int newcolumn_pos, int newvalue) {
  llNode *newNode = new llNode(newvalue);
  llNode *temp = new llNode();
  llNode *tempNew = new llNode();

  if (first == NULL) {
    temp->value = newvalue;
    temp->row_pos = newrow_pos;
    temp->column_pos = newcolumn_pos;
    temp->next = NULL;

    first = temp;
  } else {
    temp = first;
    while (temp->next != NULL) {
      temp = temp->next;
    }
    tempNew->value = newvalue;
    tempNew->row_pos = newrow_pos;
    tempNew->column_pos = newcolumn_pos;
    tempNew->next = NULL;
    temp->next = tempNew;
  }
}

//這邊的function先前沒細講，主要是寫來把矩陣印出的。
//（不然也不知道自己寫得對不對）
void LinkedList::printMatrix(int m, int n) {
  llNode *current = first;
  int **matrix;
  matrix = new int *[m];
  for (int i = 0; i < m; i++) {
    matrix[i] = new int[m];
    for (int j = 0; j < n; j++) {
      matrix[i][j] = 0;
    }
  }
  while (current != NULL) {
    int row, col;
    row = current->row_pos;
    col = current->column_pos;

    matrix[row][col] = current->value;
    current = current->next;
  }
  for (int i = 0; i < m; i++) {
    for (int j = 0; j < n; j++) {
      cout << matrix[i][j] << " ";
    }
    cout << endl;
  }
}

LinkedList LinkedList::matrixAdd(LinkedList A, LinkedList B) {
  LinkedList C;
  llNode *nodeA = A.first;
  llNode *nodeB = B.first;

  while (nodeA != NULL && nodeB != NULL) {
    if (nodeA->row_pos == nodeB->row_pos) {
      if (nodeA->column_pos == nodeB->column_pos) {
        int sum = nodeA->value + nodeB->value;
        C.addNode(nodeA->row_pos, nodeA->column_pos, sum);
        nodeA = nodeA->next;
        nodeB = nodeB->next;
      } else {
        if (nodeA->column_pos < nodeB->column_pos) {
          C.addNode(nodeA->row_pos, nodeA->column_pos, nodeA->value);
          nodeA = nodeA->next;
        } else {
          C.addNode(nodeB->row_pos, nodeB->column_pos, nodeB->value);
          nodeB = nodeB->next;
        }
      }
    } else {
      if (nodeA->row_pos < nodeB->row_pos) {
        C.addNode(nodeA->row_pos, nodeA->column_pos, nodeA->value);
        nodeA = nodeA->next;
      } else {
        C.addNode(nodeB->row_pos, nodeB->column_pos, nodeB->value);
        nodeB = nodeB->next;
      }
    }
  }
  while (nodeA != NULL) {
    C.addNode(nodeA->row_pos, nodeA->column_pos, nodeA->value);
    nodeA = nodeA->next;
  }
  while (nodeB != NULL) {
    C.addNode(nodeB->row_pos, nodeB->column_pos, nodeB->value);
    nodeB = nodeB->next;
  }
  return C;
}
```

input:
```
Enter the number of row and column:
> 4 5

Enter the element of matrix A:
> 1 2 5 8 0
> 2 3 4 55 0
> 3 7 0
> 1 100 0

Enter the element of matrix B:
> 2 8 3 4 0
> 2 44 0
> 1 9 3 6 0
> 0
```

output:
```
The matrix A:
2 0 0 0 8
0 3 0 55 0
0 0 7 0 0
100 0 0 0 0

The matrix B:
0 8 4 0 0
0 44 0 0 0
9 0 6 0 0
0 0 0 0 0

The matrix A+B:
2 8 4 0 8
0 47 0 55 0
9 0 13 0 0
100 0 0 0 0
```