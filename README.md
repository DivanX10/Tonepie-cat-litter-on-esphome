# Модернизация кошачьего туалета Tonepie
Проект для ESPHome и Home Assistant


<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/3ccaa24d-f8f3-4367-8f9f-fe86b93e63e3" width=50%>


### Описание
Кошачий туалет Tonepie работает под управлением Tuya, который можно перешить на ESPHome. Мы отвязываем лоток от облачного сервиса Tuya и можем программировать свой лоток так, как считаем нужным

> Внимание! Все материалы этого проекта (прошивки, схемы и т.п.) предоставляются "КАК ЕСТЬ". Всё, что вы делаете с вашим оборудованием, вы делаете на свой страх и риск. Автор не несет ответственности за результат и ничего не гарантирует. Перепрошивка кошачьего лотка требует вмешательства, что автоматически лишит вас гарантии.


> Важно! Насыпайте наполнитель в выключенный лоток. После подачи питания, автоматический лоток делает автокалибровку и обнуляет вес наполнителя и только после этого лоток будет правильно определять вес питомца. Если же наполнитель насыпать в лоток когда он включен, то лоток будет думать, что внутри находится питомец. В таком случае обесточьте лоток по питанию и снова подайте питание. Подключите например к умной розетке, тогда удобно будет перезагружать лоток по питанию. Таким образом лоток будет запускать автокалибровку и обнулять вес наполнителя

**Функции лотка:**
1) Автоуборка
2) Уборка вручную
3) Ионизатор
4) Защита от детей
5) Инфракрасный датчик
6) Удаление наполнителя
7) Калибровка весов для наполнителя и бака для отходов
8) Автокалибровка при включении
9) Сенсоры:
    * вес питомца
    * длительность посещения
    * мусорный бак
    * питомец в лотке
    * уборка


<details>
  <summary><b>Перепрошиваем на ESPHome</b></summary>

На плате установлен чип WBR3. Подробнее о WBR3 можете ознакомится [здесь](https://developer.tuya.com/en/docs/iot/wbr3-module-datasheet?id=K9dujs2k5nriy)

<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/13c54298-f1b6-4954-8e76-75da6ae8de8b" width=70%>


Перед тем как выпаять чип WBR3, на всякий случай припаяйте два провода к контактам RXD и TXD и поснимайте логи, посмотрите, будут ли у вас работать сенсоры при добавлении компонента [Tuya MCU](https://esphome.io/components/tuya.html#tuya-mcu). Если сенсоры работают то можете продолжать процедуру дальше.

> Для справки. Обычно, чтобы снять логи подключив к контактам RXD и TXD, то подключение делается наоборот(скриншот ниже), но на мое удивление подключение было прямым, т.е не RXD>TXD и TXD>RXD, а RXD>RXD и TXD>TXD. Так что проверяйте оба варианта
>
> <img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/7361cdf6-ef66-4dec-8c91-2c80179d5288" width=30%>


<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/00c2be8c-6beb-4407-bfe3-4b0f76afba76" width=50%>

***

Чтобы включить режим отладки и выводить в логи пакеты, необходимо добавить в строки следующее. Таким образом можно увидеть пакеты на каждую команду, когда нажимаете на кнопки в приложении Tuya или через панель управления самого лотка
```
#Включить компонент Tuya MCU
tuya:

uart:
  tx_pin: GPIO1
  rx_pin: GPIO3
  baud_rate: 9600
  stop_bits: 1
  data_bits: 8
  parity: none
  debug:
    direction: BOTH
    dummy_receiver: false
```

***

Вместо WBR3 будет использоваться ESP12-F. Говорят, что WBR3 можно перепрошить, но я не имею опыта и не хочу стирать прошивку в WBR3, так как сам чип может пригодится в будущем, например припаять обратно и поснимать логи. Делайте на свое усмотрение, можете залить прошивку сразу в WBR3 или заменить на ESP12-F. 

В ESP12-F залить прошивку можно двумя способом

1) Купить программатор для модуля ESP8266

<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/95595588-8338-466d-8a28-cb83634944c6" width=50%>


2) Подключить ESP12-F к USB-TTL

<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/ddbf0afd-2b02-4627-a498-203b2f690b5d" width=100%>


> Для справки! Чтобы залить прошивку, нужно замыкать контакты GPIO0, GPIO15 и GND до подачи питания(до того как вставите USB-TTL в USB разъем компьютера), а не после, тогда ESP12-F перейдет в режим прошивки

