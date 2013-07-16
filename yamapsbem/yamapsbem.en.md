# Yandex.Maps API and BEM

One of the most frequent cases of Yandex.Maps API usage is the creation of the menu to show on a map different types of POI (geoobject collections). Such menu helps an end user to chose onle the sertain types of POI to visualize them on a map. Here is an [hyperlink](http://dimik.github.com/ymaps/examples/group-menu/menu03.html) example. But let’s remake this example using BEM methodology.

## Firsts steps
BEM developers have already created a project-stub for easy development start.

````bash
git clone https://github.com/bem/project-stub.git shopsList
cd shopsList
npm install
````

Now we can use the project locally. Let’s test that everything works properly. Open the folder and run make, wait some time for project to start and than open [localhost:8080/desktop.bundles/index/index.html](http://localhost:8080/desktop.bundles/index/index.html) in your browser. You’ll see something like this:

<img src="http://zloylos.me/other/imgs/ymapsbem/project_stub.png" title="" alt="" border="0"/>

Now we can proceed to the next step.

## General description of the project

We need to create a block «map» for the map itself, block «sidebar» — right or left column for the block «menu» to show the list of organizations by categories. According to BEM methodology the blocks shouldn’t «know» about each other, so we need to create one more intermediate block, to accept menu clicks and interact with a map. For example, «i-geo-controller».

Чтобы лучше понять зачем нужен промежуточный блок, как он работает и что такое «миксы», рекомендуем посмотреть [рассказ Кира Белевича «Миксы во вселенной БЭМ»](http://events.yandex.ru/events/yasubbotnik/msk-sep-2012/talks/327/).

## Page description, bemjson coding

As we thought about the page structure and main blocks from the very beginning, now we should just write a json-style code. You can watch the sourcecode for better understanding. And the page structure will be this:

````
    b-page
    == container
    ==== map
    ==== sidebar
    ====== menu
    ======== items
````

<img src="http://zloylos.me/other/imgs/ymapsbem/index_bemjson.png" alt="">


And bemjson:
````js
{
    block: 'b-page',
    content: [
        {
            block: 'container',
            content: [
                {
                    block: 'map'
                },
                {
                    block: 'sidebar',
                    content: [
                        {
                            block: 'menu',
                            content [ /* menu items */ ]
                        }
                    ]
                }
            ]
        }
    ]
}
````

Watch tis for the details: [desktop.bundles/index/index.bemjson.js](https://github.com/zloylos/ymaps-and-bem/blob/master/desktop.bundles/index/index.bemjson.js)

## Map block

Let’s start from the main block — map. First of all have to enable the API with all the necessary parameters. We could create a new block i-API, but it is better to use one block and modifiers. For the block map we’ll create modifier «api» with, в котором для начала разместим значение — «ymaps».
В примере мы будем использовать [динамический API](http://api.yandex.ru/maps/doc/jsapi/), но нужно помнить, что мы можем использовать и [static API](http://api.yandex.ru/maps/doc/staticapi/). Это можно реализовать в рамках модификаторов.

Для удобной работы с картой нам стоит продумать интерфейс добавления меток на карту без лишней головной боли. Для этого стоит сделать обработку поля: geoObjects, в котором будут храниться метки или коллекции. Для метки сделаем такой интерфейс:

````js
{
    coords: [], // координаты метки
    properties: {}, // данные метки
    options: {}, // опции метки
}
````

Для коллекции:
````js
{
    collection: true, // флаг, указывающий, что это коллекция / группа меток
    properties: {}, // свойства группы
    options: {}, // опции группы
    data: [], // массив меток.
}
````
Это покрывает 90% всех кейсов.

## Блок menu

Здесь нам нужно сделать двухуровневое меню. Создаем блок menu, который будет распознавать клики по группам и элементам. Соответственно, нам нужны такие элементы:
- item — элемент меню;
- content — контейнер для элементов;
- title — заголовок группы.

Вкладывая один блок меню в другой, можно добиться необходимой иерархичности.

Например, простейшее меню, описанное в bemjson будет выглядеть так:
````js
{
    block: 'menu',
    content: [
        {
            elem: 'title',
            content: 'menu title'
        },
        {
            elem: 'content',
            content: [
                {
                    elem: 'item',
                    content: 'menu-item-1'
                },
                {
                    elem: 'item',
                    content: 'menu-item-2'
                }
            ]
        }
    ]
}
````

## Блок i-geo-controller

Блок-контроллер, который подписывается на события блоков menu — «menuItemClick» и «menuGroupClick», реагирует на них и совершает определенные действия на карте. В нашем примере у него следующие задачи:
- при клике на метку нужно переместить ее в центр карты и открыть балун;
- при клике на группу нужно либо скрыть ее, либо показать, если до этого она была скрыта.

Кроме того, чтобы правильно взаимодействовать с картой, блок-контроллер должен знать, готова ли карта к тому, чтобы объектами на ней можно было управлять. Для этого блок map у себя будет пробрасывать событие «map-inited», а i-geo-controller — слушать его и запоминать ссылку на экземпляр карты.


<img src="http://zloylos.me/other/imgs/ymapsbem/blocks_scheme.png" alt="">


## Заключение

Готовый пример можно посмотреть по ссылке: [http://zloylos.github.io/ymapsbem/](http://zloylos.github.io/ymapsbem/).

Возможно, с использованием методологии БЭМ пример и получился более громоздким, чем без неё, но зато у нас есть более структурированный и удобный для поддержки код. А главное, его довольно просто масштабировать и расширять, что без использования методологии вызвало бы значительные проблемы и чаще всего привело бы к полному переписыванию кода.

<img src="http://zloylos.me/other/imgs/ymapsbem/ready.png" alt="">

Спасибо Саше Тармолову (@tarmolov) за ценные советы и помощь.

<!--(Begin) Article author block-->
<div class="article-author">
    <div class="article-author__photo">
        <img class="article-author__pictures" src="http://zloylos.me/other/imgs/ymapsbem/denis.png" alt="Фотография Денис Хананеин">
    </div>
    <div class="article-author__info">
        <div class="article-author__row">
             <span class="article-author__name">Денис Хананеин,
        </div>
        <div class="article-author__row">
            Разработчик интерфейсов API Яндекс.Карт
        </div>
        <div class="article-author__row">
             <a class="article-author__social-icon b-link" target="_blank" href="http://twitter.com/kandasoft">twitter.com/kandasoft</a>
        </div>
        <div class="article-author__row">
             <a class="article-author__social-icon b-link" target="_blank" href="http://github.com/zloylos">github.com/zloylos</a>
        </div>
    </div>
</div>
<!--(End) Article author block-->