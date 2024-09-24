---
marp: true
theme: uncover
paginate: true
---

<style>
    /* Theme modifying */

    section {
        justify-content: start;
        background-color: #0f1124;
        color: white;
        background-size: contain;
        background-repeat: no-repeat;
    }

    section::after {  
        color: white;
    }

    section > pre[is=marp-pre] {
        margin: 0 !important;
    }

    ul {
        margin: 0;
    }

    p,
    li {
        text-align: justify;
        text-justify: inter-word;
    }

    ul {
        list-style-image: url("assets/list-pointer.svg");
        list-style-position: outside;
    }

    ul li {
        padding-left: 20px;
    }

    img {
        border-radius: 40px;
        border: 2px solid #ffffff40;
        padding: 10px;
        box-shadow: -12px 16px 0 0 #ffffff40;
    }

    code {
        border-radius: 40px;
        background-color: #ffffff13 !important;
        color: white !important;
    }

    .hljs-comment {
        color: #ff0037 !important;
    }

    section > pre {
        margin-bottom: 1rem !important;
    }

    img.emoji {
        border-radius: 0;
        border: none;
        box-shadow: none;
    }

    h1 {
        border-bottom: 2px solid #ff0037;
        padding-bottom: 8px;
        width: fit-content;
        white-space: nowrap;
        font-size: 1.5rem;
    }

    h1,
    h2,
    h3,
    h4 {
        text-align: start;
    }

    p+p {
        margin-top: 0.05rem;
    }

    li+li {
        margin-top: 0.05rem;
    }

    section :is(pre,marp-pre)::part(auto-scaling) {
        max-height: 650px !important;
    }

    iframe {
        color: transparent !important;
    }

    /* Slides modifying */

    section.only-title {
        justify-content: center;
        align-items: center;
    }

    section.only-title::before {
        content: "";
        background-size: cover; 
        background-repeat: no-repeat;
        background-position: center;
        opacity: 0.2;
        position: absolute;
        top: 0px;
        right: 0px;
        bottom: 0px;
        left: 0px;
    }

    section.only-title :is(h1, h2, h3, h4) {
        text-align: center;
        font-size: 2rem;
    }

    section.title-with-image p {
        margin: auto;
    }

    section.only-code {
        justify-content: center
    }

    /* Other */

    .bordered-text {
        border: 3px solid white;
        border-radius: 80px;
        width: fit-content;
        padding: 25px;
        padding-right: 45px;
        padding-bottom: 35px;
        background-color: #0f1124;
    }

    .highlighted-title {
        background-color: #ff0037;
        width: fit-content;
        border-radius: 20px;
        padding-left: 30px;
        padding-right: 50px;
    }

    .hollow-text {
        color: transparent;
        -webkit-text-stroke: 2px #FFFFFF;
        text-stroke: 1px #FFFFFF;
    }

    .definition {
        background: #ff0037d0;
        border-radius: 30px;
        padding-left: 34px;
        padding-right: 14px;
        padding-bottom: 9px;
        box-shadow: -8px 4px 0 0 white;
    }

    .definition-use {
        border: 2px solid #ff0037;
        border-radius: 30px;
        padding-left: 34px;
        padding-right: 14px;
        padding-bottom: 9px;
        box-shadow: -8px 4px 0 0 white;
    }
</style>

<style scoped>
    h1 {
        margin-top: 2rem;
        font-size: 1.6rem;
        margin-bottom: 0;
        z-index: 10;
    }
    h2 {
        margin-bottom: auto;
        font-size: 1.5rem;
        font-weight: 400;
        z-index: 10;
    }
    p {
        font-size: 0.8rem;
        margin-bottom: 0.7rem;
    }
    span.after-1 {
        font-size: 0.5rem;
        margin-left: 0.6rem;
    }
    div.rect1-1{
        position: absolute;
        background-color: #ff0037;
        top: -700px;
        right: -700px;
        width: 1000px; 
        height: 1000px;
        rotate: -35grad;
        z-index: 0;
    }
</style>

<div class="rect1-1"></div>

<h1 class="highlighted-title"> Конечные автоматы </h1>

## как универсальный язык описания бизнес-логики

<span class="definition-use"> Холин Владимир </span> <span class="after-1"> Intaro, PHP Программист </span>


---

# О чем будем говорить

* Пофилософствуем
* Поймем, что из себя в принципе представляют автоматы
* Выделим понятия, полезные в практике
* Подробно разберем несколько примеров автоматов
* Разберемся, как реализовать свой автомат
* Исследуем существующие решения для аналитики, бэка и фронта

---

<!-- _class: only-title -->

<style scoped>
    section::before {
        background-image: url("assets/илон.jpg");
        opacity: 0.15;
    }
</style>

# Подумать подумать

---

<!-- _class: only-title -->

### Что такое "состояние" по вашему?

---

<style scoped>
    ul {
        padding: 0
    }

    li {
        list-style: none;
    }

    section li:last-child {
        text-align: center;
        margin-top: 2.8rem;
        font-size: 1.4rem
    }

    p {
        line-height: 1.4rem;
    }
</style>

# Постоянство и изменение

* > <span class="definition">Состояние</span> - "отпечаток" системы в конкретный момент времени
* > <span class="definition">Переход</span> - изменение состояния с одного на другое
* **Согласны?** :smirk_cat:

