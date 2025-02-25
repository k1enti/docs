# Отправлять уведомления на почтовые ящики в вашем домене

{% note warning %}

Только [администратор](../role-model.md) может настроить отправку уведомлений на внешние почтовые адреса.

{% endnote %}

По умолчанию {{ tracker-name }} отправляет уведомления на адрес электронной почты, с помощью которого пользователь осуществляет вход в сервис. Если еще до подключения {{ tracker-name }} у вас был настроен почтовый домен и заведены почтовые ящики для сотрудников, вы можете настроить перенаправление почты при помощи сервиса [{{ ya-360 }}]({{ support-business-link }}). В этом случае сотрудники смогут осуществлять вход в {{ tracker-name }} при помощи своих почтовых адресов в вашем домене, а также получать уведомления от {{ tracker-name }} на эти адреса. При этом не имеет значения, каким почтовым сервисом вы пользуетесь — после настройки вы сможете продолжить его использовать.

{% note info %}

Вы можете [настроить федерацию](../../organization/add-federation.md) в сервисе {{ org-full-name }}. В этом случае пользователи также смогут осуществлять вход в {{ tracker-name }} при помощи своих почтовых адресов в вашем домене, а также получать уведомления от {{ tracker-name }} на эти адреса. 

{% endnote %}

Чтобы настроить перенаправление почты на ящики в вашем домене, подключите домен к организации. Для этого:

1. Авторизуйтесь в [{{ ya-360 }}]({{ link-ya-360 }}) с учетной записью администратора.

1. Перейдите на [страницу **Домены**]({{ link-ya-360-domains }}).

1. [Подключите домен и подтвердите его]({{ support-business-domain-main }}).

1. Дождитесь подтверждения домена. Если у вас подключено несколько доменов, выберите основной: нажмите кнопку ![](../../_assets/tracker/menu.png) и выберите **Сделать основным**.

1. Убедитесь, что MX-записи вашего домена по-прежнему указывают на почтовые серверы, которые вы используете.

    {% cut "Как просмотреть MX-запись" %}

    1. Перейдите на сайт dig-утилиты [http://www.ip-ping.ru/dig/?host=&rt=3&server=](http://www.ip-ping.ru/dig/?host=&rt=3&server=) или [http://www.digwebinterface.com/?hostnames=&type=MX&ns=resolver&useresolver=8.8.4.4&nameservers=](http://www.digwebinterface.com/?hostnames=&amp;type=CNAME&amp;ns=resolver&amp;useresolver=8.8.4.4&amp;nameservers=).

    1. Укажите имя вашего домена (например, `yourdomain.tld`) в поле **Адрес сайта:** или **Hostnames or IP addresses:**.

    1. В выпадающем списке выберите **MX** и нажмите кнопку **Запрос** или **Dig**.

    {% endcut %}

1. [Создайте]({{ support-connect-account }}) в {{ ya-360 }} учетные записи пользователей так, чтобы логины пользователей в {{ ya-360 }} совпадали с именами ящиков на вашем почтовом сервисе. Также вы можете [импортировать]({{ support-business-import }}) почтовые ящики пользователей из других сервисов.

    {% note alert %}

    За использование сервиса {{ ya-360 }} взимается плата в соответствии с [тарифами]({{ link-ya-360-tariffs }}).

    {% endnote %}

    Теперь все письма, которые приходят на ящики пользователей, будут перенаправляться на ящики в вашем домене.

    {% note tip %}

    Настройте [Сбор потерянной почты]({{ support-connect-services }}) на случай, если вы ошиблись при создании учетных записей сотрудников. Затем создайте ящик с таким же именем на своем почтовом сервисе.

    {% endnote %}