Скомпилируйте прошивку в ESPHome используя конфигурацию на выбор. Конфигурации смотреть [здесь](https://github.com/DivanX10/Tonepie-cat-litter-on-esphome/tree/main/files/ESPHome/ru)

1) Базовая конфигурация имеет только управление и сенсоры без логики управления
2) Расширенная конфигурация имеет логику управления и статусы, а также может иметь расписание уборки. Смотрите комментарии в конфигурации.

Заливал прошивку на ESP12-F через NodeMCU Flasher. Скачать NodeMCU Flasher можно [здесь](https://github.com/nodemcu/nodemcu-flasher/tree/master). 

После заливки прошивки припаиваем ESP12-F вместо WBR3 и замыкаем контакты резисторами номиналом 10кОм. Припаивать резистор к контактам EN и 3.3v, GPIO15 и GND. Почему я не припаял перемеычку, замкнув GPIO15 и GND? Замерив мультиметров я увидел сопротивление в 326-327 кОм, а так как чип ESP12-F был уже припаян, а свободного не имелось под рукой, то не было возможности проверить контакты GPIO15 и GND на чипе и на плате лотка. Поэтому я не стал рисковать и во избежание короткого замыкания я замкнул GPIO15 и GND резистором.  

<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/c0acb144-cc21-4b69-9fdf-5c48d39733d3" width=50%>

                
</details>


<details>
  <summary><b>Фотографии</b></summary>


<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/59bfd05d-251d-46b6-8d8a-bd20bc308e6a" width=70%>
<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/bb33698b-7d6d-4e1b-aa3a-07fe911ac01d" width=70%>
<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/69f1177d-d231-48a8-b179-60d68354fe74" width=70%>
<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/dabf01f8-466f-4d3c-899f-fc6344673bc6" width=70%>
  
</details>

<details>
  <summary><b>Home Assistant</b></summary>
  

<img src="https://github.com/DivanX10/Tonepie-cat-litter-on-esphome/assets/64090632/7a338e83-c8a7-4242-aa44-fece33375e09" width=100%>

**Для работы карточки необходимо установить компоненты**
* [History explorer card](https://github.com/alexarch21/history-explorer-card)
* [Button Card](https://github.com/custom-cards/button-card)

**Карточка**
* Код карточки можно взять [здесь](https://github.com/DivanX10/Tonepie-cat-litter-on-esphome/tree/main/files/Home%20Assistant/ru)
  

Код обратного таймера. Это нужно для карточки, чтобы видеть оставшееся время работы ионизатора
```
timer:
  cat_toilet_ionizer_timer:
    name: "Кошачий лоток. Ионизатор. Таймер"
    duration: "00:30:00"
    icon: mdi:creation

- sensor:
    - name: 'Cat toilet: Ionizer. Remaining Time'
      unique_id: cat toilet ionizer remaining time
      state: >
          {% set f = state_attr('timer.cat_toilet_ionizer_timer', 'finishes_at') %}
          {{ '00:00:00' if f == None else (as_datetime(f) - now()).total_seconds() | timestamp_custom('%H:%M:%S', false) }}
      icon: mdi:timer
```

</details>

<details>
  <summary><b>3D модель</b></summary>


Я спроектировал защитный съемный бортик, так как его у меня в комплекте не было, а покупать за безумные деньги у производителя я не захотел. В итоге спроектиорвал и распечатал защитный съемный бортик. Защитный съемный бортик блокирует попадание наполнителя внутрь бака, т.е когда кошки закапывают наполнитель, то наполнитель без бортика попадает под бак. Можно купить у производителя съемный бортик

Скачать модель можно [здесь](https://github.com/DivanX10/Tonepie-cat-litter-on-esphome/tree/main/files/3D%20Printer)
***

Защитный съемный бортик для автоматического лотка туалета Tonepie

<img src="https://github.com/DivanX10/Tonepie-cat-litter-on-esphome/assets/64090632/b82b7e5f-060f-4c00-8e14-f9b11a177923" width=40%>

***

Защитный съемный бортик распечатанный на 3D принтере

<img src="https://github.com/DivanX10/Tonepie-cat-litter-on-esphome/assets/64090632/f95de27e-227f-41ed-8495-18f336531e05" width=30%>
<img src="https://github.com/DivanX10/Tonepie-cat-litter-on-esphome/assets/64090632/f2202e57-0271-498c-9660-602d294095da" width=30%>






</details>
