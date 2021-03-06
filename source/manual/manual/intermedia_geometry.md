## Урок 8: Продвинутая геометрия

### Описание геометрии с помощью внешнего источника

Описание геометрии эксперимента в виде с++ кода может быть не удобно по нескольким причинам. Основной можно считать тот факт, что приходится делать двойную работу: описывать геометрию эксперимента в инженерных чертежах, и снова описывать геометрию в симуляции   
Хорошо было бы иметь возможность экспортировать




### Неоднородное поле

Для того, чтобы определить поле с собственной конфигурацией, пользователь должен унаследовать абстрактный класс `G4Field` (или его потомка) и реализовать метод `GetFieldValue`.  Метод принимает на вход два массива - один содержит 4 числа, координаты точки и момент времени, для которых нужно рассчитать значение поля. Во второй массив внутри метода должен быть записан набор значений, задающих поле в заданной точке.

В качестве примера расмотрим создание неоднородного электрического поля. 
В заголовочном файле мы определим наш класс `NonUnoformField`, наследуясь от класса `G4ElectricField` - абстрактного класса электрического поля. В конструкторе класса мы установим параметры неоднородности.
```
include "G4ElectricField.hh"

class NonUniformField : public G4ElectricField {
public:
    NonUniformField(double fieldValue, double curvature, double zmax): fieldValue(fieldValue), curvature(curvature),  zmax(zmax){};
    virtual void  GetFieldValue( const G4double Point[4],
                                 G4double *Bfield ) const;
private:
    double fieldValue;
    double curvature;
    double zmax;
};

```
Расмотрим реализацию метода `GetFieldValue`. Поскольку электрическое поле это частный случай электромагнитного поля, то массиву в который мы записываем вычисленное значение полей, соответсвует следующая легенда: первые три значения будут восприниматься как компоненты вектора магнитной индукции, а следующие три как компонеты вектора напряжености электрического поля. Поскольку мы реализуем чисто электрическое поле мы обнуляем первые три компонета массива. Следует отметить что с точки зрения GEANT4 электрическое поле отличается от магнтного возможностью совершать работу, это отличие фиксируется с помощью логической переменной в классе `G4ElectricField`.
```
#include "NonUniformField.hh"
#include "G4SystemOfUnits.hh"
#include <cmath>
using namespace std;
using namespace CLHEP;

void NonUniformField::GetFieldValue(const G4double Point[4], G4double *Bfield) const {
Bfield[0] = 0.0;
Bfield[1] = 0.0;
Bfield[2] = 0.0;
double z = Point[2];

    //hyperbolic filed
//    double b = zmax; //z max
//    double a = curvature*b*b;
//    double k = (b/a)*sqrt(1 + (b*b)/(z*z)); // tangent coefficient
    // circle filed
    double k = sqrt((1/(z*z*curvature*curvature)) - 1);


    double Ez = fieldValue*sqrt(k/(1+k));
    double Ex = fieldValue*sqrt(1/(1+k));
    if (z>zmax){
         Ex = -1*Ex;
    }
    Bfield[3] = Ex;
    Bfield[4] = 0;
    Bfield[5] = Ez;
}
```



### Детекторы

GEANT4 включаетя в себя несколько шаблонных детекторов