<!-- В рамках любой системы можно выделить две концепции: постоянство и изменение.

Со стороны постоянства системы основным определением выделяется **состояние** как "отпечаток" системы в конкретный момент времени. 
Со стороны изменения системы основным понятием будет **переход** из одного состояния в другое., то есть изменение ее параметров.

Напрашивается вывод что это фундаментальное описание системы. -->

<!-- TODO -->

---

<!-- _class: title-with-image -->

<style scoped>
    h1 {
        font-size: 1rem;
    }
    img {
        height: 13.5rem;
        padding: 0;
    }
</style>

# Программист на работе

![](assets/programmer.png)

<!-- При этом у системы может быть очень различных параметров, но мы можем абстрагироваться от их конкретных значений и выделить "логические состояния" системы. 

Если взглянуть с математической точки зрения, то можно представить состояние системы как вектор в многомерном пространстве параметров, а  "логическое" состояние как некую область в этом пространстве. 

Согласитесь, будет удобно, если система будет иметь ограниченное количество "логических" состояний. Тогда и количество возможных переходов между ними будет конечным. В этом и есть смысл конечных автоматов. Они содержат описание всех таких логических состоний и переходов между ними. -->

<!-- Состояние, Кружек кофе, Часов сна
(1,0,Все плохо),
(3,1,Может работает, а может и нет),
(6,2,Работает),
(8,1,Уже закрыл задачу),
(4,7,Вызвали скорую),
(8,5,Открыл стартап) -->

---

<!-- _class: only-title -->

<style scoped>
    section::before {
        background-image: url("assets/good.webp");
        opacity: 0.15;
    }
</style>

# Уровень понимания: <span class="hollow-text"> junior </span> 

---

<style scoped>
    section {
        align-items: center;
    }

    p {
        margin: auto;
    }

    img {
        height: 13rem;
    }
</style>

![](assets/firefox_ESUHfl6rzT.png)

---

<style scoped>
    section {
        align-items: center;
    }

    p {
        margin: auto;
    }

    img {
        height: 14rem;
    }
</style>

![](assets/Tt7.webp)

---

<!-- _class: only-title -->

<style scoped>
    section::before {
        background-image: url("assets/otkryvaem-1024x512.jpg");
        opacity: 0.1;
    }
</style>

# Немножко матана

---

<!-- _class: title-with-image -->

<style scoped>
    img {
        height: 10rem;
    }
</style>

# Откуда всё пошло

![](assets/300px-Aa_mili_ex1.png)

<!-- **Абстрактный автомат** - математическая абстракция, модель дискретного устройства, имеющего один вход, один выход и в каждый момент времени находящегося в одном состоянии из множества возможных. На вход этому устройству поступают символы одного алфавита, на выходе оно выдаёт символы (в общем случае) другого алфавита.

Под символами можно воспринимать некоторые абстрактные обозначения наборов данных, а под алфавитом - все возможные такие наборы данных.

Абстрактный автомат можно описать по разному, но в общем функционирует так:
- Имеет начальное состояние (а1 на рисунке)
- Принимает входные сигналы z(t)
- Осуществляет переход в новое состояние a(t+1) на основе функции переходов (дельта)
- Выдает выходные сигналы w(t) на основе функции выходов (лямбда)
- Новое состояние a(t+1) может быть конечным (не имеет исходящих стрелочек). На рисунке нет конечных состояний -->

---

<style scoped>
    ul {
        padding: 0
    }

    section > ul > li {
        list-style: none;
    }

    p {
        line-height: 1.4rem;
    }
</style>

# Откуда всё пошло

* > Автомат <span class="definition">конечный</span>, если у него конечное число состояний.

<!-- Конечный автомат может быть:

- **Автомат Мили** - выходные значения также зависят от входных (нужна дополнительная табличка для описания).

- **Автомат Мура** - частный случай автомата Мили, где выходные значения зависят только от внутреннего состояния. -->

* > Автомат <span class="definition">детерминированный</span>, если:
    > - мы знаем, какое будет следующее состояние в зависимости от текущего и входных данных
    > - выход зависит только от текущего состояния и текущего входа

<!-- Автомат детерминированный, если мы можем точно определить следующее и его выход -->

---

<style scoped>
    p {
        line-height: 1.4rem;
    }
</style>

# Откуда всё пошло

> <span class="definition">Автоматное программирование</span> - парадигма программирования, при использовании которой программа или её фрагмент осмысливается как модель какого-либо формального автомата.

---

<style scoped>
    h1 {
        font-size: 1.4rem;
    }
</style>

# "Что мне дадут конечные автоматы?"

* Интуитивность и наглядность
* Простоту визуализации
* Декларативное описание
* Четкое разделение логики
* Удобство тестирования и поддержки
* Моделирование параллельных процессов

<!-- - Многие бизнес-процессы и пользовательские сценарии можно легко представить в виде состояний и переходов между ними. Это делает конечные автоматы интуитивно понятными для разработчиков, аналитиков и заказчиков
- Конкретные конечные автоматы легко визуализируются и, соответственно, легко понимаются
- Есть возможность декларативно описать состояния и переходы между ними. Возможность обеспечить тесную связь документации и реализации
- Происходит четкое разделение логики между обработчиками состояний
- Легко тестировать и поддерживать: заранее известны все возможные состояния и переходы между ними
- Хорошо подходят для моделирования параллельных процессов -->

