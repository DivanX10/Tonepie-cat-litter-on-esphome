# Модернизация кошачьего туалета Tonepie
Проект для ESPHome и Home Assistant

![image](https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/4e8f79ed-e9bd-4044-a7ab-67163ae847cc)

### Описание
Кошачий туалет Tonepie работает под управлением Tuya, который можно перешить на ESPHome. Мы отвязываем лоток от облачного сервиса Tuya и можем программировать свой лоток так, как считаем нужным

> Внимание! Все материалы этого проекта (прошивки, схемы и т.п.) предоставляются "КАК ЕСТЬ". Всё, что вы делаете с вашим оборудованием, вы делаете на свой страх и риск. Автор не несет ответственности за результат и ничего не гарантирует. Перепрошивка кошачьего лотка требует вмешательства, что автоматически лишит вас гарантии.


<details>
  <summary><b>Перепрошиваем на ESPHome</b></summary>

На борту лотка установлена плата WBR3. Подробнее о WBR3 можете ознакомится [здесь](https://developer.tuya.com/en/docs/iot/wbr3-module-datasheet?id=K9dujs2k5nriy)

![1697068042669](https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/d67f6376-0f15-46ec-99c6-4aac71a149f3)

Перед тем как выпаять чип WBR3, на всякий случай припаяйте два провода к контактам RXD и TXD и поснимайте логи, посмотрите, будут ли у вас работать сенсоры при добавлении компонента [Tuya MCU](https://esphome.io/components/tuya.html#tuya-mcu). Если сенсоры работают то можете продолжать процедуру дальше.

> Для справки. Обычно, чтобы снять логи подключив к контактам RXD и TXD, то подключение делается зеркально, но на мое удивление подключение было прямым, т.е не RXD>TXD и TXD>RXD, а RXD>RXD и TXD>TXD. Так что проверяйте оба варианта

![image](https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/9a8d9b3a-10f6-4b3c-badb-704981a91678)


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

Вместо WBR3 будет использоваться ESP12-F. В ESP12-F залить прошивку можно двумя способом

1) Купить программатор для модуля ESP8266

<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/b5d0d863-def9-41d5-8c12-4779ecf15343" width=30%>

2) Подключить ESP12-F к USB-TTL

<img src="https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/97daad19-a566-4dfa-b5f8-c4cbf5071fa0" width=100%>

> Для справки! Чтобы залить прошивку, нужно замыкать контакты GPIO0, GPIO15 и GND до подачи питания(до того как вставите USB-TTL в USB разъем компьютера), а не после, тогда ESP12-F перейдет в режим прошивки

Скомпилируйте прошивку в ESPHome используя конфигурацию на выбор

1) Базовая конфигурация имеет только управление и сенсоры без логики управления, а дальше вы можете реализовать свою логику управления, выводить свои статусы. Смотреть [здесь](https://github.com/DivanX10/tonepie-cat-litter-on-esphome/blob/main/files/ESPHome/ru/esp-cat-litter-tonepie(база).yaml)
2) Расширенная конфигурация имеет логику управления и статусы. Смотреть [здесь](https://github.com/DivanX10/tonepie-cat-litter-on-esphome/blob/main/files/ESPHome/ru/cat-litter-tonepie(управление%20%2B%20статусы).yaml)

Заливал прошивку на ESP12-F через NodeMCU Flasher. Скачать NodeMCU Flasher можно [здесь](https://github.com/nodemcu/nodemcu-flasher/tree/master). 

                
</details>


<details>
  <summary><b>Фотографии</b></summary>


![image](https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/4c44927a-4bc4-436e-8566-0b363a3f0643)
![image](https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/61a17280-af63-4fee-b80c-47691e9cec2c)
![1696940907510](https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/70a9e146-7429-44bd-b5dd-edddfcafcc08)
![1696940907540](https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/dd3552f8-29a1-4be2-8264-a9dc1543f8fa)
![1697068042495](https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/10955435-a9fd-4472-8621-0dc56c1f1e16)
![1697068042553](https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/46d8f5ba-ebf4-4073-b514-4cbdb0df90ed)

  
</details>

<details>
  <summary><b>Home Assistant</b></summary>


![image](https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/5991b682-c59c-4b0e-a93a-d3c8bee6533d)
![image](https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/f12e2cab-5a63-4f11-9570-3da68a9d743b)
![image](https://github.com/DivanX10/tonepie-cat-litter-on-esphome/assets/64090632/2ebac520-c851-456b-9b08-60032c2b1672)

**Для работы карточки необходимо установить компоненты**
* [History explorer card](https://github.com/alexarch21/history-explorer-card)
* [Button Card](https://github.com/custom-cards/button-card)

**Карточка и шаблоны**
* Код карточки можно взять [здесь](https://github.com/DivanX10/tonepie-cat-litter-on-esphome/blob/main/files/Home%20Assistant/ru/Карточка%20управления%20лотком.yaml)
  
</details>
