---
title: Сломанная иерархия
date: 2019-12-18
image: images/blog/art_010_foto.jpg
author: Smile
---

> *Рвав-рвав, здесь и сейчас с вами собака Смайл!*
>
> *Для начала немного о том, почему я грустный:* 
>
> *Санкт-Петебрург, середина декабря (!) - дождь, грязюка, холоднюка.*
>
> *Приходится носить всякую фигню. Сопротивление бесполезно. Надеюсь, вы тоже мучаетесь, так как вместе - веселее.*
>
> *Сегодня хотелось бы описать работу с так называемой "сломанной" иерархией данных, когда уровни иерархии / подчинения не структурированы.*


**Пример:**

В качестве исходных данных поступила информация следующего вида: 

-- основной набор, содержащий ID пользователя, номер документа и сумму;

-- справочник, содержащий информацию о структуре подчинения между сотрудниками.

![art_010_screen_1](https://kkadikin.ru/images/blog/art_010_screen_1.jpg)


**Задача:**

На основе представленных данных реализовать следующие требования:

-- построить иерархию данных таким образом, чтобы у каждого сотрудника было одинаковое количество уровней иерархии;

-- убрать повторяющиеся значения на разных уровнях иерархии, то есть, произвести ее "схлопывание".


**Структурирование иерархии:**

**<li>** В целях устранения хаоса в исходных данных для начала нам необходимо понять, является ли конкретное значение конечным элементом иерархии. В этом нам может помочь столбец "End Link", в принципе, делать его необязательно, но в случае большого объема данных он сможет помочь при проверке конечного результата:

![art_010_screen_2](https://kkadikin.ru/images/blog/art_010_screen_2.jpg)

Формула указанного столбца выглядит следующим образом:

```dax
End Link = 
CALCULATE (
    COUNTROWS ( 'Staff Hierarhy' );
    ALL ( 'Staff Hierarhy' );
    'Staff Hierarhy'[Parent Key] = EARLIER ( 'Staff Hierarhy'[User ID] )
) = 0
```

**<li>** Следующим шагом является построение цепочки родительских значений, для последующего определения уровня иерархии. В этом нам поможет столбец "Hierarchy View", рассчитанный при помощи функции "PATH":

![art_010_screen_3](https://kkadikin.ru/images/blog/art_010_screen_3.jpg)

Формула указанного столбца рассчитывается так:

```dax
Hierarchy View = 
PATH ( 'Staff Hierarhy'[User ID]; 'Staff Hierarhy'[Parent Key] )
```

**<li>** Подсчет уровня иерархии, проиллюстрированнный в столбце "Hierarchy Level", можно произвести при помощи функции "PATHLENGHT":

![art_010_screen_4](https://kkadikin.ru/images/blog/art_010_screen_4.jpg)

Формула указанного столбца выглядит следующим образом:

```dax
Hierarchy Level = 
PATHLENGTH ( 'Staff Hierarhy'[Hierarchy View] )
```

**<li>** Выполнив указанные шаги, можно приступать непосредственно к построению уровней иерархии при помощи функции "PATHITEM":

![art_010_screen_5](https://kkadikin.ru/images/blog/art_010_screen_5.jpg)

Формула столбца для 1-го уровня иерархии - "1st level" представляет собой следующую конструкцию:

```dax
1st level = 
LOOKUPVALUE (
    'Staff Hierarhy'[Name];
    'Staff Hierarhy'[User ID]; PATHITEM ( 'Staff Hierarhy'[Hierarchy View]; 1; INTEGER )
)
```

Формула столбца для 2-го уровня иерархии - "2nd level" представляет собой следующую конструкцию:

```dax
2nd level = 
IF (
    'Staff Hierarhy'[Hierarchy Level] >= 2;
    LOOKUPVALUE (
        'Staff Hierarhy'[Name];
        'Staff Hierarhy'[User ID]; PATHITEM ( 'Staff Hierarhy'[Hierarchy View]; 2; INTEGER )
    );
    'Staff Hierarhy'[1st level]
)
```

Формула столбца для 3-го уровня иерархии - "3rd level" представляет собой следующую конструкцию:

```dax
3rd level = 
IF (
    'Staff Hierarhy'[Hierarchy Level] >= 3;
    LOOKUPVALUE (
        'Staff Hierarhy'[Name];
        'Staff Hierarhy'[User ID]; PATHITEM ( 'Staff Hierarhy'[Hierarchy View]; 3; INTEGER )
    );
    'Staff Hierarhy'[2nd level]
)
```





> *Рвав-рвав, до новых встреч!*
>
> *Ваш Смайл*