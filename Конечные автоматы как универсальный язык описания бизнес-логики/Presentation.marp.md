---
marp: true
theme: uncover
paginate: true
---

<style>
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

    section.only-title {
        justify-content: center;
        align-items: center;
    }

    section.only-title :is(h1, h2, h3, h4) {
        text-align: center;
    }

    section.title-with-image p {
        margin: auto;
    }

    section.only-code {
        justify-content: center
    }

    section > pre[is=marp-pre] {
        margin: 0 !important;
    }

    section::before {
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
        background-color: #ffffff10 !important;
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
        border-bottom:  3px  solid #ff0037;
        padding-bottom: 8px;
        width: fit-content;
        white-space: nowrap;
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

    section :is(pre,marp-pre)::part(auto-scaling) {
        max-height: 650px !important;
    }

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
    }
    h2 {
        margin-bottom: auto
    }
    span.after-1 {
        font-size: 0.7rem;
        margin-left: 0.6rem;
    }
    p {
        margin-bottom: 0.7rem;
    }
</style>

<h1 class="highlighted-title"> Конечные автоматы </h1>

## как универсальный язык описания бизнес-логики

<span class="definition-use"> Холин Владимир </span> <span class="after-1"> Intaro, Программист PHP </span>

---

# О чем будем говорить

