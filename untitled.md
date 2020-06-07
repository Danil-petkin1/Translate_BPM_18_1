# 10.1.1 Создание собственных тегов АR.

В репозиторий `rbx2` входит пакет `rbx2_ar_tags`. Мы создадим несколько AR-тегов и сохраним их в подкаталоге данных данного пакета. Разумеется, вы также можете хранить эти теги в выбранном вами пакете.

 Сначала перейдите в каталог тегов:

```text
$ roscd rbx2_ar_tags/data
```

Теперь запустите утилиту createMarker, входящую в пакет ar\_track\_alvar. Команда createMarker принимает идентификационный аргумент в интервале 0-65535, указывающий конкретный шаблон, который мы хотим сгенерировать. Начнем с ID 0:

```text
$ rosrun ar_track_alvar createMarker 0
```

Эта команда создаст файл под названием MarkerData\_0.png в текущем каталоге.  
 Вы можете просмотреть изображение с помощью утилиты, такой как eog \("глаз гнома"\):

```text
$ eog MarkerData_0.png
```

![should result in the following image:](.gitbook/assets/image%20%286%29.png)

Если вы запустите утилиту createMarker без аргументов, то увидите меню опций:

```text
$ rosrun ar_track_alvar createMarker
```

```text
Usage:
 /opt/ros/indigo/lib/ar_track_alvar/createMarker [options] argument
 65535 marker with number 65535
 -f 65535 force hamming(8,4) encoding
-1 "hello world" marker with string
-2 catalog.xml marker with file reference
-3 www.vtt.fi marker with URL
 -u 96 use units corresponding to 1.0 unit per 96 pixels
 -uin use inches as units (assuming 96 dpi)
 -ucm use cm's as units (assuming 96 dpi) <default>
 -s 5.0 use marker size 5.0x5.0 units (default 9.0x9.0)
 -r 5 marker content resolution -- 0 uses default
 -m 2.0 marker margin resolution -- 0 uses default
 -a use ArToolkit style matrix markers
 -p prompt marker placements interactively from the user 
```

Из перечисленных опций, вы, вероятно, максимально будете использовать параметр -s для регулировки размера \(по умолчанию 9 х 9 единиц\). Вы также можете использовать параметр -u для изменения единиц измерения \(по умолчанию - см\). Поэтому, чтобы сделать маркер с ID 7 размером 5 х 5 см, используйте команду:

```text
$ rosrun ar_track_alvar createMarker -s 5 7
```

**ПРИМЕЧАНИЕ:** требуется пространство между -s и 5 по сравнению с вышеприведенными значениями.  
Большие теги полезны для навигации и локализации, так как их легче распознать на большом расстоянии. Меньшие теги могут быть использованы для маркировки объектов, которые будут просматриваться на более близком расстоянии.