---

<!-- _class: only-title -->

<style scoped>
    section::before {
        background-image: url("assets/norm.jpg");
        opacity: 0.15;
    }
</style>

# Уровень понимания: <span class="hollow-text"> middle </span>

---

<!-- _class: title-with-image -->

<style scoped>
    h1 {
        font-size: 1rem;
    }
    img {
        height: 11rem;
    }
</style>

# Пример: дверь как автомат

![](assets/firefox_7b5enEaevi.png)

<!-- - тут можно обойтись без состояния "без замка" и раскрыть его, но так мы явно указываем, что закрытая и открытая дверь будет именно "без замка"
- детерминированный

***Какие еще состояния или переходы вы бы добавили в автомат двери с замком?*** -->

---

<!-- _class: title-with-image -->

<style scoped>
    h1 {
        font-size: 1rem;
        margin-bottom: 0.3rem;
    }

    img {
        height: 14rem;
    }
</style>

# Пример: дверь как автомат

![](assets/mermaid-door.svg)

---

<!-- _class: title-with-image -->

<style scoped>
    h1 {
        font-size: 1rem;
    }
    img {
        height: 13rem;
    }
</style>

# Пример: переменная как автомат

![](assets/firefox_jmBIy3xpku.png)

<!-- даже работу с ней можно описать как state machine

- Начальное состояние - определена
- Входные данные - новое значение переменной или ее удаление
- Выходные данные - значение переменной или какая-либо ошибка (нет значения или не определена) 

- конечный, т.к. кол-во состояний конечно, не смотря на то, что переменная может иметь очень много разных значений. В смысле данного автомата важно только, установлено значение или нет, а значение переменной хранится в контексте. Само значение имеет смысл уже при использовании переменной где-либо

***Детерминированный или нет?***
- недетерминированный, т.к. при входе эвента "Получить значение" в состоянии "Содержит значение" выход зависит не только от входа и текущего состояния, а также от значения value в контексте-->

---

<style scoped>
    table {
        font-size: 0.7rem;
    }

    h1 {
        font-size: 1rem;
    }
</style>

# Пример: переменная как автомат

| Текущее состояние         | Эвент              | Вход     | Новое состояние   | Вывод |
| ----------------- | ------------------ | -------- | ----------------- | ----- |
| *Определена*        | Присвоить значение | newValue | Содержит значение | value |
| *Определена*        | Удалить            |          | Не определена     |
| Содержит значение | Присвоить значение | newValue | Содержит значение | value |
| Содержит значение | Получить значение  |          | Содержит значение | value |
| Содержит значение | Удалить            |          | Не определена     |

<!-- Нам в общем случае нужна некоторая логика при получении эвента и при переходе в состояние -->

---

<style scoped>
    h1 {
        font-size: 1rem;
    }
    img {
        height: 12rem;
    }
</style>

# Пример: автомат как автомат

![](assets/firefox_CqchKaPGZp.png)

---

<style scoped>
    p {
        font-size: 0.8rem;
        line-height: 1.6rem;
    }
    ul {
        padding: 0
    }
    li {
        list-style: none;
    }
</style>

# Итак

* > <span class="definition">Конечный автомат</span> (finite-state machine) - содержит <span class="definition-use">обработчики состояний</span>, которые производят <span class="definition-use">переходы</span> между конечным набором определенных состояний в ответ на <span class="definition-use">эвенты</span> (события) в зависимости от <span class="definition-use">контекста</span> автомата и <span class="definition-use">входа</span> (входных данных) и, возможно, изменяют контекст автомата и\или возвращают <span class="definition-use">вывод</span> (выходные данные).

---

<style scoped>
   p {
        font-size: 0.7rem;
        line-height: 1.4rem;
    }

    /* p+p {
        margin-top: 0.03rem;
    } */

    ul {
        padding: 0
    }
    li {
        list-style: none;
    }
</style>

* > <span class="definition">Контекст</span> - множество переменных, представляющих собой "память" автомата.

* > <span class="definition">Эвент</span> - сигнал к переходу в следующее состояние.

* > <span class="definition">Обработчик эвента</span> - решает, какому обработчику состояния отдать входные данные.

* > <span class="definition">Вход</span> - множество значений, поступающих в обработчик состояния.

* > <span class="definition">Обработчик состояния</span> - логическая единица обработки конкретного состояния.

* > <span class="definition">Переход</span> - изменение состояния (возможно, на то же самое).

<!-- TODO ? Диаграмма процесса разработки автомата -->

---

<!-- _class: only-title -->

<style scoped>
    section::before {
        background-image: url("assets/bad.webp");
        opacity: 0.15;
    }
</style>

# Уровень понимания: <span class="hollow-text"> senior </span>

---

<style scoped>
    p {
        font-size: 0.9rem;
    }
</style>

# Бот в Телеграм

* **Начальное состояние** - начало диалога, когда боту еще не отправлено ни одного сообщения

* **Эвент** - от Телеграм пришло обновление (создание, обновление или удаление сообщения, нажатие на inline-кнопку и т.д.)

* **Входные данные** - содержимое обновления

* **Выходные данные** - HTTP-запросы к Телеграм

* **Состояния и тд.** - зависят от логики бота

---

<!-- _class: title-with-image -->