* Пофилософствуем
* Поймем, что из себя в принципе представляют автоматы
* Выделим понятия, полезные в практике
* Подробно разберем несколько примеров автоматов
* Разберемся, как реализовать свой автомат
* Исследуем существующие решения для бэка и фронта

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

    li+li {
        margin-top: 0.5rem;
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
* > <span class="definition">Переход</span> - изменение состояние с одного на другое
* **Согласны?** :smirk_cat:

<!-- В рамках любой системы можно выделить две концепции: постоянство и изменение.

Со стороны постоянства системы основным определением выделяется **состояние** как "отпечаток" системы в конкретный момент времени. При этом у системы может быть очень различных параметров, но мы можем абстрагироваться от их конкретных значений и выделить "логические состояния" системы. 

Если взглянуть с математической точки зрения, то можно представить состояние системы как вектор в многомерном пространстве параметров, а  "логическое" состояние как некую область в этом пространстве. 

Со стороны изменения системы основным понятием будет **переход** из одного состояния в другое., то есть изменение ее параметров.

Согласитесь, будет удобно, если система будет иметь ограниченное количество "логических" состояний. Тогда и количество возможных переходов между ними будет конечным. В этом и есть смысл конечных автоматов. Они содержат описание всех таких логических состоний и переходов между ними.

Напрашивается вывод что это фундаментальное описание системы. -->

---

<!-- _class: only-title -->

<style scoped>
    h1 {
        font-size: 1.5rem;
    }

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

<!-- Это может быть:

- **Автомат Мили** - выходные значения также зависят от входных (нужна дополнительная табличка для описания).

- **Автомат Мура** - частный случай автомата Мили, где выходные значения зависят только от внутреннего состояния. -->

* > Автомат <span class="definition">детерминированный</span>, если:
    > - мы точно знаем, какое будет следующее состояние в зависимости от текущего и входных  данных
    > - выход зависит только от текущего состояния и текущего входа

---

<style scoped>
    p {
        line-height: 1.4rem;
    }
</style>

# Откуда всё пошло

> <span class="definition">Автоматное программирование</span> - парадигма программирования, при использовании которой программа или её фрагмент
осмысливается как модель какого-либо формального автомата.

---

# "Что мне дадут автоматы?"

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

<style scoped>
    p {
        margin: auto;
    }

    img {
        padding: 20px;
    }
</style>

# Небольшой оффтоп

[![](assets/stately-logo.svg)](https://stately.ai)

Инструмент для моделирования и визуализации логики приложения в виде автоматов и диаграмм состояний.

---

<!-- _class: title-with-image -->

<style scoped>
    img {
        width: 20rem;
    }
</style>

# Режим дизайна

![](assets/studio-dm.png)

<!-- режим симуляциии увидим дальше -->

---

<!-- _class: title-with-image -->

<style scoped>
    h1 {
        font-size: 1rem;
    }

    section::before {
        content: none;
    }
</style>

# Дверь как автомат

<iframe
        loading="lazy"
        src="https://stately.ai/registry/editor/embed/7bd17f6e-ca1d-4c8d-8328-a873d67768b7?machineId=9d08b8f6-a74a-440d-8288-4b6af7fcca2f&mode=Design&colorMode=dark"
        style="display: block; width: 100%; height: 100%">
    <a
       href="https://stately.ai/registry/editor/embed/7bd17f6e-ca1d-4c8d-8328-a873d67768b7?machineId=9d08b8f6-a74a-440d-8288-4b6af7fcca2f&mode=Design"></a>
</iframe>

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

# Дверь как автомат

![](assets/mermaid-door.svg)

---

<style scoped>
    h1 {
        font-size: 1rem;
    }

    section::before {
        content: none;
    }
</style>

# Переменная как автомат

<iframe
        loading="lazy"
        src="https://stately.ai/registry/editor/embed/7bd17f6e-ca1d-4c8d-8328-a873d67768b7?mode=design&machineId=7a179269-2c32-49ff-a645-474b7f808a59&mode=Design&colorMode=dark"
        style="display: block; width: 100%; height: 100%">
    <a
       href="https://stately.ai/registry/editor/embed/7bd17f6e-ca1d-4c8d-8328-a873d67768b7?mode=design&machineId=7a179269-2c32-49ff-a645-474b7f808a59&mode=Design"></a>
</iframe>

<!-- даже работу с ней можно описать как state machine

- Начальное состояние - определена
- Входные данные - новое значение переменной или ее удаление
- Выходные данные - значение переменной или какая-либо ошибка (нет значения или не определена) 

- конечный, т.к. кол-во состояний конечно, не смотря на то, что переменная может иметь очень много разных значений. В смысле данного автомата важно только, установлено значение или нет, а значение переменной хранится в контексте. Само значение имеет смысл уже при использовании переменной где-либо
- недетерминированный, т.к. при входе "Получить значение" в состоянии "Содержит значение" выход зависит не только от входа и текущего состояния, а также от значения value в контексте-->

---

<style scoped>
    table {
        font-size: 0.75rem
    }

    h1 {
        font-size: 1rem;
    }
</style>

# Переменная как автомат

| Состояние         | Эвент              | Вход     | Новое состояние   | Вывод |
| ----------------- | ------------------ | -------- | ----------------- | ----- |
| Определена        | Присвоить значение | newValue | Содержит значение | value |
| Определена        | Удалить            |          | Не определена     |
| Содержит значение | Присвоить значение | newValue | Содержит значение | value |
| Содержит значение | Получить значение  |          | Содержит значение | value |
| Содержит значение | Удалить            |          | Не определена     |

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

<style scoped>
    section {
        padding: 0.3rem;
    }
    p {
        margin: auto;
    }
    img {
        height: 16rem;
    }
</style>

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
if (isCreated && isEditable && isValid && !isDeleted {...}

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

К тому не возможные комбинации таких флагов будут соответствовать реальным состояниям. -->

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
int main() { // <-- Автомат (без валидации переходов)
    enum states { 
        before, inside, after // <-- Описание состояний (без таблицы переходов)
    } state; // <-- Хранит идентификатор состояния
    state = before; // <-- Начальное состояние

    // Контекста нет :(

    int c; // <-- Используется для записи входа в обработчик состояния
    while ((c = getchar()) != EOF) { // <-- Формируем вход и бросаем эвент
        switch (state) { // <-- Обработчик эвента
            case before:
                if (c == '\n') { // <-- Обработчик состояния
                    putchar('\n'); // <-- Вывод (тут в консоль)
                } else if (c != ' ') {
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
                if (c == '\n') {
                    putchar('\n');
                    state = before;
                }
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
    table > * {
        font-size: 0.85rem
    }
</style>

# Как всегда паттерны

| Понятие              | Реализация                        |
| -------------------- | --------------------------------- |
| Автомат              | Класс контекста                   |
| Контекст             | Переменные в классе контекста        |
| Эвент                | Вызов метода в классе контекста   |
| Обработчик состояния | Класс состояния          |
| Обработчик перехода  | Метод setState в классе контекста |

---

<!-- _class: only-code -->

```php
// Описание состояний

abstract class State
{
    protected Context $context;

    public function setContext(Context $context)
    {
        $this->context = $context;
    }

    abstract public function handle1(): void;
    abstract public function handle2(): void;
}
```

<!-- Преимущества данного примера:
- При добавлении нового состояния нужно будет создать новый дочерний класс состояния, реализовать там новую логику и изменить обработчики эвентов в некоторых существующих классах событий, откуда может осуществиться переход в новое состояние  
- При добавлении нового возможного перехода в идеале нужно будет поменять только один обработчик эвента в состоянии, откуда осуществляется переход -->

---

<!-- _class: only-code -->

```php
// Название класса состояния является идентификатором состояния
class ConcreteStateA extends State 
{ // <-- Весь класс является обработчиком состояния
    public function handle1(): void
    {
        echo "ConcreteStateA handles request1.\n"; // <-- Вывод
        echo "ConcreteStateA wants to change the state of the context.\n";
        $this->context->setState(new ConcreteStateB()); // <-- Переход
    }

    public function handle2(): void
    {
        echo "ConcreteStateA handles request2.\n";
    }
}
```

---

<!-- _class: only-code -->

```php
class ConcreteStateB extends State
{
    public function handle1(): void
    {
        echo "ConcreteStateB handles request1.\n";
    }

    public function handle2(): void
    {
        echo "ConcreteStateB handles request2.\n";
        echo "ConcreteStateB wants to change the state of the context.\n";
        $this->context->setState(new ConcreteStateA());
    }
}
```

---

<!-- _class: only-code -->

```php
class Context // <-- Автомат
{
    // Сам по себе контекст мог бы быть тут :(

    public function __construct(private State $state)
    {
        $this->setState($state); // <-- Начальное состояние
    }

    /** Контекст позволяет изменять объект Состояния во время выполнения */
    public function setState(State $state): void // <-- Обработчик перехода
    {
        // Тут можно выводить ошибку при невозможном переходе в новое состояние
        echo "Context: Transition to " . get_class($state) . ".\n";
        $this->state = $state;
        $this->state->setContext($this);
    }

    /** Контекст делегирует часть своего поведения текущему объекту Состояния */
    public function request1(): void // <-- Обработчик эвента request1
    {
        $this->state->handle1();
    }

    public function request2(): void
    {
        $this->state->handle2();
    }
}
```

<!-- Недостатки данного примера:
- Если эвентов очень много, а переходов между состояниями - мало, то большинство дочерних классов стейта будут иметь много boilerplate методов -->

---

<!-- _class: only-code -->

```php
$context = new Context(new ConcreteStateA());
$context->request1(); // <-- Бросаем эвент
$context->request2();
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
Она будет доступна в материале, ссылка на который будет в конце
 -->

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
        font-size: 0.8rem;
        line-height: 1.4rem;
    }
</style>

# Основные концепты XState

<span class="definition-use">Автомат</span>

<span class="definition">Актеры</span> - "живые" объекты, которые могут взаимодействовать друг с другом посредством асинхронной передачи сообщений.

<!-- Актер основан на [Actor model](https://en.wikipedia.org/wiki/Actor_model) -->

---

<!-- _class: only-code -->

<!-- Некоторые возможности конфигурирования автомата: -->

```javascript
import { createMachine, assign, createActor } from 'xstate';

const textMachine = createMachine({
  context: { // <-- Контекст (в XState - всегда иммутабельный)
    committedValue: '',
    value: '',
  },
  initial: 'reading', // <-- Начальное состояние
  states: { // <-- Описание состояний
    reading: { // <-- Обработчик состояния reading
      on: {
        'text.edit': {  // <-- Обработчик эвента text.edit
            target: 'editing' // <-- Переход
        },
      },
    },
    editing: {
      on: {
        'text.change': {
          actions: // <-- Реакция на эвент
            assign({ // <-- Ф-ция assign нужна для изменения контекста
                value: ({ event }) => event.value,
            }),
        },
        'text.commit': {
          guard: 'isValid', // <-- Валидатор перехода
          actions: assign({
            committedValue: ({ context }) => context.value,
          }),
          target: 'reading', // <-- Защищенный переход (выполнится только, если isValid вернет true)
        },
        'text.cancel': {
          actions: assign({
            value: ({ context }) => context.committedValue,
          }),
          target: 'reading',
        },
      },
    },
  },
  {
    guards: { // <-- Логика валидаторов
      isValid: ({ context }) => {
        return context.committedValue.length > 0;
      },
    },
  },
});
```

---

<!-- _class: only-code -->

<!-- Как автомат используется: -->

```javascript
const textActor = createActor(textMachine).start();

textActor.subscribe((state) => { // <-- Для логирования
  console.log(state.context.value);
});

textActor.send({ type: 'text.edit' }); // <-- Бросаем эвент
// logs ''
textActor.send({ type: 'text.change', value: 'Hello' }); // <-- Бросаем эвент с входными данными
// logs 'Hello'
textActor.send({ type: 'text.commit' });
// logs 'Hello'
textActor.send({ type: 'text.edit' });
// logs 'Hello'
textActor.send({ type: 'text.change', value: 'Hello world' });
// logs 'Hello world'
textActor.send({ type: 'text.cancel' });
// logs 'Hello'
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

<!-- # Пример конфигурации -->

```yaml
# config/packages/workflow.yaml
framework:
  workflows:
    blog_publishing:
      type: 'workflow' # или 'state_machine'
      audit_trail:
        enabled: true # <-- Включение детальных логов активности
      events_to_dispatch: ['workflow.leave', 'workflow.completed'] # <-- Опциональное указание бросаемых эвентов
      metadata: # <-- Опциональные метаданные
        title: 'Blog Publishing Workflow'
      marking_store: # <-- Настройки поля для хранения идентификатора состояния
        type: 'method'
        property: 'currentPlace'
      supports:
        - App\Entity\BlogPost # <-- Класс, участвующий в рабочем процессе
      initial_marking: draft # <-- Начальное состояние
      places:      # <-- Места
        - draft:
          metadata: # <-- Опциональные метаданные
            max_num_of_words: 500
        - reviewed
        - rejected
        - published
      transitions: # <-- Переходы
        to_review: # <-- Название перехода
          from: draft # <-- В from также можно указать массив
          to:   reviewed
        publish:
          guard: "is_authenticated" # <-- Пример валидации перехода
          from: reviewed
          to:   published
          metadata: # <-- Опциональные метаданные
            hour_limit: 20
            explanation: 'You cannot publish after 8 PM.'
        reject:
          guard: "is_granted('ROLE_ADMIN')"
          from: reviewed
          to:   rejected
```

---

<!-- _class: only-code -->

<!-- #### Пример сущности -->

```php
// src/Entity/BlogPost.php
namespace App\Entity;

class BlogPost
{
    // Тип string указывает Symfony на то, что модель может быть только в одном состоянии в одно время
    // Чтобы она могла быть сразу в нескольких состояниях, нужен тип array тут и в геттере и сеттере
    private string $currentPlace; // <-- Указано в конфиге
    private string $title;
    private string $content;
    
    // Геттер и сеттер обязательны, если $currentPlace не public
    public function getCurrentPlace(): string
    {
        return $this->currentPlace;
    }

    public function setCurrentPlace(string $currentPlace, array $context = []): void 
    {
        $this->currentPlace = $currentPlace;
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
        // Проверки возможности осуществления перехода
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

        // Работа с метаданными
        $metadata = $this->workflow->getMetadataStore();

        $title = $metadata ->getWorkflowMetadata()['title'] ?? 'Default title';
        $maxNumOfWords = $metadata->getPlaceMetadata('draft')['max_num_of_words'] ?? 500;

        $aTransition = $this->workflow->getDefinition()->getTransitions()[1];
        $hourLimit = $metadata->getTransitionMetadata($aTransition)['hour_limit'] ?? 20;

        // Также можно через универсальный метод
        $title = $metadata->getMetadata('title');
        $maxNumOfWords = $metadata->getMetadata('max_num_of_words', 'draft');
        $hourLimit = $metadata->getMetadata('hour_limit', $aTransition);
    }
}
```

---

<!-- _class: only-code -->

<!-- #### Работа с созданным workflow -->

```twig
{% if workflow_can(post, 'publish') %}
    <a href="...">Publish</a>
{% endif %}

{# Цикл по возможным переходам #}
{% for transition in workflow_transitions(post) %}
    <a href="...">{{ transition.name }}</a>
{% else %}
    No actions available.
{% endfor %}

{# Проверить, что объект в определенном месте #}
{% if workflow_has_marked_place(post, 'reviewed') %}
    <p>This post is ready for review.</p>
{% endif %}

{# Проверить, что место отмечено в объекте #}
{% if 'reviewed' in workflow_marked_places(post) %}
    <span class="label">Reviewed</span>
{% endif %}

{# Цикл по блокировщикам перехода #}
{% for blocker in workflow_transition_blockers(post, 'publish') %}
    <span class="error">{{ blocker.message }}</span>
{% endfor %}

{# Получить метаданные #}
<p>
    <strong>Workflow</strong>:<br>
    <code>{{ workflow_metadata(blog_post, 'title') }}</code>
</p>

{# И т.д. #}
```

---

# Kernel events

Symfony бросает эвенты при каких-либо действиях с workflow в порядке от общего к частному. Например при transition будут данные эвенты:

- workflow.transition
- workflow.[workflow name].transition
- workflow.[workflow name].transition.[transition name]

<!-- Аналогичные эвенты есть при прохождении валидации, входе в или выходе из place, успешном transition и т.д. -->

---

<!-- _class: only-code -->

<!-- #### Пример листенера при переходе из состояний конкретного workflow -->

```php
// src/App/EventSubscriber/WorkflowLoggerSubscriber.php
namespace App\EventSubscriber;

use Psr\Log\LoggerInterface;
use Symfony\Component\EventDispatcher\EventSubscriberInterface;
use Symfony\Component\Workflow\Event\Event;
use Symfony\Component\Workflow\Event\LeaveEvent;

class WorkflowLoggerSubscriber implements EventSubscriberInterface
{
    public function __construct(
        private LoggerInterface $logger,
    ) {
    }

    public function onLeave(Event $event): void
    {
        $this->logger->alert(sprintf(
            'Blog post (id: "%s") performed transition "%s" from "%s" to "%s"',
            $event->getSubject()->getId(),
            $event->getTransition()->getName(),
            implode(', ', array_keys($event->getMarking()->getPlaces())),
            implode(', ', $event->getTransition()->getTos())
        ));
    }

    public static function getSubscribedEvents(): array
    {
        return ['workflow.blog_publishing.leave' => 'onLeave'];
    }
}
```

---

<!-- _class: only-code -->

<!-- #### Пример листенера на конкретный переход -->

```php
class ArticleWorkflowEventListener
{
    #[AsTransitionListener(workflow: 'my-workflow', transition: 'published')]
    public function onPublishedTransition(TransitionEvent $event): void
    {
        // ...
    }
    // ...
}
```

---

<!-- _class: only-title -->

#### Мне удалось убедить вас, что использовать конечные автоматы - это хорошо и вовсе не страшно?

---

<!-- _class: title-with-image -->

<style scoped>
    img {
        height: 11rem;
        border: none;
        box-shadow: none;
    }
</style>

# Материал доклада

![](assets/material-link.png)