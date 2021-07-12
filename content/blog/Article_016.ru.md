---
title: Различные способы достижения цели в MS Power BI
date: 2021-07-10
image: images/blog/art_016_foto.jpg
author: Smile
---

> *Рвав-рвав, вот таким я был ребенком!*
>
> *Пока хозяин умиляется моим детским фотографиям, мы с вами поговорим о том, насколько универсальным инструментом является такой продукт как "MS Power BI".*
>
> *По-хорошему, эта статья должна была бы быть одной из первых публикаций, но раньше любознательный собаке об этом просто не задумывался, так как все происходит с опытом. Мое личное мнение состоит в том, что Power BI, особенно в связке с такими продуктами, как Power Apps и Power Automate является практически "серебряной пулей", впрочем, к делу.*


**Пример:**

**<li>** Допустим, имеется таблица, состоящая из одного столбца "Код валюты", в котором в качестве значений содержатся "USD" и "EUR":

![art_016_screen_1](https://kkadikin.ru/images/blog/art_016_screen_1.jpg)


**Задача:**

**<li>** Вывести в визуальный элемент текстовое описание, соответствующее указанному коду валюты, используя стандартный функционал Power BI.

> *Рвав-рвав, применительно к обозначенной задаче, я нашел целых 5 способов реализации, и не совсем уверен, что это действительно все способы, а потому буду рад вашим комментариям.*
>
> *Также хотелось бы предупредить, что опытные пользователи ничего нового для себя, скорее всего, не узнают, но вот остальным материал будет полезен, особенно в части создания пользовательской функции.*


**Уровень преобразования данных (Power Query):**

**<li>** Первое, что приходит на ум, находясь на уровне преобразования данных -- это создание условного столбца, которое происходит в диалоге, и является интуитивно понятным для конечного пользователя:

![art_016_screen_2](https://kkadikin.ru/images/blog/art_016_screen_2.jpg)

**<li>** Вторым вариантом на ум пришло создание настраиваемого столбца, действие практически аналогично указанному выше, но тут придется немного поработать руками:

![art_016_screen_3](https://kkadikin.ru/images/blog/art_016_screen_3.jpg)

> *Рвав-рвав, картинки прикладывать не буду, но с точки зрения кода Power Query два предыдущих шага полностью идентичны, так как у нас подобран такой пример), но сам по себе настраиваемый столбец имеет более широкий функционал.*

**<li>** Третьим вариантом является создание и последующий вызов так называемой пользовательской функции, при этом здесь придется работать и руками, и головой:

![art_016_screen_4](https://kkadikin.ru/images/blog/art_016_screen_4.jpg)

> *Рвав-рвав, открою вам маленький секрет, именно ради этого все и затевалось, а потому этот вариант мы рассмотрим подробнее.*

**<li>** Для создания функции обычно используется пустой запрос, который редактируется при помощи расширенного редактора и по завершении процесса написания кода самостоятельно трансформируется в функцию:

![art_016_screen_5](https://kkadikin.ru/images/blog/art_016_screen_5.jpg)

Также любой запрос можно трансформировать в пользовательскую функцию, используя контекстное меню, доступное при нажатии правой клавиши мыши.

**<li>** После того, как функция была написана и протестирована, необходимо осуществить ее вызов в базовом запросе, для этого используется кнопка "Вызвать настраиваемую функцию", которая находится на вкладке "Добавление столбца".

**<li>** При нажатии указанной кнопки появляется дополнительное окно, в котором нужно указать имя создаваемого столбца, а также имя используемой функции:

![art_016_screen_6](https://kkadikin.ru/images/blog/art_016_screen_6.jpg)

**<li>** После нажатия кнопки "ОК" в цепочке преобразований появится соответствующий шаг, и будет произведено то действие, которое вы описали в созданной ранее функции:

![art_016_screen_7](https://kkadikin.ru/images/blog/art_016_screen_7.jpg)

> *Рвав-рвав, на этом этапе любознательный собакен себя исчерпал, хотя можно было бы еще подумать над созданием столбца из примеров, но по факту это опять первые 2 случая, или написание кода с "чистого листа" в расширенном редакторе, а потому сейчас мы переместимся на уровень модели.*


**Уровень модели (Data Analysis eXpressions):**

**<li>** Здесь для решения задачи можно создать расчетный столбец, формула которого будет выглядеть следующим образом:

```dax
Расчетный столбец (DAX) = 
IF ( 'Таблица'[Код валюты] = "USD", "Доллар США", "Евро" )
```

**<li>** Ну и последнее, что было придумано -- это, конечно же, мера:

```dax
Мера (DAX) = 
MINX (
    VALUES ( 'Таблица'[Код валюты] ),
    IF ( 'Таблица'[Код валюты] = "USD", "Доллар США", "Евро" )
)
```

**<li>** В результате последовательного выполнения действий, описанных выше, мы сможем получить визуальный элемент следующего вида:

![art_016_screen_8](https://kkadikin.ru/images/blog/art_016_screen_8.jpg)


> *Рвав-рвав, вот так и живем, и по-хорошему, эти способы нужно комбинировать.*
>
> *И спасибо за внимание. А статья появилась просто-напросто потому, что заниматься прокрастинацией в плане реализации преобразования данных при помощи пользовательских функций хозяину просто-напросто надоело...*
>
> *Ваш Смайл*