<style scoped>
    img {
        height: 10rem;
    }
</style>

# Примеры inline-клавиатур

![Примеры inline-клавиатур](assets/firefox_elHfSMgvl4.png)

---

<!-- _class: title-with-image -->

<style scoped>
    h1 {
        font-size: 0.5rem;
        margin-bottom: 0.2rem;
    }

    img {
        height: 14.5rem;
    }
</style>

# Обобщенная диаграмма последовательности обработки

![](assets/mermaid-general-seq-diag.svg)

---

<!-- _class: title-with-image -->

<style scoped>
    h1 {
        font-size: 0.5rem;
        margin-bottom: 0.2rem;
    }

    img {
        height: 14.5rem;
    }
</style>

# Теперь наш случай с ботом

![](assets/mermaid-telegram.svg)

---

<!-- _class: only-title -->

<style scoped>
    section::before {
        background-image: url("assets/150706231113.jpg");
        opacity: 0.15;
    }
</style>

# Имплементируем

---

<style scoped>
    p {
        margin: auto;
    }

    img {
        padding: 20px;
    }
</style>

# Визуализация

[![](assets/stately-logo.svg)](https://stately.ai)

Инструмент для моделирования и визуализации логики приложения в виде автоматов и диаграмм состояний.

---

<!-- _class: title-with-image -->

<style scoped>
    img {
        width: 20rem;
    }
</style>

# Режим дизайна в Stately

![](assets/studio-dm.png)

<!-- режим симуляциии увидим дальше -->

---

<style scoped>
    h1 {
        font-size: 0.8rem;
        margin-bottom: 0.2rem;
    }
    section :is(pre,marp-pre)::part(auto-scaling) {
        max-height: 550px !important;
    }
</style>

# Как точно делать не надо (boolean explosion)

```javascript
if (isCreated && isEditable && isValid && !isDeleted) {...}

// Или

if (isCreated) {
    ...
    if (isEditable) {
        ...
        if (isValid) {
            ...
        } else {
            ...
        }
    } else {
        ...
    }
} else {
    ...
}
```

<!-- Можно описать конкретные состояния логическими условиями на основе флагов (**флаговое программирование**), а их обработку внутри if-else блоков, но со временем таких флагов будет становиться все больше (**Boolean explosion**)

Если у нас N флагов, то возможных обработчиков может быть 2^N.

К тому не все возможные комбинации таких флагов будут соответствовать реальным состояниям. -->

---

<style scoped>
    h1 {
        font-size: 1rem;
    }
</style>

# Имплементируем на функциях и почему

<!-- Можно использовать для реализации конкретных небольших задач (обработка строки и тд.). -->

| Понятие              | Реализация                        |
| -------------------- | --------------------------------- |
| Автомат              | Функция                           |
| Контекст             | Переменные, объявленные в функции |
| Обработчик состояния | Блок кода внутри функции          |

<!-- Блок кода внутри функции, в который перешел поток выполнения в зависимости от названия состояния -->

---

<!-- _class: only-code -->

<!-- Пример программы в автоматном стиле: -->

```c
int main() { // <-- Автомат
    enum states { 
        before, inside, after // <-- Описание состояний
    } state; // <-- Хранит идентификатор состояния
    state = before; // <-- Начальное состояние

    // Контекста нет :(

    int c; // <-- Используется для записи входа в обработчик состояния
    while ((c = getchar()) != EOF) { // <-- Формируем вход и бросаем эвент
        switch (state) { // <-- Обработчик эвента
            case before:
                if (c != ' ') { // <-- Обработчик состояния
                    putchar(c);
                    state = inside; // <-- Переход
                }
                break;
            case inside: 
                switch (c) { // <-- Вложенное состояние
                    case ' ':
                        state = after;
                        break;
                    case '\n':
                        putchar('\n');
                        state = before;
                        break;
                    default: putchar(c);
                }
                break;
            case after:
                // ...
        }
    }
  
    return 0;
}
```

---

<!-- _class: only-title -->

<style scoped>
    section::before {
        background-image: url("assets/1455004988192498999.jpg");
        opacity: 0.1;
    }
</style>

## Имплементируем на классах и почему

<!-- Этот вариант является уже архитектурным стилем и подойдет для больших и развивающихся проектов. -->

---

<!-- _class: title-with-image -->

<style scoped>
    img {
        height: 8rem;
    }

    p {
        margin-top: 0;
        margin-bottom: 0;
    }

    p > img {
        margin-top: 0.5rem;
    }
</style>

# Как всегда паттерны

Паттерн "Состояние"

![](assets/W3sDesign_State_Design_Pattern_UML.jpg)

<!-- Участники:

- State: определяет интерфейс состояния
- Классы State1 и State2 - конкретные реализации состояний
- Context: представляет объект, поведение которого должно динамически изменяться в соответствии с состоянием. Выполнение же конкретных действий делегируется объекту состояния -->

---

<style scoped>
    section {
        align-items: center;
    }

    p {
        margin: auto;
    }

    img {
        height: 13rem;
    }
</style>

![](assets/firefox_ESUHfl6rzT.png)

---

<style scoped>
    table > * {
        font-size: 0.85rem
    }
</style>

# Как всегда паттерны

| Понятие              | Реализация                        |
| -------------------- | --------------------------------- |
| Автомат              | Класс                   |
| Контекст             | Переменные в классе автомата        |
| Эвент                | Вызов метода в классе автомата   |
| Обработчик состояния | Класс состояния          |
| Обработчик перехода  | Метод setState в классе автомата |

---

<!-- _class: only-code -->

```php
// Описание состояний

abstract class State
{
    protected BlogPublishingStateMachine $machine;

    public function setMachine(BlogPublishingStateMachine $machine)
    {
        $this->machine = $machine;
    }

    abstract public function handle(string $event, array $data = []): void;
}
```

---

<!-- _class: only-code -->

```php
// Название класса состояния является идентификатором состояния
class DraftState extends State 
{ // <-- Весь класс является обработчиком состояния
    public function handle(string $event, array $data = []): void
    {
        // Тут может быть общая логика обработчика состояния

        // Можно выводить ошибку при невозможном переходе
        $this->$event($data);
    }

    public function toReview(array $data = []): void {
        $this->machine->setState(new ReviewState()); // <-- Переход
    }

    public function publish(array $data = []): void {
        $this->machine->setPublisher($data['publisher'])

        $this->machine->setState(new PublishedState());
    }
}
```

---

<!-- _class: only-code -->

```php
class ReviewState extends State
{
    public function handle(string $event, array $data = []): void
    {
        $this->$event($data);
    }

    public function publish(array $data = []): void {
        $this->machine->setPublisher($data['publisher'])

        $this->machine->setState(new PublishedState());
    }

    public function reject(array $data = []): void {
        $this->machine->setState(new DraftState());
    }
}

```

---

<!-- _class: only-code -->

```php
class PublishedState extends State
{
    public function handle(string $event, array $data = []): void
    {
        $this->$event($data);
    }

    public function expire(array $data = []): void {
        $this->machine->setPublisher(null)

        $this->machine->setState(new DraftState());
    }
}

```

<!-- Преимущества данного примера:
- При добавлении нового состояния нужно будет создать новый дочерний класс состояния, реализовать там новую логику и изменить необходимые классы состояния (обработчики эвентов) в некоторых существующих классах событий, откуда может осуществиться переход в новое состояние  
- При добавлении нового возможного перехода в идеале нужно будет поменять только необходимые классы состояния (обработчики эвентов), откуда осуществляется переход -->

---

<!-- _class: only-code -->

```php
class BlogPublishingStateMachine
{ // <-- Автомат
    private BlogPost $blogPost; // <-- Контекст
    private ?string $publisher; //

    private State $state;

    public function __construct(BlogPost $blogPost)
    {
        $this->setState(new DraftState()); // <-- Начальное состояние
    }

    /** Контекст позволяет изменять объект Состояния во время выполнения */
    public function setState(State $state): void
    { // <-- Обработчик перехода
        echo "Transition to " . get_class($state) . ".\n";
        $this->state = $state;
        $this->state->setMachine($this);
    }

    /** Делегируем часть поведения текущему объекту Состояния */
    public function send(string $event, array $data = []): void
    { // <-- Обработчик эвента
        $this->state->handle($event, $data);
    }

    // getters and setters
}
```

<!-- Недостатки данного примера:
- Нет валидации на переходы -->

---

<!-- _class: only-code -->

```php
$machine = new BlogPublishingStateMachine($blogPost);

$machine->send('toReview'); // <-- Бросаем эвент
// logs 'Transition to ReviewState'
$machine->send('reject');
// logs 'Transition to DraftState'
$machine->send('toReview');
// logs 'Transition to ReviewState'
$machine->send('publish');
// logs 'Transition to PublishedState'
$machine->send('expire');
// logs 'Transition to DraftState'
$machine->send('publish');
// logs 'Transition to PublishedState'
```

---

<style scoped>
    h1 {
        font-size: 1rem;
        margin-bottom: 0.2rem
    }
    table > * {
        font-size: 0.7rem
    }
</style>

# "Не понимаю, как лучше"

<!-- Чтобы гибко реализовать архитектуру вашего автомата, могу предложить ориентироваться на такую таблицу.
И, конечно же, на диаграмму последовательности обработки
Она будет доступна в материале, ссылка на который будет в конце -->

| Понятие           | Реализация                      |
| ----------------- | ------------------------------- |
| Автомат           | Функция                         |
|                   | Класс                           |
| Контекст          | Переменные в функции или классе |
|                   | Отдельный класс (DTO)                 |
| Эвент             | Условие в функции               |
|                   | Вызов метода автомата           |
|                   | Вызов метода отдельного класса  |
| Обработчик эвента | Блок кода                       |
|                   | Метод автомата                  |
|                   | Отдельный класс                 |

---

<style scoped>
    h1 {
        font-size: 1rem;
        margin-bottom: 0.2rem
    }
    table > * {
        font-size: 0.5rem
    }
</style>

# "Не понимаю, как лучше"

| Понятие                 | Реализация                                                                                       |
| ----------------------- | ------------------------------------------------------------------------------------------------ |
| Входные данные          | Переменные                                                                                       |
|                         | Параметры метода                                                                                 |
| Идентификатор состояния | Значение переменной                                                                              |
|                         | Название метода в автомате                                                                       |
|                         | Название класса состояния                                                                        |
| Обработчик состояния    | Блок кода в функции (внутри if\else или switch)                                                  |
|                         | Метод в автомате (обращение через variable functions в PHP)                                      |
|                         | Метод в отдельном классе-обработчике                                                             |
|                         | Класс состояния (обращение через полиморфизм)                                                    |
| Переход                 | Изменение переменной, содержащей идентификатор состояния                                         |
|                         | Вызов метода автомата (условный `setState`) с передачей названия или экземпляра класса состояния |
| Валидатор переходов     | Внутри `setState` в автомате                                                                     |
|                         | Внутри отдельного класса, вызываемого в `setState` в автомате                                    |

---

<!-- _class: only-title -->

<style scoped>
    section::before {
        background-image: url("assets/6107504543.jpg");
        background-position-y: 70%;
        opacity: 0.1;
    }
</style>

# Едем на готовом тандеме

---

# Front-end

Stately тесно интегрирован с

[![](assets/x-state-logo.svg)](https://xstate.js.org/)

<!-- Решение для управления состоянием и оркестрации приложений JavaScript и TypeScript, использующее событийно-ориентированное программирование, конечные автоматы, диаграммы состояний и модель актеров для обработки сложной логики предсказуемыми, надежными и визуальными способами. Он не имеет никаких зависимостей с другими библиотеками. -->

---

<style scoped>
    section {
        align-items: center;
    }
    img {
        height: 16rem;
    }
</style>

<!-- Stately позволяет создать конфигурацию для XState и в других форматах из графического представления автомата: -->

![](assets/firefox_OPhtTiQ97Y.png)

---

<style scoped>
    p {
        line-height: 1.5rem;
    }

    p+p {
        margin-top: 0.4rem;
    }
</style>

# Основные концепты XState

<span class="definition-use">Автомат</span>

<span class="definition">Акторы</span> - "живые" объекты, которые могут взаимодействовать друг с другом посредством асинхронной передачи сообщений. Запущенный автомат становится актором.

<!-- Актор основан на [Actor model](https://en.wikipedia.org/wiki/Actor_model) -->

---

<!-- _class: only-code -->

```javascript
import { createMachine, assign, createActor } from 'xstate';

const blogPublishingMachine = createMachine({
  context: { // <-- Контекст (в XState - всегда иммутабельный)
    blogPost: { title: '', content: '' },
    publisher: '',
  },
  initial: 'draft', // <-- Начальное состояние = "Черновик"
  // ...
});
```

---

<!-- _class: only-code -->

```javascript
import { createMachine, assign, createActor } from 'xstate';

const blogPublishingMachine = createMachine({
    // ...
    states: { // <-- Описание состояний
      draft: { // <-- Обработчик состояния "Черновик"
        on: {
          'to_review': {  // <-- Обработчик эвента "Направить на рецензию"
            target: 'review' // <-- Переход
          },
          'publish': {
            // ...
            actions: // <-- Реакция на эвент
              assign({ // <-- Ф-ция assign нужна для изменения контекста
                publisher: ({ event }) => event.adminName,
              }),
            target: 'published'
          },
        },
      },
      // ...
    },
    // ...
  });
```

---

<!-- _class: only-code -->

```javascript
import { createMachine, assign, createActor } from 'xstate';

const blogPublishingMachine = createMachine({
    // ...
    states: {
      // ...
      review: {
        on: {
          'publish': {
            // ...
            actions: assign({
              publisher: ({ event }) => event.adminName,
            }),
            target: 'published'
          },
          'reject': { target: 'draft' }
        },
      },
      // ...
    },
    // ...
  });
```

---

<!-- _class: only-code -->

```javascript
import { createMachine, assign, createActor } from 'xstate';

const blogPublishingMachine = createMachine({
    // ...
    states: {
      // ...
      published: {
        on: {
          'expire': {
            actions: assign({
              publisher: ({ event }) => '',
            }),
            target: 'draft'
          },
        },
      },
    },
    // ...
  });
```

---

<!-- _class: only-code -->

```javascript
import { createMachine, assign, createActor } from 'xstate';

const blogPublishingMachine = createMachine({
  states: {
    draft: {
      on: {
        // ...
        'publish': {
          guard: 'canPublish', // <-- Валидатор перехода
          // ...
          target: 'published' // <-- Защищенный переход
        },
      },
    },
    review: {
      on: {
        'publish': {
          guard: 'canPublish',
          // ...
        },
        // ...
      },
    },
    // ...
  },
  {
    guards: { // <-- Логика валидаторов
      canPublish: ({ context }) => { /* ... */ },
    },
  },
});
```

---

<!-- _class: only-code -->

<!-- Когда вы запускаете автомат, он становится актором - процессом, который может получать и отправлять события и изменять свое поведение на основе событий, которые он получает, что может вызвать эффекты за пределами актора. -->

```javascript
const blogActor = createActor(blogPublishingMachine).start();

blogActor.subscribe((state) => { // <-- Для логирования
  console.log(state.context.publisher);
});

blogActor.send({ type: 'to_review' }); // <-- Бросаем эвент
// logs ''
blogActor.send({ type: 'reject' });
// logs ''
blogActor.send({ type: 'to_review' });
// logs ''
blogActor.send({ type: 'publish', adminName: 'Админ1' }); // <-- Бросаем эвент с входными данными
// logs 'Админ1'
blogActor.send({ type: 'expire' });
// logs ''
blogActor.send({ type: 'publish', adminName: 'Админ2' });
// logs 'Админ2'
```

<!-- В документации на каждый компонент конфигурации автомата в том числе есть шпаргалки (cheatsheets) с примерами кода. -->

<!-- ***Какие недостатки вы видите в XState на первый взгляд?*** -->

---

<style scoped>
    p {
        font-size: 0.9rem;
        line-height: 1.4rem;
    }
    img {
        margin: auto;
        height: 5rem;
    }
</style>

# Back-end (Symfony)

<!-- В Symfony конечные автоматы используются в рамках workflows. -->

> <span class="definition">Workflow (рабочий процесс)</span> - это жизненный цикл объекта, который состоит определяется множеством из мест (places) и переходов (transitions) между ними. 

![](assets/states_transitions.png)

<!-- Очень похожи на [сети Петри](https://ru.wikipedia.org/wiki/%D0%A1%D0%B5%D1%82%D1%8C_%D0%9F%D0%B5%D1%82%D1%80%D0%B8) -->


<!-- Более формально:
Place (место) - стадия рабочего процесса.
Transition (переход) - описание действия, необходимого, чтобы добраться от одного места до другого.
Definition (определение) состоит из множества мест и переходов. -->

---

**Конечный автомат в Symfony** - это частный случай рабочего процесса, и его цель - сохранять текущее состояние модели.

**Отличия от рабочего процесса:**
- Рабочие процессы могут находиться более чем в одном place одновременно
- При применении перехода рабочий процесс требует также, чтобы объект находился во всех определенных предыдущих местах перехода, а не только хотя бы в одном из них, как требует конечный автомат.

---

<!-- _class: only-code -->

```yaml
# config/packages/workflow.yaml
framework:
  workflows:
    blog_publishing:
      type: 'workflow' # или 'state_machine'
      marking_store: # <-- Настройки поля для хранения идентификаторов состояний
        type: 'method'
        property: 'currentPlace'
      supports:
        - App\Entity\BlogPost # <-- Класс, участвующий в рабочем процессе
      initial_marking: draft # <-- Начальное состояние
      # ...
```

---

<!-- _class: only-code -->

```yaml
# config/packages/workflow.yaml
framework:
  workflows:
    blog_publishing:
      # ...
      places:      # <-- Места
        - draft
        - reviewed
        - rejected
        - published
      transitions: # <-- Переходы
        to_review: # <-- Название перехода
          from: draft
          to:   review
        publish:
          from: [draft, review]
          to:   published
          # ...
        reject:
          from: review
          to:   draft
          # ...
        expire:
          from: published
          to:   draft
```

---

<!-- _class: only-code -->

```yaml
# config/packages/workflow.yaml
framework:
  workflows:
    blog_publishing:
      # ...
      transitions:
        # ...
        publish:
          from: reviewed
          to:   published
          guard: "is_authenticated" # <-- Пример валидации перехода
        reject:
          from: reviewed
          to:   rejected
          guard: "is_granted('ROLE_ADMIN')"
```

---

<!-- _class: only-code -->

<!-- #### Пример сущности -->

```php
// src/Entity/BlogPost.php
namespace App\Entity;

class BlogPost
{
    private string $currentPlace; // <-- Указано в конфиге
    private string $title;
    private string $content;

    public function getCurrentPlace(): string
    {
        return $this->currentPlace;
    }

    public function setCurrentPlace(string $currentPlace, array $context = []): void 
    {
        $this->currentPlace = $currentPlace;
    }

    // ...
}
```

---

<!-- _class: only-code -->

<!-- #### Работа с созданным workflow -->

```php
use App\Entity\BlogPost;
use Symfony\Component\Workflow\WorkflowInterface;

class MyClass
{
    // Autowiring по имени
    public function __construct(
        private WorkflowInterface $blogPublishingWorkflow
    ) {
    }

    // или

    // Autowiring по атрибуту Target
    public function __construct(
        #[Target('blog_publishing')] private WorkflowInterface $workflow
    ) {
    }

    public function someMethod(BlogPost $post): void
    {
        // ...
    }
}
```

---

<!-- _class: only-code -->

<!-- #### Работа с созданным workflow -->

```php
use App\Entity\BlogPost;
use Symfony\Component\Workflow\WorkflowInterface;

class MyClass
{
    // ...

    public function someMethod(BlogPost $post): void
    {
        // Проверить, что можем перейти
        $this->workflow->can($post, 'publish'); // False
        $this->workflow->can($post, 'to_review'); // True

        try {
            $this->workflow->apply($post, 'to_review'); // <-- Переход
        } catch (LogicException $exception) {
            // ...
        }

        // Получить доступные переходы для текущего состояния
        $transitions = $this->workflow->getEnabledTransitions($post);

        // Получить переход 'publish' для текущего состояния
        $transition = $this->workflow->getEnabledTransition($post, 'publish');
    }
}
```

---

<!-- _class: only-code -->

<!-- Работа с созданным workflow -->

```twig
{# Проверить, что можем опубликовать #}
{% if workflow_can(post, 'publish') %}
    <a href="...">Опубликовать</a>
{% endif %}

{# Цикл по возможным переходам #}
{% for transition in workflow_transitions(post) %}
    <a href="...">{{ transition.name }}</a>
{% else %}
    Нет доступных действий
{% endfor %}

{# Проверить, что объект в определенном месте #}
{% if workflow_has_marked_place(post, 'reviewed') %}
    <p>Пост готов к рецензии</p>
{% endif %}

{# Проверить, что место отмечено в объекте #}
{% if 'reviewed' in workflow_marked_places(post) %}
    <span class="label">Пост рассмотрен</span>
{% endif %} 

{# Цикл по блокировщикам перехода #}
{% for blocker in workflow_transition_blockers(post, 'publish') %}
    <span class="error">{{ blocker.message }}</span>
{% endfor %}

{# И т.д. #}
```

---

<style scoped>
    li {
        text-align: start;
        font-size: 0.9rem;
    }
</style>

# Kernel events

Symfony бросает эвенты при каких-либо действиях с workflow в порядке от общего к частному. Например при завершении перехода в состояние это будут:

1. workflow.entered
2. workflow.[workflow_name].entered
3. workflow.[workflow_name].entered.[state_name]

<!-- Аналогичные эвенты есть при прохождении валидации, входе в или выходе из place, успешном transition и т.д. -->

---


<!-- _class: only-code -->

<!-- Мы можем указать эвенты, которые в принципе будут брошены -->

```yaml
# config/packages/workflow.yaml
framework:
  workflows:
    blog_publishing:
      # ...

      # Опциональное указание бросаемых эвентов
      events_to_dispatch: ['workflow.entered', 'workflow.completed']
      
      # ...
```


---

<!-- _class: only-code -->

<!-- #### Пример листенера при переходе из состояний конкретного workflow -->

```php
// src/App/EventSubscriber/BlogPublishedSubscriber.php
namespace App\EventSubscriber;

use Psr\Log\LoggerInterface;
use Symfony\Component\EventDispatcher\EventSubscriberInterface;
use Symfony\Component\Workflow\Event\Event;
use Symfony\Component\Workflow\Event\LeaveEvent;

class BlogPublishedSubscriber implements EventSubscriberInterface
{
    public function onPublished(Event $event): void
    {
        // Можем получить некоторые данные
        $transition = $event->getTransition()->getName();
        $placesBefore = array_keys($event->getMarking()->getPlaces());
        $possibleTransitions = $event->getTransition()->getTos();

        // Отправляем email, что пост опубликован...
    }

    public static function getSubscribedEvents(): array
    {
        // Эвент на завершение перехода в состояние "Опубликовать"
        return ['workflow.blog_publishing.entered.published' => 'onPublished'];
    }
}
```

---

<!-- _class: only-code -->

<!-- Также листенер можно указать при помощи атрибута -->

```php
class BlogPublishedListener
{
    #[AsTransitionListener(workflow: 'blog_publishing', transition: 'published')]
    public function onPublished(TransitionEvent $event): void
    {
        // ...
    }
    // ...
}
```

---

<!-- _class: only-title -->

<style scoped>
    h2 {
        font-size: 1.5rem;
    }
</style>

## Мне удалось убедить вас, что конечные автоматы - это очень гибкий инструмент для реализации ваших задач?

---

<style scoped>
    p.thanks{
        position: absolute;
        background-color: #0f1124;
        /* background-color: #ec0033; */
        border: 9px solid #ec0033;
        top: 30px;
        right: 140px;
        /* width: 1000px; 
        height: 1000px; */
        rotate: 4grad;
        z-index: 1;
        font-size: 3.2rem;
        font-weight: 800;
        border-radius: 80px;
        padding-left: 16px;
        padding-right: 16px;
        padding-bottom: 20px;
    }
    
    p.attention{
        position: absolute;
        background-color: #00e2c4;
        top: 220px;
        right: 60px;
        /* width: 1000px; 
        height: 1000px; */
        rotate: -5grad;
        z-index: 0;
        font-size: 2.5rem;
        font-weight: 800;
        color: black;
        padding-left: 16px;
        padding-right: 16px;
        padding-bottom: 15px;
        font-style: oblique;
    }
    img {
        height: 8rem;
        border: none;
        box-shadow: none;
    }
    .footer {
        display: flex;
        flex-direction: row;
        width: 100%;
        justify-content: center;
        align-items: center;
        margin-top: auto;
    }
    img.icon {
        height: 1.5rem;
        border-radius: 0;
    }
    .footer__block {
        display: flex;
        flex-direction: row;
        align-items: center;
    }
    .footer__block + .footer__block {
        margin-left: 30px;
    }
    .footer__link {
        font-size: 0.7rem;
        margin: 0;
    }
    .footer__my-name {
        font-size: 0.7rem;
        font-weight: 800;
        /* width: 250px; */
        margin: 0;
        /* margin-left: 50px; */
        /* margin-right: auto; */
        /* text-align: center; */
    }
    /* img.icon-wrapper {
        width: 1rem;
        height: 1rem;
    } */
</style>

<p class="thanks">Спасибо</p>
<p class="attention">за внимание!</p>

![](assets/material-link.png)

<div class="footer">
    <div class="footer__block">
        <p class="footer__my-name">Холин Владимир</p>
    </div>
    <div class="footer__block">
        <img class="icon" src="assets/telegram.svg" alt="">
        <p class="footer__link">@Terqaz</p>
    </div>
    <div class="footer__block">
        <img class="icon" src="assets/email.svg" alt="">
        <p class="footer__link">holinvova@gmail.com</p>
    </div>
</div>
