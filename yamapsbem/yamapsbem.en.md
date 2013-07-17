# Yandex.Maps API and BEM

One of the most frequent cases of Yandex.Maps API usage is the creation of the menu to show on a map different types of POI (geoobject collections). Such menu helps an end user to chose onle the sertain types of POI to visualize them on a map. Here is an [example](http://dimik.github.com/ymaps/examples/group-menu/menu03.html). But let’s remake this example using BEM methodology. 

## Firsts steps
BEM developers have already created a project-stub for easy development start.

````bash
git clone https://github.com/bem/project-stub.git shopsList
cd shopsList
npm install
````

Now we can use the project locally. Let’s test that everything works properly. Open the folder and run make, wait some time for project to start and than browse to [localhost:8080/desktop.bundles/index/index.html](http://localhost:8080/desktop.bundles/index/index.html). You’ll see something like this:

<img src="http://zloylos.me/other/imgs/ymapsbem/project_stub.png" title="" alt="" border="0"/>

Now we can proceed to the next step.

## General description of the project

We need to create a block «map» for the map itself, block «sidebar» — right or left column for the block «menu» to show the list of organizations by groups. According to BEM methodology the blocks shouldn’t «know» about each other, so we need to create one more intermediate block, to accept menu clicks and interact with a map. For example, «i-geo-controller». 

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

￼
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

Watch this for the details: [desktop.bundles/index/index.bemjson.js](https://github.com/zloylos/ymaps-and-bem/blob/master/desktop.bundles/index-en/index-en.bemjson.js).

## Map block

Let’s start from the main block — «map». First of all have to enable the API with all the necessary parameters. We could create a new block «i-API», but it is better to use one block and modifiers. For the block «map» we’ll create modifier «api» with value «ymaps». In our example we’ll use [JavaScript maps API](http://api.yandex.ru/maps/doc/jsapi/), but we also can use [Static maps API](http://api.yandex.ru/maps/doc/staticapi/). It can be realized via modifiers. 

Давайте начнем разработку с главного блока — карты. Прежде всего нужно подключить API с необходимыми опциями. Можно было бы создать отдельный блок i-API, но, кажется, куда удобнее реализовать все это в рамках одного блока, используя модификаторы. Для блока map мы создадим модификатор «api», в котором для начала разместим значение — «ymaps».
В примере мы будем использовать [динамический API](http://api.yandex.ru/maps/doc/jsapi/), но нужно помнить, что мы можем использовать и [static API](http://api.yandex.ru/maps/doc/staticapi/). Это можно реализовать в рамках модификаторов.

To make our work with the map easier, we have to think about the interface of placemarks adding. We should add a field «geoObjects», where we’ll put [placemarks or placemark collections](http://api.yandex.com/maps/doc/jsapi/2.x/dg/concepts/geoobjects.xml). The following interface is for the placemark:

````js
{
    coords: [], 
    properties: {}, 
    options: {}
}
````

And for placemark collection:
````js
{
    collection: true,
    properties: {},
    options: {},
    data: []
}
````
This code can be used in 90% of use-cases.

## Block «menu»

We should make a two-level menu. Create a block «menu» to catch the clicks on the groups and elements. We need the following elements:
— item — menu item;
— content — container for the items;
— title — group title.

Than we build an hierarchy by using this menu blocks together. 

For example, this is the most simple menu in bemjson:
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

## Block «i-geo-controller»

Block-controller which subscribes on the events of the menu-blocks «menuItemClick» and «menuGroupClick», reacts on it and takes some actions on the map.

In our example this block has the following tasks: 
— If there is a click on the placemark, a controller should center the map on this placemark and open the balloon;
— If there is a click on the group, a controller should show or hide this group. 

Besides to interact with the map properly block-controller have to know, if the map is ready for objects management. So our block «map» will fire event «map-inited» and «i-geo-controller» will listen this event and remember the link to the map.
￼

<img src="http://zloylos.me/other/imgs/ymapsbem/blocks_scheme-en.png" alt="">


For exaple, please, browse [zloylos.github.io/ymapsbem/index-en.html](zloylos.github.io/ymapsbem/index-en.html).

<img src="http://zloylos.me/other/imgs/ymapsbem/ready-en.png" alt="">

Maybe with BEM methodology the example becomes more complicated, but we get structured and easy-to-support code. Moreover we can easily scale this code. 

Thanks to Alexander Tarmolov (@tarmolov) for advice and support. ￼

<img src="http://zloylos.me/other/imgs/ymapsbem/denis.png" alt="">

<!--(Begin) Article author block-->
<div class="article-author">
    <div class="article-author__photo">
        <img class="article-author__pictures" src="http://zloylos.me/other/imgs/ymapsbem/denis.png" alt="Photo Denis Khananein">
    </div>
    <div class="article-author__info">
        <div class="article-author__row">
             <span class="article-author__name">Denis Khananein
        </div>
        <div class="article-author__row">
            Yandex Maps API Frontend Developer
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
