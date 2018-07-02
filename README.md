# magento2-thoughts
Мысли после первого знакомства с magento2

### Установка

Используем ветку 2.3 т.к. там несколько более адекватные зависимости (php 7.2 и проч).  
По нормальному устанавливать через composer, но ветка еще не релизилась, поэтому клон из гита.  

mysql:  
увеличить max_allowed_packed до 150mb  
создать бд magento2  

```bash
git clone -b 2.3-develop https://github.com/magento/magento2.git magento2.loc
cd magento2.loc

php composer.phar update --no-dev

php bin/magento setup:install --admin-firstname=admin --admin-lastname=admin --admin-email=email@admin.admin --admin-user=admin --admin-password=admin123 --cleanup-database --base-url=http://magento2.loc/ --db-host=localhost --db-name=magento2 --db-user=root --db-password=

php bin/magento deploy:mode:set developer
```

### не работает на данный момент для 2.3-develop
```bash
cd /
git clone -b 2.3-develop https://github.com/magento/magento2-sample-data.git magento2-sample-data

php -f magento2-sample-data/dev/tools/build-sample-data.php -- --ce-source="s:\OpenServer\domains\magento2.loc"

php bin/magento setup:upgrade
```

### генерация автокомплита по xsd
```bash
php bin/magento dev:urn-catalog:generate .idea/misc.xml
```


### Общие впечатления
- Писали хохло-индусы
- Плохая безсистемная документация. Набрасывающая лишь обрывки сведений.
- используется сильно устаревший технологический стек (zend framework 1!!!!)
- нет желания рефакторить старое. т.е. дальше будет хуже
- велосипедостроение. игнорируют менйстрим. ни о какой доктрине, например, не может быть и речи
- улучшают только то, что можно промаркетить для бизнеса, т.е. фасад. с фундаментом все плохо
- Дикая каша в конфигурациях. Влоть до завязки частей имен файлов на поведение.
- нет нормального хелпера для роутов. сами роуты - это какой-то пздц (многие проблемы, имхо, являются следствием основной проблемы идиотских конфигураций).
- Фишки РСУБД практически не используются. Даже банально связи и те далеко не всегда проставлены. Следовательно, в данном разрезе не имеет значения используем мы mysq/postgres или другую РСУБД. Возможно предпочтительнее даже будет перейти на nosql бд типа монги.

### Неструктурированный сумбур

https://github.com/Smile-SA/magento2-module-debug-toolbar/blob/master/composer.json  
debug toolbar  
завязка на "magento/framework": "~101.0.0"  

использование репозиториев и критериев фильтрации (привычный и имхо более адекватный подход к поиску по БД):  
https://devdocs.magento.com/guides/v2.2/extension-dev-guide/searching-with-repositories.html  
https://github.com/magento/magento2/blob/2.2/app/code/Magento/Customer/Model/ResourceModel/CustomerRepository.php#L342  

https://devdocs.magento.com/guides/v2.2/extension-dev-guide/plugins.html  
Interceptors - это декораторы! а не прокси https://ru.wikipedia.org/wiki/%D0%94%D0%B5%D0%BA%D0%BE%D1%80%D0%B0%D1%82%D0%BE%D1%80_(%D1%88%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD_%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F)  

https://github.com/netz98/n98-magerun2 утилита для управления мажентой (не поддерживает пока что 2.3 https://github.com/netz98/n98-magerun2/issues/376)  

в маженте 2.3 значительно более адекватные версии https://github.com/magento/magento2/blob/2.3-develop/composer.json (хотя все равно до адекватности далеко)  

мажента использует ПЕРВЫЙ зендфреймворк даже в вктке 2.3!!! https://github.com/magento/magento2/blob/2.3-develop/composer.json#L41  
и походу слезать с этой наркоты даже не собирается  
отсюда кстати и проблемы с совместимостью с новыми версиями php. т.к. зендом этот фреймворк более не поддерживается  

пулл реквест с симфоневским вардампером в маженту https://github.com/magento/magento2/pull/15538  
