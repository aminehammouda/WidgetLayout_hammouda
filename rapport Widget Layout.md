# Programming Widget Layout

## objective

create some Graphical User Interface(GUI) using C++ programming combined with some Qt functionality  

## Introduction

GUI is a graphics-based operating system interface that uses icons, menus,**widgets** and **layouts**.  

### what are widgets and layouts :

**Widgets :** are the primary elements for creating user interfaces in Qt. ***Widgets*** can display data and status information, receive user input, and provide a container for other widgets that should be grouped together. A widget that is not embedded in a parent widget is called a ***window***.
**layout :** are an elegant and flexible way to automatically arrange child widgets within their container. Each widget reports its size requirements to the layout through the ***sizeHint*** and ***sizePolicy***  properties, and the layout distributes the available space accordingly.

Creating the forms below:
-----------------------------------------

*   [Experimenting with QHBOXLayout](#experimenting-with-qhboxlayout)
*   [Nested Layouts](#nested-layouts)
*   [Bug report Form](#bug-report-form)
*   [Grid Layout](#grid-layout)


Experimenting with [QHBOXLayout](#https://doc.qt.io/qt5/qhboxlayout.html)
==============================
> we will use here the *[QLineEdit](#https://doc.qt.io/qt-5/qlineedit.html) and [QPushButton](#https://doc.qt.io/qt-5/qpushbutton.html) Classes.*
   
  ###  `hBox.h:`
    
```cpp
class Dialog : public QWidget
{

public:
    explicit Dialog(QWidget *parent = nullptr);

protected:
  void createWidgets();
  void placeWidgets();
  void makeConnexions();
protected:
  QLabel *label ;
  QLineEdit *line;
  QPushButton *search;
  QHBoxLayout *layout;
};
```
### `hBox.cpp:`
```cpp

Dialog::Dialog(QWidget *parent) : QWidget(parent)
{
    createWidgets();
    placeWidgets();
    makeConnexions();

}
void Dialog::createWidgets()
{

   label = new QLabel("name: ");
   line = new QLineEdit();
   search = new QPushButton("Search");
   layout = new QHBoxLayout();
   setLayout(layout);

}
void Dialog ::placeWidgets(){
    layout->addWidget(label);
    layout->addWidget(line);
    layout->addWidget(search);

}

void Dialog::makeConnexions(){

}
```
### `main.cpp:`
```cpp

int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
    auto  window = new Dialog;
    window->setWindowTitle("QHBoxLayout test");
    window->show();
    return app.exec();
}
```
* `here is our output`

![this is our output](https://raw.githubusercontent.com/cheddad17/qtMohammedAliCheddad-ZakarieZaoui/main/img/hBox.PNG)
   
Nested Layouts
==============

we will tray to achieve this form using **Nested layouts** :


![](https://anassbelcaid.github.io/CS311/homeworks/04_forms/nesting.png)
 
  ### `nestedLayout.h:`
    
```cpp
class nestedLayout : public QWidget
{

public:
    //constructor
    explicit nestedLayout(QWidget *parent = nullptr);
    //destructor
    virtual ~nestedLayout();
protected:
  void createWidgets();
  void placeWidgets();
  void makeConnexions();

private:
  QLabel *label;
  QLineEdit *line;
  QPushButton *search;
  QPushButton *close;
  QCheckBox *match;
  QCheckBox *backword;
  QHBoxLayout *layoutP;
  QVBoxLayout *layoutS;
};
```
### `nestedLayout.cpp:`
```cpp
nestedLayout::nestedLayout(QWidget *parent) : QWidget(parent)
{
    createWidgets();
    placeWidgets();
    makeConnexions();
}

nestedLayout::~nestedLayout(){
delete search;
    delete close;
    delete label;
    delete match;
    delete backword;
    delete layoutP;
    delete layoutS;
    delete line;
}
void nestedLayout::createWidgets(){
    label = new QLabel("Name:");
    line = new QLineEdit();
    search = new QPushButton("Search");
    close = new QPushButton("Close");
    match = new QCheckBox("Match case");
    backword = new QCheckBox("Search Backward");

   layoutP = new QHBoxLayout();
   layoutS = new QVBoxLayout();
    }
void nestedLayout::placeWidgets(){
    auto mainLayout = new QHBoxLayout;
    auto leftLayout = new QVBoxLayout;
    auto topLeftLayout = new QHBoxLayout;
    auto RightLayout = new QVBoxLayout;

          setLayout(mainLayout);
          leftLayout->addLayout(topLeftLayout);
          leftLayout->addWidget(match);
          leftLayout->addWidget(backword);

          topLeftLayout->addWidget(label);
          topLeftLayout->addWidget(line);

          RightLayout->addWidget(search);
          RightLayout->addWidget(close);
          RightLayout->addStretch(1);

          mainLayout->addLayout(leftLayout);
          mainLayout->addLayout(RightLayout);
}

void nestedLayout::makeConnexions(){
//until next pc
}
```
### `main.cpp:`
```cpp
int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
    auto  window = new nestedLayout;
       window->setWindowTitle("Netsted layout test");
       window->show();
    return app.exec();
}
```
>*we also used the [QCheckBox](#https://doc.qt.io/qt-5/qcheckbox.html) Class and [addStretch](#https://doc.qt.io/qt-5/qboxlayout.html#addStretch) method.* 

 * `we get this output:`  
 

![enter image description here](https://raw.githubusercontent.com/cheddad17/qtMohammedAliCheddad-ZakarieZaoui/main/img/nestedLayout.PNG)


Bug report Form
===============

Now we will create a interface that allow us to report bugs.
  
  ### `bugReportForm.h:`
    
```cpp
class bugreport : public QWidget{
    Q_OBJECT
  public:
//constructor
    bugreport(QWidget *parent = nullptr);
    void createWidgets();
    void positionWidgets();

  private:
//initialization
    QLineEdit* nameEdit ;
    QLineEdit* companyEdit ;
    QLineEdit* phoneEdit ;
    QLineEdit* emailEdit ;
    QLineEdit* problemEdit ;
    QTextEdit* summaryEdit ;
    QComboBox* reproducibilityCombo;
    QDialogButtonBox* buttonBox;
};
```
### `bugReportForm.cpp:`
```cpp
void bugreport::createWidgets(){
//creating lineEdits
    nameEdit = new QLineEdit;
    companyEdit = new QLineEdit;
    phoneEdit = new QLineEdit;
    emailEdit = new QLineEdit;
    problemEdit = new QLineEdit;
    summaryEdit = new QTextEdit;
//creating the comboBox
    reproducibilityCombo = new QComboBox;
    reproducibilityCombo->addItem(tr("Always"));
    reproducibilityCombo->addItem(tr("Sometimes"));
    reproducibilityCombo->addItem(tr("Rarely"));
//creating button box
    buttonBox = new QDialogButtonBox;
    buttonBox->addButton(tr("Submit Bug Report"),
                         QDialogButtonBox::AcceptRole);
    buttonBox->addButton(tr("Don't Submit"),
                         QDialogButtonBox::RejectRole);
    buttonBox->addButton(QDialogButtonBox::Reset);
}
void bugreport::positionWidgets(){
//setting the form layout
    QFormLayout *layout = new QFormLayout;
            layout->addRow(tr("Name:"), nameEdit);
            layout->addRow(tr("Company:"), companyEdit);
            layout->addRow(tr("Phone:"), phoneEdit);
            layout->addRow(tr("Email:"), emailEdit);
            layout->addRow(tr("Problem Title:"), problemEdit);
            layout->addRow(tr("Summary Information:"),
                           summaryEdit);
            layout->addRow(tr("Reproducibility:"),
                           reproducibilityCombo);
//setting the main layout
            QVBoxLayout *mainLayout = new QVBoxLayout;
            mainLayout->addLayout(layout);
            mainLayout->addWidget(buttonBox);
            setLayout(mainLayout);
}
bugreport::bugreport(QWidget* parent):QWidget(parent){
//calling the methods
    createWidgets();
    positionWidgets();

                    setWindowTitle(tr("Report Bug"));
                }
```
### `main.cpp:`
```cpp

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    auto b =new bugreport;
    b->show();
    return a.exec();
}

```
 * `Our output:`  
 
 ![enter image description here](https://raw.githubusercontent.com/cheddad17/qtMohammedAliCheddad-ZakarieZaoui/main/img/bugReportForm.PNG)

Grid Layout
===========

now using  [QGridLayout](#https://doc.qt.io/qt-5.15/qgridlayout.html) lets create a Numeric Keypad.

  ### `calculator.h:`
    
```cpp
class calculator : public QWidget
{
public:
    //Constructor.
    explicit calculator(QWidget *parent = nullptr);
    //Methods
    void creatingWdgets();
    void positionWidgets();
    void makeConnections();
private:
	//Attributes initialization.
    QPushButton *buttons[10];
    QPushButton *bEnter;
    QLCDNumber *lcd;
    QVBoxLayout *mainLayout;
    QGridLayout *grid;
};
};
```
### `calculator.cpp:`
```cpp
    void calculator ::creatingWdgets(){
    	//creating the Pushbuttons
        for(int i=0;i<10;i++){
           QString s = QString::number(9-i);
            buttons[i]=new QPushButton(s);
        }
        bEnter =new QPushButton("enter");
        //creating the lcdNumber
        lcd = new QLCDNumber();
        lcd->setSegmentStyle(QLCDNumber::Flat);

}
void calculator :: positionWidgets(){
	 //creating the grid and the main layouts	
     mainLayout = new QVBoxLayout();
     grid = new QGridLayout();
     //positioning the componants and configuring the lcd 
     int k = 0;
     for(int i=1;i<4;i++){
         for(int j=0;j<3;j++){
          grid->addWidget(buttons[k],i,2-j);
          k++;
          lcd->setMinimumHeight(80);
          lcd->setDigitCount(6);
         }
     }
      //setting the layouts
      grid->addWidget(buttons[9],4,0);
      grid->addWidget(bEnter,4,1,1,2);
      mainLayout->addWidget(lcd);
      mainLayout->addLayout(grid);
      resize(300,300);
      setLayout(mainLayout);

}
//constructor
calculator::calculator(QWidget* parent):QWidget(parent)
{
creatingWdgets();
positionWidgets();
}
void calculator :: makeConnections(){
    //until next P.C
}
```
### `main.cpp:`
```cpp
int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    auto calc = new calculator();
    calc->show();
    return a.exec();
}
```
> *In this example we used [QLCDNumber](#https://doc.qt.io/qt-5/qlcdnumber.html) Class and some of its methods.* 
 * `then we get:`  
 
![enter image description here](https://raw.githubusercontent.com/cheddad17/qtMohammedAliCheddad-ZakarieZaoui/main/img/gridLayout.PNG)
# Cocnclusion
working on this exercises we learned that the Qt widgets and layout are very useful in a very wild scale.
