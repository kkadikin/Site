---
title: Параметры на уровне Power Query
date: 2019-08-29
image: images/blog/art_005_foto.jpg
author: Smile
---

> *Рвав-рвав, здесь снова собака Смайл!*
>
> *Данная статья появилась по 2-м причинам:*
>
> **<li>** *Появился запрос пользователей на этот счет вида: "С чем это едят?".*
>
> **<li>** *Шустрый собака, в качестве помощи, решил по-быстрому найти ссылку с ответом на этот вопрос, но ничего толкового нагуглить по этому поводу в короткое время не сумел.* 
>
> *В результате пришлось на мобиле кратенько описывать данный вопрос, аж бесит (!), и где-то в планах появилось сделать собственный материал. Вся эта ситуация еще раз подтвердила справедливоть моего высказывания в статье [Создание календаря при помощи DAX](https://kkadikin.ru/ru/blog/article_001/) на тему того, что "Правильно гуглить -- тоже труд...", поэтому -- начинаем.*

Для построения примера, иллюстрирующего обозначенный функционал, нам необходим исходный набор данных любого вида, например, такой:

![art_005_screen_01](https://kkadikin.ru/images/blog/art_005_screen_1.jpg)


**Пример:**

Визуальный элемент "Список проектов" -- cодержит информацию по проекту, а именно -- руководителя проекта и дату начала проекта.


**Задача:**

Продемонстрировать применение параметров различного типа в качестве инструмента фильтрации данных.


**Параметры бывают следующих типов:** 

**<li>** Любое значение -- функционал будет продемонстрирован на примере столбца "Название проекта".

**<li>** Список значений -- функционал будет продемонстрирован на примере столбца "Дата начала проекта".

**<li>** Запрос -- функционал будет продемонстрирован на примере столбца "Руководитель проекта".


**Процесс разработки:**

**<li>** Создать таблицу "Dataset" -- это таблица, содержащая входящие данные, а именно столбцы "Руководитель проекта", "Название проекта" и "Дата начала проекта".

> *Как вы уже, наверно, догадались, никаких дополнительных действий (например, расчетов, построения связей), нам совершать не надо.*

**<li>** Создание параметров производится на уровне Power Query, с уровня DAX  на него можно перейти при помощи кнопки "Edit Queries" ("Изменить запросы"), расположенной на закладке "Home" ("Главная").

**<li>** Затем следует перейти в раздел управления параметрами, данное действие можно осуществить при поможи одноименной кнопки "Manage Parameters" ("Управление параметрами"), расположенной на закладке "Home" ("Главная"), при этом откроется пустая форма создания  параметра, которую необходимо заполнить определенным образом:

![art_005_screen_02](https://kkadikin.ru/images/blog/art_005_screen_2.jpg)

**<li>** За создание параметров отвечает кнопка "New" ("Создать").

**<li>** Параметр с типом "Любое значение" можно настроить следующим образом:

-- Name / Имя = "Проект" (задается произвольно, это имя параметра)

-- Description / Описание  = "Название проекта" (задается произвольно, в дальнейшем выводится в качестве подсказки пользователю)

-- Required / Требуется  = "Нет" (регулируется при помощи соответствующего флага, смысл установки которого -- указать обязательность заполнения реквизита)

-- Type / Тип  = "Текст" (указание ограничения на определенный тип данных в реквизите, что может быть полезно, например, в случае, когда в качестве параметра нужно использовать любое значение, так как вариантов много, но определенного типа, для того, чтобы избежать ошибочного ввода данных со стороны пользователя)

-- Suggested Values / Предлагаемые значения = "Любое значение" (тип параметра)

-- Current Value / Текущее значение = "Нет" (текущее значение праметра, может быть использовано в качестве значения по умолчанию)

![art_005_screen_03](https://kkadikin.ru/images/blog/art_005_screen_3.jpg)

**<li>** Для сохранения параметра необходимо нажать кнопку "ОК", после чего в панели "Queries" ("Запросы"), расположенной с левой стороны, появится созданный параметр.

![art_005_screen_04](https://kkadikin.ru/images/blog/art_005_screen_4.jpg)

**<li>** Параметр с типом "Список значений" можно настроить следующим образом:

-- Name / Имя = "Дата"

-- Description / Описание  = "Начало проекта"

-- Required / Требуется  = "Да"

-- Type / Тип  = "Дата"

-- Suggested Values / Предлагаемые значения = "Список значений", содержащий "01.01.2019" и "01.02.2019"

-- Default Value / Значение по умолчанию = "01.01.2019" (значение по умолчанию выбиратся из внесенного списка)

-- Current Value / Текущее значение = "01.01.2019" (данное значение также может отличаться от значения по умолчанию)

![art_005_screen_05](https://kkadikin.ru/images/blog/art_005_screen_5.jpg)

**<li>** Для сохранения параметра необходимо нажать кнопку "ОК".

**<li>** До того, как приступить к настройке параметра с типом "Запрос", необходимо пердварительно создать объекты подобного типа в системе. Создание запроса можно осуществать, например, воспользовавшись контекстным меню в столбце таблицы путем выбора действия "Add As New Query"("Сохранить как отдельный запрос"). Ниже представлен запрос "Менеджеры", содержащий список руководителей проекта.

![art_005_screen_06](https://kkadikin.ru/images/blog/art_005_screen_6.jpg)

**<li>** Параметр с типом "Запрос" можно настроить следующим образом:

-- Name / Имя = "Ответственный"

-- Description / Описание  = "Руководитель проекта"

-- Required / Требуется  = "Нет"

-- Type / Тип  = "Текст"

-- Suggested Values / Предлагаемые значения = "Запрос"

-- Query /  Запрос = "Менеджеры" (значение выбирается из списка существующих запросов)

-- Current Value / Текущее значение = "Нет"

![art_005_screen_07](https://kkadikin.ru/images/blog/art_005_screen_7.jpg)

**<li>** Для использования созданных параметров в качестве фильтра их необходимо привязать к соответствующим столбцам данных. Привязка осуществляется путем наложения фильтра на столбец с данными. Возможности фильтрации зависят от типа данных столбца.

![art_005_screen_08](https://kkadikin.ru/images/blog/art_005_screen_8.jpg)

**<li>** После выбора нужного действия фильтрации, в качестве опции значения нужно выбрать параметр, и укзать его название. Окно настройки фильтрации данных представлено ниже:

![art_005_screen_09](https://kkadikin.ru/images/blog/art_005_screen_9.jpg)

**<li>** После настройки фильтра необходимо нажать кнопку "ОК".

> *Рвав-рвав, важно понимать, что поскольку параметр накладывает фильтр на набор данных, то с каждым следующим параметром вы можете так уменьшить конечную выборку,что в итоге получится пустой набор значений. Поэтому для целей демонстации функционала далее будет показан пример применения  одного из параметров -- "Ответственный".* 

**<li>** После привязки параметра к столбцу данных необходимо применить все изменения при помощи кнопки "Close & Apply" ("Закрыть и применить").


**Использование функционала:**

**<li>** Изменение значения параметра осуществляется по кнопке "Edit Queries" ("Изменить запросы") -> "Edit Parameters" ("Изменить параметры"):

![art_005_screen_10](https://kkadikin.ru/images/blog/art_005_screen_10.jpg)

**<li>** После ввода в параметр значения, отличного от текущего, в окне на холсте страницы в верней ее части появится полоса с кнопкой "Apply changes" ("Применить изменения").

![art_005_screen_11](https://kkadikin.ru/images/blog/art_005_screen_11.jpg)

**<li>** В результате применения изменений, укаазанных пользователем через параметр, к базовому набору значений будет применен настроенный фильтр.

![art_005_screen_12](https://kkadikin.ru/images/blog/art_005_screen_12.jpg)


**Возможное применение:**

**<li>** Параметры можно использовать в качестве нежесткого средства ограничения выборки данных, устанавливаемом пользователем самостоятельно. Это может понадобиться, если объем данных, подгружаемых при обновлении, достаточно большой, и при этом не все они нужны для текщего анализа. В Power BI Desktop отсутствует так называемое "инкрементное обновление", поэтому процесс полного обновления данных может занимать продолжительное время.  Уменьшение объема загружаемых данных как раз можно осуществить, используя систему параметров, описанную выше.


> *Рвав-рвав, статья подошла к концу.*
>
> *Уморившаяся собака Смайл пошел дрыхнуть, оставив задние лапы...*
>
> *Ваш Смайл*

**ДЛЯ ЛЮБИТЕЛЕЙ ПОНАЖИМАТЬ НА КНОПОЧКИ**

Увы и ах, тут нажимать нечего, а по этому -- сами знаете, что :-)